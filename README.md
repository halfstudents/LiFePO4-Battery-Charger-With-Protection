# LiFePO4-Battery-Charger-With-Protection
Single cell 1A battery charger, works with LIFEPO4, in the same form factor of the legend TP4056. CC and CV modes with protection features.
See the full deatils from here: https://www.instructables.com/LiFePO4-Battery-Charger-With-Protection/
Order PCBs from PCBWAY: https://www.pcbway.com/project/shareproject/LiFePO4_Battery_Charger_With_Protection_e1b29de4.html


In the previous few months I tried to cover almost all types of battery charging integrated circuits. Nowadays I am working with LiFePo4 which are basically lithium iron phosphate batteries. Compared to the old lithium ions they are safer because of the type of chemistry used. Although their energy density is almost halved of lithium ions. But a typical lithium ion can handle a typical 700-1000 cycles on the other hand these lifepo4 batteries are extremely durable and go up to 3000-5000 cycles. Which is good for long term usage, such as consumer electronics related to toys, medical equipment and all.

I got my hands on a battery from orange, it is 3C rated. Yes it is a little costlier side around 2x a simple lithium ion of same specs. Although I got the same 18650 form factor with a 2000mAH capacity. The C rating 3C here describes the max allowable changing and discharging current. So it looks very similar to our Lithium ion battery and so does our designed charger module.

The charging and discharging characteristics are basically common for this one and lithium ion, but we can’t simply put the same charging module in the TP4056. Because that one is not compatible with the voltage, LiFePO4 has a max changing voltage of 3.6Volts and base voltage is around 3.2 Volts. On the other hand, the lithium ion one has full charge voltage 4.2Volts and rated 3.7V base. This project is sponsored by PCBWAY. I ordered the PCBs from there. They look perfect, I have shared all the information below. It is easy to make project consider if you are looking for a good LiFePo4 Charger.

CN3058:

While searching the web, I found this IC for LiFePo4 charging. Charging modes are the same as TP4056 which will be discussed in the next few sections. CN3058 is from the same company I designed previously for NiCd/NimH but that was based on CN3085. A little different choice for name, yet this IC does everything which is needed to protect and charge the battery safely.  The device contains an on-chip power MOSFET and eliminates the need for the external sense resistor and blocking diode. The regulation voltage is internally fixed at 3.6V with 1.5% accuracy, it can also be adjusted with an external resistor. Some are listed below:

Input voltage range from 3.8V to 6V
On-chip Power MOSFET
No external Blocking Diode or Current Sense Resistors Required
Preset 3.6V Regulation Voltage with an external resistor
Programmable Continuous Charge Current Up to 1A
Constant-Current/Constant-Voltage/Constant Temperature
Automatic Low-Power Sleep Mode
Status Indication for LEDs or uP Interface
C/10 Charge Termination
Automatic Recharge  Battery Temperature Sensing

It is listed for 1A, but while testing I found it a little hot around the IC even at 700mA. So I reduced the current and saved my IC, btw it is always considered to use a small heatsink with it.

Circuit Diagram:
The circuit looks so clean and simple to understand in a go, here we have input capacitor C1, If using a type C keep it typically around 10uF and some more in parallel for better decoupling. A resistor which connected to CHARGE and DONE LED turned on when any of the 6,7 number pins pulled to low. Then VIN and Ground tab for power, as with USB, the input voltage is around 5v within the limit of this IC. The feedback pin has a resistor Rx, if using a single cell this can shorten directly. Otherwise we have to calculate as per the battery voltage, for example if we want to increase the charging voltage beyond a point.

Given by the equation: Vbat ＝ 3.6＋3.61×10-6×Rx

If Rx is chosen 10K then the battery charging voltage will be: 3.6 + 0.00361. Yes it will not affect much, although you never want to change the charging voltage because it is internally calibrated, so we will not use this function in our circuit.

After this we have Iset Resistor to set the maximum current which is given by this equation:
ICH = 1218V / RISET

If RISET is around 1218 Ohms it will charge at 1A. But I set it to 2K to keep the current a little low, because of heating issues as discussed earlier. The 2000 ohms will set the charging current to 600mA. At this point no need to use any kind of heatsink or any cooling device. The IC also has temperature sensing options which are usable in some harsh condition regions. But no need to use this here with this simple module.

Modified Circuit:

I changed the circuit to fit on the board, I do not want to unnecessarily make it more complex. As it is only for a single cell, that is the only one mode I am using with this. The bigger the BOM the higher the cost. So I used a minimal yet full protection featured one to do the job. Here is the circuit diagram I am using, always using different color LEDs for charge indication.

Battery Charging Modes:

The CN3058E uses a unique architecture to charge a battery in a constant-current and constant-voltage.  The charge cycle begins when the voltage at the VIN pin rises above the under voltage lockout level, a current set resistor is connected from the ISET pin to ground. The CHRG pin outputs a logic low to indicate that the charge cycle is ongoing. At the beginning of the charge cycle, if the voltage at FB pin is below 2.5V, the charger is in precharge mode to bring the cell voltage up to a safe level for charging. The charger goes into the fast charge constant-current mode once the voltage on the FB pin rises above 2.5V.
In constant current mode, the charge current is set by RISET. When the battery approaches the regulation voltage, the charge current begins to decrease as the CN3058E enters the constant-voltage mode. When the current drops to charge termination threshold, the charge cycle is terminated,is pulled low by an internal switch and CHRG pin assumes a high impedance state to indicate that the charge cycle is terminated. The charge termination threshold is 10% of the current in constant current mode. To restart the charge cycle, remove the input voltage and reapply it. In constant current mode the charge current delivered to the battery equals 1218V/RISET. If the power dissipation of the CN3058E results in the junction temperature approaching 135℃, the amplifier Tamp will begin decreasing the charge current to limit the die temperature to approximately 135℃. I will try to replicate the module behaviour using a simple power supply in the next article through which we get more command over constant current and voltage modes.

PCB Designs:

I designed the PCB and shared the design with PCBWAY. It is one of the finest PCB fabricators till 10 years old. It will fulfill all the basics to advance prototyping requirements. I tried to match the form factor to the TP4056 as it sets a benchmark in the single cell charging market. It is the most sold module till date.

https://www.pcbway.com/project/shareproject/LiFePO4_Battery_Charger_With_Protection_e1b29de4.html

I put a type C connector with two 5.1K resistors at CC pins to make it compatible for Type C chargers. This PCB design can be found with all the required files on PCBWAY hub. To assemble the PCBs I am not using stencil for now, but if found difficult PCBWAY also supplies that with PCBs. I gathered all the components and soldered them one by one with my hands. If working with SMT, it is better to have a zoom lens plugged into your camera to see those small pins. Soldering errors might be there!

Testing and Working:

After soldering and assembling the main thing to check if it is working as an aspect or not. To do that we need to check the open circuit voltage at the Vbat terminal. Don’t attach the battery and see the voltage, it should be 1.55-1.65 volts. If not there should be any design error. Btw mine board and circuit is fully tested with Lifepo4, 2000mAH battery. After getting the right voltage on the output terminal, attach the battery and see if it is switching the mode to charging through the onboard indicator.

I attached the battery to a holder and then wired two simple wires from module to battery. Use a type C connector and it works in the first go. If you have any kind of USB meter device available you can test the module more easily. Moreover If you like the content and then join me on patreon. ( https://www.patreon.com/c/THEIITBOY ).  Try PCBWAY services to turn your prototypes into real products.

https://www.pcbway.com/project/shareproject/LiFePO4_Battery_Charger_With_Protection_e1b29de4.html
