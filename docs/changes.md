# Forager Choc v2 / 19 mm: Changes

This fork modifies the [original Forager](https://github.com/carrefinho/forager) PCB. It moves the board from **Choc v1 hotswap at 18 × 17 mm** to **Choc v2 hotswap at 19 × 19 mm**, and adds a power switch, an optional reset button and status LEDs to each half.

> **Status:** the schematic and PCB are complete and routed (DRC: 0 errors, 0 unconnected items; ERC: no new errors compared with the original). The [build guide](build-guide.md), the [case](../case/) and the Gerbers still describe the **original** design. See [Not done yet](#not-done-yet).

## Summary

| Area | Original | This fork |
| ---- | -------- | --------- |
| Switches | Choc v1 hotswap | Choc v2 hotswap (post-less footprint) |
| Key spacing | 18 × 17 mm | 19 × 19 mm |
| Board size (per half) | - | about 8 mm wider and 6 mm taller |
| Power switch | none | Alps SSSS811101 on each half |
| Reset button | XIAO onboard button only | optional Panasonic EVQ-PUC02K on each half |
| Status LEDs | XIAO onboard RGB LED | 3 × SK6812MINI-E per half, behind a P-FET power switch |

## What changed

### 19 × 19 mm grid

- Every key was moved to a 19 × 19 mm pitch. The stagger is kept; only the pitch changed.
- The home row and the inner column stay where they were. Each column moves outward by 1 mm per column, and the top and bottom rows move 2 mm away from the home row.
- Thumb keys keep their 25° angle at the 19 mm pitch, with 1 mm of extra drop to keep the keycap gap.
- Traces, vias, outline, snap-off tabs and GND pours were remapped to match, and the XIAO antenna keepouts moved with the XIAOs.

### Choc v2 hotswap footprint

- Footprint: `forager:SW_choc_v2_HS_CPG135001S30_1u_nopost`, in [`forager-pcb/lib/forager.pretty/`](../forager-pcb/lib/forager.pretty/).
- It is derived from marbastlib's `SW_choc_v2_HS_CPG135001S30_1u`, which is vendored with its license in [`forager-pcb/lib/marbastlib-xp-choc.pretty/`](../forager-pcb/lib/marbastlib-xp-choc.pretty/).
- The centre hole is **5.0 mm**. It was not widened, to avoid switch rattle.
- The footprint has **no side positioning post**, because the post collided with XIAO pins, J1, diodes, test points and a standoff. If your switches have the post, clip it off.
- The socket is still the CPG135001S30, with its pads in the same positions as the v1 footprint.
- Choc v2 has a 5.0 mm centre stem (v1 has 3.4 mm) but the same socket pin positions. Kailh's "Pro" Choc switches (Red, Pink, Light Blue Pro) are still **v1**.

### XIAO

- Both XIAOs moved 0.3 mm toward the top edge so the outer top key's 5 mm hole clears XIAO pins 4 and 11.
- A ground tie trace now connects pad 15 (front) and pad 9 (back) to the GND fill.

### Power switch (both halves)

- Part: **Alps SSSS811101**. The land pattern was checked against the Alps drawing (terminals at -2.25, +0.75 and +2.25 mm, with ② as the common).
- Footprint: `forager:SW_SPDT_Alps_SSSS811101`, on the back side at the outer edge below the XIAO. The slider overhangs the board edge by about 1 mm.
- Circuit: battery (J1/J2 and test pad) → switch → XIAO BAT+. The new nets are `LBAT_SW` and `RBAT_SW`.
- **Slider up = on** on both halves. The down-end pin is not connected.
- Routing changes: the left battery trace was rerouted; on the right, `RROW2` and `RROW3` jog about 1.5 mm inward and the battery traces hop to the front copper. Three GND stitching vias under the left switch were removed.

### Reset button (optional, both halves)

- Part: **Panasonic EVQ-PUC02K**, side-push with bosses, per Panasonic drawing ES103 No. 2.
- Footprint: `forager:SW_Panasonic_EVQPUC02K`, on the back side at the outer edge **below the power switch**, with the push plate about 1 mm past the edge.
- Circuit: XIAO RST (pad 21) to GND, on the new nets `LRST` and `RRST`.
- It is optional. The XIAO's onboard reset button still works if you leave it off. Each button has an "RST" silkscreen label.

### Status LEDs (both halves)

- 3 × **SK6812MINI-E** per half, reverse-mounted on the top inner edge and shining up through board cutouts. LED pitch is 4.5 mm.
- Data: XIAO **P0.05** → 330 Ω → LED1 → LED2 → LED3. LED3's DOUT is not connected.
- Power: an **AO3401A** P-FET switches the LED supply from the XIAO's 3V3 pin, so the LEDs draw nothing when off. The enable line is active low, with a 1k series resistor and a 100k pull-up, plus a 1 µF capacitor on the switched rail.
- **This uses every remaining free GPIO on both XIAOs.**

| Signal | Left half | Right half |
| ------ | --------- | ---------- |
| LED data (DIN) | P0.05 (pad 6) | P0.05 (pad 6) |
| LED enable (active low) | P1.15 (pad 11) | P1.11 (pad 7) |
| LED supply | 3V3 (pad 10) | 3V3 (pad 10) |

- **LED order:** the chain runs left to right (+x) on both halves. On the left half LED1 is the outer LED (nearest the XIAO); on the right half LED1 is the inner LED. Firmware has to map the indices accordingly.
- **Light pipes:** the case needs one 1.75 mm hole centred on each LED, at the six (x, -47.75) positions. Check these against the top-case key openings.
- **Soldering:** four LED ground pads use a via in the pad. Some solder wicks into the open hole, so use a little more solder, or order filled or capped vias.
- Per-LED 100 nF decoupling capacitors were left out.

## Added parts (per half)

| Part | Value / part number | Ref (L / R) | Count | LCSC |
| ---- | ------------------- | ----------- | ----- | ---- |
| Power switch | Alps SSSS811101 | LSW1 / RSW1 | 1 | - |
| Reset button (optional) | Panasonic EVQ-PUC02K | - | 1 | - |
| RGB LED | SK6812MINI-E | LLED1-3 / RLED1-3 | 3 | C5149201 |
| P-FET | AO3401A (SOT-23) | LQ1 / RQ1 | 1 | C347476 |
| Resistor, 1206 | 100k | LR1 / RR1 | 1 | C17900 |
| Resistor, 1206 | 330 Ω | LR2 / RR2 | 1 | C25374 |
| Resistor, 1206 | 1k | LR3 / RR3 | 1 | C4410 |
| Capacitor, 1206 | 1 µF | LC1 / RC1 | 1 | C1848 |

Multiply by two for a full keyboard. The switches themselves are not in the build guide's parts list, but they now need to be **Choc v2 switches** (for example Kailh Black Cloud) with **MX-compatible keycaps**. The hotswap socket (CPG135001S30) is unchanged.

## Not done yet

- **Case:** the files in [`case/`](../case/) are still for 18 × 17 mm and Choc v1. They need the 19 × 19 outline, Choc v2 plate cutouts (MX-style 14 mm; rethink the puller grooves), moved standoffs, outer-wall slots for the power slider and reset button, six light-pipe holes, and moved XIAO and USB-C openings.
- **Firmware:** the key matrix is unchanged. The LED support still has to be written (a ZMK module with a WS2812 driver on P0.05 via SPI, and the active-low enable GPIO).
- **Fab outputs:** `forager-pcb/GERBER-forager-pcb.zip` is **stale** and still the original board. Regenerate Gerbers, drill, BOM and position files, and check that the LED cutouts appear in Edge.Cuts and that the plotted silkscreen font (Geist) is right.
- **Build guide:** still describes the original build, including the case steps and the onboard-LED light guides.

## Known issues and review items

- Silkscreen: the LED reference labels overlap the LED cutouts. Optionally add an "ON ▲" mark next to the power switches.
- Worth a visual check in KiCad: the `RBAT_SW` reroute on the right half, the `RROW2`/`RROW3` jog around the right power switch, `R3V3` running between the right resistor's pads, and the two-via hop on `RLED_G`.
- Remaining DRC warnings (about 40) are inherited or cosmetic: missing library paths, `lib_footprint_mismatch`, silkscreen overlaps, and schematic-parity items for board-only parts (standoff nuts, snap-off tabs, J1/J2 mounting pads). `copper_edge_clearance` is ignored in this project, as in the original.
- Several footprints (standoffs, switch holes, the XIAO) have no courtyard, so fit checks have to use pads and holes.
