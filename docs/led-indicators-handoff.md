# Forager status-LED indicators: hardware handoff

Branch: `led-indicators` (from `choc-v2-19mm`). Date: 2026-10-06.
Status: hardware (schematic and PCB) is implemented, routed and passes DRC. Committed on this branch.
Source spec: `forager-led-indicators-spec.md` (Part 1, Hardware). Part 2 (firmware) and the case are not started.

## Result

- ERC: same as before this change (2 pre-existing BAT+ "power pin not driven" errors, 4 library-path warnings).
- DRC (after zone refill): **0 errors, 0 unconnected items.**
- New DRC warnings, all cosmetic:
  - LED reference text overlaps the LED cutouts and outlines (`silk_*`).
  - 16 `lib_footprint_mismatch` warnings, one per new footprint. This KiCad 10 quirk already appeared on the stock J1/J2 before this change.

## Circuit (per half, identical)

XIAO 3V3 → Q1 source. Q1 drain → VLED. R1 100k from gate to source. XIAO EN pin → R3 1k → gate (active low). C1 1 µF from VLED to GND.
XIAO DIN pin → R2 330 Ω → LED1 DIN → LED2 → LED3. LED3 DOUT is not connected. The 100 nF per-LED caps were **dropped** at the user's request.

| Ref (L / R) | Part | Footprint | LCSC |
|---|---|---|---|
| LLED1-3 / RLED1-3 | SK6812MINI-E | `LED_SMD:LED_SK6812MINI-E_3.2x2.8mm_P1.5mm_ReverseMount` (stock KiCad 10) | C5149201 |
| LQ1 / RQ1 | AO3401A | `Package_TO_SOT_SMD:SOT-23` | C347476 |
| LR1 / RR1 | 100k | `Resistor_SMD:R_1206_3216Metric_Pad1.30x1.75mm_HandSolder` | C17900 |
| LR2 / RR2 | 330 | same R 1206 hand-solder | C25374 |
| LR3 / RR3 | 1k | same R 1206 hand-solder | C4410 |
| LC1 / RC1 | 1 µF | `Capacitor_SMD:C_1206_3216Metric_Pad1.33x1.80mm_HandSolder` | C1848 |

Nets: `L3V3/R3V3`, `LVLED/RVLED`, `LLED_G/RLED_G`, `LLED_EN/RLED_EN`, `LLED_DIN/RLED_DIN`, `L/RLED_D1..D3`, GND = the existing global `LGND/RGND`. PWR_FLAGs `#FLG01/02` are on VLED.

**Pins (firmware):**
- DIN = **P0.05** (pad 6) on both halves.
- EN = **P1.15** (pad 11) on the left and **P1.11** (pad 7) on the right. Active low: drive low to power the LEDs.
- 3V3 comes from pad 10.

**Pin order verified:** in the datasheet's bottom view (the solder side for a reverse-mounted LED), DIN is top-left, VDD top-right, GND bottom-left (chamfered) and DOUT bottom-right. That matches KiCad's footprint. KiCad's pin numbers differ from the datasheet's, but its symbol and footprint agree with each other.

## Placement (back side, KiCad coordinates; the right half is mirrored in x)

| Part | Left | Right | Rot |
|---|---|---|---|
| LED (D1 / D2 / D3) | x −38.5 / −34.0 / −29.5, y −47.75 | x 29.5 / 34.0 / 38.5 | 270° |
| R2 | (−41.0, −40.0), 90° | (26.5, −40.75), 90° | |
| Q1 | (−30.5, −41.5) | (30.5, −41.5) | 0° |
| C1 | (−26.5, −40.75), 90° | (37.5, −41.5), 0° | |
| R1 | (−23.0, −36.0) | (23.0, −36.0) | 0° |
| R3 | (−22.5, −31.0), 90° | (22.5, −31.0), 90° | |

- **Pitch:** 4.5 mm, chosen by the user. The LEDs sit about 2.6 mm from the inner M2 standoff at (∓21.92, −47.75).
- **Chain direction:** the chain runs **left to right (+x) on both halves**, because the footprint can't be mirrored without flipping it.
  - Left half: LED1 is the outer LED (nearest the XIAO).
  - Right half: LED1 is the inner LED.
  - The firmware must map LED indices accordingly.
- **Case light pipes:** one 1.75 mm hole centred on each LED at the six (x, −47.75) positions. Each hole has about 1.9 mm of top-case material to the LCH5/RCH1 keycap edge and about 3 mm to the outer edge. Check this against the top-case key openings, which may be wider because of the puller grooves.

## Routing

