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
   A **waveform visualization tool** that: * Loads the **VCD file** * Allows designers to inspect **waveforms, clock cycles, and output transitions** This flow facilitates **debugging**, **verification** and provides **insight into design operation** by visualizing simulation results. 📊

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

 <br>

**Visualization:** <img width="1913" height="1076" alt="Screenshot from 2025-09-26 16-24-32" src="https://github.com/user-attachments/assets/82a00556-b831-407f-9827-5fb89eae83e6" />

The **Verilog Design** and its **Test Bench** are compiled and run by the **Iverilog simulator**. During simulation, the tool produces a **VCD file (Value Change Dump format)**, which records **signal changes over time**. To visualize the signal transitions and verify behavior, this **VCD file** is loaded into **GTKWave**, a waveform viewer that aids **debugging** and **analysis**
<br>
**Notes:**

* Testbench itself does **not expose primary inputs/outputs**.
* Ensures design correctness under **all input conditions**.
* Same testbench can be reused for **RTL & synthesized netlist verification**.
* The design may have **one or more primary input/output signals**. <br> * The test bench itself **does not expose any primary I/Os** directly. Its sole function is to **generate, monitor, and validate** the design's responses within a controlled simulation.

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
  
$$ T_{CLK} > T_{CQ\_A} + T_{COMBI} + T_{SETUP\_B} $$

* Fast cells → reduce combinational delay
* Slow cells → prevent hold violations

**Explanation:** Circuit speed is limited by logic path delays. * Using **fast gates** → reduces delay, allows higher clock frequencies * Using **slow gates** → prevents early arrival at flip-flops (hold violations)

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

$$
T_{HOLD\_B} < T_{CQ\_A} + T_{COMBI}
$$

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

**Explanation:** Gate speed depends on **ability to drive capacitive loads**. * **Fast cells** (wide transistors) → low delay, higher area/power * **Slow cells** (narrow transistors) → save area/power but increase delay

> Fast cells trade **speed for area/power** 🔄


<br>

### 4️⃣ Labs – Day 1

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

```bash
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-19-00" src="https://github.com/user-attachments/assets/2d332943-5486-48bd-a501-75fc0f0b1b2a" />  

<br>
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-22-45" src="https://github.com/user-attachments/assets/7248fe64-e27f-4cd6-b0d0-d380089ebd05" />

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

```bash
write_verilog -noattr good_mux_netlist.v
```
<img width="1920" height="1080" alt="Screenshot from 2025-09-22 10-33-07" src="https://github.com/user-attachments/assets/cff43080-ae2c-47f4-98c1-a21e4ce0b977" />



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



## 📌 Day 2 – Timing Libraries & Efficient RTL Coding (Brief)

* **Timing Libraries**: `.lib` files provide delays, drive strengths, and functional info for gates.
* **Hierarchical vs Flat Synthesis**:

  * **Hierarchical**: Keeps module boundaries, good for reuse.
  * **Flat**: Optimizes globally but removes hierarchy.
* **Flop Coding Styles**: Proper use of blocking (`=`) vs non-blocking (`<=`) assignments.

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

