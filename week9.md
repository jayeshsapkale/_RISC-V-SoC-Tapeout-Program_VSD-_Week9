# 🏁 VSDBabySoC — Week 9 Final Documentation  

---

# 📂 Repository Structure Used for This Final Report
This final documentation consolidates all work done across my repositories:

- **_RISC-V-SoC-Tapeout-Program_VSD-_Week1**
- **RISC-V-SoC-Tapeout-Program_VSD_Week2**
- **_RISC-V-SoC-Tapeout-Program_VSD_Week_3**
- **_RISC-V-SoC-Tapeout-Program_VSD-_Week4**
- **_RISC-V-SoC-Tapeout-Program_VSD-_Week6**
- **_RISC-V-SoC-Tapeout-Program_VSD** (base repo)

Each week’s screenshots, logs, and text were referenced directly from these repos.
✔ All work done in this program is consolidated here.
---

# 🕒 WEEK-WISE DOCUMENTATION

---

# **📘 Week 1 – RTL Design, Logic Synthesis, and Gate-Level Simulations**
**Topics completed from my Week1 repo:**
- Basics of Verilog RTL Design & Synthesis  
- Timing Libraries, Flop Styles, Blocking/Non-Blocking  
- Simulation–Synthesis Mismatch  
- Combinational & Sequential Optimization  
- GLS using Icarus Verilog  
- Synthesis using Yosys  


**Screenshots used:** 

 ### 🔧 Lab Work: Icarus + GTKWave

<img width="940" height="663" alt="image" src="https://github.com/user-attachments/assets/14674452-4e36-4c78-aeb3-12d3aeaa8ca6" />

### 🔧 Lab Work: Yosys Synthesis  

Hands-on synthesis of a Verilog design.

<img width="940" height="686" alt="image" src="https://github.com/user-attachments/assets/a81d314b-f99d-4b20-9082-b0d043777314" />


## 🔹 What `.lib` Contains
1. **Cell Definitions**
     - Pin names
     - Pin directions (input/output)
     - Logic functions

2. **Timing Information**
   - Propagation delay  
   - Setup & hold times  
   - Recovery & removal times  
   - Timing arcs (input → output)

3. **Power Information**
   - Internal switching power  
   - Leakage power  
   - Dynamic vs. static consumption

4. **PVT Corners**
   - `.lib` files are created for multiple **process-voltage-temperature (PVT)** corners:
     - **P (Process):** fast, slow, typical fabrication variations  
     - **V (Voltage):** supply variations  
     - **T (Temperature):** operating environment  
   
   what is meaning of sky130_fd_sc_hd__tt_025C_1v80.lib
- **tt** → typical process  
- **025C** → 25 °C  
- **1v80** → 1.8 V supply
  
