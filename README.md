# Contents
 
 <div class="toc">
  <ul>
    <li><a href="#header-0"> Overview</a></li>
	</ul>
 </div>
 <div class="toc">
  <ul>
    <li><a href="#header-1">Day 1 Floorplanning and flop ratio calculation</a></li>
	<ul>
        <li><a href="#header-1-1"> Summary</a></li>
  </ul>
		<ul>
			<li><a href="#header-1-2">Paths and setup</a></li>
		</ul>
		<ul>
			<li><a href="#header-1-3">Running Synthesis to figure out flop ratio</a></li>
		</ul>
 </div>
        
# <h0 id="header-0">Overview</h0>

Term  | Description
| :---: | :---
Skywater 130nm | Skywater 130nm is a open source PDK (Process Design Kit) released in collaboration between Google and Skywater. It has Apache 2.0 licensing.
OpenLane  | OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. The flow performs all ASIC implementation steps from RTL all the way down to GDSII.

**OpenLane Flow:**
![image](https://github.com/user-attachments/assets/72d3394c-116d-4976-a4bc-8feed420e8b3)

The goal of this course is to use OpenLane to go through RTL2GDS flow for a design called ***picorv32a*** on ***skywater_130A_sky130_fd_sc_hd*** technology.

# <h1 id="header-1">Day 1</h1>

## <h1 id="header-1-1">Summary</h1>
The goal of Day 1 is to synthesize the design and figure out the flop ratio.

## <h1 id="header-1-2">Paths and setup</h1>
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

## <h1 id="header-1-3">Running Synthesis to figure out flop ratio</h1>
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


# Day 2

## Summary
The goal of Day 2 is to perform floorplanning and placement.

## Theory

### Floor Planning considerations:

Core vs Die:

![image](https://github.com/user-attachments/assets/cf74b383-5f76-4617-9790-dbaf223c216c)

Core is the inner region and die is the total region. Pad are placed in the middle.

$$ \texttt{utilization ratio} = {\texttt{Total area of the netlist} \over \texttt{Total area of the core}} $$

$$ \texttt{aspect ratio} = {\texttt{core height} \over \texttt{core width}} $$

Utilization ratio is generally kept between 0.5-0.6 in order to allow room for optimizations, decaps, routing. All these parameters can be controlled using keywords.

Floorplanning is performed before placement in order to place blocks (pre placed cells). This fixes the positions and blockages are created for the placer in this region. Blocks are created by cutting the logic and deciding pins. These are assigned to block owners for implementation with guidance on area and dimensions. Blocks are cells which are generally repeated multiple times throughout the design or some complicated logic.

The blocks placement is an optimization problem where the parameters are closeness to respective ports, order in which the blocks operate, etc. Flylines are used in this stage to determine their placement.

Decoupling capacitors provide a local source of charge for instances rather than from the power supply. These charge when there is no switching happening and provide charge when the instances switch. These help alleviate dynamic voltage drops to some extent. Decaps are generally placed all around the blocks.

Having just one pathway for power delivery causes IR drop irrespective of presence of decaps as decaps can charge upto Vsource - IR. Creating a grid structure is important as it ensures multiple path ways for various different instances.

The area between the core and die is blocked using logical cell placement blockage so that the placer does not place any cells here. Pins are placed here, pins can be equidistant or at random distance, Also the pin placement should take into consideration the block inputs. Clock pins are larger than signal as they have huge fanout and need less resistance.

## Floorplanning lab

***run_floorplan*** command is used to run floor planning. All the system default commands are present in the following folder:

![image](https://github.com/user-attachments/assets/69bae8fc-43ed-48f3-bf36-87ce57f42c7a)

README.md file contains information about all keywords for all different steps as shown below for floorplanning.

![image](https://github.com/user-attachments/assets/670b53da-1489-494b-9636-299e9aa72cca)

Running floorplanning:

![image](https://github.com/user-attachments/assets/e791d1ae-545b-4e1a-ae4c-af93eec42a49)

Output is a def file:

![image](https://github.com/user-attachments/assets/097762b3-485c-4f0f-960a-7a1a4a16b1a5)

$$ \texttt{ Die Area} = \texttt{length} * \texttt{height} = {(660685-0) \over 1000} * {(671405-0) \over 1000} = 443587.21 \texttt{sq microns} $$

Core area: 
![image](https://github.com/user-attachments/assets/eab46e48-5abc-4416-b54c-b9a58a8ca656)

$$ \texttt{ Core Area} = \texttt{length} * \texttt{height} = (655.04 - 5.52) * (658.24-10.88) = 420473.26 \texttt{sq microns} $$

chip area reported during synthesis:

![image](https://github.com/user-attachments/assets/ed0d76b8-cb36-4df2-8eb1-cc969aa1fd28)

core utilization ratio reported:

![image](https://github.com/user-attachments/assets/cd05068d-ab6e-4ef9-a4bd-85e85703d882)

$$ \texttt{Calculated core utilization ratio from observation} = {147712.92 \over 420473.26} = 35.13\texttt{%} $$

