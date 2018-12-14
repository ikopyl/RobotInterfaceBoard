### Robot Interface Board

The Robot Interface Board (RIB) is a breakout board for the [ESP32-DevKitC Arduino module](https://www.digikey.com/product-detail/en/espressif-systems/ESP32-DEVKITC-32D/1965-1000-ND/9356990). The Dev Kit C is a reference design of the ESP32-WROOM-32 module which houses the WiFi and bluetooth Antenae. The ESP32-WROOM-32 module houses an ESP32 microcontroller. 

#### Datasheet

Arduino compatiple DevkitC : https://esp-idf.readthedocs.io/en/latest/get-started/get-started-devkitc.html

ESP32 Microcontroller: https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf




## PCB

![Image of  Board](docs/board.png)

## ESP32Dev Board PINMAP

![Pin Functions](docs/esp32_pinmap.png)

## Pins to Never Use:

0       - This is the boot pin, if held low during a reset it will prevent the device from booting

6-11    - These pins are hookups for external system flash and are used by the system flash. Unless you are extending the chips flash capibilities, do not use these. 

1 and 3 -  these are used to program the device and are serial port pins.

2 is a Strapping Pin on GPIO

## Output Only Pins
```
13 
15 
5
```
## Input Only pins

34-39 are input only. They have no output modes at all. Analog input and digital input are availible.
```
35
34
39
36
```

## Availible Input/Output

```
4
12
14-19
21-23
25-27
32-33
```
## External Use pins

1 and 3 are the serial port used for programming and print statements. This should not be used for user functions, but can be used if serial functionality is needed.

22 and 21 have 4.7kOhm pullups on them and are connected to the Wii accessory port. These can be used with other i2c devices.
See: https://github.com/acamilo/RobotInterfaceBoard/issues/31

## Availible Servo/PWM/AnalogWrite Pins
```
4
5
12
14-19
21-23
25-27
32-33
```

### Timer distribution for PWM
The ESP32 has 4 timers availible for use in PWM generation. All PWM channels using a given timer have the same base frequency. Changing the frequency of a channel will change the frequency of other channels. Note the chart for how the PWM channel allocations coorospond to timers:

```
/*
 * LEDC Chan to Group/Channel/Timer Mapping
** ledc: 0  => Group: 0, Channel: 0, Timer: 0
** ledc: 1  => Group: 0, Channel: 1, Timer: 0
** ledc: 2  => Group: 0, Channel: 2, Timer: 1
** ledc: 3  => Group: 0, Channel: 3, Timer: 1
** ledc: 4  => Group: 0, Channel: 4, Timer: 2
** ledc: 5  => Group: 0, Channel: 5, Timer: 2
** ledc: 6  => Group: 0, Channel: 6, Timer: 3
** ledc: 7  => Group: 0, Channel: 7, Timer: 3
** ledc: 8  => Group: 1, Channel: 0, Timer: 0
** ledc: 9  => Group: 1, Channel: 1, Timer: 0
** ledc: 10 => Group: 1, Channel: 2, Timer: 1
** ledc: 11 => Group: 1, Channel: 3, Timer: 1
** ledc: 12 => Group: 1, Channel: 4, Timer: 2
** ledc: 13 => Group: 1, Channel: 5, Timer: 2
** ledc: 14 => Group: 1, Channel: 6, Timer: 3
** ledc: 15 => Group: 1, Channel: 7, Timer: 3
*/
```

When using ESP32PWM objects, changing the frequency will check for timer cross-talk. If you re-set the frequency of a PWM indirectly, the object will print this warning:

```
	WARNING PWM channel 1 shares a timer with 0
	changing the frequency to 330.00 Hz will ALSO change channel 0 
	from its previous frequency of 50.00 Hz
```

If you get this warning you can space out the attach events of the hardware using ESP32PWM using dummy objects. To consume a channel without activating a pwm, you can add this to your code between attach method calls. 

```
ESP32PWM dummy;
dummy.getChannel();
```
## Availible DAC pins
```
25
26
```
These pins when used with analogWrite will produce an 8 bit analog value on the given pin. The value is from 0-3.3v mapped to 0-255 values. The api is to simply use analogWrite().



## Interrupt

For code examples: https://techtutorialsx.com/2017/09/30/esp32-arduino-external-interrupts/

Availible interruptable pins:
```
0-39
```

# Development Computer Options  

## Option 1) A lab machine

Availible to all students. 

Note that Eclipse *should be installed by you* in your My Documents folder.  Each install of eclipse should be personal and not shared.

Note that the driver is installed on these computers

Note that Arduino with the ESP32 toolchain is already installed in C:\WPIAPPS\arduino-1.8.3\

##  Option 2) Personal Machine

### Supported for this class

Windows 10 Pro 

A user name with no " " in the file path. Generally it is safe to use your WPI username as the username on your computer.

Fresh install is genearlly reccomended every 6 months. Install disk are availible to students as a resource from the WPI Helpdesk.

OneDrive and Dropbox conflict with the install process and must be fully removed from the user file paths. 

Eclipse should be installed in C:\eclipse

Arduino should be installed in C:\RBE-arduino

After installation of both, ensure your user has write access to the directories. 

### Unsupported OS's

MacOS is unsupported and only intermittantly working. Drivers have been an issue with programming our board, and virtualization of Windows within OSX is tested non-working. If you have Mac OSX please install a fresh copy of Windows 10 nativly and dual boot. 

Ubuntu 16.04 is unsupported but works well.

Ubuntu 18.04 is unsupported but works well with some creative directions following. This will take more effort and would require pre-existing proficency in Linux. If you have 18.04 please install Windows 10 or 16.04. 

### How to get Windows 10 as a Student for free from WPI

To get your student copy of Windows go here: 

https://onlinestore.wpi.edu/

Select windows 10, and download it. For students you get one copy and it is free. 

You can follow this tutorial to install it:

https://www.youtube.com/watch?v=aTVOTY93XXU

# Arduino and the ESP32 Toolchain

## Driver

This is installed on the lab machines already. 

After extracting the Zip file, install the 64 bit version of the driver. 

https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers


## Personal Computer install Windows  (Supported)
download:

https://github.com/WPIRoboticsEngineering/RBE2002_template/releases/download/0.0.1/RBE-arduino110718.zip

And extract it on your computer in 

```
C:/RBE-arduino
```
Run Arduino in the extracted folder.

Make a sketchbook folder in C:/, or (R:/ on the lab machines) make a folder:

```
C:/Sketchbook/
```

Make sure it is user level read-write

Open the Preferences in Arduino and set the Sketchbook location to:

```
C:/Sketchbook/
```

## Personal Computer install Linux / Mac  (Unsupported)

Linux Instructions (16.04 works well, 18.04 is a bit fiddely and needs extra steps but can be made to work):

https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/debian_ubuntu.md


Mac instructions (NOT SUPPORTED BY THE LAB, HAS LOTS OF PROBLEMS):

https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/mac.md


# Arduino Libraries
## HOWTO
For detailed instructions on how libraries work, see: https://www.arduino.cc/en/Guide/Libraries

Open Arduino and select Sketch->Libraries -> Manage Libraries
## Which Libraries

### 1001

ESP32Servo

ESP32Encoder
### 2001
Search for and install:

ESP32Servo

ESP32Encoder

Esp32SimplePacketComs

SimplePacketComs

PID

### 2002
Search for and install:

ESP32Servo

ESP32Encoder

Adafruit_BNO055

BNO055SimplePacketComs

Esp32SimplePacketComs

SimplePacketComs

WiiChuck

DFRobotIRPosition

PID
