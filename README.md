
# Contents
 
<div class="toc">
	<ul>
    		<li><a href="#header-0"> Overview </a></li>
	</ul>
</div>
<div class="toc">
	<ul>
    		<li><a href="#header-1"> Day 1: Synthesis and flop ratio calculation </a></li>
	<ul>
        	<li><a href="#header-1-1"> Summary </a></li>
 	</ul>
	<ul>
		<li><a href="#header-1-2"> Paths and setup </a></li>
	</ul>
	<ul>
		<li><a href="#header-1-3"> Running Synthesis to figure out flop ratio </a></li>
	</ul>
	</ul>
 </div>
 <div class="toc">
  	<ul>
    		<li><a href="#header-2"> Day 2: Floorplanning, placement and introduction to cell design and characterization </a></li>
	<ul>
        	<li><a href="#header-2-1"> Summary </a></li>
	</ul>
	<ul>
		<li><a href="#header-2-2"> Theory </a></li>
	<ul>
		<li><a href="#header-2-2-1"> Floor Planning </a></li>
	</ul>
	<ul>
		<li><a href="#header-2-2-2"> Placement </a></li>
	</ul>
	<ul>
		<li><a href="#header-2-2-3"> Cell Design and Characterization flows </a></li>
	</ul>
	<ul>
		<li><a href="#header-2-2-4"> Timing Characterization </a></li>
	</ul>
	</ul>
	<ul>
		<li><a href="#header-2-3"> Floorplanning lab </a></li>
	<ul>
		<li><a href="#header-2-3-1"> Running Floorplanning </a></li>
	</ul>
	<ul>
		<li><a href="#header-2-3-2"> Reviewing results </a></li>
	</ul>
	</ul>
	<ul>
		<li><a href="#header-2-4"> Placement lab </a></li>
	<ul>
		<li><a href="#header-2-4-1"> Running Placement </a></li>
	</ul>
	<ul>
		<li><a href="#header-2-4-2"> Reviewing results </a></li>
	</ul>
	</ul>
	</ul>
</div>
<div class="toc">
	<ul>
    		<li><a href="#header-3"> Day 3: Jumpstarting, spice simulations and DRCs </a></li>
	<ul>
        	<li><a href="#header-3-1"> Jumpstarting </a></li>
 	</ul>
	<ul>
		<li><a href="#header-3-2"> 16 Mask layers based CMOS fabrication process </a></li>
	</ul>
	<ul>
		<li><a href="#header-3-3"> Spice simulation of CMOS inverter using NGSpice </a></li>
	<u1>
		<li><a href="#header-3-3-1"> Lab steps to git clone vsdstdcelldesign </a></li>
	</u1>
	<u1>
		<li><a href="#header-3-3-2"> Reviewing the layout </a></li>
	</u1>
	<u1>
		<li><a href="#header-3-3-3"> Spice simulation </a></li>
	</u1>
	</ul>
	<ul>
		<li><a href="#header-3-4"> DRC checks </a></li>
	</ul>
	</ul>
 </div>
<div class="toc">
	<ul>
    		<li><a href="#header-4"> Day 4: Using custom cell in PD flow, STA and CTS </a></li>
	<ul>
        	<li><a href="#header-4-1"> Running with custom cell </a></li>
	<u1>
		<li><a href="#header-4-1-1"> Dumping lef output </a></li>
	</u1>
	<u1>
		<li><a href="#header-4-1-2"> Reducing slack </a></li>
	</u1>
 	</ul>
	<ul>
		<li><a href="#header-4-2"> STA on a netlist provided in the standard cell design </a></li>
	</ul>
	<ul>
		<li><a href="#header-4-3"> Running CTS </a></li>
	</ul>
	</ul>
 </div>
<div class="toc">
	<ul>
    		<li><a href="#header-5"> Day 5: pdn and routing </a></li>
	<ul>
        	<li><a href="#header-5-1"> Generating PDN </a></li>
 	</ul>
	<ul>
		<li><a href="#header-5-2"> Running routing </a></li>
	</ul>
	</ul>
 </div>


