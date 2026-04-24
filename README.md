# RTL Design And Synthesis Workshop using Sky130

## Day 1 - Introduction to Verilog RTL design and Synthesis

## Introduction to open-source simulator iverilog

## Video-1 : Introduction to iverilog design test bench

Simulator - RTL design is checked to adherence to the spec by simulating the design.Simulator is the tool used for simulating the design.

iverilog is the tool used for this course.

Design-Design is the actual Verilog code or set of Verilog codes which has the intended functionality to meet with the required specifications.

Testbench- Testbench is the setup to apply stimulus to the design to check its functionality.

The design and the testbench setup:

![Workshop image](IMAGES/IMG_01.jpeg)

iverilog based simulation flow

![Workshop image](IMAGES/IMG_02.png)

## Labs Using iverilog and gtkwave

## Video-2 : Lab 1 - Introduction to lab

### File setup

In this Course, the ready to use cloud-based lab is used for RTL Design and Synthesis using open-source tools such as Yosys, Icarus Verilog, and GTKWave. All the tools run inside a GitHub Codespace with noVNC desktop access, which requires no installation.
Once the noVNC desktop terminal is openend, all the workshop files including the library files,verilog files and the standard cell verilog model are present inside the directory named sky130RTLDesignAndSynthesisWorkshop inside the workspaces folder. The below image is the all files going to use in the project.

![Workshop image](IMAGES/IMG_03.png)

## Video -3 : Lab 2 - Introduction to iverilog and gtkwave - Part 1
Loading the verilog file mux into the simulator (iverilog). 

![Workshop image](IMAGES/IMG_04.png)

To load into the simulator,the design name and the testbench name should be given along with the command iverilog.
Once its loaded in the simulator, a.out files will be created.
When executing the a.out file, its going to dump the vcd file (the output of the simulator will be the vcd file).

![Workshop image](IMAGES/IMG_05.png)

The vcd file will be loaded for the waveform analysis in the gtkwave and check its functionality.

## Waveform Analysis of Mux
![Workshop image](IMAGES/IMG_06.png)

## Video -4 : Lab 3 - Introduction to iverilog and gtkwave - Part 2

## File Structure of design and testbench files

![Workshop image](IMAGES/IMG_07.png)

![Workshop image](IMAGES/IMG_08.png)

## Introduction tp Yosys and Logic Synthesis

## Video-5 : Introduction to yosys

Synthesizer : Tool used for converting the RTL to netlist. Yosys is used as the synthesizer in this course.

Netlist - The representation of the design in the form of the cells present in the .lib file (standard cell).
```
read_verilog //command to read the design
read_liberty //command to read the .lib file
write_verilog //command to write the netlist
```

![Workshop image](IMAGES/IMG_09.png)

To verify the Synthesis

![Workshop image](IMAGES/IMG_10.png)

The stimulus should be same as the output observed during the RTL simulation.
The set of Primary inputs/primary outputs will remain same between the RTL design and synthesized netliset and the same testbench can be used.

## Video -6 Introduction to Logic Synthesis Part 1
Logic Synthesis

RTL Design - Behavioural representation of the required specification,which will be written in the HDL language.

Instead of RTL Code, if the digital logic circuit(hardware circuit) is needed. How to map these two?
Answer : Synthesis - RTL to code Gate level translation. The design is converted into gates and the connections are made between the gates. The file written out is called netlist.

What is .lib?
Collection of logical modules. Includes basic logic gates like AND,OR,NOT,ETC.,. It may contain different flavors of same gate.

Why do we need different flavours of Gate?
Combinational delay in logic path determines the maximum speed of operation of digital logic circuit.

![Workshop image](IMAGES/IMG_11.png)

The clock cycle needs to accomodate the propogation delay of the Clock A and the combinational delay and the setup time of the clock B and that should be the minimum requirement of the clock period.That will decide the maximum frequency.

## Video -7 Introduction to Logic Synthesis Part 2

The faster cells in the .lib are sufficient to make T_combination small. 
Why do we need slow cells?

![Workshop image](IMAGES/IMG_12.png)

The delay from FF_A to FF_B should be greater than the Hold time. The hold time of the flop is the time after the edge of the clock, the data shouldn't be changed.
#Faster cells are needed to meet the setup and slower cells are needed to meet the hold.

The Capcitance is the load in the digital logic circuit.

Faster cells vs Slower cells

Faster the charging or discharging the capacitance, the lesser will be the propogation delay.

To charge or discharge the capacitance fast , the more current sourcing capability which is wide transistors are needed. The current carrying capability of a transistor is the function of its width.The wider the transistor, the progation delay will be low and more area and power. The narrow the transistor, the propogation delay will be more and less the area and power.

Selection of Cells

The synthesizer need to be guided to select the appropriate flavour of cells that is optimum for the implementation of logic circuit is called Constraints.

