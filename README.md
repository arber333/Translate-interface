# Translate-interface
Small interface board to connect existing Johannes rev. 2 board with Ampera inverter directly

I have designed a PCB that holds connectors for driver stage as well as current sensor cables. Also there is a built in isolated voltage sensing side that controls precharge and DC contactor. This interface is meant for Johannes mainboard either Rev. 1 or Rev. 2.
In rev. 2 there is already Fault sensing pin that can sense driver fault output and shutdown PWM in that case. Ampera signal is open collector type and it is pulled to GND in case of normal operation. If there is a fault, signal floats for 5us approximately. On my interface board this signal is pulled up to 5Vdc by 4K7 and fed to NPN transistor base. In case signal floats (Failure) base sees 5V from pullup resistor and NPN pulls mainboard Fault pin to GND which in software is actual Fault condition.

Inverter chip SN74LS06N is used to invert signals. Ampera drivers operate from inverted signals. That means 0V will turn IGBT on and 5V will shut it down. That is why i had to include this chip. I could opt to select open collector outputs in software and run signals directly from Olimex, but i decided to use interface because i wasnt sure drivers would see 3V3 signal reliably.

There is also provision to sense temperature from Ampera IGBT module. Regular heatsink temp pin from mainboard is connected to temp signal from IGBT. This is just a 5K thermistor connected to GND on one side. So i have to pull up the other side of sensing pin in order to make the signal reading proportional to temp change.

Isolated voltage reading is exactly the same as johannes new sensor board.

Current sensor connector signals are pulled to GND by 4K7 to remove EMI and there is a 1uF cap for better supply. Also mainboard has to be modified a bit. I add one pulldown 6K8 resistor per channel AFTER 3K3 inline resistor to prepare signal to 1.67V median reading instead of 2.5V usual. This conforms better to 3V3 Olimex chip.

First iteration was meant only to transfer signals and to experiment. It still used old rev. 2 sensor board. However i found out that i could just transfer sensor board functions without large op-amp. And best of all system works without negative attributes to current gain and values.
