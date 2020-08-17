## IPOCS Model Railway

IPOCSMR is an Open Source Software implementation of an IP based interface to control Model Railway (MR) objects such as points/switch motors, lamps and detectors. To effectively control an MR, a system with distributed electronic units are needed to control objects and obtain statuses, an Object Controller System (OCS).  Hence the system name IPOCSMR. 
IPOCSMR is designed using ESP8266 as the WiFi link to the object controllers and Arduino UNO to realize the direct object control. The software together with the described hardware form units similar to DCC decoders, but at a significantly lower cost and higher flexibility/capability.
Controlling an MR is not only a matter of having an efficient and cost-effective OCS, but also a central commas system. JMRI is such a system and an interface utilizing MQTT to connect IPOCSMR and JMRI has also been developed.
In a system each OC consists of an ESP8266, an Arduino UNO and a hardware interface to the objects. At deployment each OC is given a unique ID and the SSID/PW for the specific MR. This is set using a web server page in the ESP. If not configured the ESP starts as an AP and is connected to by connecting a computer to that AP.
 
IPOCSMR is fully IP and WiFi based and once the ID is set all interactions are done via WiFi and even software updates in the ESP8266 and Arduino UNO are done over the air (OTA).

![Image](images\IPOCS_System_structure.png)

**Features of IPOCSMR**
* WiFi based, less cables throughout the MR
* Based on COTS low cost hardware
* Interfaces directly to JMRI
* One hardware for all purposes, outputs/inputs/servos.
* Open software, use as is or modify/add yourself.

### Work in progress

This site is a work in progress...
