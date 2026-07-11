# CMOS Lab: What Actually Matters

## 1. Noise Margins & The PFET Tweak
* **What I learned:** Noise margins tell me how much electrical garbage/distortion a gate can handle before it misreads a 1 as a 0. I found the limits by looking at where the VTC curve drops at a steep 45-degree angle (slope = -1).
* **The Catch:** NFETs are naturally faster and stronger than PFETs. If they are the same size, the gate is lopsided. To balance it out and make the switching symmetrical, we have to widen the PFET channel (usually making it double the width of the NFET).

## 2. Delay: Contamination vs. Propagation
* **Contamination Delay ($t_{cd}$):** The absolute shortest time it takes for the output to *start* changing after an input flips. Think of it as the "best-case speed limit."
* **Propagation Delay ($t_{pd}$):** The maximum time it takes for the gate to completely settle down and finish its job. This is the "worst-case delay" that determines how fast our CPU clock can run.

## 3. The "Forbidden" NFET Pull-Up
* **The Rule:** NFETs are excellent at passing a strong ground (0V) but terrible at passing a full supply voltage (Vdd). They suffer from a threshold drop, meaning a 1V supply gets degraded down to something like 0.7V.
* **Why it sucks:** If you use an NFET on top, your "High" voltage state drops significantly, which completely crushes your High Noise Margin ($NM_H$). The circuit becomes incredibly fragile to noise.

## 4. Jade Current ($I(V_{dd})$) Diagnostics
* **What the graph means:** Voltage graphs can lie to you because a strong NMOS can force a correct '0' output even if the wiring is wrong. 
* **The Ultimate Check:** Look at the current chart. 
  - Sharp, quick dips that return to 0A mean the circuit is perfect (only drawing power dynamically while switching).
  - A flat line stuck far below 0A means I messed up De Morgan's dual layout, causing both networks to stay open at the same time, short-circuiting power straight to ground.