```
gvim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/229af8ac-36d0-4eb0-bece-118336af5c5f" />

## 🔹 Why `.lib` is Important
- Used in **logic synthesis** to map RTL → standard cells.  
- Ensures **timing closure** by accounting for delays at different corners.  
- Essential for **EDA tools** like Yosys, OpenSTA, Synopsys DC, Innovus, etc. 
---

# **📗 Week 2 – BabySoC Fundamentals & Functional Modelling**


**Outputs:**  
- Explain the theory behind SoC design
- Demonstrate BabySoC functional modelling with simulation waveforms
  
### Key Concepts

1. **What is a System-on-Chip (SoC)?**  
   - A SoC integrates **CPU, memory, peripherals, and interconnects** on a single chip.  
   - Enables compact, energy-efficient, and high-performance embedded systems.  

2. **Components of a Typical SoC**  
   - **CPU/Core**: Executes instructions (here, RVMYTH RISC-V core).  
   - **Memory**: Stores instructions and data.  
   - **Peripherals**: I/O modules like DAC, timers, UART, etc.  
   - **Interconnect/Bus**: Transfers data between components.  

3. **Why BabySoC?**  
   - Simplified SoC model for learning purposes.  
   - Focuses on key components without overwhelming complexity.  
   - Enables hands-on understanding of **functional modelling, RTL simulation, and dataflow**.  

4. **Role of Functional Modelling**  
   - Functional modelling is done **before RTL synthesis and physical design**.  
   - Helps verify **module interconnections, clocking, reset behavior, and dataflow**.  
   - Reduces design errors and makes debugging easier before gate-level simulation.

### Waveform Viewing
```bash
gtkwave pre_synth_sim.vcd
```
waveform :
<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/e311d301-609a-4c6c-9cf1-02996ebaffe5" />

## Signals to Observe

| Signal         | Purpose                                  |
|----------------|------------------------------------------|
| `CLK`          | Clock input to RVMYTH core              |
| `reset`        | Reset signal for initialization         |
| `RV_TO_DAC[9:0]` | 10-bit digital output from RVMYTH    |
| `OUT`          | DAC digital output                      |
| `OUT (real)`   | DAC analog output (optional, step mode)|

## 5. Analyze Waveforms

### Reset Operation
- Observe that outputs remain stable when `reset` is asserted.  
- Outputs start updating after `reset` is de-asserted.  

**Explanation:** Reset holds all outputs stable until initialization is complete.

### Clocking
- Verify that the `CLK` signal toggles periodically.  
- Check that sequential logic updates on the rising edge of the clock.  

**Explanation:** Shows proper synchronous operation of RVMYTH core and other modules.

### Dataflow Between Modules
- Observe `RV_TO_DAC[9:0]` changes propagate to `OUT`.  
- Confirm functional interaction between CPU, memory, and DAC.  

**Explanation:** Demonstrates correct dataflow and module interaction.

---

# **📙 Week 3 – Post-Synthesis GLS & STA Fundamentals**

- Perform Gate-Level Simulation (GLS) to validate functionality post-synthesis.
- Introduce Static Timing Analysis (STA) concepts using OpenSTA.

# POST_SYNTHESIS SIM WAVEFORMS

<img width="940" height="584" alt="image" src="https://github.com/user-attachments/assets/1886eed8-6aec-4cd2-b38d-21e961f9615d" />

# PRE_SYNTHESIS SIM WAVEFORMS
<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/4d487e4c-080b-4952-b8eb-6b2621abc0a5" />


## Compare GLS waveforms with functional simulation waveforms to verify correctness.
## Gate-Level Simulation (GLS) vs Functional Simulation Comparison

### 🔍 Summary
I inspected the two waveform screenshots — **pre-synthesis (functional simulation)** and **post-synthesis (GLS)** — for the **BabySoC** design.

- The key signals **`OUT`** and **`D[9:0]`** show **identical transitions and timing alignment** in both captures.  
- The analog real traces (**`VREFH`**, **`VREFL`**, and **`OUT`**) also exhibit the **same shape and timing window** in both waveforms.  
- No mismatches or unexpected delays were observed between the functional and GLS results.

### ✅ Conclusion
For the observed signals and time window:
> **GLS output matches the Functional simulation output.**

---

# **📕 Week 4 – CMOS Circuit Design (sky130-style)**

- Study MOSFET I–V characteristics (Id–Vds, Id–Vgs)
- Extract Threshold Voltage (Vt)
- Design and simulate a CMOS inverter
- Obtain Voltage Transfer Characteristic (VTC)
- Perform transient analysis to measure propagation delays
- Calculate noise margins
- Analyze power supply and device variation effects

## Experiment 1: MOSFET Behavior (Id vs. Vds)

### Objective
To observe the **linear and saturation regions** of an NMOS transistor by sweeping drain voltage (Vds)
for multiple gate voltages (Vgs).

### SPICE Netlist
```spice
vim day 2_nfet_idvds_LO15_WO39.spice

ngspice day 2_nfet_idvds_L015_W039.spice
plot -vdd#branch
```
<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/82de718c-4ab8-4578-b938-1fe76fa3e725" />

<img width="940" height="590" alt="image" src="https://github.com/user-attachments/assets/94b575c9-d496-46ef-8e5b-9626395d801f" />

## Observations

Linear region occurs when Vds < (Vgs - Vt)
Saturation begins when Vds ≥ (Vgs - Vt)
As Vgs increases, Id increases exponentially before saturation

##  Experiment 2: Threshold Voltage Extraction (Id vs. Vgs)

### Objective

To determine threshold voltage (Vt) using linear extrapolation from the Id–Vgs curve.

### SPICE Netlist
```
vim day 2_nfet_idvgs_LO15_WO39.spice

