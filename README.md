# RTL-design-using-Verilog-with-SKY130-Technology
![image](https://user-images.githubusercontent.com/110079774/183120297-57d7a0a9-0250-436a-943f-f87e85bf235a.png)

# Table of contents
1. [Introduction](#Introduction)</br>
2. [Day 1- Introduction to Verilog RTL design and Synthesis](#day1)
    - [2.1 Various aspects of frontend design](#day1_21)
    - [2.2 Introduction to open source simulator iverilog and gtkwave](#day1_22)
      - [2.2.1 Lab examples using iverilog and gtkwave](#day1_221)
    - [2.3 Introduction to Yosys synthesizer](#day1_23)</br>
      - [2.3.1 Labs on Yosys introduction](#day1_231)</br>
3. [Day 2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles](#day2)
    - [3.1 Introduction to timing .libs](#day2_31)
      - [3.1.1 LAB- Introduction to dot Lib](#day2_311)
    - [3.2 LAB- Hierarchical synthesis and flat synthesis](#day2_32)
    - [3.3 Various Flop coding styles and optimization](#day2_33)
      - [3.3.1 Lab- flop synthesis simulations](#day2_331)
      - [3.3.2 Interesting optimisations](#day2_332)
4. [Day 3- Combinational and sequential optmizations](#day3)
    - [4.1 Combinational logic optimization with examples](#day3_41)
    - [4.2 Sequential logic optimization with examples](#day3_42)
      - [4.2.1 Basic](#day3_421)
      - [4.2.2 Advanced](#day3_422)
      - [4.2.3 Sequential optimisation of unused outputs](#day3_423)
5. [Day 4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#day4)
    - [5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements](#day4_51)
      - [5.1.1 GLS Concepts And Flow Using Iverilog](#day4_511)
      - [5.1.2 Synthesis Simulation Mismatch](#day4_512)
    - [5.2 Lab- GLS Synth Sim Mismatch](#day4_52)
    - [5.3 Lab- Synthesis simulation mismatch blocking statement](#day4_53)
6. [Day 5- if, case, for loop and for generate](#day5)
    - [6.1 If and Case constructs](#day5_61)
      - [6.1.1 If construct](#day5_611)
      - [6.1.2 Case construct](#day5_612)
    - [6.2 Lab- Incomplete IF](#day5_62)
    - [6.3 Lab- incomplete overlapping Case](#day5_63)
    - [6.4 For Loop and For Generate](#day5_64)
    - [6.5 Lab- For and For Generate](#day5_65)
7. [Word of Thanks](#end)

# 1. Introduction <a name="Introduction"></a>
This report is a final submission of 5-day workshop from [VLSI Sytem Design-IAT](https://www.vlsisystemdesign.com/) on RTL design and synthesis using open source tools, in particular iVerilog, GTKWave, Yosy and Skywater 130nm Standard Cell Libraries
# 2. Day-1- Introduction to Verilog RTL design and Synthesis<a name="day1"></a>
## 2.1 Various aspects of frontend design<a name="day1_21"></a>
**RTL Design**: In simple terms RTL design or Register Transfer Level design is a method in which we can transfer data from one register to another. In RTL design we write code for Combinational and Sequential circuits in HDL(Hardware Description Language) like Verilog or VerilogHDL which can model logical and hardware operation. RTL design can be one code or set of verilog codes. **One key note is that we need to write RTL design with optimized and synthesizable (realizable as physical gates).**
### Sample RTL design outline:
```
module module_name (port list);
	//declarations;
	//initializations;
	//continuos concurrent assigments;
	//procedural blocks;
endmodule
```
**Test Bench**: Using Verilog we can write a test bench to apply stimulus to the RTL design and verify the results of the design by instantiating design with in test bench. Up-front verification becomes very important as design size increases in size and complexity while any project progresses. This ensures simulation results matches with post synthesis results. A test bench can have two parts, the one generates input signals for the model to be tested while the other part checks the output signals from the design under test. It can be represented as follows.
![image](https://user-images.githubusercontent.com/110079774/183134811-707308d2-3f2d-4739-b626-e8e9b092f8a9.png)
**Simulation**: RTL design is checked for adherence to its design specification using simulation by giving sample inputs. This helps finding and fixing bugs in the RTL design in the early stages of design development.

**Simulator**: Simulator is the tool used for this process. It looks for changes on input signals to evaluate outputs. No change in output if there is no change in input signals Here is the flow of frondend design:
![image](https://user-images.githubusercontent.com/110079774/183135115-c07c56ee-438a-4205-817d-b98bc55775ab.png)



## 2.2 Introduction to open source simulator iverilog and gtkwave<a name="day1_22"></a>
**iverilog**: iverilog stands for Icarus Verilog. Icarus Verilog is an implementation of the Verilog hardware description language. It supports the 1995, 2001 and 2005 versions of the standard, portions of SystemVerilog, and some extensions.

**Gtkwave**: GTKWave is a fully featured GTK+ based wave viewer for Unix, Win32, and Mac OSX which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing.


### 2.2.1 Lab examples using iverilog and gtkwave<a name="day1_221"></a>
We were introducted to Linux operating system and were made aware of the basic commands. Using git clone command we've cloned library files like standard cell library, primitives which are used for synthesis and few verilog codes for practice.


In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code.



Here is the code and gtkwave snippets:



## 2.3 Introduction to Yosys synthesizer<a name="day1_23"></a>

**Synthesis**: Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:

- Converting RTL into simple logic gates.
- Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
- Optimizing the mapped netlist keeping the constraints set by the designer intact.
**Synthesizer**: It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this workshop. Here is the flow of above processess.

![image](https://user-images.githubusercontent.com/110079774/183135464-cf293d50-f9d5-4869-b348-52818f5e1f0f.png)

**Yosys**:Yosys is a framework for RTL synthesis and more. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys is the core component of most our implementation and verification flows.

I was given an overview of the operation of the tool and the files we'll need to provide the tool to give the required netlist. We give RTL design code, .lib file which has all the building blocks of the netlist. Using these two files, Yosys synthesizer generates a netlist file. .lib basically is a collection of logical modules like, And, Or, Not etc.... These are equivalent gate level representation of the RTL code.

Below are the commands to perform above synthesis.

- RTL Design - read_verilog
- .lib - read_liberty
- netlist file- 

**Operational flow of Yosys Synthesizer**

![image](https://user-images.githubusercontent.com/110079774/183135531-c847cc7a-def8-4deb-9239-a03c97557994.png)

**Verification of Synthesized design**: In order to make sure that there are no errors in the netlist, we'll have to verify the synthesized circuit. The netlist verification flow can be seen in the below image:
![image](https://user-images.githubusercontent.com/110079774/183135584-f82ffcd9-a052-4b31-b387-72da1f33b122.png)
The gtkwave output for the netlist should match the output waveform for the RTL design file. As netlist and design code have same set of inputs and outputs, we can use the same testbench and compare the waveforms.

**Introduction to loigc synthesis**: Below is the snippet RTL code and equivalent digital circuit:

![image](https://user-images.githubusercontent.com/110079774/183135641-f9400590-281a-4395-b2ed-0676795aa340.png)

In the above image, mapping of code and digital circuit is done using Synthesis.

**.lib**: It is a collection of logical modules like, And, Or, Not etc...It has different flvors of same gate like 2 input AND gate, 3 input AND gate etc... with different performace speed.

**Need for different flavours of gate**: In order to make a faster circuit, the clock frequency should be high. For that the time period of the clock should be as low as possible. However, in a sequential circuit, clock period depends on three factors so that data is not lost or to be glitch free.

For the below circuit the three factors are
- Clock to Q of flipflop A
- Propagation delay of combinational circuit
- Setuptime of flipflop B


### 2.3.1 Labs on Yosys introduction<a name="day1_231"></a>
# 3. Day 2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles<a name="day2"></a>
## 3.1 Introduction to timing .libs<a name="day2_31"></a>
### 3.1.1 LAB- Introduction to dot Lib<a name="day2_311"></a>
## 3.2 LAB- Hierarchical synthesis and flat synthesis<a name="day2_32"></a>
## 3.3 Various Flop coding styles and optimization<a name="day2_33"></a>
### 3.3.1 Lab- flop synthesis simulations<a name="day2_331"></a>
### 3.3.2 Interesting optimisations<a name="day2_332"></a>
# 4. Day 3- Combinational and sequential optmizations<a name="day3"></a>
## 4.1 Combinational logic optimization with examples<a name="day3_41"></a>
## 4.2 Sequential Logic Optimization with examples<a name="day3_42"></a>
### 4.2.1 Basic<a name="day3_421"></a>
### 4.2.2. Advanced<a name="day3_422"></a>
### 4.2.3 Sequential optimisation of unused outputs<a name="day3_423"></a>
# 5. Day 4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch<a name="day4"></a>
## 5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements<a name="day4_51"></a>
### 5.1.1 GLS Concepts And Flow Using Iverilog<a name="day4_511"></a>
### 5.1.2 Synthesis Simulation Mismatch<a name="day4_512"></a>
## 5.2 Lab- GLS Synth Sim Mismatch<a name="day4_52"></a>
## 5.3 Lab- Synthesis simulation mismatch blocking statement<a name="day4_53"></a>
# 6. Day 5- if, case, for loop and for generate<a name="day5"></a>
## 6.1 If and Case constructs<a name="day5_61"></a>
### 6.1.1 If construct<a name="day5_611"></a>
### 6.1.2 Case construct<a name="day5_612"></a>
## 6.2 Lab- Incomplete IF<a name="day5_62"></a>
## 6.3 Lab- incomplete overlapping Case<a name="day5_63"></a>
## 6.4 For Loop and For Generate<a name="day5_64"></a>
## 6.5 Lab- For and For Generate<a name="day5_65"></a>
# 7. Word of Thanks<a name="end"></a>


