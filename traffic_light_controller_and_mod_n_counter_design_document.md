# Capstone Project Design Document: Traffic-Light Controller & Mod-N Counter

## 1. Introduction and Objectives
This project implements a digital system combining a **Mod-N Counter** and a **Traffic-Light Controller**. 
* **Mod-N Counter:** Acts as a programmable clock divider or timer generating precise clock ticks or state transitions based on a modulo-$N$ sequence (e.g., Mod-10 for decimal counting or custom timing intervals).
* **Traffic-Light Controller:** Implemented as a Finite State Machine (FSM) that cycles through standard traffic signals (Red, Yellow, Green) synchronized by the counter's timing pulses.

---

## 2. System Architecture
The overall architecture consists of two primary modules interacting synchronously:
1. **Timing Unit (Mod-N Counter):** Counts pulses from a master clock up to $N-1$ and resets to $0$, generating a tick output every $N$ cycles.
2. **Control Unit (FSM Traffic Light):** Receives the tick signal to transition between states:
   * **State 0 (Green):** Allow traffic flow in direction A.
   * **State 1 (Yellow):** Transition warning.
   * **State 2 (Red):** Stop traffic flow, allow alternative direction.

---

## 3. Mod-N Counter Design
* **Parameters:** $N$ (e.g., $N = 10$ for a decade counter or configurable timer).
* **State Variables:** $q[3:0]$ for a 4-bit implementation.
* **Behavioral Logic:**
  * On the rising edge of the clock:
    $$\text{If } q = N-1, \text{ then } q \leftarrow 0$$
    $$\text{Else } q \leftarrow q + 1$$
* **Outputs:** 
  * Current count state $q$.
  * Terminal count tick pulse: $\text{tick} = (q == N-1)$.

---

## 4. Traffic-Light Controller FSM Design
* **State Encoding (Moore Machine):**
  * $S_0$ (Green): Output = `001`
  * $S_1$ (Yellow): Output = `010`
  * $S_2$ (Red): Output = `100`

* **State Transition Table:**
  * Current State $S_0$ + Timer Tick $\rightarrow$ Next State $S_1$ (after preset duration)
  * Current State $S_1$ + Timer Tick $\rightarrow$ Next State $S_2$ (after preset duration)
  * Current State $S_2$ + Timer Tick $\rightarrow$ Next State $S_0$ (after preset duration)

---

## 5. Implementation & Verification Plan
1. **Schematic Capture / HDL Description:** Combine the Mod-N counter output as the clock enable or transition trigger for the traffic light FSM.
2. **Simulation:** Verify proper timing sequences using waveform simulations to ensure lights remain green/yellow/red for the exact required clock cycles.
3. **Synthesis:** Map onto target digital logic components (multiplexers, flip-flops, and combinatorial gates).