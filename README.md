# Mendelmax 1.5 With MKS base v1.5 (RAMPS 1.4) -Klipper0.10.0
 sainsmart Coreception 300  with Klipper. This will hold general installation and config

1. Get printer and setup with Bltouch in Z+ endstop spot . 
2. get Debian Pc/laptop/rpi/orangepi/ect/ect
3. install OS (i chose debian 11 and used KIAUH to install klipper/mainsail/moonraker)
4. compile klipper for your mcu which should be a mega 2450 board on stock coreception. 
5. run "make menuconfig", setup for the mcu mega, stock settings work great. 
6. run "make"
7. "make flash" works fine. upload to board if you have USB setup. 
8. copy over printer.cfg which contains all pinouts to control printer as well as macros for calibration and setup
9. [if your using ubuntu and cannot find your MCU path](https://unix.stackexchange.com/a/674936)
9. [verify all printer components and tune extruder/Bed PID](https://github.com/Klipper3d/klipper/blob/master/docs/Config_checks.md) 
10. [Setup Extruder Steps (if Not Stock)](https://www.klipper3d.org/Rotation_Distance.html)
11. [Setup ZOffset/probe calibrate](https://github.com/Klipper3d/klipper/blob/master/docs/Probe_Calibrate.md)
12. testprint
13. [Setup pressure advance](https://github.com/Klipper3d/klipper/blob/master/docs/Pressure_Advance.md)
14. [Every Pres_adv change retune retraction (click here for FW_retract tuning which i recommend)](https://www.printables.com/model/236366-klipper-stringing-test-with-firmware-retraction)
15. [setup the Rpi to ALSO be a secondary MCU for resonance testing](https://www.klipper3d.org/RPi_microcontroller.html)
16. [Check your Resonance](https://github.com/Klipper3d/klipper/blob/master/docs/Resonance_Compensation.md)
17. [Check resonance to check belt tension on coreXY machines](https://www.ifixit.com/Guide/Adding+ADXL345+Accelerometer/147745)
18. testprint
19. [setup Start and end Gcodes (also check the included ones)](https://allpro3d.com/quick-tip-start-and-end-gcode-in-klipper/)
20. enjoy

## some commands used to get everything setup 
sudo apt-get install git -y
cd ~/klipper/
make menuconfig
make
ls /dev/serial/by-id/*
sudo service klipper stop
cd ~
git clone https://github.com/th33xitus/kiauh.git
./kiauh/kiauh.sh

i ended up using spi 5 and needed these commands on my Rpi to active spi5
dtoverlay=spi5-1cs
dtparameter=cs0_pin=12

PID_CALIBRATE HEATER=extruder TARGET=170
PID_CALIBRATE HEATER=heater_bed TARGET=60

SHAPER_CALIBRATE

~/klipper/scripts/calibrate_shaper.py /tmp/calibration_data_x*.csv -o /tmp/shaper_calibrate_x.png
~/klipper/scripts/calibrate_shaper.py /tmp/calibration_data_y*.csv -o /tmp/shaper_calibrate_y.png

TUNING_TOWER COMMAND=SET_RETRACTION PARAMETER=RETRACT_LENGTH START=0 FACTOR=0.05 #this test will test retract length from 0 to 5 on a 20mm tower

##sources
[ramps pinout for mks 1.5](https://github.com/makerbase-mks/Klipper-for-MKS-Boards/tree/main/MKS%20Gen%20l)
1. [copied a lot from this](https://www.reddit.com/r/coreception/comments/peyx17/fluidd_config_for_klipper_guide_and_also_just/)

2. [beanisfat leaves a comment i use](https://www.reddit.com/r/coreception/comments/nhtl3p/klipper_tmc2208_config_for_stock_printer/)

3. [more config discussion inclusing making 2208 uart and coreception](https://www.reddit.com/r/coreception/comments/k619b1/klipper_on_elfcoreception/)

4. [GREAT klipper print tuning guide i found after doing all this](https://github.com/AndrewEllis93/Print-Tuning-Guide#extrusion-multiplier)

5. [if your using ubuntu and cannot find your MCU path](https://unix.stackexchange.com/a/674936)
