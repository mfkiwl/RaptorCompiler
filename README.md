[![main](https://github.com/RapidSilicon/Raptor/actions/workflows/main.yml/badge.svg)](https://github.com/RapidSilicon/Raptor/actions/workflows/main.yml)

# Raptor Preview Release

Rapid Silicon Raptor Design Suite

Raptor is a classic RTL (User design + IPs) 2 Bitstream FPGA compiler.
It can be ran in batch mode or GUI mode with complete Tcl scripting capability.

## INSTALLATION

See [`Install Raptor`](INSTALL.md)

## LICENSING

Raptor uses the FlexLM License manager, please setup the license file:
```setenv LM_LICENSE_FILE <path to license file or lmgrd IP address>```

## RAPTOR OPTIONS

```
raptor --help
-----------------------------------------------
--- Rapid Silicon Raptor Design Suite help  ---
-----------------------------------------------
Options:
   --help           : This help
   --version        : Version
   --batch          : Tcl only, no GUI
   --script <script>: Execute a Tcl script
   --project <project file>: Open a project
   --mute           : mutes stdout in batch mode
Tcl commands (Available in GUI or Batch console or Batch script):
   help                       : This help
   open_project <file>        : Opens a project in the GUI
   run_project <file>         : Opens and immediately runs the project
   create_design <name> ?-type <project type>? : Creates a design with <name> name
               <project type> : rtl (Default), gate-level
   target_device <name>       : Targets a device with <name> name (1GE75)
   add_design_file <file list> ?type?   ?-work <libName>?   ?-L <libName>? 
              Each invocation of the command compiles the file list into a compilation unit 
                       <type> : -VHDL_1987, -VHDL_1993, -VHDL_2000, -VHDL_2008, -VHDL_2019, -V_1995, -V_2001, -SV_2005, -SV_2009, -SV_2012, -SV_2017> 
              -work <libName> : Compiles the compilation unit into library <libName>, default is "work"
              -L <libName>    : Import the library <libName> needed to compile the compilation unit, default is "work"
   add_simulation_file <file list> ?type?   ?-work <libName>?   ?-L <libName>? 
              Each invocation of the command compiles the file list into a compilation unit 
                       <type> : -VHDL_1987, -VHDL_1993, -VHDL_2000, -VHDL_2008, -VHDL_2019, -V_1995, -V_2001, -SV_2005, -SV_2009, -SV_2012, -SV_2017, -C, -CPP> 
              -work <libName> : Compiles the compilation unit into library <libName>, default is "work"
              -L <libName>    : Import the library <libName> needed to compile the compilation unit, default is "work"
   clear_simulation_files     : Remove all simulation files
   read_netlist <file>        : Read a netlist (.blif/.eblif) instead of an RTL design (Skip Synthesis)
   add_include_path <path1>...: As in +incdir+    (Not applicable to VHDL)
   add_library_path <path1>...: As in +libdir+    (Not applicable to VHDL)
   add_library_ext <.v> <.sv> ...: As in +libext+ (Not applicable to VHDL)
   set_macro <name>=<value>...: As in -D<macro>=<value>
   set_top_module <top> ?-work <libName>? : Sets the top module
   add_constraint_file <file> : Sets SDC + location constraints
                                Constraints: set_pin_loc, set_mode, all SDC Standard commands
   set_pin_loc <design_io_name> <device_io_name> ?<internal_pinname>?: Constraints pin location (Use in constraint.pin file)
   set_property mode <io_mode_name> <device_io_name> : Constraints pin mode (Use in constraint.pin file)
   set_clock_pin -device_clock <device_clock_name> -design_clock <design_clock_name>
                              : like gemini has 16 clocks clk[0],clk[1]....,clk[15].
                                and e.g. user clocks are clk_a, clk_b and want to map clk_a with clk[15]
                               Constraints : set clocks for repacking constraints (Use in constraint.pin file)
   script_path                : Returns the path of the Tcl script passed with --script
   keep <signal list> OR all_signals : Keeps the list of signals or all signals through Synthesis unchanged (unoptimized in certain cases)
   add_litex_ip_catalog <directory> : Browses directory for LiteX IP generators, adds the IP(s) to the IP Catalog
   ip_catalog ?<ip_name>?     : Lists all available IPs, and their parameters if <ip_name> is given 
   ip_configure <IP_NAME> -mod_name <name> -out_file <filename> -version <ver_name> -P<param>="<value>"...
                              : Configures an IP <IP_NAME> and generates the corresponding file with module name
   ipgenerate ?clean?         : Generates all IP instances set by ip_configure
   message_severity <message_id> <ERROR/WARNING/INFO/IGNORE> : Upgrade/downgrade RTL compilation message severity
   verific_parser <on/off>    : Turns on/off Verific Parser
   synthesis_type Yosys/QL/RS : Selects Synthesis type
   custom_synth_script <file> : Uses a custom Yosys templatized script
   max_threads <-1/[2:64]>    : Maximum number of threads to be used (-1 is for automatic selection)
   synth_options <option list>: RS-Yosys Plugin Options. The following defaults exist:
                              :   -effort high
                              :   -fsm_encoding binary if optimization == area else onehot
                              :   -carry auto
                              :   -clke_strategy early
     -effort <level>          : Optimization effort level (high, medium, low)
     -fsm_encoding <encoding> : FSM encoding:
       binary                 : Compact encoding - using minimum of registers to cover the N states
       onehot                 : One hot encoding - using N registers for N states
     -carry <mode>            : Carry logic inference mode:
       all                    : Infer as much as possible
       auto                   : Infer carries based on internal heuristics
       none                   : Do not infer carries
     -fast                    : Perform the fastest synthesis. Don't expect good QoR.
     -no_simplify             : Do not run special simplification algorithms in synthesis. 
     -clke_strategy <strategy>: Clock enable extraction strategy for FFs:
       early                  : Perform early extraction
       late                   : Perform late extraction
     -cec                     : Dump verilog after key phases and use internal equivalence checking (ABC based)
   analyze ?clean?            : Analyzes the RTL design, generates top-level, pin and hierarchy information
   synthesize <optimization>  ?clean? : RTL Synthesis, optional opt. (area, delay, mixed, none)
   pin_loc_assign_method <Method>: Method choices:
                                in_define_order(Default), port order pin assignment
                                random , random pin assignment
                                free , no automatic pin assignment
   pnr_options <option list>  : VPR options
   clb_packing   <directive>  : Performance optimization flags
                 <directive>  : auto, dense
                        auto  : CLB packing automatically determined to optimize performance
                       dense  : Pack logic more densely into CLBs resulting in fewer utilized CLBs however may negatively impact timing
   pnr_netlist_lang <blif, edif, verilog, vhdl> : Chooses post-synthesis netlist format
   set_channel_width <int>    : VPR Routing channel setting
   set_limits <type> <int>    : Sets a user limit on object of type <type>
                       <type> : dsp, bram
                          dsp : Max usable DSPs
                         bram : Max usable BRAMs
   architecture <vpr_file.xml> ?<openfpga_file.xml>?
                              : Uses the architecture file and optional openfpga arch file (For bitstream generation)
   custom_openfpga_script <file> : Uses a custom OpenFPGA templatized script
   bitstream_config_files -bitstream <bitstream_setting.xml> -sim <sim_setting.xml> -repack <repack_setting.xml> -key <fabric_key.xml>
                              : Uses alternate bitstream generation configuration files
   set_device_size XxY        : Device fabric size selection
   packing ?clean?            : Packing
   place ?clean?              : Detailed placer
   route ?clean?              : Router
   sta ?clean? ?opensta?      : Statistical Timing Analysis with Tatum (Default) or OpenSTA
   bitstream ?force? ?clean?  : Bitstream generation
   set_top_testbench <module> : Sets the top-level testbench module/entity
   simulation_options <simulator> <phase> ?<level>? <options>
                                Sets the simulator specific options for the specified phase
                      <phase> : compilation, elaboration, simulation, extra_options
   simulate <level> ?<simulator>? ?<waveform_file>?: Simulates the design and testbench
                      <level> : rtl, gate, pnr. rtl: RTL simulation, gate: post-synthesis simulation, pnr: post-pnr simulation
                  <simulator> : verilator, ghdl, icarus
   diagnostic <type>: Debug mode. Types: packer
   chatgpt <command> "<message>" ?-c <path>?: Send message to ChatGPT
                    <command> : Support two commands: send and reset
                         send : Send message
                        reset : Reset history
                    -c <path> : Specify ini file path with API key. The key needs to be set only once for a session
                                [OpenAI]
                                API_KEY: <api key>
   wave_*                     : All wave commands will launch a GTKWave process if one hasn't been launched already. Subsequent commands will be sent to the launched process.
   wave_cmd ...               : Sends given tcl commands to GTKWave process. See GTKWave docs for gtkwave:: commands.
   wave_open <filename>       : Load given file in current GTKWave process.
   wave_refresh               : Reloads the current active wave file
   wave_show <signal>         : Add the given signal to the GTKWave window and highlight it.
   wave_time <time>           : Set the primary marker to <time>. Time units can be specified, without a space. Ex: wave_time 100ps.
   program_device <-b> "<bitstream_file>" <-c> "<config_file>" <-n> "<index>": Perform device programming.
                                                                           -b: <bitstream_file>: Specify bitstream file path to program. Ex: -b /home/user/mybitstream.bit
                                                                           -c: <config_file>: Specify config file. Ex: -c gemini.cfg
                                                                           -n: <index>: Optional. Default value is 0. Specify index of the device. Ex. -n 0
-----------------------------------------------
```

## RAPTOR EXAMPLE DESIGNS

```
Example designs are installed with Raptor under:

<install_path>/share/raptor/examples (ie: /usr/local/share/raptor/examples)

To run one of the examples, cd into any local user directory and run:

raptor --script $RAPTOR_PATH/share/raptor/examples/and2_gemini/raptor.tcl (GUI mode)
or
raptor --batch --script $RAPTOR_PATH/share/raptor/examples/and2_gemini/raptor.tcl (Batch mode)

A project directory gets created with all the artefacts generated by Raptor.

A log file gets created too: raptor.log with all the compilation steps logs agregated.

Example designs:

and2_gemini       : an AND2 design targeting the 1GE75 device
                    raptor --script $RAPTOR_PATH/share/raptor/tcl_examples/and2_gemini/raptor.tcl

oneff             : an async-reset FF design targeting the 1GE75 device with Verilator simulation
                    raptor --script $RAPTOR_PATH/share/raptor/tcl_examples/oneff/raptor.tcl

aes_decrypt_fpga  : an AES decryption design targeting the 1GE75 device
                    raptor --script $RAPTOR_PATH/share/raptor/tcl_examples/aes_decrypt_fpga/aes_decrypt.tcl

ip_gen_axis_conv  : a LiteX IP generation example targeting the 1GE75 device (Requires LiteX installed separately)
                    raptor --script $RAPTOR_PATH/share/raptor/tcl_examples/ip_gen_axis_conv/raptor.tcl

sasc_testcase    : a FIFO design targeting the 1GE75 device
                    raptor --script $RAPTOR_PATH/share/raptor/tcl_examples/sasc_testcase/raptor.tcl
```

## User design


### Synthesis considerations

```
Each call to the "add_design_file" creates a separate Compilation Unit (In the Verilog sense),
the files in that call share the same macro definition space.

Verilog and VHDL files cannot be mixed in the same "add_design_file" call.

VHDL Libraries are not supported at this time, only individual .vhdl file compilation is.

```

### SDC considerations

```

A .sdc file (Tcl file with special handling of square brackets[]) can be provided using the "add_constraint_file" command.
The file and contain classic SDC commands, Tcl scripting and set_pin_loc commands.
SDC are honored by the Placement and STA stages.
Synthesis will preserve with best effort the names of the signals referenced in the contraints in order for the Placement and STA to find them in the netlist.

```
