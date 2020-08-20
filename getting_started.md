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

### Prerequisits

* JMRI
* MQTT broker

### Configuring JMRI

#### MQTT broker

This one is really up to you - any broker will do. JMRI does require it to be
running when it is started.

#### The system connection

To connect IPOCSMR to JMRI you need to set up a MQTT connection in JMRI.
The receive topics for each type of object you intent to use must be seperate
from the send topics. For turnouts, for example, the receive topic could be
`track/turnout/{0}/state` and the send topic `track/turnout/{0}`.
If `{0}` is at the end of a topic, it can be omitted. `{0}` will be replaced by
JMRI during operation for the system name of the turnout (or other object).

#### Configuring objects

##### Turnout

A turnout should be configured as `MONITORING` to allow it to be updated from
the status sent by IPOCSMR.

##### Other objects

No special settings are needed.

### Protocol translation gateway

IPOCSMR uses a [binary protocol][ipocs] over wifi, not MQTT. Because of this a
translation service needs to be running on a machine. We call this service
`IPOCS.JMRI`. This is a self-contained .NET Core application and should thus run
just about anywhere.

You can download it off it's [GitHub Releases][ijr] page.

As for configuration, it need the IPOCSMR configuration file (described later),
the JMRI profile for the system it will translate for, which panel file is to
be used and the address to the MQTT host.

This data then goes into the `appconfig.json` file located inside the
distribution.

```json
{
  "jmriProfile": "./",
  "jmriPanelFile":  "panel.xml",
  "ipocsConfig": "config.xml",
  "mqttHost": "localhost"
}
```

You can then start it by running the `IPOCS.JMRI` executable (how you do this
depends on your OS).

[ijr]: https://github.com/ipocsmr/ipocs.jmri/releases
[ipocs]: https://github.com/ipocsmr/documentation/blob/master/IPOCS.md

#### Object controller configuration

To create the IPOCSMR configuration, use the ipocs_dpt software, obtainable
from it's repositories [GitHub Releases][dptrel] page.

[dptrel]: https://github.com/ipocsmr/ipocs_dpt/releases

The ipocs_dpt (Data Preparation Tool) will be explained in more detail in the future.

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

##### First boot

When the ESP does not have a network configured or cannot connect to the
previous one it will, after a timeout of about 30 seconds, start a softAP. If you connect to this AP
and enter `192.168.4.1` into your browser, you will come to a configuration
portal for the system. This portal is available later as well, but the IP will
depend on the network you connect it to, as it will be obtained using DHCP.

The minimum to configure here now is UnitID, the boards identity, and network
settings. The Site Data will be automatically updated when it connects to
IPOCS.JMRI.

