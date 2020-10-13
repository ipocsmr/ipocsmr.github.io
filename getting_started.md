## Getting started

### A note before beginning

Writing a comprehensive guide on how to set up this system is not the intent of
this page. Instead, see this as a primer on how the system works. We can't
teach you all the things required - but we can try to explain what we think you
need to know.

So, with that in mind, to be able to set up this system you will probably need
at least a basic understanding of electronics and arduino development.
Note though that we do not use the Arduino IDE, but rather PlatformIO.

This page will take a top down approach to describing what you need to do to get
the system up and running. We will assume that you will integrate with JMRI.

If you feel that something is missing, could be clarified or should be changed
in another way - please file a ticket on one of our repositories.

### Prerequisits

* JMRI

### Configuring JMRI

#### The system connection


Starting with JMRI 4.21.4 IPOCS has a system connection in JMRI that you can use
to connect the concentrator units to JMRI.

By default, mDNS/ZeroConf/Bonjour is used to tell the concentrator units where
to connect to. Changeing the port is optional, but possible if you have
something else already using the default port. 

#### Configuring objects

When creating an object, the JMRI User Name is the name IPOCS uses to address
objects. Make sure that the IPOCS name aligns with the JMRI User Name, other
than that - there's nothing special to configure. 

#### Object controller configuration

To create the IPOCSMR configuration, use the ipocs_dpt software, obtainable
from it's repositories [GitHub Releases][dptrel] page.

[dptrel]: https://github.com/ipocsmr/ipocs.dpt/releases

The ipocs.dpt (Data Preparation Tool) will be explained in more detail in the future.

### Hardware

We now support both a single ESP32 unit or a ESP8266 and an Arduino working as a
pair.
The ESP32 has mostly the same capabilities as the ESP8266/Arduino combination.

#### Preparing a ESP32

Download and flash, an initial version of IPOCS. Most ESP32 controllers come
pre-loaded with an application to allow flashing over WiFi. If your's doesn't - 
connect the USB and flash that way.

Note: Flash the filesystem (spiffs) first as this is required for the
configuration portal to work.

#### Preparing a ESP8266/Arduino pair

##### Arduino Bootloader
First off, you will need to upgrade the Arduino Bootloader to Optiboot v8 if
this has not already been done. You will get older versions to work, but OTA
updates will fail.

##### Initial flash

Download and flash, over serial, an initial version (suggest the latest available
at the moment) of the ipocsmr software for both Arduino and ESP - be sure to also flash the ESP file
system or the on-board configuration website will not work.

##### Connecting the boards

Assuming you've done all the wiring of your objects to the Arduino Uno or Mega,
you also need to connect the first serial ports of the ESP and Arduino to allow
them to communicate with eachother during runtime.

### First boot

When the ESP does not have a network configured or cannot connect to the
previous one it will, after a timeout of about 30 seconds, start a softAP.
If you connect to this AP and enter `192.168.4.1` into your browser, you will
come to a configuration portal for the system. This portal is available later as
well, but the IP will depend on the network you connect it to, as it will be
obtained using DHCP.

The minimum to configure here now is Concentrator Unit Name, which is the board
identity, and networ settings. The Site Data will be automatically updated when
it connects to IPOCS.DPT.

Note: The system connection in JMRI does not currently support automatically
updating the onboard Site Data. It is strongly suggested that you use IPOCS.DPT
to test your new layout configuration before using JMRI.
