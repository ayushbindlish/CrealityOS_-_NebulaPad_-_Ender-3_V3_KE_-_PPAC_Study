
# Nebula Pad - Port USB-C - Pinout

<!--
https://www.lesimprimantes3d.fr/forum/topic/56116-creality-ender-3-v3-ke-la-d%C3%A9couverte-avant-le-test/?do=findComment&comment=580697
-->

On the Discord server https://discord.gg/d3vil-design

I found the pinout of the USB-C port (where to connect the g-sensor) on a NebulaPad
(but I still have to understand how to interpret the "1/2" entries in the "USB-C" column...)


https://discord.com/channels/1154500511777693819/1194193938962186301/1194196216649625651 from user "wmvos"

> USB-C port on the underside is actually a SPI interface with the following pinout
> 
~~~
|  USB C     | ADXL345 | Nebula Pad PCB |
| ---------- | ------- | -------------- |
| VCC        | VCC     | 2x V-regulator |
| GND        | GND     | GND            |
| D-  1/2    | SDA     | DT             |
| D+  1/2    | SDO     | DR             |
| CC  1/2    | SCL     | CLK            |
| SBU 1/2    | CS      | CE             |
~~~
> With just a cable plugged in there were also voltages across the A/B 2,3,10 & 11 ranging from 0.001 V and 3,4 V so some of those are definitely connected to something, although I couldn't immediately find what. So be careful with plugging in other stuff.
> 
> Working setup:
> 
> Nebula Pad > Thunderbolt Cable > 6/12-pin breakout board > dupont cables > ADXL345
> 
> I made a remix of @Derrick Darrell's mounting clip so it can accommodate the ADXL345 board with pin headers soldered on, it can be found here. (modifié)
> 

 

and a photo showing the pinout of a g-sensor
(but I don't know if this is standard or varies between g-sensors, so check before use)

https://discord.com/channels/1154500511777693819/1194193938962186301/1194199344597979256 from user "destinal"
<!--
ADXL345-CR-pinout.webp.fca17e731e4610e1cd5d99d90424d507.webp
-->
![image](https://github.com/PPAC37/CrealityOS_-_NebulaPad_-_Ender-3_V3_KE_-_PPAC_Study/assets/94939582/d5660055-bec3-4e57-b368-c9b0a087592c)
