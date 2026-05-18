# CMOS Standard Cell & Delay Characterization 

## 🎯 Project Overview
In modern VLSI physical design, achieving timing closure and minimizing power dissipation requires a deep understanding of standard cell behavior beyond ideal logic levels. 

This repository serves as a comprehensive, bottom-up characterization of CMOS standard cells using LTspice. It bridges the gap between theoretical semiconductor device physics and practical EDA tool application by extracting key timing, power, and physical metrics necessary for standard cell library development (.lib).

## 🛠️ Tools & Technologies
* **Simulator:** LTspice
* **Models:** 1µm / 180nm Custom Technology Models
* **Key Analyses:** `.dc` (Static/Sweeps), `.tran` (Time-domain), `.step` (Parametric Sweeps), `.meas` (Automated Data Extraction)

---

## 📂 Repository Structure

The project is structured sequentially, building from fundamental transistor physics up to complex interconnect delay modeling used in Static Timing Analysis (STA). 

### [Module 1: MOSFET Fundamental Characteristics](./01_MOSFET_Basics)
Establishes the baseline DC behaviors of NMOS and PMOS devices.
* Extraction of linear and saturation regions ($I_D$ vs. $V_{DS}$).
* Calculation of exact threshold voltages ($V_{th}$).
* **Subthreshold Leakage Analysis:** Logarithmic plotting of transfer characteristics to evaluate the primary driver of static power dissipation in idle circuits.

### [Module 2: 2nd-Order & Short-Channel Effects](./02_Second_Order_Effects)
Characterizes the non-ideal physical behaviors that dominate advanced technology nodes.
* **Channel Length Modulation (CLM):** Evaluating output resistance ($r_o$) degradation across shrinking channel lengths ($L=2\mu m$ down to $180nm$).
* **Body Effect:** Simulating $V_{th}$ shifts due to source-bulk biasing, crucial for modeling asymmetric delays in stacked logic (e.g., NAND/NOR pull-down networks).
* **Temperature Variations:** Identifying the **Zero Temperature Coefficient (ZTC)** crossover point where mobility degradation and $V_{th}$ roll-off intersect.

### [Module 3: CMOS Inverter Static Analysis](./03_Inverter_Static_Analysis)
Scales up to the foundational digital logic gate to analyze static behavior and internal power.
* **Voltage Transfer Curve (VTC):** Extraction of switching thresholds and static noise margins ($NM_H$, $NM_L$).
* **Beta Ratio Sizing:** Demonstrating the necessity of asymmetric device sizing ($W_p \approx 2.4 \times W_n$) to balance carrier mobilities and center the VTC.
* **Short-Circuit Power:** Isolating the dynamic short-circuit current spike ($I_{VDD}$) that occurs during state transitions.

### [Module 4: Transient Timing & Delay Modeling](./04_Inverter_Transient_Analysis)
Focuses on the time-domain metrics utilized by Physical Design synthesis and routing tools.
* **Propagation Delay:** Automated extraction of $t_{pHL}$ and $t_{pLH}$.
* **Load Capacitance Dependency:** Validating the linear relationship between delay and varying $C_{load}$ (0.1fF to 100fF).
* **Path Delay & Fanout:** Cascading a 5-stage inverter chain to measure additive delay and benchmark logic depth.
* **Interconnect RC Delay:** Modeling wire resistance and capacitance to demonstrate how physical routing degrades slew rates and necessitates buffer insertion.

---

## 🚀 How to Run
To run these characterizations locally:
1. Clone this repository.
2. Open the `.asc` schematic files in **LTspice**.
3. Run the simulations. The automated `.meas` scripts will output exact propagation delays, thresholds, and current maximums directly to the **SPICE Error Log** (`Ctrl + L`).
