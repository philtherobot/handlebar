
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

https://flatfootfox.com/ergogen-part5-kicad-firmware-assembly/

### Fabrication

PCB: material is FR-4.
Thickness?

https://www.golabo.com/fr/services

### Notes

https://flatfootfox.com/ergogen-part3-pcbs/

This has example of adding a PCB via: https://github.com/ergogen/ergogen/blob/675d2709dc4c2515ec34ecf4e5148127495283f3/config.yaml

PCM12 is a sliding switch. (no need it seems)

b3u1000p is a small reset push button. https://www.allaboutcircuits.com/electronic-components/datasheet/B3U-1000P-B--Omron/

This looks close to what I am trying to do: https://github.com/davidphilipbarr/Sweep/tree/main/Sweep%20half-swept


# Checklist

- [ ] MCU position on the PCB matches the one in the preview.
- [ ] Through holes are the correct size for my components.
- [ ] Fit between the diode and the switch.