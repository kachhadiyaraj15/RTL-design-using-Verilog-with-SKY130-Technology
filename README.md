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

![git_clone_both](https://user-images.githubusercontent.com/110079774/183273838-2d12fae7-01e8-4565-bf90-e1d59a6fe41b.png)
![verilog_filea_ls](https://user-images.githubusercontent.com/110079774/183273850-1cfb4f99-a94b-4027-849d-bd2fcebd7eef.png)

In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code.

![run_mux_code](https://user-images.githubusercontent.com/110079774/183274006-b3ef1ba5-7f6e-42f9-a4ae-d28138ad36a5.png)

Here is the code and gtkwave snippets:
```
module good_mux (input i0 , input i1 , input sel , output reg y); 
	always @ (*)
	begin
		if(sel)
		y <= i1;
		else 
		y <= i0;
	end
endmodule**


`timescale 1ns / 1ps
module tb_good_mux;
// Inputs
reg i0,i1,sel;
// Outputs
wire y;
  		// Instantiate the Unit Under Test (UUT), name based instantiation
	good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));
	//good_mux uut (sel,i0,i1,y);  //order based instantiation
initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
end
always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule**
```

![gtk_wave_mux](https://user-images.githubusercontent.com/110079774/183274025-fb9ae618-6a87-4314-a870-d58df88e6abc.png)


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

![image](https://user-images.githubusercontent.com/110079774/183274068-18c9451e-0bda-458e-a845-9f23d5d64e4b.png)</br>
The equation is as follows<br>
![image](https://user-images.githubusercontent.com/110079774/183274101-bd8a9247-8987-4c8b-9c80-53fea30a9d4a.png)<br>
As per the above equation, for a smaller propagation delay, we need faster cells. But again, why do we have faster cells? This is to ensure that there are no HOLD time violations at B flipflop. This complete collection forms .lib

Faster Cells vs Slower Cells: Load in digital circuit is of Capacitence. Faster the charging or dicharging of capacitance, lesser is the celll delay. However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more current i.e, we need WIDE TRANSISTORS.

Wider transistors have lesser delay but consume more area and power. Narrow transistors are other way around. Faster cells come with a cost of area and power.

Selection of the Cells: We'll need to guide the Synthesizer to choose the flavour of cells that is optimum for implementation of logic circuit. Keeping in view of previous observations of faster vs slower cells,to avoid hold time violations, larger circuits, sluggish circuits, we offer guidance to synthesizer in the form of Constraints.

Below is an illustration of Synthesis.<br>
![image](https://user-images.githubusercontent.com/110079774/183274119-a8e89b9f-9c9b-4f8f-9594-1797d82d10ba.png)


### 2.3.1 Labs on Yosys introduction<a name="day1_231"></a>
Invoking Yosys:
![yosys_version](https://user-images.githubusercontent.com/110079774/183274139-39fb09a5-dfe8-40e6-afd8-b8c407ab51fe.png)

Snippet below illustrates reading .lib, design and choosing the module to synthesize:
![read_verilog_piso_yosys](https://user-images.githubusercontent.com/110079774/183274551-b0fc9fef-2c54-43fc-9fd7-5745e8fcfb80.png)
Generating Netlist: The logic of good_mux will be realizable using gates in the sky130_fd_sc_hd__tt_025C_1v80.lib file.
![yosys_step_4_5](https://user-images.githubusercontent.com/110079774/183274614-334addee-3528-487a-a625-5f50b963f716.png)
Below is the snippet showing the synthesis results and synthesized circuit for multiplexer.
![ABC_reults](https://user-images.githubusercontent.com/110079774/183275399-f9189f71-7593-44e5-bee3-7b31d3d50eb4.png)

![piso_show_command](https://user-images.githubusercontent.com/110079774/183275406-6ddfffca-8f2a-4b1f-97ac-303e98f3bb6d.png)

**Netlist Code**<br>
![netlist_command_list](https://user-images.githubusercontent.com/110079774/183275556-d92ce25d-bf06-4445-a10a-6322e6e67dbf.png)
![netlist_file_piso](https://user-images.githubusercontent.com/110079774/183275558-e8a85336-7e89-477b-9a93-9a2a66da318a.png)

**Simplified netlist code**:<br>
This code consisits of additional switch. To further simplify, we use below command

![yosys_last_command](https://user-images.githubusercontent.com/110079774/183275788-85074534-da76-472e-8936-cb980955d2a4.png)


# 3. Day 2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles<a name="day2"></a>

## 3.1 Introduction to timing .libs<a name="day2_31"></a>

### 3.1.1 LAB- Introduction to dot Lib<a name="day2_311"></a>
This lab guides us through the .lib files where we have all the gates coded in. According to the below parameters the libraries will be characterized to model the variat

![image](https://user-images.githubusercontent.com/110079774/183275828-8b5823f3-5ae1-429b-95cb-b22556c035be.png)

With in the lib file, the gates are delared as follows to meet the variations due to process, temperatures and voltages.
![image](https://user-images.githubusercontent.com/110079774/183275841-8cc3d1d5-161a-4d53-bf6f-36c99e6cfed9.png)

For the above example, for all the 32 cominations i.e 2^5 (5 is no.of variables), the delay, power and all the related parameters for each gate are mentioned.
![image](https://user-images.githubusercontent.com/110079774/183275878-57cf7cfd-9b4f-4876-88e9-bf955a8335ac.png)

This image displays the power consumtion comparision.
![image](https://user-images.githubusercontent.com/110079774/183275892-035ed263-c96d-4636-8e14-ef036bfdeaa9.png)

Below image is the delay order for the different flavor of gates.
![image](https://user-images.githubusercontent.com/110079774/183275904-0ac70286-2da2-4778-997c-36d7edbc3df5.png)


## 3.2 LAB- Hierarchical synthesis and flat synthesis<a name="day2_32"></a>
**multiple_module**
```
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
wire net1;
sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```
This is the schematic as per the connections in the above module.
![image](https://user-images.githubusercontent.com/110079774/183275959-132f8117-d860-4c86-a1e6-ab88e7a1036a.png)

However, the yosys synthesizer generates the following schematic instead of the above one and with in the submodules, the connections are made
![piso_show_command](https://user-images.githubusercontent.com/110079774/183275966-34b878f3-b85d-4ef0-9fbb-140388d19401.png)

The synthesizer considers the module hierarcy and does the mapping accordting to instantiation. Here is the hierarchical netlist code for the multiple_modules:
```
module multiple_modules(a, b, c, y);
	  input a;
	 input b;
	 input c;
	  wire net1;
	 output y;
  sub_module1 u1 (.a(a),.b(b),.y(net1) );
  sub_module2 u2 (.a(net1),.b(c),.y(y));
endmodule

module sub_module1(a, b, y);
 wire _0_;
 wire _1_;
 wire _2_;
 input a;
 input b;
 output y;
 sky130_fd_sc_hd__and2_0 _3_ (.A(_1_),.B(_0_),.X(_2_));
 assign _1_ = b;
 assign _0_ = a;
 assign y = _2_;
endmodule

module sub_module2(a, b, y);
wire _0_;
 wire _1_;
 wire _2_;
input a;
input b;
 output y;
 sky130_fd_sc_hd__lpflow_inputiso1p_1 _3_ (.A(_1_),.SLEEP(_0_),.X(_2_) );
 assign _1_ = b;
 assign _0_ = a;
 assign y = _2_;
endmodule
```
Flattened netlist:

In flattened netlist, the hierarcies are flattend out and there is single module i.e, gates are instantiated directly instead of sub_modules. Here is the flattened netlist code for the multiple_modules:
```
module multiple_modules(a, b, c, y);
	 wire _0_;
	 wire _1_;
	 wire _2_;
	 wire _3_;
	 wire _4_;
	 wire _5_;
	 input a;
	 input b;
	 input c;
	 wire net1;
	 wire \u1.a ;
	 wire \u1.b ;
	 wire \u1.y ;
	 wire \u2.a ;
	 wire \u2.b ;
	 wire \u2.y ;
	output y;
	 sky130_fd_sc_hd__and2_0 _6_ (
	  .A(_1_),
	 .B(_0_),
	 .X(_2_)
	);
	 sky130_fd_sc_hd__lpflow_inputiso1p_1 _7_ (
	  .A(_4_),
	  .SLEEP(_3_),
	  .X(_5_)
	 );
	 assign _4_ = \u2.b ;
	 assign _3_ = \u2.a ;
	 assign \u2.y  = _5_;
	 assign \u2.a  = net1;
	 assign \u2.b  = c;
	 assign y = \u2.y ;
	 assign _1_ = \u1.b ;
	 assign _0_ = \u1.a ;
	 assign \u1.y  = _2_;
	 assign \u1.a  = a;
	 assign \u1.b  = b;
	 assign net1 = \u1.y ;
	endmodule
```

The commands to get the hierarchical and flattened netlists is shown below:

**yosys> write_verilog -noattr multiple_modules_hier.v**

	8. Executing Verilog backend. Dumping module \multiple_modules'. Dumping module \sub_module1'. Dumping module `\sub_module2'.

**yosys> !gvim multiple_modules_hier.v**

	11. Shell command: gvim multiple_modules_hier.v
	
**yosys> flatten**

	12. Executing FLATTEN pass (flatten design). Deleting now unused module sub_module1. Deleting now unused module sub_module2. <suppressed ~2 debug messages>
	
**yosys> write_verilog -noattr multiple_modules_flat.v**

	13. Executing Verilog backend. Dumping module `\multiple_modules'.

**yosys> !gvim multiple_modules_flat.v**

	14. Shell command: gvim multiple_modules_flat.v

This is the synthyesized circuit for a flattened netlist. Here u1 and u2 are flattened and directly or gates are realized.
![image](https://user-images.githubusercontent.com/110079774/183276059-226e1835-1c0d-45cc-b11c-9203ce15aac8.png)

Here is the synthesized circuit of sub_module1. We are also generating module level synthesis so that if there is a top module with multiple and same sub_modules, we can synthesize it once and can use and connect the same netlist multiple times in the top module netlist.

Another reason to generate module level synthesis and then stictch them together is to avoid errors in a top module if its massive and consists of several sub modules. Generating netlist for synthesis and then stiching it together in top level becomes easier and reduces risk of output mismatch.

We control this synthesis using **synth -top <module_name>** command
![image](https://user-images.githubusercontent.com/110079774/183276070-46f9ba75-6fcc-4a6b-8043-ea3a73947cb7.png)


## 3.3 Various Flop coding styles and optimization<a name="day2_33"></a>
**Why Flops and Flop coding styles part1**

In this session, the discussion was about how to code various types of flops and various styles of coding a flop.

**Why a Flop?**

In a combinational circuit, the output changes after the propagation delay of the circuit once inputs are changed. During the propagation of data, if there are different paths with different propagation delays, there might be a chance of getting a glitch at the output.
If there are multiple combinational circuits in the design, the occurances of glitches are more thereby making the output unstable.
To curb this drawback, we are going for flops to store the data from the cominational circuits. When a flop is used, the output of combinational circuit is stored in it and it is propagated only at the posedge or negedge of the clock so that the next combinational circuit gets a glitch free input thereby stabilising the output.

We use initialize signals or control pins called set and reset on a flop to initialize the flop, other wise a garbage value to sent out to the next combinational circuit. These control pins can be synchronous or asynchronous.

### 3.3.1 Lab- flop synthesis simulations<a name="day2_331"></a>
**d-flipflop with asynchronous reset**- Here the output q goes low whenever reset is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to low.
![image](https://user-images.githubusercontent.com/110079774/183276102-83b33d3e-7ffe-40cb-aee3-377865299675.png)
```
 module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
	always @ (posedge clk , posedge async_reset)
	begin
		if(async_reset)
			q <= 1'b0;
		else	
			q <= d;
	end
endmodule
```
### Simulation:
![image](https://user-images.githubusercontent.com/110079774/183276128-8ab444e7-6af5-461b-a4ad-d2cda77fa734.png)

### Synthesized circuit:
![image](https://user-images.githubusercontent.com/110079774/183276139-cf731cbc-63af-4a2e-a2ab-99e364dd1ae4.png)

**d-flipflop with asynchronous set**- Here the output q goes high whenever set is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to high.

![image](https://user-images.githubusercontent.com/110079774/183276144-207f05d4-54b8-4cc8-b828-c3a989e39939.png)
```
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
	always @ (posedge clk , posedge async_set)
	begin
		if(async_set)
			q <= 1'b1;
		else
			q <= d;
	end
endmodule
```
### Simulation:
![image](https://user-images.githubusercontent.com/110079774/183276158-8a4b7861-a3cd-4020-803e-7716103246c7.png)

### Synthesized circuit:
![image](https://user-images.githubusercontent.com/110079774/183276167-a9d738c0-3c8b-4ec9-a317-4954302fc8fc.png)

**d-flipflop with synchronous reset** - Here the output q goes low whenever reset is high and at the positive edge of the clock. Here the reset of the output depends on the clock
![image](https://user-images.githubusercontent.com/110079774/183276195-67af17b9-a74f-4dff-b1a1-20f477c7dc8b.png)
```
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
	always @ (posedge clk )
	begin
		if (sync_reset)
			q <= 1'b0;
		else	
			q <= d;
	end
endmodule
```
### Simulation:
![image](https://user-images.githubusercontent.com/110079774/183276210-7b13cbd5-8e0a-451f-b201-f14c9e0a7c8c.png)

### Synthesized circuit:
![image](https://user-images.githubusercontent.com/110079774/183276215-ffa06559-960c-4f02-a41b-231eaa83116d.png)

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


