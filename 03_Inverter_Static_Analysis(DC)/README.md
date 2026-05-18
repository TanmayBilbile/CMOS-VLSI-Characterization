# Module 3: CMOS Inverter Static Analysis

## 🎯 Objective
This module scales up from single-device physics to the fundamental building block of digital logic: the CMOS Inverter. Through DC static analysis, this section extracts the Voltage Transfer Curve (VTC), calculates static noise margins ($NM_H$, $NM_L$), demonstrates the necessity of Beta Ratio sizing ($\beta$), and analyzes internal short-circuit power dissipation during switching transitions.

---

## 1. Asymmetrical VTC (Equal Transistor Sizing)
Initial simulations were run with equally sized transistors ($W_p = W_n$). 

* **Simulation Setup:** `.dc Vin 0 5 0.05` with equal widths ($L=1\mu m$, $W=1\mu m$).
* **Observation:** Because electron mobility ($\mu_n = 600$) is significantly higher than hole mobility ($\mu_p = 250$), the NMOS pulls the output low much stronger than the PMOS pulls it high. 
* **Result:** The switching threshold ($V_{switch}$) shifts to the left (**~2.12V** instead of the ideal **2.5V**). This results in asymmetrical noise margins, making the gate more susceptible to high-level noise.
  * Extracted $NM_H$: **2.49V**
  * Extracted $NM_L$: **1.45V**

---

## 2. Symmetrical VTC (Beta Ratio Sizing)
To maximize overall noise immunity, standard cells must be sized asymmetrically to achieve a symmetrical VTC. The PMOS must be scaled up to compensate for its lower carrier mobility.

* **Simulation Setup:** PMOS width was increased to match the NMOS drive strength ($W_p \approx 2.4 \times W_n$).
* **Observation:** The VTC is now perfectly symmetrical. 
* **Result:** The SPICE `.meas` directives confirm that $V_{switch}$ is perfectly centered at exactly **2.5V** (for a **5V** supply). This balances the noise margins, providing maximum robustness against signal integrity issues in physical routing.

---

## 3. Short-Circuit Current (Internal Power Dissipation)
While CMOS logic ideally consumes zero static power, it consumes dynamic power during switching events. This simulation isolates the **short-circuit current** component of dynamic power.

* **Simulation Setup:** Plotted both $V_{out}$ and the PMOS drain current ($I_d(M2)$) against a swept $V_{in}$.
* **Observation:** When the input voltage is near the switching threshold ($V_{in} \approx 2.12V$), both the NMOS and PMOS are simultaneously conducting in their saturation regions. This creates a direct low-resistance path from $V_{DD}$ to Ground.
* **Result:** A sharp current spike (measured at **117µA**) occurs exactly during the state transition. 

### 💡 Relevance to Physical Design
In physical design, minimizing this short-circuit power is critical. If a signal arriving at an inverter has a slow slew rate (a long transition time due to high RC wire delay), the inverter spends more time in this "short-circuit region," drastically increasing internal power dissipation. This is a primary reason why EDA tools insert buffers to sharpen transition times across long interconnects.
