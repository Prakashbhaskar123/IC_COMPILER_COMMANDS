# IC_COMPILER_COMMANDS




Outputting Contact $$VIA12SQ_C_120_120_2_1
write_gds completed successfully!
1
icc_shell> history
     1  set sh_output_log_file "logs/init_design.log"
     2  set LIBRARY_HOME "../../../library_32nm/SAED32_EDK" ;# Parent home for synopsys libraries
     3  set LOGICAL_LIBRARY_PATH  "$LIBRARY_HOME/lib/stdcell_rvt/db_nldm " ;#  Additional search path to be added to the default search path
     4  set TARGET_LIBRARY_FILES      "saed32rvt_ss0p95v125c.db"  ;#  Target technology logical libraries
     5  ## update library files
     6  set_app_var search_path "${LOGICAL_LIBRARY_PATH} $search_path"
     7  set_app_var target_library ${TARGET_LIBRARY_FILES}
     8  set_app_var link_library "* $target_library"
     9  set MW_REFERENCE_LIB_DIRS " ${LIBRARY_HOME}/lib/stdcell_rvt/milkyway/saed32nm_rvt_1p9m " ;# milkyway reference libraries
    10  set mw_logic0_net VSS
    11  set mw_logic1_net VDD
    12  set_tlu_plus_files -max_tluplus ${LIBRARY_HOME}/tech/star_rcxt/saed32nm_1p9m_Cmax.tluplus -min_tluplus ${LIBRARY_HOME}/tech/star_rcxt/saed32nm_1p9m_Cmin.tluplus  -tech2itf_map ${LIBRARY_HOME}/tech/star_rcxt/saed32nm_tf_itf_tluplus.map
    13  create_mw_lib -technology ${LIBRARY_HOME}/tech/milkyway/saed32nm_1p9m_mw.tf -mw_reference_library $MW_REFERENCE_LIB_DIRS fifo_design.mw
    14  create_mw_lib -technology ${LIBRARY_HOME}/tech/milkyway/saed32nm_1p9m_mw.tf -mw_reference_library $MW_REFERENCE_LIB_DIRS fifo_design.mw
    15  open_mw_lib fifo_design.mw
    16  read_verilog ../../dc_synth/synth_fifo/output/fifo_HDL.v
    17  current_design FIFO
    18  uniquify_fp_mw_cel
    19  link
    20  read_sdc ../../dc_synth/synth_fifo/const/fifo.sdc
    21  save_mw_cel -as fifo_inital
    22  start_gui
    23  set sh_output_log_file "logs/floorplan.log"
    24  create_floorplan -control_type aspect_ratio -core_aspect_ratio 1 -core_utilization .75  -row_core_ratio 1  -start_first_row -left_io2core 5.0 -bottom_io2core 5.0 -right_io2core 5.0 -top_io2core 5.0
    25  derive_pg_connection -power_net VDD -ground_net VSS
    26  derive_pg_connection -power_net VDD -ground_net VSS -tie
    27  set_fp_rail_constraints  -skip_ring -extend_strap boundary
    28  set_fp_rail_constraints -add_layer  -layer M6 -direction vertical -max_strap 5 -min_strap 3 -min_width 0.056 -max_width 0.3 -spacing 0.8 -offset 10.01
    29  set_fp_rail_constraints -add_layer  -layer M5 -direction horizontal -max_strap 10 -min_strap 6 -min_width 0.05  -max_width 0.2 -spacing 0.5 -offset 5.02
    30  synthesize_fp_rail  -nets {VDD VSS} -synthesize_power_plan -use_strap_ends_as_pads -voltage_supply 0.95 -power_budget 100 -target_voltage_drop 50
    31  commit_fp_rail
    32  preroute_standard_cells  -fill_empty_rows -do_not_route_over_macros -skip_macro_pins
    33  write_def -output output/fifo_fp.def
    34  save_mw_cel -as fifo_fp
    35  puts "floorplan done!!!!!"
    36  start_gui
    37  set sh_output_log_file "logs/place.log"
    38  set_buffer_opt_strategy -effort low
    39  set_tlu_plus_files -max_tluplus $LIBRARY_HOME/tech/star_rcxt/saed32nm_1p9m_Cmax.tluplus -min_tluplus $LIBRARY_HOME/tech/star_rcxt/saed32nm_1p9m_Cmin.tluplus  -tech2itf_map $LIBRARY_HOME/tech/star_rcxt/saed32nm_tf_itf_tluplus.map
    40  place_opt -congestion
    41  save_mw_cel -as fifo_place
    42  write_def -output output/fifo_place.def
    43  report_placement_utilization > output/fifo_place_util.rpt
    44  report_qor_snapshot > output/fifo_place_qor_snapshot.rpt
    45  report_qor > output/fifo_place_qor.rpt
    46  report_timing -delay max -max_paths 20 > output/fifo_place.setup.rpt
    47  ##set_case_analysis 1 scanEn
    48  report_timing -delay min -max_paths 20 > output/fifo_place.hold.rpt
    49  report_qor
    50  report_timing
    51  Area
    52  -----------------------------------
    53  Combinational Area:     1472.510351
    54  Noncombinational Area:  1564.510504
    55  Buf/Inv Area:            102.674177
    56  Total Buffer Area:            33.04
    57  Total Inverter Area:          69.64
    58  Macro/Black Box Area:      0.000000
    59  Net Area:                698.540233
    60  Net XLength        :        5872.85
    61  Net YLength        :        5892.68
    62  -----------------------------------
    63  start_gui
    64  set sh_output_log_file "logs/cts.log"
    65  man clock_opt
    66  clock_opt -only_cts
    67  save_mw_cel -as fifo_cts
    68  set icc_snapshot_storage_location  "reports" ;# This setting is needed when default snapshot directory is not available
    69  report_placement_utilization > reports/fifo_cts_util.rpt
    70  report_qor_snapshot > reports/fifo_cts_qor_snapshot.rpt
    71  report_qor > reports/fifo_cts_qor.rpt
    72  report_timing -max_paths 20 -delay max > reports/fifo_cts.setup.rpt
    73  report_timing -max_paths 20 -delay min > reports/fifo_cts.hold.rpt
    74  set sh_output_log_file "logs/route.log"
    75  man route_opt
    76  route_opt
    77  verify_zrt_route -open_net true -drc true
    78  save_mw_cel -as fifo_route
    79  write_def -output output/fifo_route.def
    80  report_placement_utilization
    81  report_placement_utilization > reports/fifo_route_util.rpt
    82  report_qor_snapshot > reports/fifo_route_qor_snapshot.rpt
    83  report_qor > reports/fifo_route_qor.rpt
    84  report_timing -max_paths 20 -delay max > reports/fifo_route.setup.rpt
    85  report_timing -max_paths 20 -delay min > reports/fifo_route.hold.rpt
    86  set_physical_signoff_options -exec_cmd {icv} -drc_runset {../../../library_32nm/SAED32_EDK/tech/icv_drc/saed32nm_1p9m_drc_rules.rs}
    87  signoff_drc -show_stream_error_environment false -read_cel_view -run_dir {./signoff_drc_run} -max_errors_per_rule 1000
    88  set sh_output_log_file "logs/extract.log"
    89  extract_rc  -coupling_cap  -routed_nets_only  -incremental
    90  write_parasitics -output ./output/fifo_extracted.spef -format SPEF
    91  write_sdf ./output/fifo_extracted.sdf
    92  write_sdc ./const/fifo_extracted.sdc
    93  write_verilog ./output/fifo_extracted.v
    94  report_power
    95  report_timing -max_paths 20 -delay max > reports/fifo_extracted.setup.rpt
    96  report_timing -max_paths 20 -delay min > reports/fifo_extracted.hold.rpt
    97  report_power > reports/fifo_power.rpt
    98  save_mw_cel -as fifo_extracted
    99  start_gui
   100  # Replace with the actual filler cell names in your SAED32 library
   101  # Usually named something like SHFILL1, SHFILL2, SHFILL64, etc.
   102  insert_stdcell_filler -cell_with_metal {SHFILL128 SHFILL64 SHFILL32 SHFILL16 SHFILL8 SHFILL4 SHFILL2 SHFILL1}                       -connect_to_power VDD -connect_to_ground VSS
   103  get_cells -all *FILL*
   104  # OR
   105  get_lib_cells */*FILL*
   106  insert_stdcell_filler -cell_with_metal {SHFILL128_RVT SHFILL64_RVT SHFILL3_RVT SHFILL2_RVT SHFILL1_RVT}                       -connect_to_power VDD -connect_to_ground VSS
   107  gui_start
   108  # Replace the path with your actual mapping file path if you have one.
   109  # If you don't have a map file, the tool will use default internal mapping.
   110  set_write_stream_options -child_depth 99 -output_pin {text geometry}
   111  # If you don't know your library name, type 'current_mw_lib' to find it.
   112  write_stream -format gds              -lib_name [current_mw_lib]              -cells {fifo_extracted}              ./output/fifo_final.gds
   113  current_mw_lib
   114  # Save the current state with fillers included
   115  save_mw_cel -as fifo_final_ready
   116  # Set the stream options again
   117  set_write_stream_options -child_depth 99 -output_pin {text geometry}
   118  # Write the GDS using the newly saved cell name
   119  write_stream -format gds              -lib_name [current_mw_lib]              -cells {fifo_final_ready}              ./output/fifo_final.gds
   120  write_stream -format gds              -lib_name fifo_design.mw              -cells {fifo_final_ready}              ./output/fifo_final.gds
   121  history
icc_shell> 