ngspice day 2_nfet_idvgs_L015_W039.spice
plot -vdd#branch
```
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/86ad882a-f78d-4794-a2e0-e5c9bcd1483d" />
<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/8624ef67-f332-4b44-b49f-e97aa95948e4" />

### Observations
Velocity saturation noticeable for short-channel devices

##  Experiment 3: CMOS Inverter VTC

### Objective
To study inverter Voltage Transfer Characteristic (VTC) and identify switching threshold (Vm).

### spicenetlist
```
vim day 3_inv_vtc_Wp084_Wn036.spice

ngspice day 3_inv_vtc_Wp084_Wn036.spice
plot out vs in
```
<img width="940" height="592" alt="image" src="https://github.com/user-attachments/assets/26f92899-0fe4-4f1b-b677-b0396be5aa43" />
<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/8fcc51f3-8235-48f9-a5da-3a35adf152d3" />

### Observations

The inverter switches near VDD/2.
The PMOS sizing (2× NMOS width) ensures symmetrical switching.

##  Experiment 4: Transient Analysis (Rise/Fall Delays)

### Objective
To find propagation delays using a pulse input.

### SPICE Netlist
```
vim day3_inv_tran_Wp084_Wn036.spice

ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs in
```
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/93aae014-6ad9-422e-96f8-c60a09576b98" />
<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/1ad2e79b-03cc-4814-9121-9ebaa602c6f3" />

### Observations
The rise delay (PLH) > fall delay (PHL) due to PMOS having lower mobility.

##  Experiment 5: Noise Margin Analysis

### Objective
To calculate noise margins from the VTC curve.

### spice netlist
```
vim day4_inv_noisemargin_Wp1_wn036.spice

