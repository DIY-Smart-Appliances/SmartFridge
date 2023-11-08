# Basic home fridge with DIY smart addition with an ESP32Cam

This code is for a standard fridge with a fridge section above and a freezer section below modified to become a smart fridge with an ESP32Cam.

The modifications are:
- take out the old thermostat and the temperature probes for the fridge and the freezer
- Fit the ESP32Cam in the light and thermostat compartment
- mount the camera so that it "looks" into the fridge
- take the power from the cables going to the lamp before the switch
- connecting the door opening sensor to the ESP32Cam input pin
- connecting the door lamp to power through a relay connected to the ESP32Cam
- connect a DHT22 inside the fridge to the ESP32Cam
- connect a DHT22 inside the freezer to the ESP32Cam - For this you can use the path to the old temperature sensor
- connect the compressor relay to the ESP32Cam

Since in most freezer compartments there is no door opening sensor because there is no light the door opening sensor has to be fitted-in somehow. The cables can follow the same path as the temperature sensor, but then it will be necessary to make some modifications to the freezer door to fit a reed relay.
