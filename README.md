![Forager keyboard in white](/docs/images/forager_hero.jpg)

# Forager

Forager is a Voyager-inspired 34-key low-profile wireless split keyboard powered by ZMK.

> why does the large Voyager not simply eat the small Forager?

## Choc v2 / 19 mm fork

This fork of [carrefinho/forager](https://github.com/carrefinho/forager) reworks the PCB for **Choc v2 hotswap switches at 19 × 19 mm spacing** (each half is about 8 mm wider and 6 mm taller) and adds, on each half:

- a power switch (Alps SSSS811101)
- an optional reset button (Panasonic EVQ-PUC02K)
- three SK6812MINI-E status LEDs behind a P-FET power switch

The schematic and board are done. A new case for the left half is designed (see below). The LED firmware, the Gerbers and the build guide have not been updated yet and still describe the original design. See [the full list of changes](/docs/changes.md).

### Case (left half)

![Forager left half, assembled with keycaps](/renders/forager_left_assembled_keycaps.png)

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

**Status LEDs:** each half has three SK6812MINI-E RGB LEDs at a 4.5 mm pitch, reverse-mounted on the PCB's top inner edge so they shine up through cutouts in the board. Three clear 1.75 mm light pipes, flush with the top plate, carry the light to its surface. A P-FET turns the LEDs' supply off when they aren't in use, so they draw no power. They are driven from the XIAO, which can set each LED to its own colour. In the render the pipes are coloured blue, green and red only to show where the light comes out. LED firmware support isn't written yet.

![Forager left half, exploded view](/renders/forager_left_exploded_keycaps.png)

The STEP and STL files are in assembly coordinates, so turn the top plate and the stand over in the slicer before printing. The right half is a mirror of the left and is not exported yet.

## Design

- Condensed [ZSA Voyager](https://www.zsa.io/voyager) layout without outer pinkie column and number row
- Hot-swap Kailh Choc V1 switches in Choc (18x17mm) spacing with Voyager-esque switch puller grooves

![Switch-plate cutouts](/docs/images/forager_switch.jpg)

- Wireless only with Seeed XIAO nRF52840 running ZMK firmware
- Magnetic tenting legs inspired by Voyager and [Unicorne](https://www.boardsource.xyz/products/unicorne)

![Forager keyboard with magnetic tenting legs attached](/docs/images/forager_tented.jpg)

- Compact silhouette without the usual blank space occupied by controller
- Fully enclosed with 3D-printed two-piece case
- Minimal BOM by utilizing XIAO nRF52840's onboard RGB LED and reset button

![Forager keyboard with LED indicator lit up in blue alongside reset button on the bottom](/docs/images/forager_led_reset.jpg)


## Build Guide and Parts List

[Find the build guide here.](/docs/build-guide.md)

## Firmware

[Find the ZMK module here.](https://github.com/carrefinho/forager-zmk-module)

## Credits

- [ZSA Voyager](https://www.zsa.io/voyager): overall layout & aesthetics
- [GEIGEIGEIST/TOTEM](https://github.com/GEIGEIGEIST/TOTEM): XIAO PCB footprint and general placement
- [ebastler/marbastlib](https://github.com/ebastler/marbastlib): symbols and PCB footprints
- [caksoylar/zmk-rgbled-widget](https://github.com/caksoylar/zmk-rgbled-widget): RGB LED functionality

## Gallery

![Forager keyboard in white](/docs/images/forager_white.jpg)
![Forager keyboard in black and white](/docs/images/forager_bw.jpg)
![Forager keyboard with detached tenting leg](/docs/images/forager_legs.jpg)
![Forager keyboard in black](/docs/images/forager_bob.jpg)