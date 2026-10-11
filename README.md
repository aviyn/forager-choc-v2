![Forager Choc v2 left half, assembled with keycaps](/renders/forager_left_assembled_keycaps.png)

# Forager Choc v2 / 19 mm

A fork of [Forager](https://github.com/carrefinho/forager) by [carrefinho](https://github.com/carrefinho), a Voyager-inspired 34-key low-profile wireless split keyboard powered by ZMK. This fork reworks the PCB for **Choc v2 hotswap switches at 19 × 19 mm spacing** (each half is about 8 mm wider and 6 mm taller) and adds, on each half:

- a power switch (Alps SSSS811101)
- an optional reset button (Panasonic EVQ-PUC02K)
- three SK6812MINI-E status LEDs behind a P-FET power switch

> [!WARNING]
> This fork has not been built yet. The first PCBs have just been ordered, so nothing here is tested on real hardware. Build it at your own risk.

The schematic and board are done, and the Gerbers in [`forager-pcb/GERBER-forager-pcb.zip`](/forager-pcb/GERBER-forager-pcb.zip) are generated from the current board. A new case for the left half is designed (see below). The LED firmware and the build guide have not been updated yet and still describe the original design. See [the full list of changes](/docs/changes.md).

Photos and design notes for the original Choc v1 Forager are in the [original Forager gallery](/docs/original-gallery.md).

## Case (left half)

The new case was designed in Fusion for the 19 × 19 mm board. Printable STEP and STL files are in [`exports/case/`](/exports/case/). It has four parts:

- **Top plate:** Choc v2 cutouts, shallower switch-puller grooves, four countersunk M2 screw holes, and three 1.75 mm holes for the LED light pipes.
- **Base:** a 1 mm deeper shell (bottom at Z −7.6) that fits a **601230** LiPo between two filleted floor ribs. It also has:
  - five bevelled screw posts, with countersunk M2 holes from below;
  - a 13 × 6.5 mm USB-C pocket;
  - a wall opening and a larger insertion relief for the power slider;
  - XIAO reset pinholes on both sides of the USB, so the same design mirrors to the right half;
  - flush magnet pockets for the stand;
  - four recesses for 10 × 3 mm bump-on feet.
- **Stand:** a 12° magnetic tenting stand that follows the case outline. Its top has pockets that clear the case's feet, and its bottom has two foot recesses. With the stand attached, all six feet are coplanar, so the board sits flat without rocking.
- **Power slider cap:** a small cap for the power switch.

All outer edges have 2 mm fillets, matched between the case and the stand.

![Forager left half, assembled without keycaps](/renders/forager_left_assembled_no_keycaps.png)

![Forager left half, side view of the outer edge](/renders/forager_left_side_edge.png)

![Close-up of the three status LED light pipes in the top plate](/renders/forager_left_leds_closeup.png)

**Status LEDs:** each half has three SK6812MINI-E RGB LEDs at a 4.5 mm pitch, reverse-mounted on the PCB's top inner edge so they shine up through cutouts in the board. Three light pipes carry the light to the top surface. Each is a piece of translucent 1.75 mm 3D-printer filament, cut to the top plate's thickness (about 2.1 mm) and pressed flush into its hole. A P-FET turns the LEDs' supply off when they aren't in use, so they draw no power. They are driven from the XIAO, which can set each LED to its own colour. In the render the pipes are coloured blue, green and red only to show where the light comes out. LED firmware support isn't written yet.

![Forager left half, exploded view](/renders/forager_left_exploded_keycaps.png)

The STEP and STL files are in assembly coordinates, so turn the top plate and the stand over in the slicer before printing.

**Right half:** only left-half files are provided. The case is designed to be mirrorable, so mirror each part across X in your slicer to print the right half. The XIAO reset pinhole on the other side of the USB lines up with the right-hand XIAO once mirrored.

## Build Guide and Parts List

[Find the build guide here.](/docs/build-guide.md) It still describes the original Choc v1 build; the Choc v2 parts (power switch, reset button, status LEDs and the new case) are not covered yet.

## Firmware

This fork uses the original [Forager ZMK module](https://github.com/carrefinho/forager-zmk-module) by carrefinho. Support for the new status LEDs isn't written yet.

## Credits

- [carrefinho/forager](https://github.com/carrefinho/forager): the original Forager keyboard, PCB, case and firmware that this fork is based on
- [ZSA Voyager](https://www.zsa.io/voyager): overall layout & aesthetics
- [GEIGEIGEIST/TOTEM](https://github.com/GEIGEIGEIST/TOTEM): XIAO PCB footprint and general placement
- [ebastler/marbastlib](https://github.com/ebastler/marbastlib): symbols and PCB footprints
- [caksoylar/zmk-rgbled-widget](https://github.com/caksoylar/zmk-rgbled-widget): RGB LED functionality

## License

The original Forager and this fork are released under the [CERN Open Hardware Licence Version 2 - Permissive](/LICENSE).
