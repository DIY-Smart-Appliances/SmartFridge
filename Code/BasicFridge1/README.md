# Basic home fridge with DIY smart addition with an ESP32Cam

This code is for a standard fridge with a fridge section above and a freezer section below, each with its own compressors, modified to become a smart fridge with an ESP32Cam.
It also adds a display. Here is the hardware used to smartify the fridge:
## Features Implemented
### Sensors
- Dallas DS18B20 temperature sensors (fridge + freezer)
- SHT41 humidity sensors (fridge + freezer)
- Door sensors (fridge + freezer)
- HLW8012 power metering for both compressors
### Actuators
- Fridge compressor relay
- Freezer compressor relay
- Fridge light relay
- Camera ESP32‑CAM module providing internal fridge camera stream
### HMI
20×4 I²C LCD display that Shows real‑time operational data, including:
- Fridge temperature
- Freezer temperature
- Fridge compressor power consumption
- Freezer compressor power consumption
- Current control mode (HA or internal)
## Control Logic
### Mode selection:
- Home Assistant control
- Internal logic

If the mode is Home Assistant you have to implement an HA automation to turn on and off the compressor. This opens the possibility of implementing "smart" logic.
If the mode in Internal the ESP32 handles the temperature control

Internal thermostat logic:
- Setpoint + hysteresis for fridge and freezer
- Minimum compressor off‑time protection
- Passive defrost (natural, no heater)
### Fallback
If Home Assistant API is unreachable, internal logic automatically takes over.

## Security
You have to user secrets.yaml to define API and OTA keys. If you import this code in the ESPHome estension in HA chances are that you already have them.

## Hardware modifications

The modifications are:
- take out the old thermostat and the temperature probes for the fridge and the freezer
- Fit the ESP32Cam and its power supply in the light and thermostat compartment
- mount the camera so that it "looks" into the fridge
- take the power from the cables going to the lamp before the switch; you will probably need a 220VAC to 5VDC power module
- connect the door opening sensor to the ESP32Cam input pin
- connect the door lamp to power through a relay connected to the ESP32Cam
- connect temperature and humidity sensors inside the fridge to the ESP32Cam
- connect temperature and humidity sensors inside the freezer to the ESP32Cam - for this, you can use the path to the old temperature sensor
- connect the compressor relay to the ESP32Cam

Since in most freezer compartments there is no door opening sensor because there is no light, the door opening sensor has to be fitted in somehow. The cables can follow the same path as the temperature sensor, but it will probably be necessary to make some modifications to the freezer door to fit a reed relay.

## Hardware
- ESP32‑CAM
- 220VAC to 5VDC power module
- 2× Dallas DS18B20
- 2× SHT41
- 2x reed relays and small magnets as door opening sensors, unless you can reuse the original one for the fridge door. Then you need only one reed and magnet.
- 2× Compressor relays
- 1× Fridge light relay
- 1× HLW8012 module (fridge compressor)
- 1× HLW8012 module (freezer compressor)
- 20×4 LCD PCF8574 I²C display
For the relays, you could also use a 3/4 relay module instead of one module per relay.

## Tools
A Dremel or something similar can come in handy.
Some water-repellent spray can also be useful. The main problem for fridges and freezers is the presence of water vapor in the cellar. For the ESP32-CAM and its power supply, probably the best choice is to put them in a box, with a hole for the camera. For the display, you will usually have to cut the fridge door, find a way to fit it in, and mask the cuts with a frame. It must also be close to the ESP32 and the SHT41, since the I2C bus must be short. If too difficult, avoid the display. After all, you see everything with HA.
