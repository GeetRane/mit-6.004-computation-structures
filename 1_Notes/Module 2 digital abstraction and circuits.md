### Real Life Applications
- We know that there is no 0s or 1s in nature, we use voltage to represent the 0s and 1s required by chips
- Say we have to encode an image, the black is 0V and the white is 1V, if we want the image to be blocky, we use this rule strictly and we skip the colors/voltages in between, as a result the bits required to encode are also short, in contrary to this if we want to encode the image almost perfectly, nearly infinite number of bits would have to be used which is impossible so we settle for something in between.
- Now How do we encode? say we divide the image in several horizontal lines and map a voltage graph according to intensity of colors (0 for no intensity and 1 for white) and then in the graph (analog) we create difference by blocking the regions as 0s, 1s, and now you have encoded the image.

## Combinational devices
- The devices that take digital inputs, manipulate/transmit them, and give digital output are known as combinational devices. If these rules are followed the device is said to ==follow static discipline==
- Also time taken for all this is known as Tpd
- Any device made from this is also a combinational device, if it does not have any closed circuit

## Real Life challenges
### Digital and Analog problem
- ==There is no specific as "Strictly Digital" in Nature, everything is continuous and analog.==
- ==We define few voltage ranges to take some values as 0 and some as 1==
  ![[Pasted image 20260702150531.png]]![[Pasted image 20260702150603.png]]
- ==We as engineers find out materials and structurally place them to create the combinational device such that it has the specific voltage thresholds, but this is not that efficient at bigger stages, then we use the specific devices such as Analog to Digital Converter (ADC)==
- Our proposed fix to the noise problem is to provide separate signaling specifications for digital inputs and digital outputs. To send a 0, digital outputs must produce a voltage equal to VOL and to send a 1, produce a voltage equal to VOH. So far this doesn’t seem very different than our previous signaling specification...

- The difference is that digital inputs must obey a different signaling specification. Input voltages less than or equal to VIL must be interpreted as a digital 0 and input voltages greater than or equal to VIH must be interpreted as a digital 1.

![[Pasted image 20260702151110.png]]

- ==The specific noise thresholds suggest that noise is inevitable and sometimes predictable==
- Also the above image is a reason why output forbidden range is higher than input forbidden range, or the **valid input range is higher than valid output range. Because noise will take place while going from output to input so input should be flexible.** 

### Buffer
https://youtu.be/4tDBaN7kKe4?si=3eOuQqOZrAxkwTFA


Signaling:
• Analog: each processing step accumulates noise
• Digital: each processing step restores output to a valid digital level

### The main application is to find the four threshold voltages from given VTC 
#### Strategy 1
- First using some mind visualize and finalize the output forbidden ranges
- Then find the input forbidden ranges such that the **input forbidden range is always smaller than output forbidden range**
- Also keep hold of the inequality relation between the four voltages

#### Strategy 2
- We have to find two points on the curve such that
- the second point's first quadrant should be empty (for decreasing)
- the first point's third quadrant should be empty (for decreasing)
- The reason the points have to be on curve is to maximize the noise immunity or the difference between input forbidden range and output forbidden range. If not then we can even shift the points in the respective quadrant direction also.


![[Pasted image 20260702171248.png]]

**==The answer of B part is less than 0.6 since the output of 1 can be even given in forbidden zone==**


1. **The Noisy Arrival**: The "parcel" arrives at the input center with a damaged label (0.1V instead of 0V).
    
2. **The Decision (Threshold)**: The clerk checks the label. The rule is: "If value < 0.5V, treat as Logic 0." The clerk sees 0.1V, confirms it is a Logic 0, and **discards the damaged parcel**.
    
3. **The Regeneration (New Parcel)**: The clerk goes to the warehouse (the Power Supply/$V_{DD}$ or Ground), picks up a **brand new, perfect parcel** (0.0V), and sends _that_ to the output center.
    
4. **The Result**: The next leg of the journey starts with a pristine 0.0V signal. The original 0.1V error never leaves the first facility.

