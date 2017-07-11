# Repetier
Firmware source for my RepRap Kossel based OREO DELTA 3D Printer.

I made this project after getting inspired from http://reprap.org/wiki/Kossel , It was cool. I loved the triangulation (a real work of robotics. :D )


# The parts
The beams are from open beam http://www.openbeamusa.com/ most of the BOM like timing belts, screws and nuts, wires,  SMPS and connectors etc from SP road. Initially got the joints printed by Ankit jain on his 3D printer, my colleague in Zynga, since it was made from a prusa mendel it was not that great. Had lot of hard fits and misfits and was a hectic job to get levels and fittings correct. So had to get a custom injection mould parts from china via an Ebay seller also sourced other parts like heated glass and stepper motors. 


SP road stepper motors were off and were useless, so had to get all the motors of same stepping. tried all the boards from the RAMPS to sanguinololu and arduino based other stuffs, used pololu stepper motor drivers which I brought along with prakash another colleague in Zynga.The final one that worked was a geeetech arduino based board clone, from china made by micro make with all stepper motor drivers. it works fine with their firmware. which is why we need to stick to CURA which is opensource. basically we can configure and use any slicer since the firmware is basically Marlin from Reprap.http://reprap.org/wiki/Marlin 


# The software
Firmware : https://github.com/repetier/Repetier-Firmware 


I forked the https://github.com/Ultimaker/cura-build almost a year ago. you can check there for internals, basically it's a glorified plotter, the cura just slices the 3d CAD based on the movement of Z axis of my printer and passes the xyz coordinates via G-Code.couple of issues are there. nothing major. dont tweak any of the settings, it took me a lot of time to get this printer working on MAC. 

The firmware used is https://github.com/repetier/Repetier-Firmware with tweaks local to length of my XYZ and specific tuning to my end stops and limit switches.


# Setup
* install Cura-15.04.0822-MacOS.dmg
* download CH341SER_MAC.ZIP, run CH34x Install.pkg and reboot so that It will install the  serial port driver, didnt work for me without reboot.
* Open Cura, choose Micromake D1 that's what our chinese mother board is during installation. we can change this later in settings, find the “Machine” menu, and choose “Install default Firmware”.
  
* The baudrate of serial port can be set to 115200 required by the chinese firmware 
  
* If you can’t connect with the printer, you can open Cura->Machine->Machine setting, ensure the Baudrate is 115200, then see the pull-down list of serial port, see if it can recognize the port, such as dev/cu.<someport>

# Heated Bed
Enable heated bed in machine settings as below
  
Push the changes via firmware to the board. --> #define HAVE_HEATED_BED 1

there is an issue when the heatbed heats up to 60Deg. the power cable heats up. suspecting a load issue. MOSFETs or power source ampere.

# Rod changes 217mm
// Delta settings
#if DRIVE_SYSTEM==DELTA
/** \brief Delta rod length (mm)
*/
#define DELTA_DIAGONAL_ROD 217 // mm
Firmware changed openDACT for repetier
  

#Beeper - not sure.
#define BEEPER_SHORT_SEQUENCE 1,1
#define BEEPER_LONG_SEQUENCE 2,2


# Z_PROBE Parameters need tweaking
#define Z_PROBE_X1 0
#define Z_PROBE_Y1 65
#define Z_PROBE_X2 64.95 (not sure)
#define Z_PROBE_Y2 -37.5
#define Z_PROBE_X3 -64.95 ( TODO try to set it around 30 and see if the centre oart filament issu is compnetsated)
#define Z_PROBE_Y3 -37.5
Microstepping correction
/** \brief Steps per rotation of stepper motor */
#define STEPS_PER_ROTATION 200

/** \brief Micro stepping rate of X, Y and Y tower stepper drivers */
#define MICRO_STEPS 32 (TODO: need to see the manual ad figure out the correct value, not happy iwth this)


# TODO
~~* Firmware upgrade~~
~~* Belt tension issues~~
* SD card printing issues.
* Heating issue board cable gauge to be increased.
* Print bed levelling issues need better solution than paper clips
~~* enable openDACT~~~
~~* Belt slip issue redesign. (add washer)
* nozzle heating thermistor control issue
* multiple fan setup update.
* end switches leveling~~
* power conversion issues. 
* Z_PROBE parameters off
* microsteping correction
* filament spool reprint and attachment
* power outage resume print fails
~~* enable open DACT and try.~~
* heatbed heating power wire.
* z axis fine tuning
* FIX IDE connection issues
* CURA memory issue on mac
~~* Arduino memory issue
* x axis end point correction.
* resolution correction >250 dpi~~
* Beeper silencings
~~* hot end design modification ( rostock delta)
* Heated bed heating mechanism (ADD to firmware)  firmware changes~~
* CURA update 15.0.05~~

# Print head leveling wizard
If the nozzle not level when printing, the 3 Axis limit switch height is incorrect setup.
Adjust the limit switch adjustment to level the nozzle using below method.
Reduce the value for the lower axis, increase the value for the higher axis.
Recommend value to adjust is 30 each time, one axis at a time.
Once level is complete, click "Save" button, and then "Reset" button, and restart the printing.
Continue adjustment until the print result is prefect.