The more the usage of the faster cells, its bad interms of power and area and it results in hold time violations. The more the usage of slower cells, may not need the performance need and the circuit will be sluggish.

## Labs using Yosys and Sky130 PDKs

## Video - 8 Lab 3 Yosys-1 good mux Part-1

Invoke the Yosys

![Workshop image](IMAGES/IMG_13.png)
```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib//command to read the library and need to give the path
read_verilog good_mux.v //command to read the design-verilog file,taken example mux
synth -top good_mux //command the module used for synthesis
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib // command to generate the netlist along the path to the lib. abc is a command which converts the RTL file into gates(the library path mentioned along the command is the library, where the gates is present that need to be used).
show // command used to show the graphical logic it has realized in the yosys.
write_verilog -noattr good_mux_netlist.v //command to write the netlist
!vim good_mux_netlist.v
```
Steps for design synthesis

![Workshop image](IMAGES/IMG_14.png)

Inference from synthesis 

![Workshop image](IMAGES/IMG_16.png)

## Video - 9 Lab 3 Yosys-1 good mux Part-2

Generating the netlist

![Workshop image](IMAGES/IMG_17.png)

Graphical synthesis logic realized in Yosys

![Workshop image](IMAGES/IMG_18.png)

## Video - 10 Lab 3 Yosys-1 good mux Part-3

Writing Netlist

![Workshop image](IMAGES/IMG_19.png)

## Day 2 - Timing libs,hierarchical vs flat synthesis and efficient flop coding styles

## Introduction to timing .libs