Output is more precise, the noise happens actually in registering the input

So this is why the input is more flexible than output, but why the curve is not straight, one explanation can be given that since input is more flexible, and output is more precise, the graph "must" be like that, and the reason the curve flattens out at both ends is that any input below Vil, the output should give a voltage that is very close to 0

### Contamination Delay
- **$t_{cd}$**: "How soon can the output _start_ to become invalid?" (Minimum delay) or The time taken to **start** the job (throw away the old letter).  This is the "minimum" time.
    
- **$t_{pd}$**: "How long until the output is _guaranteed_ to be valid?" (Maximum delay)

You are absolutely correct about the **Input Center** throwing away the noisy parcel. That part of your model is perfect.

The confusion lies in **what happens at the Output Center during the swap**. 

The output does **not** hold a "contaminated letter." Instead, for a brief moment, it holds **NO letter at all** (or a half-written one). 

Here is the precise sequence using your analogy:

1. **Before $t_{cd}$**: The Output Center is holding a **perfect, old letter** (e.g., a clean "0V" letter from the previous operation).  It is valid and safe to read.
    
2. **At $t_{cd}$ (Contamination Delay)**: The Input Center sends the signal to "Switch to 1." The Output Center clerk **throws away the old "0V" letter**.
    
    - At this exact instant, the clerk hasn't finished writing the new "1V" letter yet.
        
    - The desk is now **empty** (or the voltage is halfway between 0 and 1, e.g., 2.5V).
        
    - **This is the "Invalid" state.** If someone looks at the output now, they see nothing meaningful (garbage/forbidden zone).  The old valid data is **gone** (contaminated). 
        
3. **Between $t_{cd}$ and $t_{pd}$**: The clerk is frantically writing the new letter. The output is **unstable** and changing.  It is **invalid** the whole time. 
    
4. **At $t_{pd}$ (Propagation Delay)**: The clerk finishes writing and stamps the **new, perfect "1V" letter**.  The output is valid again.
    

### Summary Correction

- **Input Center**: Correctly filters noise (0.1V $\to$ Decision: "It's a 0"). 
    
- **Output Center**:
    
    - **Does NOT** pass the noise forward.
        
    - **Does** destroy the _old_ valid value at $t_{cd}$ to make room for the new one.
        
    - **Is Empty/Invalid** between $t_{cd}$ and $t_{pd}$.
        
    - **Produces** a fresh, clean value at $t_{pd}$. 
        

**Contamination Delay ($t_{cd}$)** is simply the guarantee of **how long the old, correct letter stays on the desk** before the clerk throws it away to start the new one.  It tells you the earliest moment the output becomes unreliable.

1. **Input Changes:** The input center sees the jump and decides "This is now a 1."
    
2. **Wait for $t_{cd}$:** For this brief period, the output center **still holds the old, perfect 0V letter**.  The output is **valid**.
    
3. **At $t_{cd}$ (Contamination):** The output center **throws away the old 0V letter**.  The output voltage **starts** to move.  It is no longer 0V, but it is not yet 1V. It enters the "forbidden zone" (noise/fluctuation). **The output is now INVALID.**
    
4. **Between $t_{cd}$ and $t_{pd}$:** The output is swinging through the garbage zone (e.g., 0.5V, 1.2V, 2.0V...). It is unstable and **cannot be trusted**. 
    
5. **At $t_{pd}$ (Propagation):** The output center finishes writing and places the **new, perfect 1V letter** on the desk.  The output is **valid again**.

So technically the output will first give 0V letters out until contamination delay, then some fluctuations and then give 1V letter as the input changes from 0V to 1V for a combinational buffer

![[Pasted image 20260708125919.png]]

---
# Contamination delay is the measure of time of how long the output gives previous signal for which input stays invalid or changed itself

# The propagation time is the delay from when the input becomes _valid_ as it crosses VIH and when the output becomes _valid_ as it crosses VOL.

