# 🚀 Week 1: RTL Design & Gate-Level Synthesis (GLS)

<div align="center">

![EDA](https://img.shields.io/badge/OpenSource-EDA-purple?style=for-the-badge\&logo=opensourceinitiative)
![RISC-V](https://img.shields.io/badge/RISC--V-OpenSource-blue?style=for-the-badge\&logo=riscv)
![VSD](https://img.shields.io/badge/VSD-TapeoutProgram-orange?style=for-the-badge\&logo=verilog)

</div>

Welcome to **Week 1 of the RTL & Synthesis Workshop**! 🛠️
This week, we dive into **Verilog RTL design**, **simulation with Icarus Verilog**, **waveform visualization with GTKWave**, and **logic synthesis with Yosys**.

The workshop is structured **day-wise** for clarity:

* **Day 1** – Introduction to Verilog RTL Design & Synthesis
* **Day 2** – Timing Libraries, Hierarchical vs Flat Synthesis, Efficient Flop Coding Styles
* **Day 3** – Combinational and Sequential Optimizations
* **Day 4** – Gate-Level Simulation (GLS), Blocking vs Non-blocking, Synthesis-Simulation Mismatch
* **Day 5** – Optimization in Synthesis

<br>

## 📌 Day 1 – Introduction to Verilog RTL Design & Synthesis

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
* * Netlist maps **gates & flip-flops** to the library cells.

 <img width="1815" height="1080" alt="image" src="https://github.com/user-attachments/assets/0705de05-04fb-41fb-8fc5-9f57748f7a24" />

<br>

### ✅ Key Takeaways – Day 1

* RTL defines **functional behavior**, testbench validates it
* **Icarus Verilog + GTKWave** → simulation & debugging workflow
* **Yosys** maps RTL → gate-level netlist with optimization
* **Faster vs slower cells** balance timing, area, and power ⚡
* Same **testbench can verify RTL & synthesized netlist**

<br>

📌 **Day 2 – Timing Libraries, Hierarchical vs Flat Synthesis, Flop Coding & RTL Optimizations**

<br>

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

**Other Flops:**

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

## 📌 Day 3 – Combinational & Sequential Optimizations (Preview)

* Use `opt_clean -purge` in Yosys to remove redundant logic.
* Constant propagation and resource sharing to reduce area and improve speed.

<br>

## 📌 Day 4 – GLS, Blocking vs Non-blocking & Simulation Mismatch (Preview)

* Gate-Level Simulation ensures RTL matches synthesized hardware.
* Blocking assignments can cause sequential dependencies → mismatch.
* Non-blocking assignments allow parallel updates → consistent hardware.

<br>

## 📌 Day 5 – Optimization in Synthesis (Preview)

* Fine-tune library selection (fast/slow cells) for timing & area trade-offs.
* Use Yosys passes (`abc`, `opt`, `flatten`) for final design efficiency.

<br>

## ✅ Summary – Day 1 Key Takeaways

* RTL design describes intended functionality in Verilog.
* Testbench applies stimulus and observes outputs for correctness.
* Icarus Verilog + GTKWave verify behavior pre-synthesis.
* Yosys maps RTL to gate-level netlist with `.lib` standard cells.
* Cell flavor choice balances speed, power, and area.

<br>

### 📝 Recommended Lab Outputs (for README)

* **GTKWave screenshot** of mux simulation.
* **Gate-level schematic screenshot** from Yosys.
* **Netlist snippet** for verification.

<br>

If you want, I can **also create the final GitHub README markdown with Day 2–5 filled in fully**, including images placeholders, tables, and lab output sections, ready to push to your repo. This will be **a complete week 1 lab guide in one README**.

Do you want me to do that?

**EDA Workshop: SkyWater 130nm RTL Design & Synthesis**

<br>

👋 **Welcome**
Week 1 marks the first hands-on lab in RTL-to-GLS synthesis. The goal is to explore **Yosys optimization**, understand **flip-flop mapping**, and verify designs using **Gate-Level Simulation (GLS)**. By the end of this week, you’ll confidently convert RTL into technology-mapped netlists and simulate them accurately.

> “RTL tells the story; GLS shows the hardware reality.”

<br>

🖥️ **Repository Setup**

Clone the workshop repository from GitHub:

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
cd sky130RTLDesignAndSynthesisWorkshop
```

<br>

## 📌 Task 1 – Yosys Optimization with `opt_clean -purge`

### 🎯 Objective

Learn how **Yosys sweeps away redundant wires, cells, and dead logic** using `opt_clean -purge`, leaving a clean and efficient netlist.

### 🛠️ Flow & Commands

| Step | Command                                                             | Purpose                                 |
| <br>- | <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>- | <br><br><br><br><br><br><br><br><br><br><br><br><br> |
| 1️⃣  | `read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | Load standard-cell library              |
| 2️⃣  | `read_verilog opt_check4.v`                                         | Load RTL design                         |
| 3️⃣  | `synth -top opt_check4`                                             | Run generic synthesis                   |
| 4️⃣  | `opt_clean -purge`                                                  | ✨ Remove unused nets and dangling cells |
| 5️⃣  | `abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`      | Map logic to technology cells           |
| 6️⃣  | `show`                                                              | Visualize netlist                       |

> Repeated the same flow for `multiple_module_opt.v`.

<br>

### 📊 Results

#### Case 1 – `opt_check4.v`

✅ Optimized netlist (`opt_check4_net.v`) is cleaner; unnecessary wires removed.

#### Case 2 – `multiple_module_opt.v`

⚡ Before optimization → extra redundant connections.
✂️ After `opt_clean -purge` → simpler, faster, and easier to read.

| Stage                      | Snapshot                  |
| <br><br><br><br><br><br><br><br>-- | <br><br><br><br><br><br><br><br>- |
| Without `opt_clean -purge` | ![Without Clean Purge](#) |
| With `opt_clean -purge`    | ![Final Netlist](#)       |

<br>

### 🧠 Key Takeaways

* 🗑️ `opt_clean -purge` = Garbage collector for netlists.
* 🚦 Removes unused nets, floating signals, and redundant cells.
* 🎯 Leads to smaller, faster, and easier-to-debug circuits.
* 🔧 Crucial for **multi-module designs** with intermediate unused wires.

> ✨ Think of it as a vacuum cleaner for your design, sweeping away the “dust” (redundant logic) so only essential circuitry remains. 🧹⚡

<br>

## 🔧 Yosys Synthesis & GLS Flow

### 📜 `Test_Synth.ys` Script Explanation

```tcl
# 1. Load the Sky130 liberty file
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 2. Load RTL design
read_verilog mux_generate.v

# 3. Generic synthesis
synth -top mux_generate

# 4. Flatten hierarchy (optional)
flatten

# 5. Map flip-flops to standard cells
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 6. Optimize redundant logic
opt_clean -purge

# 7. Technology mapping using ABC
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# 8. Clean remaining unused cells/wires
clean

# 9. Optional flatten
flatten

# 10. Write final gate-level netlist
write_verilog -noattr mux_generate_GLS.v

# 11. Generate schematic for visualization
show -format png -prefix mux_generate_show
```

Run synthesis & GLS generation:

```bash
yosys -s Test_Synth.ys
```

💡 Tip: Change the top module or input file to generate GLS for any design. Place it in the `verilog_files` folder.

<br>

## 📌 Task 2 – Constant DFF Mapping & GLS

### 🎯 Objective

Understand **constant-driven flip-flops** (`const4.v`, `const5.v`) and verify via **Icarus Verilog simulation**.

### ⚙️ Yosys Flow

| Step | Command                                                             | Purpose                  |
| <br>- | <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>- | <br><br><br><br><br><br><br><br> |
| 1️⃣  | `read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | Load library             |
| 2️⃣  | `read_verilog const4.v`                                             | Load RTL                 |
| 3️⃣  | `synth -top const4`                                                 | Run synthesis            |
| 4️⃣  | `dfflibmap -liberty ...`                                            | Map flip-flops           |
| 5️⃣  | `abc -liberty ...`                                                  | Optimize & tech-map      |
| 6️⃣  | `write_verilog const4_net.v`                                        | Save synthesized netlist |

> Repeat for `const5.v`.

### 🖥️ Icarus Verilog GLS

```bash
iverilog const4.v tb_const4.v
./a.out
```

(Similar for `const5.v`)

<br>

### 📊 Results

#### Case 1 – `const4.v`

* Both `q` and `q1` latch a constant 1.
* Yosys maps with buffer for constant propagation.
  ✅ Simulation confirms constant outputs.

#### Case 2 – `const5.v`

* `reset=1` → `q=q1=0`
* `reset=0` → `q1=1` and `q=q1`
  ✅ GLS matches expected behavior.
  ✅ Netlist shows correct DFF mapping.

<br>

### 🧠 Key Learnings

* Constant propagation works seamlessly in Yosys.
* Buffers appear when constants drive multiple outputs.
* Reset-handling ensures flip-flops behave as intended.
* GLS validates **functional correctness** (timing not included).

> ✨ Yosys smartly simplifies constant flops while preserving reset logic.

<br>

## 📌 Task 3 – MUX Using `for-generate`

### 🛠️ Flow

| Step | Command                                             | Purpose              |
| <br>- | <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br> | <br><br><br><br><br><br>-- |
| 1️⃣  | `yosys -s Test_Synth.ys`                            | Run synthesis script |
| 2️⃣  | `iverilog ... mux_generate_GLS.v tb_mux_generate.v` | Simulate GLS vs RTL  |

### ⚠️ Roadblock & Fix

* Error: Non-existent `_D_LATCH_P` during mapping.
* Fix: Replaced with SkyWater primitive `udp_dlatch$lP`.

```verilog
sky130_fd_sc_hd__udp_dlatch$lP _2_ (
    .D(_0_),
    .GATE(1'h1),
    .Q(y)
);
```

✅ Result: Simulation ran flawlessly.

### 📊 Results

* RTL & GLS waveforms match.
* `for-generate` loop scales MUX instances without repetitive manual code.

<br>

## 📌 Task 4 – DEMUX Using `generate`

### 🛠️ Flow

```bash
iverilog ... demux_generate_GLS.v tb_demux_generate.v
```

### 📊 Results

* RTL & GLS outputs align perfectly.
* `generate` enables scalable DEMUX instantiation.

<br>

## 📌 Task 5 – Ripple Carry Adder (RCA)

### 🛠️ Flow

```bash
iverilog ... rca_GLS.v tb_rca.v
```

### 📊 Results

* RTL vs GLS simulation matches → functional correctness.
* Yosys maps RCA into standard cell adders + carry chain.

<br>

## 📘 Theory Notes

| Concept                            | Explanation                                                                                 |
| <br><br><br><br><br><br><br><br><br><br><br>- | <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>- |
| **Behavioral Synthesis**           | Converts high-level RTL (`always`, `if`, `case`) into RTL netlist of muxes, registers, FSMs |
| **Timing Basics**                  | Setup/Hold time, critical path, max clock frequency                                         |
| **Liberty Files (.lib)**           | Store cell functionality, timing, power; used for mapping RTL → gates                       |
| **Hierarchical vs Flat Synthesis** | Hierarchical keeps module boundaries; flat enables global optimization                      |
| **Stacked PMOS**                   | ❌ High resistance → slower switching                                                        |
| **Flip-Flop Mapping Flow**         | `dfflibmap` maps logical FFs → real cells from .lib                                         |

<br>

## 📈 Optimization Experiments

| Example                              | Yosys Behavior                                     |
| <br><br><br><br><br><br><br><br><br><br><br><br> | <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>-- |
| Multiplier (`mul2.v`)                | Constant folding, simplification, resource sharing |
| Constant propagation (`opt_check.v`) | Removes unused logic/nets                          |

<br>

## 📝 Key GLS Lessons

| Concept            | Bad Practice      | Correct Practice                      |
| <br><br><br><br><br><br> | <br><br><br><br><br>-- | <br><br><br><br><br><br><br><br><br><br><br><br>- |
| Blocking assigns   | `q = q0; q0 = d;` | Use non-blocking: `q0 <= d; q <= q0;` |
| Incomplete case/if | Infers latches    | Always add `default`                  |
| GLS Verification   | Ignored           | Always confirm RTL = GLS              |

> ⏱️ **Rule of Thumb:**
> `=` → sequential dependency
> `<=` → parallel updates

<br>

✅ **Week 1 Takeaways**

* Learned `opt_clean -purge` = netlist cleaner.
* Understood **constant DFF mapping** and propagation.
* Practiced **Gate-Level Simulation (GLS)** for RTL ↔ netlist validation.
* Hands-on experience with **for-generate** constructs and hierarchical mapping.
* Prepared for more complex synthesis & GLS labs next week.

<br>

**Author & Repository**
**Author:** T Tushar Shenoy
**Repository:** Sky130-RTL-Design-Workshop
**Program:** VLSI System Design (VSD)

