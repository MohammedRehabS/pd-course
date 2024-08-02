

# Overview

Term  | Description
| :---: | :---
Skywater 130nm | Skywater 130nm is a open source PDK (Process Design Kit) released in collaboration between Google and Skywater. It has Apache 2.0 licensing.
OpenLane  | OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. The flow performs all ASIC implementation steps from RTL all the way down to GDSII.

**OpenLane Flow:**
![image](https://github.com/user-attachments/assets/72d3394c-116d-4976-a4bc-8feed420e8b3)

The goal of this course is to use OpenLane to go through RTL2GDS flow for a design called ***picorv32a*** on ***skywater_130A_sky130_fd_sc_hd*** technology.

<details>
<summary> Day 1 </summary>

## Summary
The goal of Day 1 is to synthesize the design and figure out the flop ratio.

## Paths and setup
The PDK files used for this project are here:
![image](https://github.com/user-attachments/assets/7e116f27-a0bd-4868-ac2d-c66e1c6e39a4)

The design files (Verilog and SDC) and the config files are present here:
![image](https://github.com/user-attachments/assets/1627f9c4-a06b-4133-a67c-12e33bffa807)

The order of precedence for config file is: 
_skywater_130A_sky130_fd_sc_hd_config.tcl_ > _config.tcl_ > _Default values in OpenLane_

Opening OpenLane:
![image](https://github.com/user-attachments/assets/a7b7727f-e2fe-4f71-bf3f-e840b55f9dac)

Importing necessary packages and preparing the design to be run (brings up the PDK merges multiple files for example lef,lib, etc):
![image](https://github.com/user-attachments/assets/a798b3eb-edc1-40cc-8944-8cfde423298c)

Output after this command:
![image](https://github.com/user-attachments/assets/c0996ef2-e877-4d28-bbc0-248bdb747941)

file/dir  | Description
| :---: | :---
cmd.log | Contains all commands run.
config.tcl | Contains all the keywords with values mentioned. If nothing is set by user, default values are reported.
logs | For each step in the flow contains the log files
reports | For each step in the flow contains the report files
tmp | Contains all the merged/trimmed PDK files after importing

## Running Synthesis to figure out flop ratio
run_synthesis command used for performing synthesis to output a gate level netlist
End of synthesis command:
![image](https://github.com/user-attachments/assets/1070d0a4-20aa-4836-8473-edad962145ca)

Output gate level netlist:
![image](https://github.com/user-attachments/assets/fe29091f-84c5-41a9-a327-d108bfa5eccb)
![image](https://github.com/user-attachments/assets/81782707-e206-4f4f-b331-54f445c3c9fc)

Checking the flop ratio:
flop ratio is given by:

$$ \texttt{flop ratio} = {\texttt{num of flops} \over  \texttt{total number of cells}} $$

Log file:

![image](https://github.com/user-attachments/assets/b8d7ab5f-12e1-4dff-ae5c-9a7968269338)


$$ \texttt{flop ratio} = {1613 \over 14876} = 0.1084 $$

$$ \texttt{percentage of flops} = 10.84 $$
