# Edge controls reference (Fusion frame)

Frame: same as `exports/step/*.step` and the plate-cutout DXFs. X = PCB X, Y = -PCB Y, Z up with the board **bottom face at Z = 0** and the board top at Z = 1.51 (parts on the back hang below Z = 0).

The right XIAO is the same module rotated 180 degrees about the vertical axis, so its USB-C faces +X and its reset button is on the -Y side of the connector. Units mm.

Source: placed KiCad geometry. XIAO is `XIAO-nRF52840_v3.step` (user-supplied nRF52840 model). Alps and Panasonic parts are the simplified box models in `forager-pcb/lib/3dmodels`, not manufacturer CAD.

## Left half (outer board edge at X = -122.25)

| Item | X | Y | Z |
|---|---|---|---|
| XIAO body (model bbox) | -123.51 to -101.03 | 33.41 to 51.19 | -4.46 to 0.00 |
| USB-C shell | -123.51 to -116.21 | 37.84 to 46.78 (centre 42.31) | -4.46 to -1.25 |
| USB-C mouth face | -123.51 (protrudes 1.26 past edge) | 37.84 to 46.78 | -4.46 to -1.25 (3.21 high) |
| XIAO reset button | -121.65 to -118.56 | 47.22 to 48.82 (centre 48.02) | -1.98 to -1.25 |
| Power switch body | -121.71 to -119.11 | 13.40 to 20.10 | -1.40 to 0.00 |
| Power slider knob | -123.21 to -121.71 (protrudes 0.96 past edge) | 16.20 to 17.30 (centre 16.75) | -1.25 to -0.15 |
| Power slider travel (assumed +/-1.5) | same | 14.70 to 18.80 | -1.25 to -0.15 |
| Reset button body | -122.25 to -118.75 | -5.85 to -1.15 | -1.65 to 0.00 |
| Reset plunger | -123.25 to -122.25 (protrudes 1.00 past edge) | -4.80 to -2.20 (centre -3.50) | -1.35 to -0.30 |

## Right half (outer board edge at X = +122.25)

| Item | X | Y | Z |
|---|---|---|---|
| XIAO body (model bbox) | 101.03 to 123.51 | 33.41 to 51.19 | -4.46 to 0.00 |
| USB-C shell | 116.21 to 123.51 | 37.82 to 46.76 (centre 42.29) | -4.46 to -1.25 |
| USB-C mouth face | 123.51 (protrudes 1.26 past edge) | 37.82 to 46.76 | -4.46 to -1.25 (3.21 high) |
| XIAO reset button | 118.56 to 121.65 | 35.78 to 37.38 (centre 36.58) | -1.98 to -1.25 |
| Power switch body | 119.11 to 121.71 | 13.40 to 20.10 | -1.40 to 0.00 |
| Power slider knob | 121.71 to 123.21 (protrudes 0.96 past edge) | 16.20 to 17.30 (centre 16.75) | -1.25 to -0.15 |
| Power slider travel (assumed +/-1.5) | same | 14.70 to 18.80 | -1.25 to -0.15 |
| Reset button body | 118.75 to 122.25 | -5.85 to -1.15 | -1.65 to 0.00 |
| Reset plunger | 122.25 to 123.25 (protrudes 1.00 past edge) | -4.80 to -2.20 (centre -3.50) | -1.35 to -0.30 |

## Not exported / needs measuring

- **XIAO reset button** comes from the v3 STEP (beside the USB-C, on the +Y side). Sanity-check it once against your physical board, since it is a third-party model.
- **Power slider travel** (+/-1.5 mm) is an assumption from the Alps SSSS8 series; the box model has no travel. Confirm in the Alps datasheet.
- **Reset plunger / slider sizes** come from the simplified models; use them for placement, and add your own clearance.

## Files

- `forager_{left,right}_edge_controls_plan.dxf`: top-down XY outlines (same frame as the plate DXF).
- `forager_{left,right}_edge_controls_wall.dxf`: side-wall elevation, sketch X = Y_step, sketch Y = Z_step, for wall cutouts.
