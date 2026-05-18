# Module 4: Transient Timing & Delay Modeling

## 🎯 Objective
In physical design and static timing analysis (STA), static logic levels are not enough; time-domain performance dictates the maximum clock frequency of a chip. This module extracts key transient metrics: intrinsic propagation delay, the impact of load capacitance ($C_{load}$), multi-stage fanout delays, and interconnect RC wire delay. 

---

## 1. Intrinsic Propagation Delay ($t_{pd}$)
Propagation delay is the time required for a signal to cross the 50% threshold from input to output. 

* **Simulation Setup:** An asymmetrically sized inverter ($W_p = 2.4\mu m$, $W_n = 1.0\mu m$) driven by a sharp 100MHz pulse, driving a standard 10fF load.
* **SPICE Directives:** `.meas TRAN tpHL` and `.meas TRAN tpLH` evaluating the 2.5V crossing points.
* **Extracted Results:** * High-to-Low Delay ($t_{pHL}$): **50.96 ps**
  * Low-to-High Delay ($t_{pLH}$): **49.02 ps**
  * Average Propagation Delay ($t_{pd}$): **49.99 ps**
* **Key Takeaway:** The intentional scaling of the PMOS width successfully balanced the drive strengths, yielding nearly identical rise and fall delays.

---

## 2. Effect of Load Capacitance on Delay
In a physical layout, standard cells must drive various fanouts and wire lengths, representing different load capacitances. 

* **Simulation Setup:** `.step param CL list 0.1f 10f 100f`
* **Extracted Results:**
  * $C_L = 0.1fF$ (Near-zero load): $t_{pd}$ = **13.58 ps**
  * $C_L = 10fF$ (Nominal load): $t_{pd}$ = **49.99 ps**
  * $C_L = 100fF$ (Heavy load): $t_{pd}$ = **258.08 ps**
* **Key Takeaway:** The data confirms the fundamental first-order timing model: propagation delay scales linearly with load capacitance ($t_d \propto C_{load} \times R_{on}$).

---

## 3. Fanout Effect & Path Delay (Inverter Chain)
Standard timing analysis calculates path delay by summing the delays of individual cells. This simulation models a 5-stage inverter chain to validate additive delay.

* **Simulation Setup:** 5 identical inverters cascaded in series. Automated `.meas` scripts extracted the delay of each individual stage and the total end-to-end delay.
* **Extracted Results:**
  * Stage 1-4 individual delays ranged from **60.6 ps to 72.6 ps** (dependent on the input slew rate from the previous stage).
  * Stage 5 delay: **58.1 ps**
  * Sum of individual stage delays: **334.44 ps**
  * Total end-to-end path delay ($V_{in}$ to $V_{out5}$): **334.44 ps**
* **Key Takeaway:** The sum of the individual gate delays perfectly matches the total measured path delay. This validates the core assumption of Static Timing Analysis (STA) tools used in physical design.

---

## 4. Interconnect RC Wire Delay
At advanced technology nodes, the delay caused by the metal routing (wires) often exceeds the delay of the logic gates themselves. 

* **Simulation Setup:** A discrete Pi-model RC network was inserted between two inverter stages. The resistance ($R$) and capacitance ($C$) were stepped to model varying wire lengths.
* **Extracted Results:**
  * $R=100\Omega, C=100fF$: Total Delay = **376.6 ps**
  * $R=500\Omega, C=100fF$: Total Delay = **395.8 ps**
  * $R=100\Omega, C=200fF$: Total Delay = **636.8 ps**
  * $R=500\Omega, C=200fF$: Total Delay = **671.5 ps**
* **Key Takeaway:** The addition of wire RC parasitics significantly degrades the signal slew rate and increases overall path delay. In physical design, this necessitates the insertion of buffers/repeaters to break up long nets and restore signal integrity.
