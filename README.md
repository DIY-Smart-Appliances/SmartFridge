# SmartFridge
Software for smart fridges based on ESP32 boards and ESPHOME software.

To "describe" a fridge there are "fridge templates" describing their physical components, like doors, compressors, sensors, and lights, in an abstract way.

The code will be written in modules corresponding to different fridge templates.

# Fridge Template Description

## Overview
Templates are used to describe how a fridge is physically made.

The model is that a fridge has doors, as you see them from left to right, with a `/` to separate door groups from top to bottom.

Each door closes a "cellar".

Templates are made by starting with the upper left door, followed by the list of its cellar components enclosed in `[ ]`.

For easier parsing doors are separated by a comma, and the same applies for cellar components.

---

## Examples

A simple fridge with a freezer compartment:  
`D(d)[T,D,L,A]`

A common fridge with a freezer in the lower part:  
`D[T,H,D,L,A]/d[T,H,D,L,A]`

A more complex fridge with two upper doors, one with an ice dispenser and the other with a touch screen display:  
`D[T,H,D,d,A,C],DH4[T,H,D,A,C1,C2]/d[T,H,D,A]`

---

## Doors

- A fridge door is indicated with an uppercase `D`.
- A freezer door is indicated with a lowercase `d`.
- If the letter is enclosed in parentheses `()`, it indicates that the door is behind the previous door (e.g., a small freezer compartment inside a fridge).
- A lowercase `i` after the letter indicates an ice dispenser (e.g., `Di` or `di`).

---

## Sensors, Lights, Cameras, and Actuators

A fridge can have sensors, lights, cameras, and actuators serving each cellar.

### Sensors

| Symbol | Meaning | Data Type |
|--------|---------|-----------|
| `T`    | Temperature | Degrees C or F |
| `H`    | Humidity   | RH in % |
| `D`    | Door state | binary (open/close) |
| `d`    | Dispenser state | binary (idle/dispensing) |

- There can be more than one temperature or humidity sensor in each cellar (e.g., `T1`, `T2`).
- There is only one door sensor and one dispenser per cellar.

### Lights
- Each cellar can have zero or more lights.
- Indicated with `L1` to `Ln` (on/off).
- A single light may be indicated with just `L` instead of `L1`.

### Cameras
- Each cellar can have zero or more cameras.
- Indicated with `C1` to `Cn` (inside view, door view).
- A single camera may be indicated with just `C` instead of `C1`.

### Cooling Actuator
- Each cellar can have one actuator to start cooling.
- Each actuator has an associated compressor and sometimes an air valve.
- **At least one cooling actuator must be present** in the whole fridge.
- Some fridges (especially small ones, under-counter, or camper models) may not have a dedicated cooling actuator for the freezer — the freezer stays below 0°C passively when the fridge section is at its upper temperature limit (≤ 8°C).
- The actuator is on/off and turns on the associated compressor and, if present, the air valve.
- Works with single-compressor systems as well as separate compressors for fridge/freezer.
- Indicated with `A`. If more than one compressor exists, use `A1`, `A2`, etc.

### Fan Actuator
- Each cellar can have a fan actuator.
- Indicated with `F`.

---

## Active Defrost

A cellar may have an **active defrost** system, typically found in freezer compartments and sometimes in the fridge section (especially in frost-free or no-frost models).

- Active defrost is indicated with the symbol `R` (for **R**esistive heater or defrost **R**elay) inside the cellar's component list.
- It represents a heating element or reverse-cycle valve that periodically melts frost buildup on the evaporator.
- If the defrost system includes its own dedicated sensor (e.g., a defrost termination thermostat or temperature probe), it is indicated as `Tr` in addition to the regular `T` sensor.
- A cellar can have at most one active defrost system.

### Active Defrost Examples

| Description | Template |
|-------------|----------|
| Freezer with active defrost and temperature sensor | `d[T,R,D,L,A]` |
| Freezer with active defrost, defrost sensor, and fan | `d[T,Tr,R,D,L,A,F]` |
| Fridge section with active defrost (less common) | `D[T,H,R,D,L,A]` |

If a cellar does **not** have active defrost, the `R` symbol is simply omitted.

---

## HMI (Human-Machine Interface)

The fridge can have an interface to display status or to set operating parameters.

- Presence of an HMI is indicated with an `H` after the `D` or `d`.
- The number after `H` indicates the HMI type (see below).
- **Important:** A `+` after the number indicates the presence of buttons. If there is no `+`, the HMI has **no buttons**.

### HMI Types

| Number | Type |
|--------|------|
| 1 | Simple RGB LED (no buttons) |
| 2 | LCD Matrix display with buttons |
| 3 | LED or OLED display with buttons |
| 4 | LED or OLED touch screen (buttons not applicable) |

### HMI Examples

| Description | Template |
|-------------|----------|
| Fridge door with HMI type 4 (touch screen, no separate buttons) | `DH4[...]` |
| Fridge door with HMI type 2 (LCD + buttons) | `DH2+[...]` |
| Freezer door with HMI type 3 (OLED + buttons) | `dH3+[...]` |
| Freezer door with HMI type 3 (OLED, no buttons — just display) | `dH3[...]` |
| Fridge door with simple RGB LED (type 1, no buttons) | `DH1[...]` |

---

## Summary of Component Symbols

| Symbol | Meaning |
|--------|---------|
| `D` / `d` | Door (fridge / freezer) |
| `i` | Ice dispenser (after door letter) |
| `()` | Door behind another door |
| `T` | Temperature sensor |
| `H` | Humidity sensor |
| `D` | Door sensor (inside component list) |
| `d` | Dispenser sensor (inside component list) |
| `L` / `L1...Ln` | Lights |
| `C` / `C1...Cn` | Cameras |
| `A` / `A1...An` | Cooling actuator(s) |
| `F` | Fan actuator |
| `R` | Active defrost system |
| `Tr` | Defrost temperature sensor |
| `H` + number | HMI interface (after door letter) |
| `+` | Buttons present (after HMI number) |

---

## Complete Template Structure

DOOR_LETTER[HMI?][COMPONENT_LIST]/DOOR_LETTER[HMI?][COMPONENT_LIST],...

Where:
- `DOOR_LETTER` = `D` (fridge) or `d` (freezer), optionally with `i` for ice dispenser or `()` for behind-another-door
- `HMI?` = optional `H` followed by a type number (1–4) and optional `+`
- `COMPONENT_LIST` = comma-separated list of symbols from the summary above, enclosed in `[ ]`
- Multiple doors are separated by `,` (left-to-right on same level)
- Door groups are separated by `/` (top-to-bottom)
