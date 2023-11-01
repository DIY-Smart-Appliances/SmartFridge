# SmartFridge
Software for a smart fridge based on ESP32 boards and ESPHOME software.

To "describe" a fridge there are "fridge templates" describing their physical components, like doors, compressors, sensors, and lights, in an abstract way.

The code will be written in modules corresponding to different fridge templates.

## Templates
Templates are used to describe how a fridge is physically made.

The model is that a fridge has doors, as you see them from left to right, with a / to separate door groups from top to bottom.

Each door closes a "cellar".

### Doors

A fridge door is indicated with an uppercase D, and a freezer door with a lowercase d.

If the letter is enclosed in parenthesis, it indicates that the door is behind the previous door. This happens with freezer compartments in small fridges.
A lowercase i after the letter indicates an ice dispenser.

### Sensors, lights, cameras and actuators

A fridge can have sensors, lights, cameras, and actuators serving each cellar.

Sensors can be:
- temperature (T, C or F)
- humidity (H, RH in %)
- door (open, close)
- dispenser (idle, dispensing)

There can be more than one temperature or humidity sensor in each cellar, but only one door sensor and one dispenser.

Each cellar can have zero or more lights. If present they are indicated with L1 to Ln. (on, off)

Each cellar can have zero or more cameras. If present they are indicated with C1 to Cn. (inside view, door view)


Each cellar can have one actuator to start the cooling in the cellar. Each actuator has an associated compressor and sometimes an air valve.

The actuator is on or off, and it turns on the associated compressor and, if present, the air valve. This setup works with systems with one compressor for fridge and freezer (combined compressor) and with separate compressors for each function.

Each cellar can have an actuator for a fan.