# Propagation delay is the time when input is valid and for how time output stays invalid, or till output stabilizes.

---

Here is the next chapter for your Obsidian vault. It covers the general CMOS recipe, the physics of structural duality, the exact definitions of $t_{pd}$ and $t_{cd}$, and the core concept of **Leniency**.

---

# CMOS Gate Synthesis and Timing Dynamics

![[Pasted image 20260710161432.png]]

## Act I: The Dual CMOS Recipe

To build any arbitrary logic function beyond a simple NOT gate, we use a structured mirroring technique. Because PFETs and NFETs require opposite gate voltages to close their switches, the pull-up network (PUN) and pull-down network (PDN) must be **topological duals** of each other. This duality is rooted directly in **De Morgan's Laws**.

![[Pasted image 20260710161455.png]]

### The Mapping Rules:

* **Series components** in the Pull-Up network map to **Parallel components** in the Pull-Down network.
* **Parallel components** in the Pull-Up network map to **Series components** in the Pull-Down network.

### Example Synthesis ($F = \bar{A} + \bar{B}\cdot\bar{C}$):

1. **Pull-Up Network (PFETs):** To output a `1`, we need $A$ to be `0` **OR** ($B$ to be `0` **AND** $C$ to be `0`).
* Structurally, PFET $A$ is placed in **parallel** with a branch containing PFET $B$ and PFET $C$ in **series**.


2. **Pull-Down Network (NFETs):** We apply the duality rule to find the complementary path to Ground.
* The parallel connection to $A$ becomes a **series** connection to NFET $A$.
* The series branch of $B$ and $C$ becomes a **parallel** network of NFET $B$ and NFET $C$.



---

## Act II: Digital Timing Specifications ($t_{pd}$ and $t_{cd}$)

![[Pasted image 20260710161524.png]]

In the physical world, voltages cannot change instantly. When an input wire changes state, it takes time for the internal parasitic capacitors to charge or discharge before the output wire stabilizes. We measure this lag using two strict limits:

### 1. Contamination Delay ($t_{cd}$)

* **Definition:** The **shortest possible time** from a valid input transition to the absolute first moment the output wire begins to change its voltage.
* **Why it matters:** It defines how long an output is guaranteed to remain stable after its inputs start changing. This is critical for preventing racing conditions in sequential logic.

![[Pasted image 20260710161608.png]]

### 2. Propagation Delay ($t_{pd}$)

* **Definition:** The **longest possible time** from a valid input transition to the moment the output wire completely settles into its final, valid digital state.
* **Why it matters:** It defines the absolute worst-case speed of the gate. This limit dictates the maximum clock frequency of the processor.

![[Pasted image 20260710161548.png]]

---


![[Pasted image 20260710161639.png]]

![[Pasted image 20260710161721.png]]

---

## Act III: Lenient Gates

A gate is defined as **Lenient** if its output can be completely determined by a subset of its inputs, making changes on the remaining inputs irrelevant for that specific transition.

### The Classic Example (2-Input OR Gate: $Y = A + B$)

* Suppose Input $A$ is a solid, stable `1`.
* Because $A$ is `1`, the output $Y$ is guaranteed to be `1`, regardless of whether Input $B$ is a `0`, a `1`, or floating in mid-air.

### How Leniency Impacts Timing Diagrams:

If Input $B$ transitions while Input $A$ is locked at `1`, **the output wire does not move at all.** * When creating the overall timing specifications for a chip, we **cannot** simply look at these lenient transitions, because their individual delay is effectively infinite (the output never reacts to that specific input).

* To find the true, global $t_{pd}$ and $t_{cd}$ of a device, we must scan the graph to find the **worst-case paths**: the absolute fastest path where the output *does* change to find $t_{cd}$, and the absolute slowest path to find $t_{pd}$.

Leniency allows complex processors to run faster in practice because the system can often bypass waiting for slow, late-arriving inputs if an early input has already definitively decided the final answer.

