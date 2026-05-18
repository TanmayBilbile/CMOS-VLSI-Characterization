# Module 1: MOSFET Fundamental Characteristics

## 🎯 Objective
To simulate and extract the baseline DC characteristics of NMOS and PMOS transistors using LTspice. This module establishes the foundational device physics—including linear/saturation regions, threshold voltage ($V_{th}$), and subthreshold conduction—required to build accurate standard cell timing and power models for physical design.

---

## 1. NMOS Output Characteristics ($I_D$ vs. $V_{DS}$)
The output characteristic maps the drain current against the drain-to-source voltage across multiple gate biases, demonstrating how the gate controls the channel.

* **Simulation Setup:** $V_{DS}$ swept from 0V to 5V. $V_{GS}$ stepped from 0V to 2V in 0.5V increments.
* **Key Observation:** The distinct transition from the linear (triode) region to the saturation region is visible as $V_{DS}$ exceeds the overdrive voltage ($V_{GS} - V_{th}$). 

![NMOS Output Curve](nmos_iv_output.png)

---

## 2. NMOS Transfer Characteristics & Threshold Voltage
The transfer characteristic maps the drain current against the gate-to-source voltage to identify the exact point the transistor "turns on."

* **Simulation Setup:** $V_{GS}$ swept from 0V to 5V with a fixed $V_{DS}$ of 2V.
* **Key Observation:** The device begins conducting heavily right at the modeled threshold voltage of $V_{th} = 0.7V$.

![NMOS Transfer Curve](nmos_iv_transfer.png)

### 🔬 Subthreshold Leakage Analysis
While ideal models assume zero current below $V_{th}$, modern VLSI design must account for subthreshold conduction. By plotting the transfer curve on a logarithmic scale, we can observe the exponential relationship of the leakage current below 0.7V. **This metric is the primary driver of static/leakage power dissipation in idle CMOS circuits.**

![Subthreshold Leakage](nmos_iv_transfer_subthreshold_leakage.png)

---

## 3. PMOS Output Characteristics
PMOS devices operate with reversed polarities and rely on hole mobility rather than electron mobility.

* **Simulation Setup:** $V_{DS}$ swept from 0V to -2V. $V_{GS}$ stepped from 0V to -2V in 0.5V increments.
* **Key Observation:** The characteristics perfectly mirror the NMOS behavior. Notice that for the same absolute gate voltage, the PMOS drives less current than the NMOS due to lower hole mobility ($\mu_p < \mu_n$). This disparity dictates why standard cells require asymmetric sizing (Beta Ratio) to achieve balanced rise and fall times.

![PMOS Output Curve](pmos_iv.png)

---

## 🚀 How to Run the Simulations
To run these characterizations locally:
1. Clone the repository and navigate to the `01_MOSFET_Basics` folder.
2. Open the `.asc` schematic files in **LTspice**.
3. Click **Run** (the running man icon) to execute the pre-configured `.dc` sweeps.
4. Probe the drain terminal of the transistors to view the plotted currents.