- **LED chain:** each DOUT→DIN link runs down the centre of the 1.27 mm gap between cutouts, 0.533 mm from each cutout edge (the rule is 0.5).
- **VLED:** a 0.3 mm bus at y −43.4 under the bottom pad row, connected to Q1's drain and C1.
- **GND:**
  - The four boxed-in VSS pads (LLED1/2, RLED1/2) have a **via in the pad** (0.6/0.3) to the front GND fill. Approved by the user: some solder wicks into the open hole, so use a bit more solder, or order filled/capped vias.
  - LLED3/RLED3 VSS: a short trace plus a via beside the pad.
  - B.Cu rule areas `LED_noFill_L` (−40.3, −52)…(−28, −49.6) and `LED_noFill_R` (28, −52)…(40.3, −49.6) keep the back-layer fill out of the pad row. They were added through the kipy IPC API to fix 2 copper-sliver warnings.
- **Long runs from each XIAO (DIN, EN, 3V3):** 70–100 mm each, B.Cu and F.Cu, 2 vias each. 3V3 and VLED are 0.3 mm; signals are 0.2 mm. Long runs are horizontal with 45° joints; shallow angles caused fill slivers.
- **Vias under the XIAO:** used here, as the original design already does.

### Changes to existing design (review these)
- **RBAT_SW (right half)** was re-routed. Its old 0.4 mm back-layer U-loop around XIAO pads 6/7 sealed them in. It now goes on F.Cu from the BAT+ through-hole pad (111.817, −37.855) with 1 via at (115.8, −30.5) and rejoins the old trace at (113.63, −27.85).
- **LD5** moved from x −41.333 to −42.15 (it was blocking the outer LED). Its LROW0 link to LD4 was re-drawn.
- **LD5 anode (`Net-(LD5-A)`)** re-routed: LD5.2 → via (−41.933, −45.45) → F.Cu → LCH5 pin 2 (−35, −30.05). The old B.Cu trace ran through LR2's pad.
- **LR1/RR1** moved 0.5 mm toward the LEDs to clear the GND stitching vias at (∓20, −37).
- **XIAO no-connect flags** removed from 3V3 on both halves. The XIAO pad nets were updated with F8 (the Konnect sync tool doesn't re-assign `unconnected-*` pads or push new fields).
- **Back silkscreen block** "Forager / Choc v2 / rev. 1.0 / left|right" moved (by the user) into the battery outline (Δ ∓23, +55.5).

## Findings and gotchas

- **Footprints with no courtyard:** the M2 SMD standoffs, the Choc v2 5 mm stem holes and 3 mm pin holes (the hotswap courtyard covers only the socket), and the XIAO. Every fit check must use pads and holes, not just courtyards. Both early mistakes in this work came from this.
- **Battery pocket:** under the back silk rectangle at x ∓47.6…∓81.6, y 3…17. Keep parts out.
- **Inner-edge column:** the first idea (a vertical column on the inner edge) had easy routing. The user preferred the top edge.
- **Top-edge row topology:** each rotated LED has one power pad and one data pad on the edge side, and only one trace fits between cutouts. A single-layer layout is therefore impossible for a row of 3. That is why the GND pads use vias in the pad. The alternatives were 5.0 mm pitch with two lanes, or the inner-edge column.
- **LED cutout size:** 3.234 × 3.634 mm, larger than the LED body. At 4.25 mm pitch only 1.0 mm is left between cutouts, which is too narrow.
- **Hatched GND fill:** it makes slivers next to shallow-angle traces and squeezed pads. Fixed by straightening the runs and adding the rule areas.

## Open items

1. ~~Commit the branch~~ (done; `renders/` is kept local and untracked).
2. **Silkscreen:** hide or move the LED reference labels that overlap the cutouts.
3. **Visual review in KiCad:** the RBAT_SW re-route, R3V3 running between RR2's pads (under the resistor body), and RLED_G's 2-via hop at (28.4, −38.65)/(28.4, −40.05).
4. **Gerbers:** check that the LED cutouts appear in Edge.Cuts. They come from the footprint; DRC treats them as board edges.
5. **Firmware (spec Part 2):** custom ZMK module, SPI3 WS2812 driver on P0.05, enable GPIO active-low on P1.15 (left) / P1.11 (right), LED index order as noted above.
6. **Case:** redesign for the 19 mm layout (separate work), plus the light-pipe holes and the battery option B+.

## Files

- Schematic: `forager-pcb/forager-pcb.kicad_sch` (new blocks to the right of each matrix).
- Board: `forager-pcb/forager-pcb.kicad_pcb`.
- Images: `renders/led_feasibility*.png` (placement study) and `renders/led_routing_{L,R,C}.png`.
- The router, checker and kipy scripts lived in the session scratchpad. They aren't in the repo and aren't needed going forward.
