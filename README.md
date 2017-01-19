CmdrArduinoMS
===========

CmdrArduinoMS is a fork of Don Goodman-Wilson's [CmdrArduino](https://github.com/Railstars/CmdrArduino) DCC command station library 
designed for use with the [Arduino Motor Shield R3](https://www.arduino.cc/en/Main/ArduinoMotorShieldR3).  

Due to the Motor Shield's pinout, this only works with the Mega, as it's the only Arduino with
timer compare outputs on the right pins.

Just like the original, CmdrArduinoMS is GPLv3.

Functionality
----------
CmdrArduinoMS uses timer 1 to create a logic-level DCC waveform on pin OC1C
(otherwise known as PB7 or Arduino digital output 13).  This, conveniently,
lines up with the "Direction B" input on the Motor Shield R3, which allows
you to put a track-ready DCC signal out on the B side of the motor shield.

Output Compare module C only exists on some AVR processors (not on the
ATMega328 used on the Uno), and the ATMega2560 used on the Arduino Mega 2560 is unique in placing it on the
right output pin to hit the motor shield input.

To make this work, you need to pay attention to Arduino digital I/O D8. 
This is the "brake" input for the B channel of the motor shield, and can
screw things up if it's used for something else.  You either need to make it
an output and set it low, or if you want to use it for other purposes, cut
the "BRAKE B DISABLE" bridge on the motor shield.

You also need to make digital i/o D11 an output.  Setting D11 high will turn
on the DCC signal, and setting it low will turn off the DCC signal.

The current being drawn off channel B of the DCC shield will come in on
analog input A1, as long as the SNS1_ENABLE bridge is intact.  Given the
pulsed nature of most decoders' motor drivers, you'll want to sample and
average this value.  I typically average 64 values.

The analog input is 1.65V/amp of load, so assuming a 5V reference for the
AVR's ADC reference, that's 337 counts per amp of load. 

milliamps = (analogRead(A1) * 1000) / 337

Installation
----------

To install, see the general instructions for Arduino library installation here:
http://arduino.cc/en/Guide/Environment#libraries

