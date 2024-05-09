
Online design tool at https://ergogen.cache.works/.

Elite-C https://deskthority.net/wiki/Elite-C footprint from ceoloide: https://github.com/ceoloide/ergogen-footprints


Running ergogen:

```
cd handlebar
ergogen . -o output
```

Pass a folder instead of an input file name because handlebar has custom footprints and that's how this works.


### Parts

- TRRS PJ-320A jack, 2 parts (Keebio)
- Elite-C v4, 2 parts
- Diodes 1N4148W, SOD-123, 50 parts
- Choc v1 switches
- DMK keycaps, 1U
- Push buttons -model-, 2 parts
- USB-C/USB cable
- RGB LEDs, QMK compatible (SK-6812?) X parts (Keeio)
- TRRS cable
- PCBs, 2 parts
- To make the MCUs replaceable
  - Mill Max sockets, for 2 MCUs
  - Pins

### KiCAD 

Tutorial: https://flatfootfox.com/ergogen-part5-kicad-firmware-assembly/

We are better to use constraints from makers. JLCPCB seems to have less capability than others. I found a Kicad board with JLC's capability set. See `fab/JLCPCB_1-2Layer template`.

Steps after ergogen has been executed:
- Import board settings from JLCPCB_1-2Layer template. File/Board Setup/Import from another board. Turn User.Drawings back on.
- add the resistors footprints, R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal
- complete the net connections for the resistors
- add a via below the indicators for VCC
- wire keys pin 1 (2 of them) to diode 2 on the back
- wire columns on the back
- route columns around the top and through the middle of the MCU, starting with n_inner
- wire rows on the front: they enter the MCU from the left
- wire reset, place it on the right
- wire VCC on the left of reset *and* go from the second resistor to the via on the *back*
- wire ground, going between n_vcc and n_scl on the TRRS connector, up on the left of VCC
- wire ground to all ground pins on the MCU
- indicator grounds
- indicator "next" and to the MCU

### Fabrication

Prepare Gerber files:
- File/Fabrication/Gerbers
- Check the layers
- Plot
- Generate drill files

Upload to JLCPCB (https://jlcpcb.com/)

PCB: material is FR-4.
Thickness? Default at JLC is 1.6mm.

Capacité technique, Le Labo: https://www.golabo.com/fr/services
- Min ligne: 0.076mm (0.003")
- Espace min entre 2 cuivres: 0.076
- Sérigraphie min: 0.102mm (0.004")


### Notes

https://flatfootfox.com/ergogen-part3-pcbs/

This has example of adding a PCB via: https://github.com/ergogen/ergogen/blob/675d2709dc4c2515ec34ecf4e5148127495283f3/config.yaml

PCM12 is a sliding switch. (no need it seems)

b3u1000p is a small reset push button. https://www.allaboutcircuits.com/electronic-components/datasheet/B3U-1000P-B--Omron/

This looks close to what I am trying to do: https://github.com/davidphilipbarr/Sweep/tree/main/Sweep%20half-swept

Redox schematic https://github.com/mattdibi/redox-keyboard/blob/master/redox/pcb/Redox-schematic.pdf

# Checklist

- [x] MCU position on the PCB matches the one in the preview.
- [x] Through holes are the correct size for my components.
- [x] Fit between the diode and the switch.
- [x] Pull up 4.7KΩ resistors, SDA to VCC, SCL to VCC
- [x] Must I²C be connected to those two specific pins? D0 and D1? Yes, let's do that.
- [ ] Should I ground all unused pins?
- [x] Are the back diodes pads connected to the traces on front? No and I added them.
- [x] Decide on PCB thickness: 1.6mm
