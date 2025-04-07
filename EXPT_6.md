# **Current Mirror**

**Part 1: Current Mirror as an Active Load**

The current mirror circuit is one of the widely used designs in IC
design to provide a stable biasing current for multiple transistors at
once. Ideally, we would bias each transistor using a current source, to
ensure stability, but since this is not feasible, we use the current
mirror design and set the corresponding (W/L) ratio of each transistor
according to how much I<sub>D</sub> we need to flow in that transistor.

![image](https://github.com/user-attachments/assets/094c8770-c3c2-42ef-9d0f-d2abdc466493)


As we can see, using a single I<sub>Ref</sub>, we can bias the
transistor M<sub>Ref</sub>, using which we can set the current flowing
through M<sub>1</sub>, making it act as an active load, and use it to as
our I<sub>D</sub> value in further transistors.

**Design Question:**
![2](https://github.com/user-attachments/assets/9879d40f-fcf9-4c0f-80e8-ba31fb6ce9a8)

**Design: -**

From the question, we can see I<sub>total</sub> = P/V<sub>DD</sub> = 1
mW /1.8 V = **0.555 mA**

Next, we know that Itotal = I<sub>ref</sub> + I<sub>X</sub>, where
I<sub>X</sub> = I<sub>D</sub>. Since the (W/L) ratio specified to us is
1:1, I<sub>X</sub> + I<sub>ref</sub> = (I<sub>total</sub>/2) = 0.2777
mA. In the next question, where (W/L) ratio is 1:2, the value of
I<sub>X</sub> = (I<sub>total</sub>/3).

Next, we need to calculate how much input voltage we will need to give
the N-channel MOSFET, for it to work as an amplifier (with gain ≥ 10
V/V).

We know that A<sub>V</sub> = -g<sub>m</sub>R<sub>out</sub> =
-gm(r<sub>o1</sub>\|\|r<sub>o2</sub>), where r<sub>o1</sub>,
r<sub>o2</sub> are the dynamic output resistances of the NMOS, PMOS
respectively, due to channel length modulation.

Since we know that r<sub>o1</sub> = 1/λ<sub>1</sub>I<sub>D1</sub>,
r<sub>o2</sub> = 1/λ<sub>2</sub>I<sub>D2</sub>, where I<sub>D1</sub> =
I<sub>D2</sub> = I<sub>D</sub>.

r<sub>o1</sub> \|\| r<sub>o2</sub> = 1/I<sub>D</sub> (λ<sub>1</sub> +
λ<sub>2</sub>) = 1/ (0.277 x 10<sup>-3</sup> x (1.382 + 1.329)) =
1331.65 Ω = **1.331 kΩ**

Note that values for λ<sub>1</sub>, λ<sub>2</sub> have been obtained
from the TSMC 180 nm process technology .lib file, where the parameter
for these values is **PCLM (Channel Length Modulation Parameter).**

We can also write g<sub>m</sub> as 2I<sub>D</sub>/V<sub>OV</sub>.
Finally, substituting all of this back into our original equation, we
get,

A<sub>V</sub> = (2I<sub>D</sub>/V<sub>OV</sub>). R<sub>out</sub>, from
which we can calculate **V<sub>OV</sub> as 0.0737V.**

V<sub>GS</sub> = V<sub>OV</sub> + V<sub>t</sub> = 0.0737 + 0.496 =
**0.569V**

Therefore, our input voltage for the NMOS is 0.569 V.

**Circuit Diagram: -**

![3](https://github.com/user-attachments/assets/8d813d32-ccdf-41e1-96d8-d0f0308acda5)


**For the P-channel MOSFETs, M1, M2: -** **W = 10 µm, L = 180 nm**

**For the N-channel MOSFETs, M3: -** **W = 31.23 µm, L = 180 nm**

**i. Using L = 180 nm**

1.  **DC Analysis**

![4](https://github.com/user-attachments/assets/7757ed57-27f8-4c29-9aa5-89c9491e61a3)


From here, we can see that the simulation is closely matching with our
calculated values, since the current flowing through the NMOS (M3), PMOS
(M1, M2) are nearly identical. From this, we can verify whether the
circuit is following our power requirements, which is a key part in the
design of any IC or chip.

P = I<sub>total</sub> \* V<sub>DD</sub> = 0.55401 mA \* 1.8 V = **0.9972
mW**, which is within our power budget.

Next, we move on to transient analysis, where we can verify whether our
V/V gain requirement is satisfied or not. To do this, we will configure
our input voltage, V<sub>in</sub> and convert it into a sinusoidal input
with DC offset of 0.569V.

2.  **Transient Analysis**

![5](https://github.com/user-attachments/assets/1546781d-baaa-468b-98a7-fc85ea9237f8)


![6](https://github.com/user-attachments/assets/78ac6d3a-99c9-44e3-b6c6-1143dd6a0ea9)


![7](https://github.com/user-attachments/assets/c2a70a2e-225b-460b-9fe0-f0b615a65108)
From here, we can see that
the peak output voltage is 1.2381 V, for an input peak voltage of 10 mV.
Now, we can calculate our gain, and verify our whether our gain
requirement is satisfied or not.

A<sub>V</sub> = V<sub>out</sub>/V<sub>in</sub> = (0.96 – 1.2381)/(10 mV)
= **-27.81 V/V = 28.884 dB**

This definitely satisfies our gain requirement, since we needed a V/V
gain of ≥ 10 V/V.

3.  **AC Analysis**

![8](https://github.com/user-attachments/assets/61853b48-76f1-4fbc-94c1-0dbbb57c3ba2)
Finally, we move on to AC
analysis, where we can calculate the 3 dB bandwidth and verify our
calculated dB gain.

> From here, we can see our 3 dB BW = 629.997 MHz – 0 = **629.997 MHz**
>
> The midband gain we are obtaining from our frequency response, i.e.,
> **29.298 dB** is also similar to the value we calculated, **28.884
> dB**.

**Circuit 2: Ratio of 1:2 using L = 180 nm**

For ratio of 1:2, then I<sub>ref</sub> = I<sub>total</sub>/3 = **0.183
mA**.

**Circuit Diagram –**
![9](https://github.com/user-attachments/assets/c6834559-8f5b-440c-9265-49e9e6ca0e2d)

**DC Analysis –**

![image](https://github.com/user-attachments/assets/aeacf7c8-27e9-418f-8323-040e93700326)


**Transient Analysis –** 
![image](https://github.com/user-attachments/assets/53cda7cb-90f4-466d-ba91-103563fb4ab4)


Gain (V/V) = (1.26006-0.96)/ (-10 mV) = **30.006 V/V = 29.544 dB** for
an input voltage of 10 mV peak voltage, frequency of 1 kHz, sine wave.
Let us perform the frequency response to verify this.

**AC Analysis –**

![image](https://github.com/user-attachments/assets/8a0256d8-7ebb-405b-bd3d-6ce16fef2030)

From here, we can see that our midband gain is around **29.325 dB**,
which is a close match with our calculated value of 29.544 dB, which
gives a V/V gain of around **29.25 V/V**, not that far off from our
calculated gain of 30.006 V/V.

**ii. Analysis using L = 500 nm**

1)  **1:1 using L = 500 nm**

![image](https://github.com/user-attachments/assets/112bee00-a7d6-45e5-822f-0022f3bce997)


For CMOSN (M3), **W = 65.19 µm, L = 500 nm**

For CMOSP (M1, M2), **W = 10 µm, L = 500 nm**

**DC Analysis:**

![image](https://github.com/user-attachments/assets/1405c79e-8207-4dee-9f0c-1861b85fad78)


**Transient Analysis:**

![image](https://github.com/user-attachments/assets/ca38db47-69e0-4834-87d6-d2f965a4c3e2)


**AC Analysis –**

![image](https://github.com/user-attachments/assets/e59c1e43-d85f-4042-988b-023928582362)

**Gain = 37.87023 dB, 3 dB BW = 122.329 MHz**


**ii. Circuit 2: Ratio of 1:2 using L = 500 nm**

For CMOSN (M3), **W = 85.243 µm, L = 500 nm**

For CMOSP (M1), **W = 10 µm, L = 500 nm**

For CMOSP (M2), **W = 20 µm, L = 500 nm**

**DC Analysis –**

![image](https://github.com/user-attachments/assets/1f0f40b7-37fa-4ae1-817f-4f033bb5c5c7)


**Transient Analysis –**

![image](https://github.com/user-attachments/assets/eeedd0c2-4f60-4c28-9683-24cc9613c062)

Gain (V/V) = **-62.063 V/V = 35.855 dB**


**AC Analysis –**

![image](https://github.com/user-attachments/assets/94baf13d-39d4-4899-9b90-f9b43b8ebf68)


**3 dB BW = 93.0732 MHz, Gain (dB) = 38.015 dB**

**ii. Analysis using L = 1 µm**

1.  **Ratio of 1:1**

For CMOSN (M3), **W = 93.35 µm, L = 1 µm**

For CMOSP (M1), **W = 10 µm, L = 1 µm**

For CMOSP (M2), **W = 10 µm, L = 1 µm**

![image](https://github.com/user-attachments/assets/6e64999a-dce2-4205-9641-740e2690c468)


**DC Analysis –**

![image](https://github.com/user-attachments/assets/79851a15-408e-4775-9940-051068dd7ccc)


**Transient Analysis –**

![image](https://github.com/user-attachments/assets/86535f61-f412-4029-85f9-93cdb1f1df9f)


Gain (V/V) = **-60 V/V = 35.563 dB**

**AC Analysis –**

![image](https://github.com/user-attachments/assets/defc40cc-8234-49ec-8ecd-733e15169936)


**3 dB BW = 98.90171 MHz, Gain (dB) = 35.481 dB**

**ii. Analysis using L = 1 µm**

1.  **Ratio of 1:2**

For CMOSN (M3), **W = 121.51 µm, L = 1 µm**

For CMOSP (M1), **W = 20 µm, L = 1 µm**

For CMOSP (M2), **W = 10 µm, L = 1 µm**

![image](https://github.com/user-attachments/assets/2720841f-08a8-46da-8029-544767c6a25a)


**DC Analysis –**

![image](https://github.com/user-attachments/assets/53290c6e-6c88-487f-b594-41697ceedf2e)


**Transient Analysis –**

![image](https://github.com/user-attachments/assets/293df305-337b-4bf2-b9b3-980577d8f5de)


**AC Analysis –**

![image](https://github.com/user-attachments/assets/da11431d-2f16-4862-aa09-dfe1a446c901)


**3 dB BW = 57.2524 MHz, Gain (dB) = 38.69341 dB**

**Table of Comparisons**

| **Parameter** | **L = 180 nm (1:1)** | **L = 180 nm (1:2)** | **L = 500 nm (1:1)** | **L = 500 nm (1:2)** | **L = 1 μm (1:1)** | **L = 1 μm (1:2)** |
|----|----|----|----|----|----|----|
| **NMOS Width** | 31.23 μm | 41.127 μm | 65.19 μm | 85.243 μm | 93.35 μm | 121.51 μm |
| **PMOS Width (M1)** | 10 μm | 10 μm | 10 μm | 10 μm | 10 μm | 20 μm |
| **PMOS Width (M2)** | 10 μm | 20 μm | 10 μm | 20 μm | 10 μm | 10 μm |
| **Voltage Gain (V/V)** | 27.81 | 30.006 | 61.00 | 62.063 | 60 | 86.03 |
| **Gain (dB)** | 29.298 | 29.325 | 37.87 | 38.015 | 35.481 | 38.693 |
| **3dB Bandwidth (MHz)** | 629.997 | 438.012 | 122.329 | 93.073 | 98.902 | 57.252 |

**Part 2: Differential Amplifier**

Now that we have understood the basics of the current mirror, we will
move onto using it to design our differential amplifier, using the same
parameters that we designed for the differential amplifier simulation.
![image](https://github.com/user-attachments/assets/2d544f0f-8e17-494f-981b-72b8b4857d0a)


For the N-channel MOSFETs (CMOSN), the W/L ratios are as follows: -

**M1, W = 108.5 µm, L = 180 nm**

**M2, W = 108.5 µm, L = 180 nm**

**M3, W = 22.42 µm, L = 180 nm**

**M6, W = 10 µm, L = 180 nm**

The reason for the (W/L) ratio of M3 being more than double that of M6
is because the current flowing through M3 should be double than the
current of M6, i.e., I<sub>M3</sub> = 1.222 mA, while I<sub>M6</sub> =
0.611 mA.

This is also the same reason why M4, M5 have the same (W/L) ratios, so
that equal amounts of current flows through both transistors.

For the P-channel MOSFETs (CMOSP), the W/L ratios are as follows: -

**M4, W = 52 µm, L = 180 nm**

**M5, W = 52 µm, L = 180 nm**

**M6, W = 10 µm, L = 180 nm**

**DC Analysis**

![image](https://github.com/user-attachments/assets/42a9d3b0-ab86-42b8-a6cc-577019d76d99)


As we can see, for our (W/L) ratios, the values we obtain from the DC
analysis closely match the ones we require.

**Transient Analysis**

![image](https://github.com/user-attachments/assets/a5f87f1b-ff21-419f-b55b-7e96e5d6b8d9)


We have supplied a differential input of 10 mV peak voltage, 1 kHz
frequency, sine wave. The input is differential, i.e., only to the gate
of M1, since the output of a differential amplifier is the difference in
drain voltages of M1, M2. Since the output is a difference, if we supply
common input, the amplifier will reject the signal and provides very
little to no gain (ideally).

A<sub>v</sub> = (1.092 – 0.95)/ (-10 mV) = **-14.2 V/V = 23.045 dB**

**Results and Inferences**

The experiment demonstrates several important characteristics of current
mirror circuits with varying channel lengths and W/L ratios:

1.  **Gain-Bandwidth Trade-off**

There is a clear inverse relationship between gain and bandwidth across
the different channel lengths. The 180 nm designs show modest gain (~29
dB) but exceptional bandwidth (\>600 MHz), while the 1 μm designs
provide much higher gain (~35-39 dB) at the expense of reduced bandwidth
(\<100 MHz).

2.  **Channel Length Effects**

Increasing the channel length from 180 nm to 500 nm and then to 1 μm
results in:

1.  Increased gain: Due to reduced channel length modulation effects (λ)
    in longer channel devices, resulting in higher output resistance.

2.  Decreased bandwidth: Due to increased parasitic capacitances
    associated with larger transistor dimensions.

3.  Larger transistor widths required: NMOS widths had to be
    significantly increased (31 μm → 121 μm) to maintain the same
    current levels.

<!-- -->

3.  **W/L Ratio Impact**

The 1:2 ratio consistently outperforms the 1:1 ratio in terms of gain
across all channel lengths:

1.  For 180 nm: Modest improvement (29.298 dB → 29.325 dB)

2.  For 500 nm: Slight improvement (37.87 dB → 38.015 dB)

3.  For 1 μm: More significant improvement (35.481 dB → 38.693 dB)

The current mirror circuit's performance can thus be tailored to
specific application needs by selecting appropriate channel lengths and
W/L ratios, with clear trade-offs between gain, bandwidth, and silicon
area (as evidenced by the increasing transistor widths required for
longer channel designs).