ngspice day4_inv_noisemargin_Wp1_wn036.spice
plot out vs in
```
<img width="940" height="593" alt="image" src="https://github.com/user-attachments/assets/6a87865d-727c-4de6-b453-7f0299fd1998" />
<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/3768d38f-2ec2-4e7f-a4b1-fcc17cab210b" />

##  Experiment 6: Power Supply & Device Variation

### Objective
To study the effect of VDD and transistor sizing on switching threshold and noise margin.

### SPICE Netlist (VDD Sweep)
<img width="940" height="577" alt="image" src="https://github.com/user-attachments/assets/a6106858-3eee-4aed-904f-d2abc7c89eac" />
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/374ab76b-3dd6-4a2c-978d-bc4267cbb47b" />
<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/2b7ef0e5-7aff-43b9-bd4d-63cf258d3d62" />
<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/34155bd8-46a9-4429-bc7e-e243b5e56934" />

### Observation
Decreasing VDD shifts switching threshold lower and increases delay.
Increasing PMOS width improves logic high strength but increases capacitance.

### Analysis & Discussion
Device Physics: Increasing Vgs increases inversion channel charge, boosting Id until velocity saturation limits current.
STA Link: The transistor-level propagation delays correspond to the “cell delay” STA models.
Variation Impact: Changes in VDD or device W/L directly affect delay and slack — illustrating STA’s need for margining.
Symmetry: Proper PMOS sizing ensures balanced rise/fall times and symmetrical VTC.

### Conclusion
This CMOS circuit design study using the SKY130 PDK demonstrates:
How transistor parameters affect circuit behavior
How VTC and delay depend on device sizing and VDD
How noise margins ensure logic reliability
The connection between physical transistor limits and STA timing constraints
---

# **📙 Week 6**
## Objective:
To perform hands-on physical design using the OpenLane flow on the pre-configured VDI image, exploring the complete process from synthesis to layout and verification — including floorplanning, placement, routing, DRC, and STA.

## 🧾 Step 1: Setup and Synthesis
<img width="940" height="923" alt="image" src="https://github.com/user-attachments/assets/be52f72f-dab4-4ac9-a63b-dbbde1d56e15" />
<img width="940" height="898" alt="image" src="https://github.com/user-attachments/assets/eec24d0b-6892-412a-ac64-0a774ea9251e" />

### Description:

The interactive mode helps visualize and control each OpenLane stage step-by-step.
prep -design picorv32a initializes the design environment, creates a runs directory, and sets up all necessary configuration files.
run_synthesis performs logic synthesis converting RTL → gate-level netlist

Outputs:
synthesis.log
synthesis_stat.txt (contains number of cells, area, timing, etc.)
<img width="940" height="900" alt="image" src="https://github.com/user-attachments/assets/b7d2ada2-368f-4d85-ba30-db94cf3a798b" />

Floorplanning

Utilization Factor = area occupied by netlist / total core area.
Aspect Ratio = height / width (1 means square die).
Pre-placed Cells are IPs like memory, mux, comparator, etc. placed before auto-placement.
Decoupling capacitors stabilize voltage during switching.

Result: Die width = 660.685 µm, Die height = 671.405 µm
View floorplan in Magic:
<img width="940" height="926" alt="image" src="https://github.com/user-attachments/assets/baa9ed6a-5388-46da-a8ca-f8b2c3380c99" />
<img width="940" height="964" alt="image" src="https://github.com/user-attachments/assets/63731c5a-c29a-423b-93c9-71886e36dfb9" />

Placement

Concepts:

Library Binding: Connects logical cells to their physical library definitions (LEF + LIB).
Placement: Assigns physical locations to each standard cell on the floorplan.
Optimization: Minimizes wirelength and delay (inserting repeaters if needed).
Congestion-aware placement (RePlAce): Reduces routing congestion.

<img width="940" height="928" alt="image" src="https://github.com/user-attachments/assets/dc767c70-9308-4c87-a564-cdf404bfef30" />
<img width="940" height="902" alt="image" src="https://github.com/user-attachments/assets/24282d58-bafd-4e9c-b254-637d9e57efe1" />

Custom Cell Design and Characterization (Inverter)
<img width="940" height="1001" alt="image" src="https://github.com/user-attachments/assets/45fef96f-b660-4ae6-9a05-287a12925bf4" />

CMOS Fabrication (16-mask process)
Select substrate
Define active region
N-well/P-well creation
Gate formation
LDD formation
Source/drain diffusion
Contact and interconnect
Higher-level metal layers

Spice Extraction
<img width="940" height="903" alt="image" src="https://github.com/user-attachments/assets/2b13654e-0a3b-4175-be95-82a31a29d933" />
<img width="940" height="921" alt="image" src="https://github.com/user-attachments/assets/0f2dd3dc-5384-4745-8338-7ee540617995" />
<img width="940" height="985" alt="image" src="https://github.com/user-attachments/assets/f5f8e3fd-36df-490e-9290-d9dd174ada54" />

DRC and LVS Corrections
Goal: Learn DRC correction and rules file structure.
<img width="940" height="991" alt="image" src="https://github.com/user-attachments/assets/18b2a743-de40-483d-9196-dc3073ea2c5f" />

IO Placer Revision
<img width="940" height="985" alt="image" src="https://github.com/user-attachments/assets/507d7b18-e68f-4666-9259-6f5834e507c4" />

Track Information Extraction
Example Grid Values:

Grid 0.46um 0.34um 0.23um 0.17um

This defines routing grid pitch in X and Y directions for metal layers.

<img width="940" height="994" alt="image" src="https://github.com/user-attachments/assets/d37881c0-39a8-4af6-aed8-99c3e2989296" />

---







# 🏆 Conclusion
This final Week-9 documentation summarizes my complete journey in the **RISC-V SoC Tapeout Program by VSD**, covering every stage from week 1 to week 6 only
due to college work week 7 and week8 are not complete

I learned:
- RISC-V ISA fundamentals  
- Full ASIC flow using OpenLane  
- Hands-on SoC design steps similar to real tapeouts  

---
