# Horizon Library Convention

Because Horizon's part management is based around a single pool, all parts need to follow a clear convention for a consistent look and feel. If you want to add a part to the official Horizon pool, check this convention first before submitting a pull request.

Note that, however, this convention is a *very* rough draft and far from complete. Suggestions for rules are very welcome!

## Overview and general rules

### File names
1. File are adapted versions of the pool entry's name.
2. Do not use spaces in file names. Use a dash `-` instead. In general, use only characters from the following sets:
    - Alphanumeric characters (`a` to `z`, `A` to `Z`, `0` to `9`)
    - Underscore `_` and dash/hyphen `-`
    - Period/dot `.`

### General naming and organisation
1. All file names, descriptions, etc. must be in English
2. Avoid plural forms. For example, a folder with diode library entries should be called `diode` and not `diodes`.

## Parts
1. Parts should be named for the exact MPN (manufacturer part number). Use the exact spelling from the "ordering information" section of the device's datasheet.
2. The `Value` field does not necessarily contain the full MPN. It should be clear from the value what the part is without occupying too much space in the schematic. For example, for a general purpose resistor the resistance is generally enough for the schematic and a `INA219AIDCNR` can be shortened to `INA219A`
3. Include sensible information like a description. Use the manufacturer's website for the datasheet link and not a third-party datasheet site.
4. Use inheritance for only slightly differing parts (like different packaging, temperature range, etc.).

## Packages

### Naming and folder structure
1. For a manufacurer-specific footprint, use the respective subfolder in `manufacturer`. Use the exact name of the package from the package or device datasheet.
2. For generic footprints, do not include a dash after the package type (`TO`, `DIP`, `LQFP`, ...): Use `DO41` or `TO220-5` instead of `DO-41` or `TO-220-5`.
3. When dimensions appear in the file/package name, the unit must be specified. Prefer metric units when appropriate.
    - For the package name, place a space before the unit, like `LED 5 mm`
    - The file name must not include spaces, e. g. write `SO8_3.2x5.7mm_P1.27mm`

### Silkscreen
1. All silkscreen text and drawings should have a line width of 0.15 mm. Text should have a size of 1 mm.
2. The silkscreen layer must contain a reference designator (`$RD`). This should be the only text on the silkscreen layer.
3. Silkscreen must not intersect with pads or package. All silkscreen has to be visible after assembly.
4. Silkscreen text and drawings must have a clearance of 0.2 mm to package outline and copper layer.
5. A pin 1 indicator is established by extension of an existing outline or by omitting a line. Do not use a dot or text or any other marking.

### Courtyard
1. The courtyard polygon is the hull around package body and pads. This means that at a courtyard expansion if 0 mm, the courtyard polygon touches the outermost pad outlines/package outlines.
2. The courtyard polygon must be parametrised by the courtyard expansion parameter with a parameter program.
3. The courtyard must not intersect with itself at any courtyard expansion. If this happens, the courtyard polygon is probably too complex and can be simplified.

### Package layer
1. The package layer has to contain the physical size of the part as a polygon or shape.
2. Further annotations must not be added.
3. Use a line width of 0 mm.

### Assembly layer
1. The assembly layer is similar to the package layer in that it is based on the physical outline of the part.
2. Use a line width of 0 mm for drawings and text.
3. The assembly layer must include a pin 1 designator in the form of a bevelled corner (if pin 1 is in a corner) or a triangular "dent" (if pin 1 is on an edge). This marking should be 1.2 mm.
4. Include a text `$RD`.

### Copper
1. Use the recommended footprint from the manufacturer's device or package datasheet.
2. If there are multiple recommendations, e. g. for different soldering methods, create alternate packages.

## Padstacks

If you create a package, chances are that you don't need a new padstack, as the existing general padstacks are parametrised. If you do need to create a new padstack, take the following rules into account:

1. If the package you're creating requires a padstack for a special pad geometry, the JSON file should be placed in the package's `padstacks` directory and not in the root `padstacks` directory. The latter is reserved for generic padstacks and shouldn't be cluttered with rarely-needed padstacks.
2. Draw the necessary shapes/polygons in all layers, not just the copper layer.
3. Use parameter programs to make the padstack as generic as possible. As a minimum, solder mask expansion and paste mask contraption must be parametrised.

## Entities
1. Name the entity for the most general part it applies to. For example, do not create a entity `ATtiny24` which is implicitly also used for the ATtiny44 and ATtiny84 microcontrollers. Instead, use a name like `ATtinyX4`.

### Prefixes (reference designators)
| Prefix | Symbols                                   |
| ------ | ----------------------------------------- |
| A      | Sub-assembly or plug-in module            |
| AT     | Attenuatur, isolator                      |
| B      | Blower, Motor                             |
| BT     | Battery                                   |
| C      | Capacitor                                 |
| CB     | Circuit breaker                           |
| CN     | Capacitor network                         |
| D      | Diode, zener diode, TVS diode, DIAC       |
| DC     | Directional coupler                       |
| DL     | Delay line                                |
| DS     | Display, lamp                             |
| F      | Fuse                                      |
| FD     | Fiducial                                  |
| FL     | Filter                                    |
| G      | Generator, oscillator                     |
| H      | Hardware (mounting screws, etc.)          |
| HY     | Circulator                                |
| J      | Connector                                 |
| J      | Jumper, link                              |
| K      | Relay, Contactor                          |
| L      | Inductor, coil, ferrite bead              |
| LS     | Loudspeaker, buzzer                       |
| M      | Meter                                     |
| MG     | Motor-generator                           |
| MH     | Mounting hole                             |
| MK     | Microphone                                |
| MP     | Mechanical part (SMD spacer, etc.)        |
| PS     | Power supply                              |
| Q      | Transistor, thyristor, TRIAC              |
| R      | Resistor                                  |
| RN     | Resistor network                          |
| RT     | Thermistor                                |
| S      | Switch                                    |
| T      | Transformer                               |
| TC     | Thermocouple                              |
| TP     | Test point                                |
| U      | Integrated circuit, inseparable assembly  |
| V      | Electron tube                             |
| W      | Wire, cable, cable assembly               |
| Y      | Crystal, ceramic oscillator               |

## Symbols
1. Symbols must have one text `$REFDES` and one `$VALUE`. They both should be sized 1.5 mm.
2. Use names as generic as possible (cf. entities).
3. All pin connection points must be on the 1.25 mm grid and at the outside of the symbol.

### Discrete components


### General symbols (ICs, etc.)
1. Group pins by function, not by pin number. For example, a LED driver's SPI pins should be placed next to each other, even if they are far apart on the physical device.
2. Make sure that pin names do not collide. Consider all alternate pin names.
3. Use pin decorations (clock, inverted, etc.) only for digital pins.
4. Do not use a "inverted" decoration for pins whose name already indicates inversion (`n` or `/` in front, overbar, etc.)
5. The symbol must have a border around it. the `$REFDES` text is to be placed above the border, `$VALUE` below. All other text must be within the border.

## Units
1. Use the pin names exactly like they are written in the device's datasheet.
2. Assign electrical functions to the pins according to the device datasheet.
3. List all alternate pin names. For example, microcontrollers' pins often have a lot of alternate functions, which should all be listed here.
4. Do not include annotations for the pin names from the datasheet like footnotes or other markings with a special meaning only explained in the datasheet.