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
# 2. Day-1- Introduction to Verilog RTL design and Synthesis<a name="day1"></a>
## 2.1 Various aspects of frontend design<a name="day1_21"></a>
## 2.2 Introduction to open source simulator iverilog and gtkwave<a name="day1_22"></a>
### 2.2.1 Lab examples using iverilog and gtkwave<a name="day1_221"></a>
## 2.3 Introduction to Yosys synthesizer<a name="day1_23"></a>
### 2.3.1 Labs on Yosys introduction<a name="Introduction"></a>
# 3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles<a name="day2"></a>
## 3.1 Introduction to timing .libs<a name="day2_31"></a>
### 3.1.1 LAB- Introduction to dot Lib<a name="day2_311"></a>
## 3.2 LAB- Hierarchical synthesis and flat synthesis<a name="day2_32"></a>
## 3.3 Various Flop coding styles and optimization<a name="day2_33"></a>
### 3.3.1 Lab- flop synthesis simulations<a name="day2_331"></a>
### 3.3.2 Interesting optimisations<a name="day2_332"></a>
# 4. Day3- Combinational and sequential optmizations<a name="day3"></a>
## 4.1 Combinational logic optimization with examples<a name="day3_41"></a>
## 4.2 Sequential Logic Optimization with examples<a name="day3_42"></a>
### 4.2.1 Basic<a name="day3_421"></a>
### 4.2.2. Advanced<a name="day3_422"></a>
### 4.2.3 Sequential optimisation of unused outputs<a name="day3_423"></a>
# 5. DAY4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch<a name="day4"></a>
## 5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements<a name="day4_51"></a>
### 5.1.1 GLS Concepts And Flow Using Iverilog<a name="day4_511"></a>
### 5.1.2 Synthesis Simulation Mismatch<a name="day4_512"></a>
## 5.2 Lab- GLS Synth Sim Mismatch<a name="day4_52"></a>
## 5.3 Lab- Synthesis simulation mismatch blocking statement<a name="day4_53"></a>
# 6. DAY5- if, case, for loop and for generate<a name="day5"></a>
## 6.1 If and Case constructs<a name="day5_61"></a>
### 6.1.1 If construct<a name="day5_611"></a>
### 6.1.2 Case construct<a name="day5_612"></a>
## 6.2 Lab- Incomplete IF<a name="day5_62"></a>
## 6.3 Lab- incomplete overlapping Case<a name="day5_63"></a>
## 6.4 For Loop and For Generate<a name="day5_64"></a>
## 6.5 Lab- For and For Generate<a name="day5_65"></a>
# 7. Word of Thanks<a name="end"></a>


