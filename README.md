# Goals

The goal is to make a cheap DIY passive filter for use in class D amp measurements, much like the AP AUX-0025.  The AP AUX-0025 specs as read off the graph on the AP website are approximately:
* -20dB @ 150kHz
* -40dB @ 200kHz
* -60dB @ 250kHz
* 20-40kHz deviation < 0.1dB

We also want:
* Typical RMS voltage 2.83V (8V P-P) for standard "1 watt" amp performance measurements
* Max P-P voltage 100V (35V/300W RMS into 4 ohms) into any load, limited by the power ratings of the components.  Into loads >10k it should be good up to 200V or more, limited by the voltage ratings of the capacitors.

# Design discussion

The filter design is inspired by the AUX-0025 but differs in a few ways.  The component values are more standard, and are reused across the design to reduce the number of different components needed.  Capacitor and inductor tolerances are 5% instead of 2.5%.  A switchable set of 8.2V protection diodes is added to ensure the measurement device doesn't get overloaded, as is a switchable voltage divider for high voltage measurements.  There is also a set of test jacks for connecting a multimeter to monitor true RMS output.

Pin1 on each XLR socket is connected to a ground pour on the back of the PCB.  There are no other connections to the ground pour.  The chassis is floating, as the chassis ground pins on the XLR sockets are unconnected.

# Build tips



# BOM

R1,R2 500ohm 3W (https://au.element14.com/rcd-resistors-coils-delaylines/135-5000-fbw/wirewound-resistor-500-ohm-3w/dp/1614476)
R9,R10 22ohm 1W (https://au.element14.com/vishay/ac01000002209ja100/res-22r-5-1w-axial-wirewound/dp/1735039)
R7,R8 3.3k 1W (https://au.element14.com/multicomp/mcf-1w-3k3/res-3k3-5-1w-axial-carbon-film/dp/9337865)
R5,R6 68ohm 1W (https://au.element14.com/te-connectivity/cfr100j68r/res-68r-5-1w-axial-carbon-film/dp/2329469)
R3,R4 330ohm 1W (https://au.element14.com/vishay/mbe04140c3300fc100/resistor-metal-film-330r-1w-axial/dp/4140145)
C1,C2,C3,C4,C5,C6 220p 250V (https://au.element14.com/wima/fkp2o102201d00jssd/cap-220pf-1-kv-5-pp-through-hole/dp/1519285)
C7,C8,C9,C10 4.7n 700V (https://au.element14.com/epcos/b32652a2472j000/cap-4700pf-2kv-film-radial/dp/3518977)
L1,L2,L3,L4 1u 1ohm 800mA (https://au.element14.com/wurth-elektronik/7447480102/inductor-1000uh-5-10-5x10-5mm/dp/2211736)

J1 XLR female NC3FAAH2 (https://au.element14.com/neutrik/nc3faah2/socket-xlr-pcb-horizontal-3pole/dp/131002301)
J2 XLR male NC3MAAH (https://au.element14.com/neutrik/nc3maah/plug-xlr-pcb-horizontal-3pole/dp/1310044)
J3,J4 Test connectors (https://au.element14.com/cliff-electronic-components/fcr7350r/socket-pcb-4mm-r-a-s16n-pc-red/dp/1854508)