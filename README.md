![](RackMultipart20201111-4-iavl9m_html_1968544ca5893979.png)

# _Rainwater Tank Real Time Monitoring – Civil Defence_

**Submitted by** :

Harjeev Sharma (2208358)

October 2020

**Supervised by:**

_Frank Beinersdorf_

_Induka Werellagama_

Submitted as the Final Report for the MG7101 Engineering Development Project

for the Bachelor of GradDipEng Programme Mechatronics Major

# Abstract

A considerable portion of the world&#39;s populations still solely depends on rainwater for drinking water. A few studies, conducted over a limited number of tanks for a limited time frame, concluded that many times water quality of rainwater tanks fail to comply with the drinkable water standards. In fact, there is no precise data available when it comes to rainwater tanks.

This problem can be addressed using the implementation of the Internet of Things concept. This will help get the accurate real-time data of some dominant parameters of water e.g. pH and Electrical Conductivity (EC). This paper highlights all the technological platforms used to set up client and server, challenges in achieving it, calibration of sensors, and what type of information can be deduced by water experts from these parameters. In addition to data gathering, an SMS alert service is used so that users can take appropriate actions instantaneously.

# Project Design

I was motivated to work closely with the real-life scenario and technologies used to accomplish the task, though it was a prototype of a concept. This project is implemented around the concept of Internet of Things (IoT). Major steps involve reading sensors&#39; values, uploading readings to a server, storing the values in a database, and show results on a webpage. Based on the functionality this project was divided into two parts – Client and Server.

**5.1 Client:**

Here, the ESP32 microcontroller acts as the controlling unit that establishes a communication channel with a WiFi network, reads the sensor values, uploads values to the server, and sends alert messages when needed. Client measures the following parameters of the rainwater tank.

| **S. No.** | **Parameter** | **Sensor/ Module** |
| --- | --- | --- |
| 1 | pH (Potential of Hydrogen) | Gravity Analog pH Sensor kit DFRobot |
| 2 | Electrical Conductivity | Electrical Conductivity Sensor (Cell Constant 1 cm -1 ) |
| 3 | Pressure | BMP180 |
| 4 | Temperature | DS18B20 |
| 5 | Luminous | TEMT6000 |

**Client Development: -**

- The power source of this section is a 12V, 2A adapter, or Two 18650 Li-ion rechargeable batteries (series connection). At this section, two different voltages were required - 5V for ESP32 Kit and sensors and 4V for the SIM-800L GSM module. This was achieved by 7805 Voltage regulator and a Buck-Boost converter tunned at 4 V. DPDT (Double-Pole Double-Throw) Relays were used to switch between battery and adapter.
- For the Programming of ESP32, Arduino Framework was used because there were a lot of libraries and support available online. Below is the list of libraries used and their role:

| **S. No.** | **Library \&lt;library.h\&gt;** | **Function** |
| --- | --- | --- |
| 1 | \&lt;Wifi.h\&gt; | Interface inbuilt WiFi module of ESP32 Kit with WiFi connection (……Mode) |
| 2 | \&lt;PubSubClient.h\&gt; | MQTT server communication and upload data |
| 3 | \&lt;Wire.h\&gt; | Interface Pressure Sensor (BMP180) on I2C bus |
| 4 | \&lt;OneWire.h\&gt; | Interface Temperature Sensor (DS18B20) on One Wire bus |
| 5 | \&lt;DallasTemperature.h\&gt; | Read Temperature sensor and return Temperature in Celsius |
| 6 | \&lt;SoftwareSerial.h\&gt; | Develop serial communication on Digital pins and interface with SIM-800L GSM Module. |

- Using the above libraries Pressure Sensor, Temperature, and pH Module were interfaced.
- Light Sensor (TEMT6000) has a linear relation between luminous and signal voltage. It was interfaced using the Analog pin of ESP32.
- SIM-800L GSM Module works on AT Command Set. The following are the commands that were used to configure the module into text mode.

| **S. No.** | **AT Command** | **Function** |
| --- | --- | --- |
| 1 | AT | Handshake with Module |
| 2 | AT + CMGF =1 | Configure in text mode |
| 3 | AT+CMGS=&quot;+64273279722&quot; | Mobile Number of Text Message |
| 4 | Decimal Code 26 | Send Message |

- Based on water parameters, an alert message using the GSM technology is sent to the user on a predefined mobile number.

**5.2 Server:**

- A Raspberry Pi board acts as an MQTT broker, time-series database, and graphical user interface. This board runs on Raspberry Pi OS, a Linux distro, and the following packages were installed to meet the requirements.

| **S. No.** | **Package** | **Application** | **Port** |
| --- | --- | --- | --- |
| 1 | Mosquitto | MQTT Broker | 1883 |
| 2 | InfluxDB | Time Series Database | 8086 |
| 3 | NodeRed | The interface between Broker and Database | 1880 |
| 4 | Grafana | The graphical interface of the database | 3000 |
| 5 | NoIP | DDNS (Dynamic Domain Name System) service | -- |

- Once the above installations were working, then to get the services in the public domain. Some setting in the home&#39;s modem was done. These included:

1.     DHCP:  To assign a fixed local IP (192.168.1.xx) to Raspberry Pi.

2.     Port Forwarding: To forward public IP&#39;s ports to Raspberry Pi&#39;s port.

3.     DDNS: To provide a DDNS name to the modem.

- Once modem and No-IP were configured, a Dynamic Update Client (DUC) was run on Raspberry Pi to keep updating the DDNS name with a frequency of 5 minutes.




