

### Ergogen

Online design tool at https://ergogen.cache.works/.

Elite-C https://deskthority.net/wiki/Elite-C footprint from ceoloide: https://github.com/ceoloide/ergogen-footprints

Running ergogen:

```
cd handlebar
ergogen . -o output
```

Pass a folder instead of an input file name because handlebar has custom footprints and that's how this works.


### Parts

- Choc v1 switches (Keebd)
- DMK keycaps, 1U (Keebd)
- Elite-C v4, 2 parts (Omega Keys)
- TRRS PJ-320A jack, 2 parts (Clickety Split)
- TRRS cable (Clickety Split)
- Diodes 1N4148W, SOD-123, 50 parts (Digikey 4530-1N4148WCT-ND)
- Push buttons FSMCTTR, 2 parts (Digikey 450-2133-1-ND)
- RGB LEDs FZ2812-5050, QMK compatible (WS2812B) (Digikey, 4544-FZ2812-5050CT-ND)
- MCU socket 801-43-064-10-002000 (Digikey, ED10864-64-ND)
- PCBs, 2 parts
- USB-C/USB cable

The Atmega32u4 on the Elite-C has 32KB flash, 2.5KB SRAM, 16MHz CPU.

#### Base

Mild steel plate, 1/16", 5/64" or 3/32" thick. 1.5mm to 2.5mm thick. 200x150mm or 8"x6", two times.

We plan to either solder the standoff or screw them.

- 8mm long, M2 standoff, 6 times
- dome head M2 screws, 6mm of thread, 6 times
- dome head M2 screws, 4mm of thread, 8 times
- flat head M2 screws, 6mm full length, 6 times
- flat head M2 screws, 10mm full length, 8 times
- M2 nuts, 8 times


### Cost

Elite-C at Omega Keys, 24$ each.
Keycaps and switches, 114$
PCB, JLCPcb, 
TRRS cable
Digi-key

#### Not used, extra
Switches, 13$
TRRS cable, Keebd, 7$ (not used)

### KiCAD 

Tutorial: https://flatfootfox.com/ergogen-part5-kicad-firmware-assembly/

We are better to use constraints from makers. JLCPCB seems to have less capability than others. I found a Kicad board with JLC's capability set. See `JLCPCB_1-2Layer template`.

*DO NOT* edit the elite-c footprint as this will introduce a lot of errors! The errors are introduced because the nets that were setup by Ergogen are lost during the footprint edition.

Steps after ergogen has been executed:
- Import board settings from JLCPCB_1-2Layer template:
  - File/Board Setup/Import from another board. 
  - Turn User.Drawings back on.
  - Change track width to 0.25mm in Net Classes
- add the resistors footprints, R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal
  - under the TRRS
  - do not show the reference
- change footprint of indicators to Personal:WS2812B
- (useful: Route/Interactive Router Setting, try Highlight + Free angle or Walk around)
- complete the net connections for the resistors on front of board (VCC on the right, SDA at the top, SCL at the bottom)
- add a via below the indicators for VCC
- wire keys pin 1 (2 of them) to diode 2 on the back
- wire diode pads to holes on the front and back
- wire columns on the back
- route columns around the top and through the middle of the MCU, starting with n_inner
- wire rows on the front: they enter the MCU from the left
- wire reset, place it on the right
- wire VCC on the left of reset *and* go from the second resistor to the via on the *back*
- wire ground, going between n_vcc and n_scl on the TRRS connector, up on the left of VCC
- wire ground to all ground pins on the MCU
- wire SDA from TRRS to the MCU on the back
- indicator grounds
- indicator "next" and to the MCU

Optional:
- give a little bit of space around pads and traces
- add the name and version on the silkscreens


### Fabrication

Prepare Gerber files:
- File/Fabrication/Gerbers
- Check the layers
  - *.Cu, *.Paste, *.Mask, *.Silkscreen, Edge.Cuts
- Set a clean, new, empty directory
- Plot
- Generate drill files
- Zip the output files, flat, no directory

Upload to JLCPCB (https://jlcpcb.com/)

PCB material is FR-4. Use the defaults (1.6mm). Remove JLC's serial numper. Pick your color. 


### Notes

Elite-C information:
- https://deskthority.net/wiki/Elite-C
- Development history, reveals a lot about how it works: https://blog.keeb.io/case-of-the-wayward-elite-c/
- Atmega34U4, Atme AVR

https://flatfootfox.com/ergogen-part3-pcbs/

This has example of adding a PCB via: https://github.com/ergogen/ergogen/blob/675d2709dc4c2515ec34ecf4e5148127495283f3/config.yaml

PCM12 is a sliding switch. (no need it seems)

b3u1000p is a small reset push button. https://www.allaboutcircuits.com/electronic-components/datasheet/B3U-1000P-B--Omron/

This looks close to what I am trying to do: https://github.com/davidphilipbarr/Sweep/tree/main/Sweep%20half-swept

Redox schematic https://github.com/mattdibi/redox-keyboard/blob/master/redox/pcb/Redox-schematic.pdf


# Hardware Checklist

- [x] MCU position on the PCB matches the one in the preview.
- [x] Through holes are the correct size for my components.
- [x] Fit between the diode and the switch.
- [x] Pull up 4.7KΩ resistors, SDA to VCC, SCL to VCC
- [x] Must I²C be connected to those two specific pins? D0 and D1? Yes, let's do that.
- [x] Should I ground all unused pins? No. And there is no need to cover the unused space with ground copper.
- [x] Are the back diodes pads connected to the traces on front? No and I added them.
- [x] Decide on PCB thickness: 1.6mm
- [x] What is the voltage needed by the RGB LEDs? 3V, so VCC is good. Crossed fingers.