# <h1 id="header-0">Overview</h1>

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


# <h1 id="header-2">Day 2</h1>

## <h1 id="header-2-1">Summary</h1>
The goal of Day 2 is to perform floorplanning and placement.

## <h1 id="header-2-2">Theory</h1>

### <h1 id="header-2-2-1">Floor Planning:</h1>

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

### <h1 id="header-2-2-2">Placement:</h1>

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

### <h1 id="header-2-2-3">Cell Design and Characterization flows:</h1>

![image](https://github.com/user-attachments/assets/662a8ab2-6471-4804-a6c5-8bc00db5068d)

We need to first design a cell according to the inputs as shown above. Spice models contain fixed information from foundry like VTHO, tox, capacitances, etc. There could be a lot of user defined specification like cell height, drive strengths, VM (point where Vin == Vout), position of pins, supply voltage, pin layers required, drawn gate length.

In circuit design the circuit is implemented to get required functionality and W/L is found for PMOS and NMOS for creating layout.

Euler's path is used to come up with stick diagrams and are then implemented according to DRC rules.

The stage is design is complete. Now the characterization is performed to obtain .libs.

Charactization is of three types and performed in GUNA(spice simulator):

![image](https://github.com/user-attachments/assets/8dab416c-f6ad-4444-92e0-d37c0eb9b5bc)

### <h1 id="header-2-2-4">Timing Characterization:</h1>

For timing characterizations following definitions are important to calculate slew and delays:

![image](https://github.com/user-attachments/assets/ebfca498-3457-486d-9305-aa1347a9ab63)

Propagation delays can be negative if input slew is too high or the thresholds defined are weird.

## <h1 id="header-2-3">Floorplanning lab</h1>

### <h1 id="header-2-3-1">Running Floorplanning</h1>

***run_floorplan*** command is used to run floor planning. All the system default commands are present in the following folder:

![image](https://github.com/user-attachments/assets/69bae8fc-43ed-48f3-bf36-87ce57f42c7a)

README.md file contains information about all keywords for all different steps as shown below for floorplanning.

![image](https://github.com/user-attachments/assets/670b53da-1489-494b-9636-299e9aa72cca)

Running floorplanning:

![image](https://github.com/user-attachments/assets/e791d1ae-545b-4e1a-ae4c-af93eec42a49)

### <h1 id="header-2-3-2">Reviewing results</h1>

#### <h1 id="header-2-3-2-1">Manually checking the def</h1>

Output is a def file:

![image](https://github.com/user-attachments/assets/097762b3-485c-4f0f-960a-7a1a4a16b1a5)

409 pins present with location defined

21230 components ( synthesis had 14876, difference is because of tap cells and decaps which are 6354 in number)

All logic instances don't have placement defined. Tap cells and decaps are placed.

Nets have logical connectivity defined but no routing as expected

#### <h1 id="header-2-3-2-2">Calculating Core and Die area and utilization ratio</h1>

$$ \texttt{ Die Area} = \texttt{length} * \texttt{height} = {(660685-0) \over 1000} * {(671405-0) \over 1000} = 443587.21 \texttt{sq microns} $$

Core area: 
![image](https://github.com/user-attachments/assets/eab46e48-5abc-4416-b54c-b9a58a8ca656)

$$ \texttt{ Core Area} = \texttt{width} * \texttt{height} = (655.04 - 5.52) * (658.24-10.88) = 420473.26 \texttt{sq microns} $$

chip area reported during synthesis:

![image](https://github.com/user-attachments/assets/ed0d76b8-cb36-4df2-8eb1-cc969aa1fd28)

core utilization ratio reported:

![image](https://github.com/user-attachments/assets/cd05068d-ab6e-4ef9-a4bd-85e85703d882)

$$ \texttt{Calculated core utilization ratio from observation} = {147712.92 \over 420473.26} = 0.3513 $$

#### <h1 id="header-2-3-2-3">Reviewing the def using Magic</h1>

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

## <h1 id="header-2-4">Placement lab</h1>

### <h1 id="header-2-4-1">Running placement</h1>

To run placement below command is run:

![image](https://github.com/user-attachments/assets/461bb7df-b0bc-423b-9317-25fb0b0a54ac)

placement optimizes a parameter called ***HPWL (Half parameter wirelength)*** and waits for overflow to converge

Both detailed and global placement is performed. Congestion aware placement is done.

### <h1 id="header-2-4-2">Reviewing results</h1>

Placed def is viewed in magic:
![image](https://github.com/user-attachments/assets/9ad1ca7f-3ece-4301-b702-5a9bc497706e)

![image](https://github.com/user-attachments/assets/ebbcc4e6-3bf3-4b28-89a0-5d9a34b7f273)

# <h1 id="header-3">Day 3</h1>

## <h1 id="header-3-1">Jumpstarting</h1>

![image](https://github.com/user-attachments/assets/6dfbf772-c5ca-4faf-8b77-ee0ec701e9c7)

And as can be seen floorplan changed:


![image](https://github.com/user-attachments/assets/1b4eac88-c970-42a7-a391-aa47caeb4dcd)

## <h1 id="header-3-2">16 Mask layers based CMOS fabrication process</h1>

First we need to select a substrate, here we select p type

![image](https://github.com/user-attachments/assets/0aa527d2-be4e-4c68-bd9b-67d6b690f93d)

To create active regions first we need to grow silicon oxide layer which will act as an insulator. Then deposit Si3N4 on top of it. Then we deposit photoresist and then project UV light over regions we what to be removed.Red regions are protected using a mask. Rest of the region reacts and can be washed out.

![image](https://github.com/user-attachments/assets/68b59a7f-4359-4831-9bbe-6d895dca5a50)
  
![image](https://github.com/user-attachments/assets/63f98756-3a53-48c8-8273-ead08c04227a)

![image](https://github.com/user-attachments/assets/1405c7e8-f7a0-4b16-8e8e-f5d395f9a48d)

Si3N4 is also etched out in the regions required.

![image](https://github.com/user-attachments/assets/847260b1-9685-4e07-a50c-1b90cd6db079)

Next step is to remove the photoresist.

![image](https://github.com/user-attachments/assets/ea887a0d-cf51-47f0-badc-084acf164aef)

We put the above output in an oxidation furnace which will create isolation regions. This process is called LOCOS (LOCal Oxidation of Silicon)

![image](https://github.com/user-attachments/assets/32597180-4dff-40b2-8b1f-61750bd5e4ed)


![image](https://github.com/user-attachments/assets/aec1ffd4-4ab5-4b30-af15-b50c922915cf)

Next step is to remove or etch out the Si3N4.

![image](https://github.com/user-attachments/assets/5899aacd-c163-401b-996a-17b2c2aa724b)

Now we need to create nwell and pwell

N-well is used for pmos fabrication and P-well is used for nmos fabrication. Both can't be done at the same time. We need to protect the one area while we fabricate the other.

The same steps will be done here also, deposit a layer of photoresist then define pattren of layer you want to protect. So we are using Mask2 to protect one area first.

   ![image](https://github.com/user-attachments/assets/7769effc-6a34-422e-84b4-c01214650cb2)

Next step is to expose this photoresist to the UV light. So, same this UV light will react only to the exposed photoresist area.

   ![image](https://github.com/user-attachments/assets/e64ca778-f9be-4716-a5b2-0f2489bbe2b7)

   When we wash this particular thing into a solution, that exposed area of photoresist will washed away.

   ![image](https://github.com/user-attachments/assets/58baac40-94a5-4ad8-9bd5-a2ac6296192f)


We do ion implantation with boron atoms in this region. The oxide layer will be damnaged but will be taken care later.

   ![image](https://github.com/user-attachments/assets/70f5da1e-fc9d-4c7d-973b-856449d79c15)

Same steps are performed for nwell creation with phosphorus.

   ![image](https://github.com/user-attachments/assets/9bd5d112-bb73-46aa-b49a-0f09da3733ef)


   ![image](https://github.com/user-attachments/assets/fcd22667-2e1d-431b-8155-230a1ea6cbfc)

   ![image](https://github.com/user-attachments/assets/0a495508-a5f7-4b94-9527-7580d1dd5f7c)

Now heat up the chip to reach required depth for the n and p well

   ![image](https://github.com/user-attachments/assets/c4efcec4-d0a9-4fbb-a842-be2d5ce8871b)

Threshold voltage is a function of doping concentration and oxide thickness. Hence it is important to be able to vary this.

   ![image](https://github.com/user-attachments/assets/5d45c934-713f-42d9-a8ec-bc20a1cbb156)

   ![image](https://github.com/user-attachments/assets/9ef42721-dbb4-4808-a6b7-2b5a12ea2672)


   ![image](https://github.com/user-attachments/assets/3ae10306-48cf-4546-a899-6e6871667167)

   ![image](https://github.com/user-attachments/assets/936a2108-554e-419f-8766-2eac93895317)

   ![image](https://github.com/user-attachments/assets/2933ba62-40e8-4124-9b9a-9defe21d43c9)

   ![image](https://github.com/user-attachments/assets/b69c62f8-5ed7-4206-b7d3-c91815f638b4)

   Fixing of oxide that got damaged while Ion implantations.

   ![image](https://github.com/user-attachments/assets/4d15488f-3cfc-40c1-8102-b2da31ccd0d1)

   ![image](https://github.com/user-attachments/assets/d02cb341-3707-4cff-a31a-55a3f6f386ec)

Now we need to form of gate step:

   ![image](https://github.com/user-attachments/assets/bcc47194-673b-4f3c-9513-2b25f97a70ee)

   ![image](https://github.com/user-attachments/assets/62744e7a-3c3d-439f-8004-1e916f7f54e1)

   ![image](https://github.com/user-attachments/assets/be715c8f-b0c5-488f-b91d-2565ef063127)

   ![image](https://github.com/user-attachments/assets/3d083492-98ef-4056-97c3-59a7250a8f6c)

   ![image](https://github.com/user-attachments/assets/ae966f7f-93cc-4c89-a651-7970966a3ea2)

   ![image](https://github.com/user-attachments/assets/5f44a829-0bf9-49ae-8752-55269d6f2f13)

   ![image](https://github.com/user-attachments/assets/70ba1725-0c13-418a-b0e3-caa8db0a915a)

   We try to achieve P+,P-,N in N-well when trying to fabricate the PMOS.

   Similarly, for NMOS, N+,N-,P doping profile is desired.

   ![image](https://github.com/user-attachments/assets/48fccbe5-4ef9-44a7-b64f-2aa9eea0ef05)

The reason for this:

   1. Hot electron effect:

		![image](https://github.com/user-attachments/assets/71d4de63-158f-43e4-901a-ee4c51916d16)

   2. Short channel effect:

		![image](https://github.com/user-attachments/assets/0b78b2c0-bbad-4b17-bf0f-476f7293895f)


   ![image](https://github.com/user-attachments/assets/cc357fa6-0946-40d5-872f-c85aacb03604)

   ![image](https://github.com/user-attachments/assets/07ee560b-3d77-4049-b321-c905c618f5ea)

   ![image](https://github.com/user-attachments/assets/1dd03dc8-1390-452c-827b-020782f66ae3)

   ![image](https://github.com/user-attachments/assets/76e29f5e-ac49-430e-95d8-c80330005aae)

   We add some side wall spacers to help avoid doping the p+ or n+ diffusion deep into the gate.

   ![image](https://github.com/user-attachments/assets/93090819-44a2-4c83-9709-3bd0c7bc96ae)

   ![image](https://github.com/user-attachments/assets/a28b7dcc-8956-4b0f-97d9-40eeb61f5877)

   ![image](https://github.com/user-attachments/assets/d69aac7a-c49a-410a-81ff-1ad53e8072c8)

   ![image](https://github.com/user-attachments/assets/9664121c-97ed-4bf0-9487-73745d63df27)


   The next step is to add a thin layer of screen oxide to avoid the effect of channeling where ions with high velocity could go very deep into the substrate.

   ![image](https://github.com/user-attachments/assets/1a77e5fb-ab33-4969-bbbb-8fc0688add3f)

   ![image](https://github.com/user-attachments/assets/c92cd386-e783-443b-b0be-e0a05f62f497)

   ![image](https://github.com/user-attachments/assets/7bb738cc-858d-4799-a557-42dca97e4ca4)

   ![image](https://github.com/user-attachments/assets/36c05222-67a8-4667-85f3-d7c51cdec4bd)

   ![image](https://github.com/user-attachments/assets/a9317ab5-9154-47d6-badb-e6fb542376eb)

   ![image](https://github.com/user-attachments/assets/f19a0d29-c207-49e4-a4c5-32d4ee922c7b)

   ![image](https://github.com/user-attachments/assets/0fc00b73-49a6-4387-92a0-972a24d03a67)

   So, we will put this half built pmos and nmos to high temperature furnace.

   ![image](https://github.com/user-attachments/assets/87048080-6f8d-4661-8105-c8c52c627172)

Now we need to create contacts which will be accesible to designer.

   ![image](https://github.com/user-attachments/assets/232fc1fd-3bf0-4c4e-9f35-5e0ff5d0e703)

   ![image](https://github.com/user-attachments/assets/18783ae3-4c9d-4d53-a756-2e3a1f66291d)

   ![image](https://github.com/user-attachments/assets/a1cb4afc-c770-4f36-ba06-95d327bb007e)

   ![image](https://github.com/user-attachments/assets/d22c75f2-446a-4b49-a175-29d3212cfdc2)

   ![image](https://github.com/user-attachments/assets/f07d62c9-1edf-49c4-b771-ac0b3009fca1)

   ![image](https://github.com/user-attachments/assets/70c6d039-5bcc-4444-8772-c3a37d8836c2)

   ![image](https://github.com/user-attachments/assets/7e04045c-d2dd-41fa-b768-ac0812cafac6)

   ![image](https://github.com/user-attachments/assets/01de4957-986a-4f85-a441-204f5f2ca5e7)

   ![image](https://github.com/user-attachments/assets/88a475c9-825a-473d-8646-92fc28c0a01c)

   ![image](https://github.com/user-attachments/assets/05baa5ad-bd03-4663-b6f3-62b1bf3ea218)

   ![image](https://github.com/user-attachments/assets/7b08776c-93d3-4900-8e07-9fe39e33b5e7)

   ![image](https://github.com/user-attachments/assets/0c1e05cf-7712-4e9a-a1ee-0e918ba141bf)

   To create the layer stack we need a planar surface so we need to planerize this surface using CMP.

   ![image](https://github.com/user-attachments/assets/004e0659-e5ca-4ce5-a9c4-317a82a30b44)

   ![image](https://github.com/user-attachments/assets/c0fae799-4765-4d29-ac28-c1d4e0195379)

   ![image](https://github.com/user-attachments/assets/fb9a7732-5e3f-4d6d-9e16-3c305b10163a)

   ![image](https://github.com/user-attachments/assets/d65f6ec4-949e-4636-a7e8-fd68ae5b1c0f)

   ![image](https://github.com/user-attachments/assets/d2fbb614-89a2-4c12-91e7-d3c5918db197)

   ![image](https://github.com/user-attachments/assets/d8df0306-a66c-47ca-a411-507bf0dfc835)

   ![image](https://github.com/user-attachments/assets/d3f693dc-728b-4938-b272-480c438596fd)

   ![image](https://github.com/user-attachments/assets/12503801-a485-4adb-851a-52693f4d317b)

   ![image](https://github.com/user-attachments/assets/9fd4023b-bc83-43c4-bed6-49ae5e15b49d)

   ![image](https://github.com/user-attachments/assets/281eec81-4990-4301-ad20-eb9e66886f9f)

   ![image](https://github.com/user-attachments/assets/1a20b5a3-1ab5-484f-a043-508f7580f00c)

   ![image](https://github.com/user-attachments/assets/98f0c330-8c40-4386-a3fe-f82ad4a536af)

   Till here, we have the local interconnects (0 level of metal), 1st level of interconnects (Aluminium interconnects).

   Now the next step is to again take this metal levels to the higher level metal.

   ![image](https://github.com/user-attachments/assets/43416f56-f290-4abc-a307-28ccfdf8c027)

   ![image](https://github.com/user-attachments/assets/3ac165de-2668-46c0-886b-5bc593fb5e98)

   ![image](https://github.com/user-attachments/assets/02fc3279-0195-4297-af64-5b14250fd7e8)

   TiN acts an additional layer to SiO2 and acts as the barrier between lower and higher metal layers.

   ![image](https://github.com/user-attachments/assets/6586fa91-4e71-4cf8-85e6-19c13afb89e2)

   ![image](https://github.com/user-attachments/assets/112c38cc-8b98-4082-b1a6-250480c812be)

   ![image](https://github.com/user-attachments/assets/af3ed65d-3751-419c-97ce-7fcb2c08f654)

   ![image](https://github.com/user-attachments/assets/16620b28-a6e4-4dc9-94c1-41d63797c3eb)

   Final result:

   ![image](https://github.com/user-attachments/assets/c5033982-833d-410e-9984-44ef950a8ec6)

## <h1 id="header-3-3">Spice simulation of CMOS inverter using NGSpice</h1>


### <h1 id="header-3-3-1"> Lab steps to git clone vsdstdcelldesign</h1>

Downloaded the package containing the inverter design.

![image](https://github.com/user-attachments/assets/43cc9d69-1236-445a-858a-ffce1a6e1197)

### <h1 id="header-3-3-2">Reviewing the layout</h1>

Copied and the tech file and viewed the design in Magic

![image](https://github.com/user-attachments/assets/43eb37f2-7fb1-4ad1-aa68-b622fe566d26)

![image](https://github.com/user-attachments/assets/46a13527-0675-457e-bc50-25558dcdeb53)

Checking NMOS and PMOS

![image](https://github.com/user-attachments/assets/d05532a1-b757-404a-a632-4d48ff529626)

![image](https://github.com/user-attachments/assets/15c340b3-81b3-4d98-b6a7-2bcad4b065b5)


dimensions of inverter
![image](https://github.com/user-attachments/assets/c803808f-36b5-4f0c-af11-ccb8dce3d9b5)

### <h1 id="header-3-3-3">Spice simulation</h1>

extracting the spice netlist of the design
![image](https://github.com/user-attachments/assets/adb104b5-a914-4d5a-a770-22603342a18a)

Output spice file:
![image](https://github.com/user-attachments/assets/73878d65-d1c8-46ab-bd51-d0ac9cc159fc)

Editted spice file:
![image](https://github.com/user-attachments/assets/c72ced1c-f76d-4b9c-ba2c-20146ba92c18)

running the spice netlist:
![image](https://github.com/user-attachments/assets/fce42657-0184-49f5-a219-da49c2c66cf5)

Calculating parameters:

Output rise transition time:

![image](https://github.com/user-attachments/assets/13413161-2571-4508-aef1-b48681a9b1bc)

59.48ps

Output fall transition time:

![image](https://github.com/user-attachments/assets/d7dc06e8-db90-4ffe-a5e0-e04c9fed0ebb)

42.61ps

Cell rise delay:

![image](https://github.com/user-attachments/assets/994d60c1-8d47-4c33-9baa-c1dd21ab25d7)

58.31ps

Cell fall delay:

![image](https://github.com/user-attachments/assets/678c4810-1d85-4bc5-be9d-9c2a3bebe0cc)

24.61ps

## <h1 id="header-3-4">DRC checks</h1>

Downloading drc tests

![image](https://github.com/user-attachments/assets/4db21417-28fe-4d07-8570-f276ba2bac5c)
   
![image](https://github.com/user-attachments/assets/46f0abee-e611-47ed-8d52-a290f070c6cc)

Magicrc file:

![image](https://github.com/user-attachments/assets/bdbea17a-d6fc-4a4a-85e5-9a0b0df849f4)

viewing in magic

![image](https://github.com/user-attachments/assets/6ab8fe8e-d8ad-46ea-8648-f744242849b4)

Opening met3 layout in magic.

![image](https://github.com/user-attachments/assets/1f787d06-7aa3-4046-9ef6-a479cd2d73f0)

10 DRC violations found.
 
![image](https://github.com/user-attachments/assets/0dfb5d03-6fcf-480d-b8af-f39a0a5a4d62)

Selecting a part of layout by left click and right click and hen press : drc why. It gives the reason for DRC violation and rule no.

![image](https://github.com/user-attachments/assets/392eb19f-29a1-448e-939e-f36513963a4c)

cif see VIA2
feed clear
snap int

loading poly.mag

![image](https://github.com/user-attachments/assets/4e2c6207-5158-4091-a430-9f1c851bb14a)

Rule: 
![image](https://github.com/user-attachments/assets/3f18ba6a-4510-41a7-87b2-98f9b17c7326)

Open tech file:
![image](https://github.com/user-attachments/assets/5d1aa5fe-d32e-4af6-8b3f-2fbeddd7d12a)

![image](https://github.com/user-attachments/assets/d8d5836b-96d7-405a-856e-80ab1eda2309)

No rule defined between poly resistor to poly. Adding rule:

![image](https://github.com/user-attachments/assets/3eade8c9-2e8a-4b9b-aad5-abe29cf99a22)

![image](https://github.com/user-attachments/assets/f40ad94c-39da-4731-96d8-265e42076564)


tech load sky130A.tech (generally not recommended but since just rules its okay)

Run DRC again. drc check

# <h1 id="header-4">Day 4: running with custom cell, STA and CTS</h1>

## <h1 id="header-4-1">Running with custom cell</h1>

### <h1 id="header-4-1-1">Dumping lef output</h1>

tracks info file

![image](https://github.com/user-attachments/assets/b312a07b-3269-4aa3-82bb-9ab921c9f158)

changing grid size:

![image](https://github.com/user-attachments/assets/8535a2dc-3622-43de-b23c-7794e0caf394)

width and height are 3 and 4 boxes respectively


defining direction of all pins for lef output

![image](https://github.com/user-attachments/assets/6a00af81-6da3-4cf0-8460-83c34bf3d33f)

![image](https://github.com/user-attachments/assets/235ab438-05f2-4b81-a448-0f8265c11d44)


Saving the lef file:

![image](https://github.com/user-attachments/assets/324d585d-6e47-401f-ba90-5788d76fc399)


copied lef file  and libs to src:

![image](https://github.com/user-attachments/assets/72e976b1-974d-4d38-a614-481196fb58eb)

Adding paths for libs and lefs in config.tcl

![image](https://github.com/user-attachments/assets/60ca8a71-86dd-44d1-ba59-8a6db74c82fb)

Prepping design and running synthesis:

![image](https://github.com/user-attachments/assets/cbba1238-29e0-4f46-9e2d-e3b521d70ad6)

Total slew:

![image](https://github.com/user-attachments/assets/b7841fcb-afde-4fd2-879d-465b4c624470)

### <h1 id="header-4-1-2">Reducing slack</h1>

STA reports also dumped out. Negative slack hence need to change some synthesis parameters

![image](https://github.com/user-attachments/assets/c583af30-7d1c-4ea7-9752-202519633ccd)

Let's try optimising synthesis strategy according to delay will affect PPA

set ::env(SYNTH_STRATEGY) "DELAY 3"

Enabling cell sizing: 

set ::env(SYNTH_SIZING) 1

Overwriting the design and running synthesis again:


wns and tns are good now:

![image](https://github.com/user-attachments/assets/d2bf78d3-4eec-42df-a98a-a9de9442f6a3)


before and after:
![image](https://github.com/user-attachments/assets/bfdc65ab-b035-47e6-914b-74787c95ff58)


For floor plan we need to run the following commands which are the inner commands when we call run_floorplan:

	init_floorplan
	place_io
	global_placement_or
	tap_decap_or

run_placement is run after this

Reviewing the def

![image](https://github.com/user-attachments/assets/cd91384e-b067-4a39-9a6a-62ca49266b7d)

## <h1 id="header-4-2">STA on a netlist provided in the standard cell design</h1>

Edited sta.conf

![image](https://github.com/user-attachments/assets/447bef63-446e-458f-bbbe-02881bdebd0c)

The output of sta sta.conf

![image](https://github.com/user-attachments/assets/1afe7809-c47d-4ba8-ac53-b0e3f88c435c)

slack is violating

![image](https://github.com/user-attachments/assets/febc3a09-bacc-4b05-9ae0-f3e1d5858137)
![image](https://github.com/user-attachments/assets/b4a42f8c-74fb-4d64-b1a6-da25f3440b6c)

Bringing it down by replacing cell with higher drive strength in worst paths we reach:

![image](https://github.com/user-attachments/assets/abc6954a-807a-4dfd-8963-13ae46f13efe)

Commands used:

	replace_cell instance_name cell_to_be_replaced_with
 	report_checks -fields {net cap slew input_pins} -digits 4
  	report_tns
   	report_wns

writing out the edited file:

![image](https://github.com/user-attachments/assets/3b421c8c-dcd5-4e04-be40-3f982eec7b7a)

## <h1 id="header-4-3">Running CTS</h1>

Using the command run_cts

![image](https://github.com/user-attachments/assets/d1dd3e0f-5a4b-4071-95a3-fc90d1c88f5b)

output netlist:

![image](https://github.com/user-attachments/assets/06bcadd4-389f-49cb-be5a-b12e18469104)


To check timing post CTS the following commands are run:
![image](https://github.com/user-attachments/assets/6a4e4904-95c2-491e-8f79-0ae5a8d27cd1)


The output of analysis is:
![image](https://github.com/user-attachments/assets/07b004db-f2d8-47c1-a468-54054db13cb7)


Following commands are run to check the impact of removing the smallest buffer usable in CTS

![image](https://github.com/user-attachments/assets/a7506475-38d8-43b0-a4cc-0d834432aadd)

Running open road again on this:

![image](https://github.com/user-attachments/assets/15f68213-4e0f-432b-808d-b8c841ecbddc)


reporting clock skew for setup and hold

![image](https://github.com/user-attachments/assets/389bbd47-8ba6-44b6-8c53-056a9b44e39d)

# <h1 id="header-5"> Day 5</h1>

## <h1 id="header-5-1"> Generating PDN</h1>

![image](https://github.com/user-attachments/assets/e0458daf-e757-4091-bb2e-3b64ed498391)

generated PDN:

![image](https://github.com/user-attachments/assets/aeac0fd8-473c-49bf-aeec-2dd524a89216)

## <h1 id="header-5-2"> Running routing</h1>

run_routing command is used which uses TritonRoute to accomplish this:

![image](https://github.com/user-attachments/assets/1fa02690-0517-4722-a171-8192bfecb3be)

Finished routing:

![image](https://github.com/user-attachments/assets/e0ce5216-a16e-48a1-b255-57195adb33cc)

