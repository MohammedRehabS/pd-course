# Contents
 
 <div class="toc">
  <ul>
    <li><a href="#header-0"> Overview</a></li>
	</ul>
 </div>
 <div class="toc">
  <ul>
    <li><a href="#header-1">Day 1 Synthesis and flop ratio calculation</a></li>
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
 <div class="toc">
  <ul>
    <li><a href="#header-2">Day 2 </a></li>
	<ul>
        	<li><a href="#header-2-1"> Summary</a></li>
	</ul>
	<ul>
		<li><a href="#header-2-2">Theory</a></li>
		<ul>
			<li><a href="#header-2-2-1">Floor Planning</a></li>
		</ul>
		<ul>
			<li><a href="#header-2-2-2">Placement</a></li>
		</ul>
	</ul>
	<ul>
		<li><a href="#header-2-3">Floorplanning lab</a></li>
		<ul>
			<li><a href="#header-2-3-1">Running Floorplanning</a></li>
		</ul>
		<ul>
			<li><a href="#header-2-3-2">Reviewing results</a></li>
			<u1>
				<li><a href="#header-2-3-2-1">Manually checking the def</a></li>
			</u1>
			<u1>
				<li><a href="#header-2-3-2-2">Calculating Core and Die area and utilization ratio</a></li>
			</u1>
			<u1>
				<li><a href="#header-2-3-2-3">Reviewing the def using Magic</a></li>
			</u1>
		</ul>
	</ul>
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


# <h2 id="header-2">Day 2</h2>

## <h2 id="header-2-1">Summary</h2>
The goal of Day 2 is to perform floorplanning and placement.

## <h2 id="header-2-2">Theory</h2>

### <h2 id="header-2-2-1">Floor Planning:</h2>

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

PDN is generally implemented during floorplanning but here it will happen after CTS.

### <h2 id="header-2-2-2">Placement:</h2>

Placement is done post placement of blocks and IO pins. 

The idea is to take the library information of the cells to implement the netlist on the design and come up with an optimal placement which could possibly pass timing.

![image](https://github.com/user-attachments/assets/24951443-c606-4e5b-b05e-e47e022d3ada)

As seen below first the instances are placed according to the netlist on the design instances close to the pins are placed near the IO pads. 

If there are hard paths then placement is performed and then based on wireload estimations cap is calculated and  if slew threshold is not met buffers are added to regenerate the signals and help with signal integrity.

![image](https://github.com/user-attachments/assets/656327cd-f74e-41fd-9038-da219b94c956)

After this is done then congestion aware analysis considering ideal clocks.

Generally placement is done in two stages
1. Global placement : Coarse placement and no legalizations considered
2. Detailed placement: legalizations happen here where instances are placed according to standard cell rows.

## <h2 id="header-2-3">Floorplanning lab</h2>

### <h2 id="header-2-3-1">Running Floorplanning</h2>

***run_floorplan*** command is used to run floor planning. All the system default commands are present in the following folder:

![image](https://github.com/user-attachments/assets/69bae8fc-43ed-48f3-bf36-87ce57f42c7a)

README.md file contains information about all keywords for all different steps as shown below for floorplanning.

![image](https://github.com/user-attachments/assets/670b53da-1489-494b-9636-299e9aa72cca)

Running floorplanning:

![image](https://github.com/user-attachments/assets/e791d1ae-545b-4e1a-ae4c-af93eec42a49)

### <h2 id="header-2-3-2">Reviewing results</h2>

#### <h2 id="header-2-3-2-1">Manually checking the def</h2>

Output is a def file:

![image](https://github.com/user-attachments/assets/097762b3-485c-4f0f-960a-7a1a4a16b1a5)

409 pins present with location defined

21230 components ( synthesis had 14876, difference is because of tap cells and decaps which are 6354 in number)

All logic instances don't have placement defined. Tap cells and decaps are placed.

Nets have logical connectivity defined but no routing as expected

#### <h2 id="header-2-3-2-2">Calculating Core and Die area and utilization ratio</h2>

$$ \texttt{ Die Area} = \texttt{length} * \texttt{height} = {(660685-0) \over 1000} * {(671405-0) \over 1000} = 443587.21 \texttt{sq microns} $$

Core area: 
![image](https://github.com/user-attachments/assets/eab46e48-5abc-4416-b54c-b9a58a8ca656)

$$ \texttt{ Core Area} = \texttt{width} * \texttt{height} = (655.04 - 5.52) * (658.24-10.88) = 420473.26 \texttt{sq microns} $$

chip area reported during synthesis:

![image](https://github.com/user-attachments/assets/ed0d76b8-cb36-4df2-8eb1-cc969aa1fd28)

core utilization ratio reported:

![image](https://github.com/user-attachments/assets/cd05068d-ab6e-4ef9-a4bd-85e85703d882)

$$ \texttt{Calculated core utilization ratio from observation} = {147712.92 \over 420473.26} = 0.3513 $$

#### <h2 id="header-2-3-2-3">Reviewing the def using Magic</h2>

Following command is run to open Magic to review def file:

![image](https://github.com/user-attachments/assets/4a16974a-77cf-4de9-9b72-e74a510ff381)

Keys used in magic:
Key  | Description
| :---: | :---
S | Selects instance or object
V | centers the design
Z | Zooms in
arrows | Move around in the design
left click and the right click other location | Creates a box for reference while zooming in
'what' in tkcon | query attributes of object selected

Layout window:

![image](https://github.com/user-attachments/assets/b9a25eb1-5782-4c9c-88f5-c7527cf78636)

As seen all the instances in lower left. Tap cells diagonally equidistant and all pins random but equidistant as FP_IO_MODE is set to 1.

Querying attributes of a decap:

![image](https://github.com/user-attachments/assets/5eafa019-a144-4744-9b2c-20ae4df02da3)

## <h2 id="header-2-4">Placement lab</h2>

### <h2 id="header-2-4-1">Running placement</h2>

To run placement below command is run:
![image](https://github.com/user-attachments/assets/461bb7df-b0bc-423b-9317-25fb0b0a54ac)

placement optimizes a parameter called ***HPWL (Half parameter wirelength)*** and waits for overflow to converge

Both detailed and global placement is performed. Congestion aware placement is done.

### <h2 id="header-2-4-2">Reviewing results</h2>

Placed def is viewed in magic:
![image](https://github.com/user-attachments/assets/9ad1ca7f-3ece-4301-b702-5a9bc497706e)

![image](https://github.com/user-attachments/assets/ebbcc4e6-3bf3-4b28-89a0-5d9a34b7f273)
