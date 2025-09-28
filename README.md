# 🚀 Week 1: RTL Design & Gate-Level Synthesis (GLS)

<div align="center">

![EDA](https://img.shields.io/badge/OpenSource-EDA-purple?style=for-the-badge\&logo=opensourceinitiative)
![RISC-V](https://img.shields.io/badge/RISC--V-OpenSource-blue?style=for-the-badge\&logo=riscv)
![VSD](https://img.shields.io/badge/VSD-TapeoutProgram-orange?style=for-the-badge\&logo=verilog)

</div>

Welcome to **Week 1 of the RTL & Synthesis Workshop**! 🛠️

This week, we dive into **Verilog RTL design**, **simulation with Icarus Verilog**, **waveform visualization with GTKWave**, and **logic synthesis with Yosys**.

The workshop is structured **day-wise** for clarity:

| Day / Section | Description | Link |
| ------------- | ----------- | ---- |
| 📌 Day 1 | Introduction to Verilog RTL Design & Synthesis | [Go to Day 1](#day1) |
| 📌 Day 2 | Timing Libraries, Hierarchical vs Flat Synthesis, Efficient Flop Coding Styles | [Go to Day 2](#day2) |
| 📌 Day 3 | Combinational and Sequential Optimizations | [Go to Day 3](#day3) |
| 📌 Day 4 | Gate-Level Simulation (GLS), Blocking vs Non-blocking, Synthesis-Simulation Mismatch | [Go to Day 4](#day4) |
| 📌 Day 5 | Optimization in Synthesis | [Go to Day 5](#day5) |
| 📌 Cheat Sheet | RTL Simulation & Synthesis Cheat Sheet | [Go to Cheat Sheet](#CheatSheet) |

<br>

<h3 id="day1">📌 Day 1 – Introduction to Verilog RTL Design & Synthesis</h3>

### 1️⃣ Core Concepts

| **Term**        | **Definition**                                                                           |
| --------------- | ---------------------------------------------------------------------------------------- |
| **Design**      | The Verilog code that implements the intended logic functionality.                       |
| **Testbench**   | A simulation setup that applies stimulus (input vectors) and checks outputs.             |
| **Simulator**   | Tool used to verify RTL design correctness. In this workshop, we use **Icarus Verilog**. |
| **Synthesizer** | Tool that converts RTL code to a gate-level netlist. We use **Yosys**.                   |


**Simulator Workflow:**

* Monitors input signals for changes. 🔍
* Evaluates outputs when inputs change. ⚡
* If no input changes → outputs remain stable. ✅

<br>

### 2️⃣ Icarus Verilog Simulation Flow

```
Design (Verilog) + Testbench → iverilog → a.out → .vcd file → GTKWave
```

**Visualization:** <img width="1913" height="1076" alt="Screenshot from 2025-09-26 16-25-03" src="https://github.com/user-attachments/assets/bebd1ae1-b87b-4a9c-87ff-2d840d57a130" />

<br>

### Verilog Simulation Flow using Icarus Verilog & GTKWave

**Step-by-step:**

1. **DESIGN**
   Verilog code describing system **logic** & **functionality**. 💡

2. **Test Bench**
   Separate Verilog file applied to the design. It generates **stimulus (input signals/test vectors)** to verify the design's functionality. 🧪

3. **Iverilog**

   * Compiles **Design + Test Bench**
   * Runs simulation

4. **VCD File (Value Change Dump)**
   During simulation, Iverilog generates a **VCD file** which records **value changes in signals** over time. ⏳

5. **GTKWave**
   A **waveform visualization tool** that:
   * Loads the **VCD file**
   * * Allows designers to inspect **waveforms, clock cycles, and output transitions** This flow facilitates **debugging**, **verification** and provides **insight into design operation** by visualizing simulation results. 📊

> This flow facilitates **debugging**, **verification**, and provides **insight into design operation**.

<br>

### 3️⃣ Testbench Structure

**Diagram Concept:**

```
Stimulus Generator → Design → Stimulus Observer
```

* **Stimulus Generator:** Creates input signals (clocks, resets, data) ⏱️
* **Design:** Processes inputs → produces outputs 🔄
* **Stimulus Observer:** Checks outputs vs expected values ✅

**Visualization:** <img width="1913" height="1076" alt="Screenshot from 2025-09-26 16-24-32" src="https://github.com/user-attachments/assets/82a00556-b831-407f-9827-5fb89eae83e6" />

The **Verilog Design** and its **Test Bench** are compiled and run by the **Iverilog simulator**. During simulation, the tool produces a **VCD file (Value Change Dump format)**, which records **signal changes over time**. To visualize the signal transitions and verify behavior, this **VCD file** is loaded into **GTKWave**, a waveform viewer that aids **debugging** and **analysis**
<br>

**Notes:**

* Testbench itself does **not expose primary inputs/outputs**.
* Ensures design correctness under **all input conditions**.
* Same testbench can be reused for **RTL & synthesized netlist verification**.
* The design may have **one or more primary input/output signals**.
* The test bench itself **does not expose any primary I/Os** directly. Its sole function is to **generate, monitor, and validate** the design's responses within a controlled simulation.

 <br>
 
### 4️⃣ Yosys & Gate Libraries

**What is Yosys?**

* Converts HDL → gate-level netlist ⚙️
* Performs **optimization & technology mapping**
* Ensures functional correctness via **gate-level simulation** ✅

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-01-29" src="https://github.com/user-attachments/assets/0952cfea-e919-4ad9-b046-2db15e60e963" />  

Shows the basic synthesis flow: the RTL design and a .lib library are input to a synthesizer (Yosys), producing a gate-level netlist. Yosys is specifically mentioned as the tool used.
<br>

**Yosys Setup:** <img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-01-39" src="https://github.com/user-attachments/assets/34ac10e3-8dca-4193-a007-4eedec8b003a" />

<br>

### 5️⃣ Digital Logic & Synthesis Flow

#### What is `.lib`?

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-03-24" src="https://github.com/user-attachments/assets/e0136a80-db41-46d8-a386-ae539c2bd528" />


<br>

* `.lib` – Collection of logical modules
* Includes basic logic gates: AND, OR, NOT, etc.
* Different flavors of the same gate:


| **Gate Type** | **Flavors**        |
| ------------- | ------------------ |
| 2-input AND   | Slow, Medium, Fast |
| 3-input AND   | Slow, Medium, Fast |
| 4-input AND   | Slow, Medium, Fast |
| …             | …                  |


**Explanation:**

* `.lib` contains **pre-characterized digital gates**
* Multiple **drive strengths/speeds** (slow, medium, fast)
* Allows **timing, power, and area optimization**

> Slow cells → meet **hold constraints**
> Fast cells → improve **performance** 🚀

<br>

### RTL Design

```verilog
module sample_code (
  input clk, rst,
  output result, done
);
always @ (posedge clk, posedge rst)
if (rst)
  // reset logic
else
  // normal operation
endmodule
```

**Explanation:**

* RTL (Register Transfer Level) describes **circuit behavior**
* Specifies **data flow & control over registers and combinational logic**
* Input to the **synthesis tool** 🛠️

<br>

### Why We Need Different Flavours of Gates?
* Combinational delay determines max circuit speed:

  <img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-03-31" src="https://github.com/user-attachments/assets/c1e6630c-df55-4009-a363-f7d8a3c5d751" />

<br>

$$ T_{CLK} > T_{CQ\_A} + T_{COMBI} + T_{SETUP\_B} $$

* Fast cells → reduce combinational delay
* Slow cells → prevent hold violations

**Explanation:** Circuit speed is limited by logic path delays. 
* Using **fast gates** → reduces delay, allows higher clock frequencies 
* Using **slow gates** → prevents early arrival at flip-flops (hold violations)

> Multiple gate flavors in `.lib` provide **timing optimization flexibility** ⏱️

<br>

### Selection of Cells During Synthesis

* Synthesizer chooses **optimum cell flavor** based on constraints
* Too many **fast cells** → High power/area, hold violations ⚡
* Too many **slow cells** → Sluggish performance 🐢

**Explanation:** Synthesis tools balance **speed, power, and area** by choosing appropriate cells. Timing and design constraints direct the tool in selecting **fast vs slow cells** to meet circuit requirements.

> Constraints guide the synthesizer in balancing **speed, power, and area**

<br>

### Logic Synthesis

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-03-08" src="https://github.com/user-attachments/assets/9c4b017d-c3d6-422e-abf7-218d002975db" />

* RTL → Gate-level translation
* Converts design into **gates & connections (netlist)**

**Explanation:**

* Maps RTL to a **gate-level netlist** using `.lib` cells
* Specifies **all gates & interconnections** needed for fabrication 🏭

<br>

### 6️⃣ Synthesis Illustration (Mux + Register)

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-05-57" src="https://github.com/user-attachments/assets/3403c5b1-28a6-4f54-b1bb-3b84e7b6a94a" />

```verilog
module (A,B,sel,clock,reset,Q)
input A,B,sel,clock,reset;
output Q;
wire int;
assign int = sel ? A : B;
always @ (posedge clock or posedge reset)
begin
  if (reset)
    Q <= 1'b0;
  else
    Q <= int;
end
endmodule
```

**Explanation:**

* RTL describing a **multiplexer feeding a flip-flop**
* Synthesized into **gates & registers** from `.lib`
* Output → **physical digital circuit**

<br>

### 7️⃣ Digital Logic Circuit from RTL

```verilog
module sample_code (
  input clk, rst,
  output result, done
);
always @ (posedge clk, posedge rst)
if (rst)
  // reset
else
  // normal operation
endmodule
```

**Explanation:** The RTL behavior for a flip-flop is synthesized into a **physical digital logic circuit**, i.e., a **register in hardware**.

<br>

### 8️⃣ Why We Need Slow Cells

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-04-36" src="https://github.com/user-attachments/assets/dca7c645-f9f3-4b02-b61c-d4797f7f1b98" />
<br>

$$ T_{HOLD\_B} < T_{CQ\_A} + T_{COMBI} $$

* Prevents **hold issues** at flip-flops
* Fast cells → performance
* Slow cells → timing robustness

**Explanation:**
Slow cells prevent **early data arrival**; fast cells maintain **overall performance**

<br>

### Faster Cells vs Slower Cells

* Load → Capacitance ⚡
* Faster charging → Lower delay, higher area/power
* Wider transistors → Low delay, higher area & power
* Narrow → Higher delay, lower area & power

**Explanation:** Gate speed depends on **ability to drive capacitive loads**. 
* **Fast cells** (wide transistors) → low delay, higher area/power
*  * **Slow cells** (narrow transistors) → save area/power but increase delay

> Fast cells trade **speed for area/power** 🔄

<br>

### 4️⃣ Labs – Day 1

#### Clone the Workshop Repository 🛠️

To start the lab, first clone the workshop repository from GitHub:

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
cd sky130RTLDesignAndSynthesisWorkshop
```
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 09-47-04" src="https://github.com/user-attachments/assets/59da2a7e-7618-4a75-b68f-c89e2444a421" />

This will download all the necessary files and bring you into the project folder where the lab exercises are located.

verilog_labs folder Contains all the design and testbench files:
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 09-47-36" src="https://github.com/user-attachments/assets/20dd968a-6234-4409-9b9c-9b4a90b3d6e2" />
<br>
#### Lab 1: Introduction to Icarus Verilog & Design Testbench

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-00-19" src="https://github.com/user-attachments/assets/de0dbded-01b7-4148-8529-d6ac43fe4e0b" />

**2-to-1 Multiplexer Example:**

```verilog
module good_mux (
    input i0,
    input i1,
    input sel,
    output reg y
);
always @ (*) begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

* **Inputs:** `i0`, `i1` (data), `sel`
* **Output:** `y`
* **Logic:** `sel=1 → y=i1`, else `y=i0`
* **Goal:** Compile, simulate, verify outputs

```bash
iverilog good_mux.v tb_good_mux.v -o a.out
./a.out
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 09-49-18" src="https://github.com/user-attachments/assets/54f628d3-0d23-457a-bbfc-18b144ea6452" />

<br>

#### Lab 2: GTKWave – Visualizing Waveforms

* **Goal:** Inspect logic behavior and signal transitions
* **Command:**

```bash
gtkwave tb_good_mux.vcd
```
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 09-50-10" src="https://github.com/user-attachments/assets/726f93ec-50e6-4537-a931-6dfac4112a0e" />  

* **Checkpoints:**

  * Clock cycles ⏰
  * Input transitions 🔄
  * Output correctness ✅

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 09-51-33" src="https://github.com/user-attachments/assets/0e44d250-8f10-4950-8c22-da2373371a31" />

<br>

#### Lab 3: Yosys Synthesis – Gate-Level Netlist

* **Goal:** Convert RTL → gate-level schematic

```bash
yosys
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-14-23" src="https://github.com/user-attachments/assets/53f8406b-1ae8-489b-8c79-b00c93ec0d75" />

<br>

```bash
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-19-00" src="https://github.com/user-attachments/assets/2d332943-5486-48bd-a501-75fc0f0b1b2a" />  

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-22-45" src="https://github.com/user-attachments/assets/7248fe64-e27f-4cd6-b0d0-d380089ebd05" />

<br>

```bash
show
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-24-07" src="https://github.com/user-attachments/assets/213b8379-b09b-4f91-8dd8-c3a54d6bdf74" />  

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-25-07" src="https://github.com/user-attachments/assets/3194ef92-4623-4101-9e95-c608d27dffce" />

```bash
write_verilog good_mux_netlist.v

```
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-31-26" src="https://github.com/user-attachments/assets/392050dd-4065-476d-b09f-9729706c55ea" />

<br>

```bash
write_verilog -noattr good_mux_netlist.v
```
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-33-07" src="https://github.com/user-attachments/assets/cff43080-ae2c-47f4-98c1-a21e4ce0b977" />

<br>

**Key Observations:** 

* Inputs/outputs remain unchanged → same testbench works for RTL & GLS.
* Netlist maps **gates & flip-flops** to the library cells.

 <img width="1815" height="1080" alt="image" src="https://github.com/user-attachments/assets/0705de05-04fb-41fb-8fc5-9f57748f7a24" />

<br>

### ✅ Key Takeaways – Day 1

* RTL defines **functional behavior**, testbench validates it
* **Icarus Verilog + GTKWave** → simulation & debugging workflow
* **Yosys** maps RTL → gate-level netlist with optimization
* **Faster vs slower cells** balance timing, area, and power ⚡
* Same **testbench can verify RTL & synthesized netlist**

<br>

<h3 id="day2">📌 Day 2 – Timing Libraries, Hierarchical vs Flat Synthesis, Efficient Flop Coding Styles</h3>

### 1️⃣ Understanding `.lib` Files 📚

**What is a `.lib` (Timing Library)?**

* A `.lib` file contains **pre-characterized standard cells** with info about timing, power, area, and operating conditions.
* Helps synthesis tools map **RTL → gates** efficiently while respecting performance, power, and area constraints.
* Example header from SkyWater `sky130_fd_sc_hd__tt_025C_1v80.lib`:

```tcl
library ("sky130_fd_sc_hd__tt_025C_1v80") {
    define(def_sim_opt,library,string);
    define(driver_model,library,string);
    technology("cmos");
    delay_model : "table_lookup";
    time_unit : "1ns";
    voltage_unit : "1V";
    current_unit : "1mA";
    capacitive_load_unit(1.0, "pf");
    default_max_transition : 1.5;
    operating_conditions ("tt_025C_1v80") {
        voltage : 1.8;
        temperature : 25.0;
        process : 1.0;
    }
    ...
}
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 13-04-16" src="https://github.com/user-attachments/assets/710d34b9-cb53-4963-9b4c-6ab43a6d5c39" />

**Key Highlights of `.lib`:**

| Attribute                      | Description                                                          |
| ------------------------------ | -------------------------------------------------------------------- |
| **technology**                 | CMOS or other tech                                                   |
| **delay_model**                | How delays are reported (table_lookup, etc.)                         |
| **time/voltage/current units** | Units used for characterization                                      |
| **operating_conditions**       | PVT conditions: Process, Voltage, Temperature                        |
| **cell definitions**           | Each gate (AND2, OR2, NAND, etc.) with area, leakage, waveform, etc. |

**Process-Voltage-Temperature (PVT) Analysis**:

* **Process variation**: Fabrication imperfections → different transistor speeds
* **Voltage variation**: Voltage fluctuations change switching behavior ⚡
* **Temperature variation**: Chips heat up differently → affects delay
* **Goal:** Ensure design works **across all PVT corners** using `.lib` data

<br>

### 2️⃣ Example Cell Analysis – AND2

**Sky130 standard cell examples:**

| Attribute          | AND2_0 (Slow)           | AND2_2 (Medium) | AND2_4 (Fast) |
| ------------------ | ----------------------- | --------------- | ------------- |
| Leakage Power      | Low                     | Medium          | High          |
| Area               | 6.256                   | 7.507           | 8.758         |
| Cell Footprint     | "sky130_fd_sc_hd__and2" | Same            | Same          |
| Cell Leakage Power | 0.0019                  | 0.0036          | 0.0045        |

<img width="1920" height="1080" alt="comparision" src="https://github.com/user-attachments/assets/6ea6ab60-84be-43f2-8fc3-44a96accd8cf" />
<br>

**Observation:**

* **Wider/Faster cells → lower delay, higher area & leakage**
* **Smaller/Slower cells → higher delay, lower area & leakage**

**Example `.lib` snippet for AND2:**

```tcl
cell ("sky130_fd_sc_hd__and2_0") {
    leakage_power () { value : 0.0021372; when : "!A&B"; }
    leakage_power () { value : 0.0018183; when : "!A&!B"; }
    area : 6.256;
    driver_waveform_fall : "ramp";
}
```

* **Design takeaway:** Synthesis tools pick cells (slow/medium/fast) based on **timing, power, and area tradeoffs**.

<br>

### 3️⃣ Lab 5 – Hierarchical vs Flat Synthesis 🏗️

**RTL Example: `multiple_modules.v`**

```verilog
module sub_module2(input a, input b, output y);
    assign y = a | b;
endmodule

module sub_module1(input a, input b, output y);
    assign y = a & b;
endmodule

module multiple_modules(input a, input b, input c, output y);
    wire net1;
    sub_module1 u1(.a(a), .b(b), .y(net1));
    sub_module2 u2(.a(net1), .b(c), .y(y));
endmodule
```
<img width="1920" height="1080" alt="multiple modules" src="https://github.com/user-attachments/assets/ab1bea56-cd9a-49ee-8611-e3d25da63f41" />

<br>

**Synthesis Flow using Yosys:**

```tcl
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog -noattr multiple_modules_hier.v
```
<img width="1920" height="1080" alt="synthesis multiple modules" src="https://github.com/user-attachments/assets/a8951263-b60f-428d-b22f-8c66b9fc88d6" />

<br>

<img width="1920" height="1080" alt="statistics" src="https://github.com/user-attachments/assets/af996cac-c9af-421d-8b9d-2d47aab62dc7" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 13-34-18" src="https://github.com/user-attachments/assets/b1d2fef7-0871-4b84-b56e-6acf90eca051" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 13-37-18" src="https://github.com/user-attachments/assets/ef26c3ca-bd29-4dce-96f5-1bea097402e9" />

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-23 13-33-59" src="https://github.com/user-attachments/assets/247a5941-4160-4f08-8705-8b2e1a5d7f88" />

<br>

**Observation:**

* Hierarchical netlist preserves **module structure**.
* Example: AND2 cell used as `sky130_fd_sc_hd__and2_0` instead of OR → **logical effort / transistor stacking optimization**:

```verilog
sky130_fd_sc_hd__and2_0 _3_ ( .A(_1_), .B(_0_), .X(_2_)
```

**Flattening Netlist:**

```tcl
flatten
write_verilog -noattr multiple_modules_flat.v
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 14-52-22" src="https://github.com/user-attachments/assets/18105746-413d-41fd-8835-1b7c53b29aa1" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 14-28-07" src="https://github.com/user-attachments/assets/591c380f-5243-4e34-9d5b-13598af41663" />

<br>

<img width="1920" height="1080" alt="flatten_synthesis" src="https://github.com/user-attachments/assets/ca9ada83-e4cd-4f00-9d63-7015c7c783b0" />

<br>

* Flattening merges hierarchy → **better optimization, harder to debug**
* Submodule synthesis allows **divide & conquer**, useful for repeated modules in large designs

<br>

### Comparision 
<img width="1920" height="1080" alt="Screenshot from 2025-09-23 14-31-27" src="https://github.com/user-attachments/assets/48e8c797-4640-4dfa-b415-0511e7fb0d34" />

<br>

### 4️⃣ Flop Coding Styles & Synthesis ⚡

**Why Use Flops?**

* Flops prevent **glitches** caused by combinational logic
* Synchronize signals in **clocked domains**
* Provide predictable behavior for synthesis

**Flop Variants Simulated:**

* `dff_asyncres.v` → Async Reset
* `dff_async_set.v` → Async Set
* `dff_syncres.v` → Sync Reset
* `dff_asyncres_syncres.v` → Async Reset + Sync Reset

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-25-04" src="https://github.com/user-attachments/assets/565013c8-a6c2-49ae-951b-9a67fe426684" />

<br>

**Simulation Flow for Flops:**

```tcl
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-27-54" src="https://github.com/user-attachments/assets/be76645c-e6ec-44aa-acd8-b0fbbf442e57" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-29-31" src="https://github.com/user-attachments/assets/e5f0ed3f-1a07-4a72-bc11-8936226b9698" />


**Synthesis Flow for Flops:**

```tcl
read_verilog dff_asyncres.v
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
synth -top dff_asyncres
write_verilog dff_asyncres_syn.v
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-44-10" src="https://github.com/user-attachments/assets/5400cef4-c182-475f-9c31-74fb33fd0aa3" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-44-04" src="https://github.com/user-attachments/assets/e8c95460-9420-431f-aa97-e0b1a36b6683" />


* Non-blocking assignments (`<=`) are **synthesis-friendly**
* Single clock domain per always block avoids **race conditions**

<br>

### Other Flops:

<br>

### 1. Async Set
#### Simulation

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-31-36" src="https://github.com/user-attachments/assets/724aa368-9814-4195-b6c7-c282e9555a26" />

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-32-47" src="https://github.com/user-attachments/assets/6222f028-8425-4002-820b-610ec69a3c42" />

<br>

#### Synthesis

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-46-51" src="https://github.com/user-attachments/assets/7fd6d7fa-fa84-43af-bef0-7aefcab96caf" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-46-58" src="https://github.com/user-attachments/assets/beca812f-3e51-4779-936c-01b583ba8c98" />

<br>

### 2. Sync reset

#### Simulation
<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-35-34" src="https://github.com/user-attachments/assets/4cc44c40-1b70-4224-94d5-1f4d3b894b0e" />

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-36-03" src="https://github.com/user-attachments/assets/6a09b251-eda0-4fe2-ba66-82e127c22f8f" />


<br>

#### Synthesis
<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-49-17" src="https://github.com/user-attachments/assets/c5075f41-d621-49fa-864b-4d4d99bcd414" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 15-49-05" src="https://github.com/user-attachments/assets/76d86017-a7a1-4ddf-b0ef-8f6afa206586" />


<br>


### 5️⃣ Interesting Optimizations – Part 1 🔧

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 16-45-28" src="https://github.com/user-attachments/assets/916a14e9-38dd-4564-ac12-04608e9c5d5d" />

<br>

**Example 1 – Multiply by 2**

```verilog
module mul2(input [2:0] a, output [3:0] y);
    assign y = a * 2;
endmodule
```
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 17-08-39" src="https://github.com/user-attachments/assets/fd4d6a7d-1418-4169-8020-7829cecfebbb" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 17-15-24" src="https://github.com/user-attachments/assets/9c007047-00c2-45f8-8e25-11fa4f1be9e9" />

<br>

* Synthesis recognizes this as **left shift** → **no multiplier hardware needed**

**Example 2 – Multiply by 9**

```verilog
module mult8(input [2:0] a, output [5:0] y);
    assign y = a * 9;
endmodule
```
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 17-18-23" src="https://github.com/user-attachments/assets/44307340-5f9e-4fd9-8992-c66e59e73940" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 17-23-01" src="https://github.com/user-attachments/assets/3cb711df-4bf5-4d8a-8e84-8066d113e1b2" />


<br>

* Output is **concatenation trick (`aa`)** → efficient hardware
* Synthesized netlist uses **shift + add optimization**

<br>

### ✅ Key Takeaways – Day 2

* `.lib` files provide **timing, power, and area data** for synthesis
* PVT corners ensure **robust designs** under real-world conditions
* Hierarchical vs flat synthesis affects **optimization vs debug tradeoffs**
* Proper **flop coding** prevents glitches and supports timing closure
* RTL optimizations (multiply by constant powers) reduce **hardware usage**


<br>

<h3 id="day3">📌 Day 3 – Combinational and Sequential Optimizations</h3>

## 📖 Theory

Digital design isn’t just about correctness — it’s also about **efficiency**. Optimisation in synthesis ensures the hardware meets **area, power, and timing goals** while keeping functionality intact.

### 🔹 Combinational Logic Optimisation

These are techniques applied to purely combinational circuits (no memory elements).

* **Objective:** Reduce the number of gates, simplify logic, save area & power.
* **Key Methods:**

  * **Constant Propagation:** Replace logic expressions with constants when inputs are fixed.
  * **Direct Optimisation:** Simplify obvious redundancies.
  * **Boolean Logic Optimisation:** Reduce Boolean equations using algebra, **K-Maps**, or **Quine–McCluskey Algorithm**.
  * **Gate-Level Minimisation:** Replace multi-level logic with equivalent simpler forms.

👉 **Example: Constant Propagation**


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-23 17-28-32" src="https://github.com/user-attachments/assets/d74e9f50-612b-4964-b459-ad49d5bfae7c" />

<br>

```
Y = ((AB) + C)'
If A = 0 ⇒ Y = (0 + C)' = C'
```

So instead of 3 gates (AND, OR, NOT), we just need a **NOT gate**!

<br>

### 🔹 Sequential Logic Optimisation

These apply to circuits with memory (flip-flops, registers). Optimisations not only save area but also improve timing closure.

* **Basic:**

  * **Sequential Constant Propagation** – remove flip-flops that always hold constant values.
* **Advanced:**

  * **State Optimisation** – remove unused FSM states or merge equivalent states.
  * **Retiming** – move registers across logic cones to balance delay, enabling higher clock frequency.
  * **Sequential Logic Cloning (Floorplan Aware)** – duplicate registers/logic depending on physical placement to reduce long interconnect delays.

👉 **Retiming Example**

* At **200 MHz (5 ns period)** → logic delay may be too large.
* By moving registers inside the logic cone, delay can be split → achieving **500 MHz (2 ns period)**.

<br>

### 📊 Quick Comparison

| Optimisation Type | Focus                    | Example                                    |
| ----------------- | ------------------------ | ------------------------------------------ |
| **Combinational** | Gates, logic expressions | Constant Propagation, Boolean minimisation |
| **Sequential**    | Flip-flops, FSM, timing  | State optimisation, Retiming, Cloning      |

<br>

## 🧪 Labs

### 🧩 Lab 6 – Combinational Logic Optimisations
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-14-23" src="https://github.com/user-attachments/assets/c21ceadf-9664-4b27-86c0-68f4e5d36c04" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-04-24" src="https://github.com/user-attachments/assets/79bdbb29-50b5-4662-a7d4-f1718fc25828" />


<br>

#### 🔸 Example 1 – Simple MUX → AND Gate

```verilog
module opt_check (input a , input b , output y);
  assign y = a ? b : 0;
endmodule
```

* Functionally `y = a & b`.
* Synthesis removes unused paths and directly maps to an **AND gate**.
* Command used:

  ```
  opt_clean -purge
  ```
### Synthesis 
<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-14-41" src="https://github.com/user-attachments/assets/e4888b8a-ae91-4b3f-b06e-e4e5866c8565" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-14-49" src="https://github.com/user-attachments/assets/7ccabcda-a09e-4cb3-b65f-e9ecae88eb52" />

<br>

#### 🔸 Example 2 – MUX → OR Gate

```verilog
module opt_check2 (input a , input b , output y);
  assign y = a ? 1 : b;
endmodule
```

* Equivalent to `y = a | b`.
* Synthesis recognises constant `1` and optimises into **OR gate**.

### Synthesis 

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-20-54" src="https://github.com/user-attachments/assets/ff0c7bdf-c004-4419-9508-94bcafdabb12" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-20-45" src="https://github.com/user-attachments/assets/72e1d311-0182-48bb-8a2d-337a0927ce74" />

<br>

#### 🔸 Example 3 – Nested MUX → AND Chain

```verilog
module opt_check3 (input a , input b, input c , output y);
  assign y = a ? (c ? b : 0) : 0;
endmodule
```

* Simplifies to: `y = a & b & c`.
* Multiple conditions collapse into one **AND chain**.

### Synthesis 
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-29-50" src="https://github.com/user-attachments/assets/7696d002-5461-4f21-aafd-f0f72a115104" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-29-23" src="https://github.com/user-attachments/assets/71a5a21f-80cf-4300-9677-a0369a83aba2" />

<br>

#### 🔸 Example 4 – Complex Conditional Expression

```verilog
module opt_check4 (input a , input b , input c , output y);
  assign y = a ? (b ? (a & c) : c) : (!c);
endmodule
```

* Three branches considered:

  * If `a=1, b=1 → y = a & c`
  * If `a=1, b=0 → y = c`
  * If `a=0 → y = !c`
* Synthesis reduces redundant paths. This is an example of **multi-level Boolean optimisation**.

### Synthesis 
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-34-27" src="https://github.com/user-attachments/assets/1036620f-4f0e-471e-99dc-8a9038598348" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-34-32" src="https://github.com/user-attachments/assets/2c6ae6b9-d7b3-4417-b235-5d2e420a7fef" />

<br>

#### 🔸 Example 5 – Multiple Modules with Constants

```verilog
module sub_module1(input a , input b , output y);
  assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
  assign y = a ^ b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
  wire n1,n2,n3;

  sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
  sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
  sub_module2 U3 (.a(b), .b(d) , .y(n3));

  assign y = c | (b & n1); 
endmodule
```

* `n1 = a & 1 = a`
* `n2 = a ^ 0 = a` (but unused → eliminated)
* `n3 = b ^ d` (unused → eliminated)
* Final `y = c | (b & a)`

### Synthesis Hierarchial
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-39-54" src="https://github.com/user-attachments/assets/55466e46-2cb6-4897-9901-c49b19ce649e" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-39-43" src="https://github.com/user-attachments/assets/d97ba9ff-b9cb-443a-a022-bbba00b59ddf" />


<br>

### Synthesis Flatten

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-40-56" src="https://github.com/user-attachments/assets/463a4700-119a-4417-b292-493e365aec52" />

<br>

👉 Demonstrates **module-level constant propagation** and **removal of unused nets**.

✅ **Lab 6:** Combinational logic collapses aggressively → constants simplify paths, unused modules vanish, redundant nets are removed.

<br>

### 🧩 Lab 7 – Sequential Logic Optimisations

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-54-10" src="https://github.com/user-attachments/assets/1169593c-f1b3-43e1-84d4-df85eaa143ee" />

#### 🔸 Example 1 – DFF with constant `1` input

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

* Behaviour: After reset → output always `1`.
* Synthesis replaces flip-flop with a **constant wire = 1**.

### Simulation

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-00-05" src="https://github.com/user-attachments/assets/8f6cee8b-f9e9-4b91-8f82-7170212b7c47" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 20-59-41" src="https://github.com/user-attachments/assets/e228410c-1921-4fea-9f6e-958a23af3399" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-09-55" src="https://github.com/user-attachments/assets/8705f07e-851c-4346-97b9-5fab37ec75b6" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-09-25" src="https://github.com/user-attachments/assets/79466efc-6c58-4729-b439-c890f6466df2" />

<br>

#### 🔸 Example 2 – DFF always `1`

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

* Behaviour: Output always `1` (independent of reset).
* Synthesis removes FF → direct `assign q = 1'b1`.

### Simulation

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-02-59" src="https://github.com/user-attachments/assets/eb0e55ca-3be9-4df2-911a-64f775b0533b" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-02-46" src="https://github.com/user-attachments/assets/b05f97fe-6ce2-4e97-9948-1574d9994e44" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-12-50" src="https://github.com/user-attachments/assets/3bff97a5-79b2-4dfe-8658-9a948fcfb2d3" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-12-42" src="https://github.com/user-attachments/assets/38e0259b-a26f-471a-86b3-3d7663e3c286" />

<br>

### Waveform Comparision of dff_const1.v and dff_const2.v
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-04-11" src="https://github.com/user-attachments/assets/92238c5f-cfee-4999-b462-2a0b0daab1ae" />

<br>

#### 🔸 Example 3 – Two FFs with constants

```verilog
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset) begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

* On reset: `q=1`, `q1=0`.
* After first clock: `q1=1`, then `q` follows `q1`.
* Eventually, `q` stabilises at **1 forever** → optimiser removes FFs.

### Simulation

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-48-29" src="https://github.com/user-attachments/assets/8d7881b8-2395-4dee-ba82-bb2c3cf19362" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-48-18" src="https://github.com/user-attachments/assets/09f8db23-ccd8-4517-8a03-fbb2d15b6d08" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0007fa9-716a-4cf2-8012-4b7d57c525fb" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-52-49" src="https://github.com/user-attachments/assets/d931d2d2-543e-497d-85b3-e4f1750eb06b" />

<br>

#### 🔸 Example 4 – Two FFs with constant `1`

```verilog
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

* On reset: `q=1`, `q1=1`.
* After first clock: `q1` stays `1`, `q` follows `q1 = 1`.
* Both registers always output **1** → synthesis replaces FFs with a constant `1'b1`.

### Simulation

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-56-15" src="https://github.com/user-attachments/assets/d42383b3-626e-4663-8d9f-933c39603ecc" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-56-23" src="https://github.com/user-attachments/assets/ff978e07-079e-4325-8258-e15a5ef61512" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-57-09" src="https://github.com/user-attachments/assets/34f74037-c12e-4821-ad41-11a8dcc8bf4c" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-57-01" src="https://github.com/user-attachments/assets/6f860495-cc8f-4ab8-ad13-39efd5d5eb0e" />


<br>

#### 🔸 Example 5 – Two FFs with reset to `0`

```verilog
module dff_const5(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b0;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

* On reset: `q=0`, `q1=0`.
* After first clock: `q1` becomes `1`, `q` follows `q1` (so `q=1` from 2nd cycle onwards).
* Eventually, both outputs stabilise at **1 forever** → registers collapse and synthesis replaces them with constant `1'b1`.

### Simulation

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-58-07" src="https://github.com/user-attachments/assets/b0a461f9-a5c0-464f-aa32-392369d64dcf" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-57-53" src="https://github.com/user-attachments/assets/c0b45d10-5937-40e2-aaae-27fe857d6a4b" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-59-12" src="https://github.com/user-attachments/assets/20a69c9f-1d02-4b3b-9019-c921d2b970db" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 21-59-06" src="https://github.com/user-attachments/assets/ff365106-39e2-408a-abd7-6c977fa567af" />


<br>


✅ **Lab 7:** Sequential constant propagation is powerful → registers collapse when behaviour reduces to constants.

<br>

### 🧩 Sequential Optimisation – Unused Outputs
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 22-07-27" src="https://github.com/user-attachments/assets/7de2f40c-a9ec-46b6-b724-d2647859dd1c" />

<br>

#### 🔸 Counter Example 1 – Only LSB Used

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end
endmodule
```

* `count` is 3 bits but only `count[0]` drives output.
* Synthesis removes upper bits → only **1 FF inferred**.

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 22-04-27" src="https://github.com/user-attachments/assets/62f7c8ed-a276-4dce-8832-e6fac13375da" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 22-03-20" src="https://github.com/user-attachments/assets/df82959d-8859-404c-be0c-97339af6732b" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 22-03-25" src="https://github.com/user-attachments/assets/6af525eb-441e-4a7d-b049-c3f1e332063b" />

<br>

#### 🔸 Counter Example 2 – Using Comparison

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end
endmodule
```

* Since output depends on all bits, optimiser **retains full 3-bit counter**.

✅ **Takeaway:** **Unused outputs** → optimised away. But if full vector is needed for logic, synthesis preserves all bits.

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 22-10-26" src="https://github.com/user-attachments/assets/c2d22f74-da3b-4533-bf49-62bba11b2d28" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 22-10-00" src="https://github.com/user-attachments/assets/49263035-2b6e-48a4-b359-cb20fa48b37b" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-24 22-09-51" src="https://github.com/user-attachments/assets/2db45c5a-86e2-435a-ac05-2fbe11dcb5b8" />


<br>

### ✅ Key Takeaways – Day 3

* **Combinational Optimisation:** Constant propagation, Boolean minimisation, redundant gate removal.
* **Sequential Optimisation:** Sequential constants, unused state/output removal, retiming, cloning.
* **Lab Exercises:** Show how synthesis tools aggressively simplify logic → reducing FFs, gates, and power usage.
* Use `opt_clean -purge` in Yosys to remove redundant logic.
* Constant propagation and resource sharing to reduce area and improve speed.

<br>

<h3 id="day4">📌 Day 4 – Gate-Level Simulation (GLS), Blocking vs Non-blocking, Synthesis-Simulation Mismatch</h3>


## 🔹 Introduction to GLS

**Gate Level Simulation (GLS)** is the process of running your **testbench** on the **post-synthesis netlist** instead of RTL.

👉 Why GLS?

* Verifies **logical correctness** of the synthesized design.
* Ensures RTL and netlist behave identically.
* If **delay annotations (SDF)** are included, GLS also checks **timing correctness**.

📌 **GLS Flow (with Icarus Verilog):**

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 22-50-27" src="https://github.com/user-attachments/assets/c463359c-23a6-4c72-bf97-79d6ebec54bc" />


<br>

```
Design + Gate-Level Verilog Models + Testbench
                   │
               iverilog
                   │
          VCD file generated
                   │
              GTKWave viewer
```

> 🔎 Note: Same testbench can be reused for GLS since inputs/outputs remain consistent.

<br>

## 🔹 Synthesis–Simulation Mismatches

These are differences between RTL simulation results and synthesized hardware behavior.
**Causes:**

1. **Missing Sensitivity List** – always block not triggered when input changes.
2. **Blocking (=) vs Non-Blocking (<=)** – wrong assignment style in sequential logic.
3. **Non-standard Verilog Coding** – ambiguous or tool-dependent constructs.

<br>

## 🔹 Blocking vs Non-Blocking Recap

* **Blocking (`=`)** → executes in sequence (one after the other). Best for **combinational logic**.
* **Non-blocking (`<=`)** → all RHS evaluated first, then LHS updated in parallel. Best for **sequential logic**.

⚠️ Using blocking in sequential always blocks can lead to mismatches between simulation and actual hardware.

<br>

## 🔹 Missing Sensitivity List

Bad example 👇

```verilog
module mux (input i0, input i1, input sel, output reg y);
always @ (sel) begin
	if(sel)
		y = i1;
	else
		y = i0;
end
endmodule
```

* **Issue**: This block only triggers when `sel` changes.
* If `i0` or `i1` changes, **RTL sim won’t update**, but synthesis tool infers correct combinational mux.
* → **Mismatch!**

✅ Correct version:

```verilog
always @(*) begin
	if(sel)
		y = i1;
	else
		y = i0;
end
```

## Examples used in the Lab Sessions
<br>

## 🔹 Example 1 – Ternary Operator MUX

```verilog
module ternary_operator_mux (
    input i0, input i1, input sel,
    output y
);
    assign y = sel ? i1 : i0;
endmodule
```

**Explanation:**

* Uses **ternary operator (`? :`)** for 2:1 mux.
* Behavior:

  * If `sel = 0`, `y = i0`.
  * If `sel = 1`, `y = i1`.
* **Simulation:** RTL sim gives correct output for all input combinations.
* **Synthesis:** Tool infers **2:1 mux**, no extra logic added.
* **GLS:** Post-synthesis simulation matches RTL simulation → **no mismatch**.

✅ **Takeaway:** Simple RTL mux coding avoids mismatch; ternary operator is safe.

---

## 🔹 Example 2 – Good MUX (`always @(*)`)

```verilog
module good_mux (
    input i0, input i1, input sel,
    output reg y
);
always @(*) begin
    if(sel)
        y = i1;
    else
        y = i0;
end
endmodule
```

**Explanation:**

* Uses **combinational `always @(*)` block**.
* Behavior: same as ternary mux.
* **Simulation:** All input changes immediately propagate to output.
* **Synthesis:** Correct 2:1 mux inferred.
* **GLS:** Post-synthesis simulation matches RTL sim.

✅ **Best practice:** Always use `@(*)` for combinational blocks to avoid missing sensitivity list errors.

<br>

## 🔹 Example 3 – Bad MUX (`always @(sel)`)

```verilog
module bad_mux (
    input i0, input i1, input sel,
    output reg y
);
always @(sel) begin
    if(sel)
        y = i1;
    else
        y = i0;
end
endmodule
```

**Explanation:**

* Sensitive only to `sel`.
* **Simulation behavior:**

  * Changes in `i0` or `i1` **do not update `y`** unless `sel` toggles.
* **Synthesis:** Tool still infers correct mux logic.
* **GLS:** Post-synthesis netlist behaves correctly (matches synthesized logic), but **RTL simulation differs** → mismatch.

**Reason for mismatch:** Missing sensitivity list in RTL → simulator doesn’t notice all input changes.

✅ **Fix:** Use `always @(*)` or SystemVerilog `always_comb`.


<br>

## 🔹 Example 4 – Blocking Statement Caveat

```verilog
module blocking_caveat (
    input a, input b, input c,
    output reg d
);
reg x;
always @(*) begin
    d = x & c;   // uses old x
    x = a | b;   // updates x
end
endmodule
```

**Step-by-step explanation:**

* **Blocking assignments (`=`)** execute in sequence.
* Simulation:

  * `d` uses the **previous value** of `x` (not yet updated).
* Synthesis:

  * Tools may optimize/parallelize assignments → interprets as `d = (a | b) & c`.
* **GLS observation:** Post-synthesis output **differs** from RTL simulation.

**Reason for mismatch:** Blocking statements in sequential-like logic cause order-of-execution differences.

✅ **Fix:**

* Use **non-blocking (`<=`)** if sequential behavior intended.
* Or reorder statements properly for combinational logic.


<br>

## 🔹 Example 5 – GLS Flow Command (Icarus Verilog)

**GLS simulation command:**

```bash
iverilog ../my_lib/verilog_model/primitives.v \
         ../my_lib/verilog_model/sky130_fd_sc_hd.v \
         ternary_operator_mux_net.v \
         tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

**Explanation:**

* Compiles **primitives**, **standard cell models**, **synthesized netlist**, and **testbench**.
* Generates **VCD file** → visualized in GTKWave.
* Compare GLS results with RTL simulation to detect mismatches.


<br>

## 🔹 Lab 1 – MUX Implementations

We compare **ternary operator**, **good mux**, and **bad mux**.
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-02-57" src="https://github.com/user-attachments/assets/27694482-7e8a-4f54-9a0b-0dbe1c21019c" />

<br>

### 🟠 Simulation: Ternary Operator MUX

```verilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel ? i1 : i0;
endmodule
```

* Simple, clean style.
* Simulator and synthesis tool both infer a **2:1 mux** correctly.

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-09-20" src="https://github.com/user-attachments/assets/fde2ee7e-7a18-47da-9d93-936da1a2aec5" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-09-35" src="https://github.com/user-attachments/assets/4d32d30e-813f-44c6-8ac5-04a0b9395cfe" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-10-55" src="https://github.com/user-attachments/assets/0ced3985-9a7f-4caa-9875-99cc9d4729f1" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-11-05" src="https://github.com/user-attachments/assets/83ff68c2-adff-4f85-bcd9-b713eee46ed7" />

<br>

### Netlist

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-13-12" src="https://github.com/user-attachments/assets/17515c97-2fc0-45bd-977b-71bd9a1ff113" />


<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-13-21" src="https://github.com/user-attachments/assets/e9daac54-0961-47f6-80af-184bb739c34b" />

<br>

### Gate Level Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-18-57" src="https://github.com/user-attachments/assets/fc9ab306-4f8b-4880-9ba6-1f27766ac503" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-19-08" src="https://github.com/user-attachments/assets/212f1a69-154a-47a3-8890-9ee93f503f07" />

<br>

### 🟠 Simulation: Good MUX (with `always @(*)`)

```verilog
module good_mux (input i0 , input i1 , input sel , output reg y);
always @(*) begin
	if(sel)
		y = i1;
	else 
		y = i0;
end
endmodule
```

* Uses `@(*)` → sensitive to all inputs (`i0, i1, sel`).
* Both simulation and synthesis infer correct mux. ✅

<br>

### 🟠 Simulation: Bad MUX (with `@(sel)`)

```verilog
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @(sel) begin
	if(sel)
		y = i1;
	else 
		y = i0;
end
endmodule
```

* Sensitive only to `sel`.
* If `i0`/`i1` change without `sel`, RTL sim shows wrong results.
* But synthesis tool **still infers proper mux**.
* → **Mismatch between RTL sim vs GLS.**

<br>

### 🟠 GLS Run for Ternary and Bad MUX

```bash
iverilog ../my_lib/verilog_model/primitives.v \
         ../my_lib/verilog_model/sky130_fd_sc_hd.v \
         ternary_operator_mux_net.v \
         tb_ternary_operator_mux.v
```

* GLS for **ternary_mux** matches RTL.
* GLS for **bad_mux** differs from its RTL sim → highlights **missing sensitivity list problem**.

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-21-36" src="https://github.com/user-attachments/assets/f69ef4a7-bbdc-4021-a611-0b594ac28e13" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-21-25" src="https://github.com/user-attachments/assets/2e970ad5-f4ac-4346-bfc5-fe42478e86df" />


<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-24-00" src="https://github.com/user-attachments/assets/b2f0be4d-4ff5-47af-9915-f6df2a19715b" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-23-53" src="https://github.com/user-attachments/assets/81a28528-cee4-42c2-aa65-b79e768e05bf" />


<br>

### Netlist

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-24-28" src="https://github.com/user-attachments/assets/f263d867-fb0c-4682-8204-5c9c08cfd8c3" />


<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-24-42" src="https://github.com/user-attachments/assets/b074ea02-7401-4151-be23-8506696fde9d" />


<br>

### Gate Level Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-26-35" src="https://github.com/user-attachments/assets/2ef5e9c0-5110-4680-9900-5640dd52821e" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-26-24" src="https://github.com/user-attachments/assets/1921c89a-8df8-4b96-9a20-7bf221152407" />


<br>

### Comparision-ternary_operator_mux.v and bad_mux.v
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-28-41" src="https://github.com/user-attachments/assets/f20713ca-bbf5-4eef-93c1-4995f9ad0d98" />


<br>

## 🔹 Lab 2 – Blocking Statement Caveat
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-30-31" src="https://github.com/user-attachments/assets/59a65b46-6fe7-45c9-a2d8-828be4f74e44" />

<br>

Code:

```verilog
module blocking_caveat (input a, input b, input c, output reg d);
reg x;
always @(*) begin
	d = x & c;   // uses old x
	x = a | b;   // updates x
end
endmodule
```

🔍 **What happens?**

* In RTL simulation:

  * `d` is computed using the **previous value of `x`** (since blocking executes in order).
* In synthesis:

  * Tools reorder logic and treat this as if `d = (a | b) & c`.
* → **Mismatch between RTL sim and netlist (GLS)**.

✅ Fix: Swap order or use non-blocking assignment.

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-31-41" src="https://github.com/user-attachments/assets/e52acaf6-2282-4773-8b74-54096df52e7a" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-31-28" src="https://github.com/user-attachments/assets/ab81a3b6-ba29-4b3d-841f-d39b1ce13afb" />



<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-33-51" src="https://github.com/user-attachments/assets/640dfc21-ca3a-4391-86ce-57a9c4255dc6" />



<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-33-45" src="https://github.com/user-attachments/assets/797408c9-ae69-43d2-9d06-c543eee6d0e5" />



<br>

### Netlist

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-34-32" src="https://github.com/user-attachments/assets/f403528b-872b-4969-bc02-2922d4766d68" />


<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-34-20" src="https://github.com/user-attachments/assets/655fb98f-1df7-494e-b360-3d5da1fc48c4" />



<br>

### Gate Level Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-35-39" src="https://github.com/user-attachments/assets/2155d8b6-02e4-47c5-8061-65a93534cd01" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-35-31" src="https://github.com/user-attachments/assets/c1940254-2ce1-49fb-93bf-7850561287b5" />


<br>

### Comparision-Blocking Statement Caveat
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-25 23-36-09" src="https://github.com/user-attachments/assets/0dc23776-21b5-4c26-905f-90d618e916c4" />

<br>



### ✅ Key Takeaways – Day 4

### Common GLS Issues & Verilog Coding Best Practices

#### 1️⃣ Common Issues in RTL Code

1. **Missing sensitivity list**

   * Example: `always @(sel)` vs `always @(*)`
   * Can cause simulation and synthesis mismatch.
2. **Wrong assignment style**

   * Using **blocking (`=`)** in sequential logic.
3. **Odd or non-standard Verilog code**

   * Unclear coding style or reliance on order-of-assignment.

> ⚠️ Bad coding practices can break simulation even if synthesis works.


#### 2️⃣ Blocking vs Non-blocking Assignments

| Type                | Behavior                      | Usage                               |
| ------------------- | ----------------------------- | ----------------------------------- |
| Blocking (`=`)      | Sequential execution in order | Combinational logic only            |
| Non-blocking (`<=`) | Parallel RHS evaluation       | Sequential logic (e.g., flip-flops) |

> Using blocking in sequential logic can cause **simulation–synthesis mismatches** due to sequential dependencies.
> Non-blocking ensures **consistent hardware behavior**.



#### 3️⃣ Gate-Level Simulation (GLS)

* **Purpose:** Ensures RTL and post-synthesis netlist behave identically.
* **Why needed:** Catch mismatches caused by sensitivity list errors, wrong assignment styles, or other coding issues.
* **Best practices to avoid GLS mismatches:**

  1. Use `@(*)` for combinational blocks.
  2. Use blocking (`=`) **only** for combinational logic.
  3. Use non-blocking (`<=`) for sequential logic.
  4. Follow RTL best practices to prevent costly silicon bugs.

<br>

<h3 id="day5">📌 Day 5 – Optimization in Synthesis</h3>


## 🔹 1️⃣ If-Else Statements in Verilog

**Purpose:** Conditional execution inside procedural blocks (`always`, `initial`, tasks).

**Syntax:**

```verilog
if (condition) begin
    // executed if true
end else begin
    // executed if false
end
```

**Nested If-Else:**

```verilog
if (cond1)
    // code1
else if (cond2)
    // code2
else
    // default code
```

**Hardware Mapping:**

* `if-else` → **priority logic**.
* Cascaded multiplexers implement priority; higher-priority conditions override lower ones.

**Caution:** Incomplete `if` (no `else`) in **combinational logic** → **inferred latch**.

<br>

## 🔹 2️⃣ Inferred Latches

**Problem Example:**

```verilog
module ex (
    input a, b, sel,
    output reg y
);
always @(a, b, sel) begin
    if (sel == 1'b1)
        y = a; // No else → y may retain previous value
end
endmodule
```

* If `sel = 0`, `y` is **not assigned** → synthesis infers a latch.

**Fix:**

```verilog
always @(a, b, sel) begin
    case(sel)
        1'b1: y = a;
        default: y = 1'b0; // Ensure assignment for all paths
    endcase
end
```

<br>

## 🔹 3️⃣ Labs – If-Else and Case Statements


### Lab 1: Incomplete If Statement

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-42-02" src="https://github.com/user-attachments/assets/00954892-6d64-4f62-9744-da63f984aca7" />

```verilog
module incomp_if (
    input i0, i1, i2,
    output reg y
);
always @(*) begin
    if (i0)
        y <= i1;  // No else → inferred latch
end
endmodule
```

**Observation:**

* Simulation: `y` may hold previous value if `i0=0`.
* Synthesis: Latch inferred.

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-30-56" src="https://github.com/user-attachments/assets/d696c870-2489-4809-85ca-f5db62d7cd61" />

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-30-46" src="https://github.com/user-attachments/assets/66165eb0-8d59-4a72-9ca4-c3bde853b3cf" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-33-04" src="https://github.com/user-attachments/assets/c22c1455-c263-4115-a440-71179c158aee" />

<br>


<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-32-52" src="https://github.com/user-attachments/assets/aefb6a19-fc79-4a8f-87ed-9a441ca74c39" />

<br>

### Lab 2: Incomplete Nested If-Else

```verilog
module incomp_if2 (
    input i0, i1, i2, i3,
    output reg y
);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;  // No else → inferred latch
end
endmodule
```

**Observation:** Latch inferred for `y` when `i0=0` and `i2=0`.

**Fix:** Add `else y <= default_value;`

<br>

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-32-00" src="https://github.com/user-attachments/assets/f0d20397-a415-4cc9-a16e-9c836cee1c0b" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-31-53" src="https://github.com/user-attachments/assets/31665075-203c-45e9-aebd-f378c432a133" />


<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-33-50" src="https://github.com/user-attachments/assets/748f216a-b78a-4bdd-b11c-5fe9fd3c83dd" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-33-44" src="https://github.com/user-attachments/assets/a58bf9ce-a9a1-4ce1-8403-fd58b7c49179" />


<br>

### Lab 3: Complete Case Statement

```verilog
module comp_case (
    input i0, i1, i2,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        default: y = i2;  // Covers all unassigned cases
    endcase
end
endmodule
```

✅ **No latches inferred**, purely combinational.

<br>

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-44-03" src="https://github.com/user-attachments/assets/b8d1cdf1-084e-4b31-baab-f3000c8a6701" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-43-53" src="https://github.com/user-attachments/assets/b4d0805d-6c12-466c-9036-0177c17cd789" />


<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-40-36" src="https://github.com/user-attachments/assets/2c68c801-202c-49de-90b4-926d345d85c3" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-40-22" src="https://github.com/user-attachments/assets/5fe4200b-7770-498e-9ffb-5e16b2926665" />


<br>

### Lab 4 & 5: Incomplete Case & Partial Assignment
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-45-20" src="https://github.com/user-attachments/assets/c8325495-e0f6-4c8b-b286-41050bbe6f2a" />

<br>

```verilog
module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
	endcase
end
endmodule
```
<br>

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-46-25" src="https://github.com/user-attachments/assets/5cc9eb21-5b37-48e4-bdee-c2d31416efac" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-46-11" src="https://github.com/user-attachments/assets/47e15e10-6de8-49f2-b141-62b902bdaaa0" />

<br>

### Synthesis

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-48-53" src="https://github.com/user-attachments/assets/6af7e0cf-be5d-43ea-ba0c-abd5596374a9" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-48-43" src="https://github.com/user-attachments/assets/250e6a67-4ded-453f-950f-656321582e1a" />

<br>

```verilog
module partial_case_assign (
    input i0,i1,i2,
    input [1:0] sel,
    output reg y, x
);
always @(*) begin
    case(sel)
        2'b00: begin y=i0; x=i2; end
        2'b01: y=i1;  // x not assigned → inferred latch
        default: begin x=i1; y=i2; end
    endcase
end
endmodule
```
<br>

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-59-45" src="https://github.com/user-attachments/assets/ca0ee736-bfaa-4359-8be1-0a82e068e7c4" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-59-34" src="https://github.com/user-attachments/assets/63cb5dab-dd4a-4e0f-89d5-6e0c0ffbf8c9" />


<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-51-43" src="https://github.com/user-attachments/assets/9837afb0-f094-4f70-8d69-f1ff312d4d36" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-26 00-51-36" src="https://github.com/user-attachments/assets/93d909b1-0814-48c4-bae0-b8252062c1f5" />


<br>

**Lesson:** Always assign **all outputs in all case branches**; use `default` to avoid latches.

<br>

## Lab 6: Bad Case Handling
<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-13-50" src="https://github.com/user-attachments/assets/3c8e1187-0586-4cf0-b0fb-d648cda47b2f" />

<br>

**Purpose:** Show how Bad cases can lead to unpredictable outputs.

```verilog
module bad_case (
    input i0, i1, i2, i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // Wildcard '?' is risky; incomplete cases may infer latch
    endcase
end
endmodule
```

**Key Takeaway:** Avoid wildcards unless fully understood; always assign all outputs.

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-11-03" src="https://github.com/user-attachments/assets/cf77b208-50d7-4478-8ed5-0e3af760ae6b" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-10-57" src="https://github.com/user-attachments/assets/b6be831d-dbaf-49b8-8b73-1575f7cbc9ea" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-13-03" src="https://github.com/user-attachments/assets/be7962c4-0de9-421b-99b5-0a059733a074" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-12-57" src="https://github.com/user-attachments/assets/c1d1d811-2e25-4cc3-988f-892b1515378d" />



<br>

## For Loops in Verilog
<br>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/af328fbe-8f07-4d0a-9961-6b9574afe3e1" />

<br>

**Purpose:** Repeat statements; synthesizable if iteration count is fixed.

**Lab 7: 4-to-1 MUX using loop**

```verilog
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;
always @ (*)
begin
for(k = 0; k < 4; k=k+1) begin
	if(k == sel)
		y = i_int[k];
end
end
endmodule

```

**Observation:**

* Loop unrolled during synthesis → efficient MUX logic.

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 17-55-44" src="https://github.com/user-attachments/assets/6a27f1b4-6e95-4d52-97a0-e5cd5642c97a" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 17-53-54" src="https://github.com/user-attachments/assets/99a4b2fb-e33c-4fe5-80ec-3eb352fea660" />



<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 17-56-38" src="https://github.com/user-attachments/assets/648137ea-2b8c-40d6-908b-14590336723c" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 17-56-30" src="https://github.com/user-attachments/assets/1ef57519-d694-4d5c-9e87-5652236b5614" />

<br>

## Generate Blocks in Verilog

**Purpose:** Instantiate multiple hardware modules or logic structures at compile time.

**Example: Generate AND gates**

```verilog
genvar i;
generate
    for (i = 0; i < 4; i=i+1) begin : gen_loop
        and_gate u_and (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```

**Use Case:** Scalable and synthesizable repetitive structures.


<br>

## Lab 8: 8-to-1 Demux Using Case
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-20-46" src="https://github.com/user-attachments/assets/4fd0a2f3-a75d-4aab-9e27-60068adc3026" />

<br>

**Purpose:** Show demux implementation with case statement.

```verilog
module demux_case (
    output o0,o1,o2,o3,o4,o5,o6,o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;

always @(*) begin
    y_int = 8'b0; // Default output
    case(sel)
        3'b000: y_int[0] = i;
        3'b001: y_int[1] = i;
        3'b010: y_int[2] = i;
        3'b011: y_int[3] = i;
        3'b100: y_int[4] = i;
        3'b101: y_int[5] = i;
        3'b110: y_int[6] = i;
        3'b111: y_int[7] = i;
    endcase
end
endmodule
```

**Key Takeaway:** Case-based demux is clear and easy to synthesize.

### Simulation
<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-19-08" src="https://github.com/user-attachments/assets/b01d0eeb-ffd3-4440-97e4-60550ed5a14b" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-18-57" src="https://github.com/user-attachments/assets/2aaebd8d-a3ee-4eef-b2f8-d8a353230f38" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-20-14" src="https://github.com/user-attachments/assets/3514bbcf-9b50-48c8-99ca-f99bb2cdfa83" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-20-07" src="https://github.com/user-attachments/assets/66641044-4ffc-4619-bdbb-d71fcd1e6121" />

<br>

## Lab 9: 8-to-1 Demux Using For Loop

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-25-51" src="https://github.com/user-attachments/assets/bed55975-4cc6-4149-a8fe-cc70c530e0ab" />
<br>

**Purpose:** Scalable demux using iterative approach.

```verilog
module demux_generate (
    output o0,o1,o2,o3,o4,o5,o6,o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;

integer k;
always @(*) begin
    y_int = 8'b0; // Default output
    for (k = 0; k < 8; k=k+1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```

**Key Takeaway:** For loops + default assignments → safe, scalable logic.

### Simulation
<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-23-45" src="https://github.com/user-attachments/assets/69aee4d3-741a-4cf4-adb8-c3837d01528e" />


<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-23-36" src="https://github.com/user-attachments/assets/ad32740b-0db7-4177-ba5b-dd7064ea20fb" />

<br>

### Synthesis
<br>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8c3c46a7-07c4-449f-88cf-3f1149a81ad4" />


<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-25-06" src="https://github.com/user-attachments/assets/b74f1be0-bf84-4dd9-bf55-0cf46ad0de3d" />

<br>



## Lab 10: Ripple Carry Adder (RCA) Using Generate Block
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-28-26" src="https://github.com/user-attachments/assets/ea1301e8-846e-41d9-a19e-08a9a88005b0" />

<br>

```verilog
module fa (input a,b,c, output co,sum);
    assign {co, sum} = a + b + c;
endmodule

module rca (
    input [7:0] num1, num2,
    output [8:0] sum
);
wire [7:0] int_sum, int_co;

genvar i;
generate
    for (i=1; i<8; i=i+1) begin : fa_gen
        fa u_fa (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), 
                 .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa0 (.a(num1[0]), .b(num2[0]), .c(1'b0), 
          .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

**Observation:**

* Each full adder instantiated using `generate`.
* Carry chains properly connected → correct ripple-carry behavior.

### Simulation
<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-30-19" src="https://github.com/user-attachments/assets/9c48474e-8f6d-4b9d-907d-9c52bf53214f" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-30-04" src="https://github.com/user-attachments/assets/e1219c40-d19f-4430-9556-3e3773a5dcfe" />

<br>

### Synthesis
<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-35-38" src="https://github.com/user-attachments/assets/099f6596-1973-4dfb-aa3e-0de6e17bdff8" />

<br>

<img width="1920" height="1080" alt="Screenshot from 2025-09-27 18-35-26" src="https://github.com/user-attachments/assets/31bb3399-c996-400b-a268-a2cc533a0f5c" />

<br>


### ✅ Key Takeaways – Day 4

1. **If-Else:** Good for priority logic; always cover all conditions to avoid latches.
2. **Case Statement:** Good for MUX/decoder style selection; use `default`.
3. **Inferred Latches:** Occur if output not assigned in all paths. Avoid in combinational logic.
4. **For Loops:** Synthesizable for fixed iteration counts; useful for repeated logic like MUX or arithmetic.
5. **Generate Blocks:** Efficiently create multiple module instances or logic structures.
6. **RCA Example:** Shows practical use of loops and generate blocks for arithmetic.
7. **Best Practices:** Always assign outputs for all conditions, use loops/generate for scalable RTL, avoid accidental latches.
8.  Fine-tune library selection (fast/slow cells) for timing & area trade-offs.
9.  Use Yosys passes (`abc`, `opt`, `flatten`) for final design efficiency.


<h3 id="CheatSheet">📌 🖥️ RTL Simulation & Synthesis Cheat Sheet


| Tool / Step                         | Command / Syntax                                                                                                   | Description / Notes                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| **Icarus Verilog – Compile**        | `iverilog design.v testbench.v`                                                                                    | Compile RTL and testbench. Generates `a.out`.                                          |
| **Icarus Verilog – Run**            | `./a.out`                                                                                                          | Run simulation. Generates waveform `.vcd` file.                                        |
| **GTKWave – View**                  | `gtkwave testbench.vcd`                                                                                            | Open waveform for debugging RTL signals.                                               |
| **Icarus Verilog – With Libraries** | `iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v design_net.v tb_design.v` | Compile design with standard cell libraries for gate-level simulation.                 |
| **Yosys – Launch**                  | `yosys`                                                                                                            | Invoke Yosys interactive shell.                                                        |
| **Yosys – Read Library**            | `read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`                                                | Load standard cell library for synthesis.                                              |
| **Yosys – Read RTL**                | `read_verilog design.v`<br>`read_verilog design1.v design2.v topmodule.v`                                          | Read RTL source files.                                                                 |
| **Yosys – Synthesize**              | `synth -top topmodule`                                                                                             | Synthesize the top module.                                                             |
| **Yosys – Map Logic**               | `abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`                                                     | Map logic to library cells.                                                            |
| **Yosys – Map Flip-Flops**          | `dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`                                               | Map sequential elements (flip-flops) to standard cells.                                |
| **Yosys – Inspect Synthesis**       | `show`                                                                                                             | Visualize synthesized netlist (schematic view).                                        |
| **Yosys – Write Netlist**           | `write_verilog design_net.v`<br>`write_verilog -noattr design_net.v`                                               | Export gate-level netlist. `-noattr` removes attributes for simpler netlist.           |
| **Yosys – Flatten Hierarchy**       | `flatten`                                                                                                          | Flattens module hierarchy into a single top-level netlist.                             |
| **Yosys – Prevent Purge**           | `opt_clean -nopurge`<br>`opt_clean -purge`                                                                         | `-nopurge`: Keep unused logic; `-purge`: remove unused cells/signals to clean netlist. |


### ✅ Notes:

* Always **compile and simulate RTL first** using Icarus + GTKWave to verify behavior before synthesis.
* Keep design and testbench filenames **generic** (`design.v`, `tb_design.v`) for portability.
* Use `flatten` when hierarchical netlist is not desired.
* Use `-nopurge` if you want to retain unused logic for debugging or partial designs.
* Use `-purge` to clean up netlist and remove redundant logic before final netlist writing.

<br>

**Author & Repository**

**Author:** T Tushar Shenoy

**Repository:** Week-1-RTL-Design-Gate-Level-Synthesis-GLS

**Program:** VLSI System Design (VSD)