## Video - 11 Lab 4 Introduction to .lib Part 1
```
vim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Workshop image](IMAGES/IMG_20.png)

sky130_fd_sc_hd__tt_025C_1v80.lib - Name of the library
tt- typical library
025C - Temperature
1v8-indicates the voltage

![Workshop image](IMAGES/IMG_21.png)

Once the library file is opened, we can see the operating conditions, the PVT where P-Process(there can be variations due to fabrication), V-Voltage (Variation due to voltage), T-Temperature(semiconductor are sensitive to temperature). All these three determines how the silicon is going to work.

## Video - 12 Lab 4 Introduction to .lib Part 2

Example : AND gate 

![Workshop image](IMAGES/IMG_22.png)

The keyword cell which defines the marks the beginning of the cell definition. It can contain different kind of cells like diffrent flavours of diffrent cells and different flavours of same cell. It contains different features of cell like leakage power for all the input combinations,area,power and timing informations of input pins.

## Video - 13 Lab 4 Introduction to .lib Part 3

## Different flavours of cell - AND gate.

![Workshop image](IMAGES/IMG_23.png)

We can see from the above image that, different cells of and gate where their area differs. we can find the larger area has employed wider transistors but it can be faster. Smaller area will have 
delay more , area will be less and consume more power.

##Hierarchical Flat Synthesis

## Video -14 Lab 5 Hier synthesis flat synthesis Part 1

## Hierarchical Synthesis

Example : multiple_modules(Two sub modules will be used, one sub module1 and_gate  and one sub module2 or_gate)

![Workshop image](IMAGES/IMG_24.png)
![Workshop image](IMAGES/IMG_25.png)

### Synthesising multiple_modules

```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib//command to read the library and need to give the path
read_verilog multiple_modules.v //command to read the design-verilog file,taken example multiple_modules
synth -top multiple_modules //command the module used for synthesis
```
![Workshop image](IMAGES/IMG_26.png)

##Generating Netlist
```
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib // command to generate the netlist
```
![Workshop image](IMAGES/IMG_27.png)

#Realization of Logic in Yosys

```
show // command used to show the graphical logic it has realized in the yosys.
```
![Workshop image](IMAGES/IMG_28.png)

From the above image, we can see that, it didnt mention the gates thats used, it is showing as u1 and u2. This is called Hierarchical synthesis.

## Writing Netlist
```
write_verilog -noattr multiple_modules_hier.v //command to write the netlist
!vim multiple_modules_hier.v
```
![Workshop image](IMAGES/IMG_29.png)

## Flat Synthesis

```
yosys> flatten // flatten is the command to write down the flat netlist
```
![Workshop image](IMAGES/IMG_30.png)

## Realization of Logic in Yosys

```
show // command used to show the graphical logic it has realized in the yosys.
```
![Workshop image](IMAGES/IMG_31.png)

## Writing Netlist
```
write_verilog -noattr multiple_modules_flat.v //command to write the netlist
!vim multiple_modules_flat.v
```
![Workshop image](IMAGES/IMG_32.png)
From the above image, we can see that, it has mentioned the gates thats used, instead of the submodules u1 and u2. This is called flat synthesis.

## Sub_module level Synthesis

## Synthesising multiple_modules - Sub module level

```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib//command to read the library and need to give the path
read_verilog multiple_modules.v //command to read the design-verilog file,taken example multiple_modules
synth -top sub_module1 //command the module used for synthesis
```
![Workshop image](IMAGES/IMG_33.png)

#Generating Netlist
```
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib // command to generate the netlist
```
![Workshop image](IMAGES/IMG_34.png)

#Realization of Logic in Yosys

```
show // command used to show the graphical logic it has realized in the yosys.
```
![Workshop image](IMAGES/IMG_35.png)

From the above image, we can see that, and gate instead of the submodules u1 and u2. This is called sub-module level synthesis. We controlled the module which we are synthesizing using the keyword "synth -top". The submodule level synthesis is needed when we have multiple instances of the same module. It is also used in the massive design, in the concept of divide and conquer. Instead of giving the entire massive design to the tool, giving one portion by one portion to the tool, so it can write the optimized netlist and can stitch all the netlist together to the top level.

## Various Flop Coding Styles and Optimization

## Video:16 Why Flops and Flop Coding Styles Part -1

In a Combinational Circuit, when the input is given, the output changes after the propgational delay. Because of the propogation delay, the output is going to have glitch. All logic gates add some delay to signals. These delays can cause expected and unwanted transitions in the output, called as glitches, where the output is momentarily different from the expected output. The more the combinational circuits, it can lead to more glitchy outputs. If there are continuous combinational circuits, the output cannot settle down, it'll be more glitchy. To avoid the glitches and to store the value we use flops.

In the flops, the output can only be changed at the clock edge, and if the input changes at other times, the output will be unaffected. Flop is used in between the combinational circuit because flop always gives out the stable output where the output of the combinational will settle down.that is the main purpose of flop.
To Initialize the flop, we have the control pins like reset or set.Reset and set can be synchronous and asynchronous.

## Video:17 Why Flops and Flop Coding Styles Part -2

![Workshop image](IMAGES/IMG_36.png)

The always block gets evaluated whenever there is a change in the clock or change in the set/reset.Upon the signal going the output is going low, then its asynchronous reset. The circuit is making sensitive to positive edge of the clock , upon positive edge, q is getting d. Asynchronous - irresepective of clock.

![Workshop image](IMAGES/IMG_37.png)

The always block gets evaluated upon on a asynchronous set, without the dependancy of the clock the q is setting to 1.

![Workshop image](IMAGES/IMG_38.png)

Synchronous will turn into D-pin of the flop. Reset awaiting the clock edge is called synchronous reset. Synchronous reset - the always block get evaluated only upon a clock. Even if the sync_reset is going toggle,its not going to get evaluated till the posedge of the clock. Upon the  posedge of the clock,look for the presence synchrounous clock,q goes low,else q follows d.

![Workshop image](IMAGES/IMG_39.png)

Here, Evaluating always block upon clock and as well as upon asynchronous reset. Entering always upon asynchronous reset, q goes low else the always entered because of the clock.Entered the always block because of the clock upon the clock, if there present synchronous reset, q goes low else q is getting d.


## Video:18 Lab - Flop Synthesis simulations Part - 1
![Workshop image](IMAGES/IMG_40.png)

Asynchornous reset is not awaiting the clock edge, its immediately making the q go low.

![Workshop image](IMAGES/IMG_41.png)

Asynchronous set - The changes in the d-pin are felt in the q-pin, only upon the edge of the clock.
Irrespective of the clock,asynchronous set is making q go 1.

![Workshop image](IMAGES/IMG_42.png)
![Workshop image](IMAGES/IMG_43.png)

Synchronous reset - The reset is getting applied upon the pos edge of the clock. If both the clk and d is high upon the edge of the clock, the preference is given to sync_reset.

![Workshop image](IMAGES/IMG_44.png)


## Video:19 Lab - Flop Synthesis simulations Part - 2

Synthesising the designs

#Reading the liberty file
```
yosys
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres.v
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib//command to tell to the tool explicitly,where to pick the d-flipflops in the design library
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
```
![Workshop image](IMAGES/IMG_45.png)

```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set.v
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
```
![Workshop image](IMAGES/IMG_46.png)

```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres.v
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
```
![Workshop image](IMAGES/IMG_47.png)

## Video:20 Interesting Optimisations Part-1

Example : mult_2.v
```
yosys
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mult_2.v
!vim mult_2.v
```
Verilog file 

![Workshop image](IMAGES/IMG_48.png)

No hardware  requirements including the memories,memory bits,processes and cells.

![Workshop image](IMAGES/IMG_49.png)

Netlist file

![Workshop image](IMAGES/IMG_50.png)

Realization of Logic

![Workshop image](IMAGES/IMG_51.png)

## Video:21 Interesting Optimisations Part-2

Example : mult_8.v
```
yosys
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mult_2.v
!vim mult_2.v
```
Verilog file 

![Workshop image](IMAGES/IMG_52.png)

Statistics

![Workshop image](IMAGES/IMG_53.png)

Netlist file

![Workshop image](IMAGES/IMG_54.png)

Realization of Logic
![Workshop image](IMAGES/IMG_55.png)
