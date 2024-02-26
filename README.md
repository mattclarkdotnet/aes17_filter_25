# VERSION 2 WIP

Doing:

* Space inductors out more to address excess HD3 found by @sarieri on ASR
* Move pad-down network to before filter so lower value resistors can be used
* Use serial 0.5W resistors (2k7+1k8) for 4k5 pad down to allow continuous use at 75Vrms without requiring hard to get parts
* Use 4-layer board to simplify layout
* Replace some oversized resistors with smaller 0.25W versions and improve tolerances
* Add ground jack
* Update to KiCad 8
* Use Mouser for BOM (done except for 4mm connectors, which Mouser doesn't have good ones of)
* Reduce baord width to 175mm
* Recommend Hammond 1444-862 case

Maybe:
* Switch to shielded coilcraft inductors - the issue is these have 10% tolerance instead of 5%
* Reduce Vrms goal to 60 - this is 900W into 4ohms - would allow use of smaller and cheaper parts
* Create a version with most parts SMD - this would probably not be safe into a short circuit but could be orderable through JLC
* Use LCSC for BOM to make prefab with JLC easier

# Goals

The goal is to make a cheap DIY passive filter for use in class D amp measurements, much like the AP AUX-0025.  The AP AUX-0025 specs as read off the [AP website](https://www.ap.com/analyzers-accessories/accessories/aux-family-switching-amplifier-measurement-filters/) are:
* Rejection > 50dB, 250 kHz to 20 MHz
* Frequency response +-0.05dB, 20Hz-20kHz
* Max input voltage 400V P-P (140Vrms)
* THD < -110dB

I also wanted the ability to pad down the output for high power measurements.  The resulting board looks like this:

![3D board model](./board_3d.png)

# Results

- [x] Rejection > 50dB, _230_ kHz to 20 MHz
- [x] Frequency response +-0.05dB, 20Hz-20kHz
- [x] Max input voltage 400V P-P (140Vrms)
- [x] THD = _-130dB_

The simulated FR with a 100k load is on target, with -51mdBV to +51mdBV varitation from 20Hz to 20kHz, and 50dB rejection or more from 230kHz up.  The 100mdBV variation is maintained across a wide range of loads from 15k to 450k.  Measured THD is more like -130dB, so will not add to measured distortion of amplifiers, but may impact extreme resolution DAC measurements.

![Screenshot of simulated frequency response](./freq_load100k.png)



# Design discussion

See the design on [CircuitLab](https://www.circuitlab.com/editor/#?id=9zaq989z472b). It is inspired by the AUX-0025 but differs in a few ways.  The component values are more standard, and are reused across the design to reduce the number of different components needed.  Inductor tolerances are 5% instead of 2.5%, but resistors are all 1% and the 220p capacitors are 2.5%.  The matching to the AUX-0025 response curve is very close.

A set of pad-down resistors allows for convenient high voltage measurements, by default giving -20dB of gain.

The load impedance should be >50kOhm for best performance, but a 10kOhm load is still +-0.1dB 20Hz-20kHz, albeit with 0.4dBV of insertion loss.

Pin 1 on each XLR socket is connected to a ground pour on layer 2 of the PCB, as are the XLR chassis grounding pins.  The board should be mounted in an all metal case.

A brief short circuit, even with a high voltage applied, should be OK. The 500R/3W input resistors can take a steady state 75Vrms indefinitely ($75V^2/1000R=5.6W$), although the inductors will clearly saturate unless a reasonable load is connected to limit the current to 800mA. The 140Vrms requirement into more reasonable loads is met by the voltage ratings of the capacitors. In the case of C1 and C2 this provides a rated 500Vpp between signal rails.

# Build tips

You will need to add your own switch to the build for the voltage divider. If not using this then jumper the connections. The BOM doesn't include the pin headers for the pad-down resistor switch connections, presumably if you are building this then you have those already.

If you want to use different components then you will need to ensure you model them from scratch, e.g. if changing the inductors you will need to model the changed series resistance.  

Case choice is up to you, as cost and availability varies a lot by location, but it must be all metal with all parts connected electrically.  A Hammond 1444-862 is a good choice!

# BOM

* R1,R2 500ohm 5W 1% [https://au.mouser.com/ProductDetail/Vishay-Draloric/PAC500005000FAC000?qs=sGAEpiMZZMtlubZbdhIBINC8EtgKGA%2F6i50rchkXbHU%3D] 25x6.4mm
* R3,R4 330ohm 0.25W 1% [https://au.mouser.com/ProductDetail/TE-Connectivity-Holsworthy/LR0204F330R?qs=sGAEpiMZZMtlubZbdhIBIBiJIqAHDdPFB0JYbliHBqI%3D] 3.5x2mm
* R5,R6 68ohm 1W 1% [https://au.mouser.com/ProductDetail/Vishay-Dale/CMF6568R000FKR6?qs=sGAEpiMZZMtlubZbdhIBIOM2%252B0H92tq20Jgq%252BUaQb4k%3D] 14.5x4.5mm
* R7,R8 3.3k 0.25W 1% [https://au.mouser.com/ProductDetail/KOA-Speer/MF1-4DCT52R3301F?qs=2ayCeu1TP5leCJLPVwsFCg%3D%3D] 3.5x2mm
* R9,R10 22ohm 0.25W 1% [https://au.mouser.com/ProductDetail/TE-Connectivity-Holsworthy/LR1F22R?qs=sGAEpiMZZMtlubZbdhIBIFho3SHfDXStNhO7PeBAY30%3D] 6.2x2.3mm
* R11,R12 2k7 1W 1% [https://au.mouser.com/ProductDetail/YAGEO/MFR1WSFTF52-2K7?qs=xZ%2FP%252Ba9zWqZMHNDyFGZwSA%3D%3D] 9x3.3mm
* R13,R14 1k8 1W 1% [https://au.mouser.com/ProductDetail/YAGEO/MFR1WSFTF52-1K8?qs=xZ%2FP%252Ba9zWqYtbloc%2Fjj38w%3D%3D] 9x3.3mm
* C1,C2,C3,C4,C5,C6 220p 250VDC 2.5% [https://au.mouser.com/ProductDetail/WIMA/FKP2J002201D00HSSD?qs=WI052HLiD5hzJhR9QL5EAQ%3D%3D] 7.2x4.5mm, 5mm pin
* C7,C8,C9,C10 4.7n 250VDC 5% [https://au.mouser.com/ProductDetail/WIMA/FKP2F014701I00JA00?qs=WI052HLiD5hCY8ty427gtg%3D%3D] 7.2x6.5mm, 5mm pin
* L1,L2,L3,L4 1u 1ohm 800mA [https://au.mouser.com/ProductDetail/Wurth-Elektronik/7687480102?qs=sGAEpiMZZMv126LJFLh8ywQ8%252ByAE4SZkiWIazUoK6Rs%3D] 10.5mm dia, 5mm pin
* J1 XLR female NC3FAAH2 [https://au.mouser.com/ProductDetail/Neutrik/NC3FAAH2?qs=jCymNF74TgW4A7eq9uxuLg%3D%3D]
* J2 XLR male NC3MAAH [https://au.mouser.com/ProductDetail/Neutrik/NC3MAAH?qs=jCymNF74TgXgh2x3smwFTg%3D%3D]
* J3,J4,J5 test connectors (https://au.element14.com/cliff-electronic-components/fcr7350r/socket-pcb-4mm-r-a-s16n-pc-red/dp/1854508)

