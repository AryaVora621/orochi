# Ender 5 Pro single-toolhead CoreXY upgrade: staged BOM

Goal: most performance for the least money. One toolhead, Mercury One.1 CoreXY motion, Sherpa Mini direct drive, a high-flow hotend, ABS.
This is separate from the multi-tool V1.0 BOM (`V1.0-BOM.md`); it drops the dock kits, the SKR 1.4 Turbo and the second tool.

**Price status: every price below is a search-snippet or retailer-listing figure, in mixed currencies, undated, and unverified. Amazon blocked live checks. Confirm each listing before buying. "~USD" figures are rough conversions.**

## Already owned (cost $0)

| Item | Note |
|---|---|
| Ender 5 Pro frame, 24V PSU, heated bed | PSU wattage label not yet read; check before the new heater |
| BTT SKR Mini E3 V2 (4 TMC2209 drivers, 32-bit) | Enough for CoreXY X/Y, Z, one extruder |
| 2x LDO NEMA 17, believed 42STH48-2504 | Run at 2 A. Model label not yet read |
| Sherpa Mini (or Micro) extruder | Weight not yet checked |
| BTT USB-C ADXL345 | For input shaping |
| BLTouch | Keep for now |
| Raspberry Pi running Klipper | Already in use for the Ender |
| Stock Ender-style hotend + V6 clone | Used for commissioning only |

## Stage 1: motion (CoreXY conversion)

| Item | Qty | Price (unverified) | Note |
|---|---|---|---|
| HoneyBadger Mercury One kit, **full** variant (owner pasted the listing text) | 1 | $239.99 to $269.99 (search snippets); base kit snippet $154.99, Plus listing | The listing text says the **base kit has no rails and no fans**. Base kit: 2 HoneyBadger 48 mm NEMA 17, 370 mm extrusion (Pro), 100 M3 heat inserts, BOM screws with 20% extra, 2 Omron microswitches, 50W 24V heater, 104-GT2 thermistor, 6 mm Gates belts, 8.5 mm idlers. Full kit adds 3x MGN12H stainless rails (330 mm Pro) and 5015 + 4010 fans |
| MGN12H rails (3x, 330 mm for Pro) | 1 set | In the full kit; buy separately only if base + rails + fans is cheaper than the full kit gap (about $85 per snippets) | Printed parts must match the 8.5 mm idler height |
| Fastener kit | 1 | Not needed: kit includes screws with 20% extra | |
| Printed Mercury parts | set | $0 (print on SV08) | PETG or ABS |

### Stage 1 alternative: self-source the kit (checked 2026-11-18 in the browser)

Official ZeroG BOM (docs.zerog.one, fetched): Ender 5 (Pro) needs **330 mm** MGN12H x3 (the older DREMC article's 300 mm is out of date), one 370 mm 2020 extrusion, 2 idlers 20T 6 mm, 2 pulleys 20T, a Gates LL-2GT belt (length per the store note), 20 F695-2RS bearings, 10 M5x30 precision dowel pins, 50 M3x5 heat inserts, 40 M5 precision shims (golden spacers, not washers), plus about 12 bolt sizes and 35 M3 / 20 M5 T-nuts. Recommended hotend: Rapido HF. DREMC notes: use the higher-preload Z1 block on the X axis.

AliExpress listings seen (aliexpress.us, listing prices at time of viewing, per-length options not individually opened):

| Item | Listing | Price |
|---|---|---|
| Genuine HIWIN MGN12H, Z1/Z0, 330 mm option | 1 rail + 1 block | $32.29 |
| 3x Hiwin MGN12 rails + 3 MGN12H blocks | 3 sets, 330 mm in length list | $112.69 |
| Generic MGN12H rail + block (118 sold, 4.5 stars) | 1 set | $8.81 |
| FYSETC hollow MGN12H 330 mm for E5 Pro | 1 rail | $74.03 |
| 2020 aluminum extrusion | length not confirmed | $8.32 each (1 pc) |

Not priced: fasteners, T-nuts, bearings, pulleys, idlers, belt, inserts, shims, dowel pins (listings did not load). Treat the small-parts bundle as roughly $40 to $70 until priced; this is a guess.

Rough self-source total: **about $130 to $200 with genuine Hiwin rails** vs **$240 to $270 for the full kit**. With generic rails it could drop below $100, at a quality risk. The base kit ($155) plus Hiwin rails ($113) is about $268, so it loses to both.

## Stage 2: toolhead and hotend

| Item | Qty | Price (unverified) | Note |
|---|---|---|---|
| High-flow hotend, choice open | 1 | Bambu-style 0.6 mm hardened bare ~USD 17-24 (A$24.96 and 18.40 GBP snippets) | **Not a drop-in.** Bambu hotends use their own 40W heater, thermistor, connector and mount; the kit's 50W heater and 104-GT2 thermistor suit a V6-style hotend. Choose the hotend after the toolhead mount |
| Printed toolhead mount for Sherpa + hotend + fan duct | 1 | $0 (print) | Mount STL not yet chosen |
| 24V hotend fan (4010) and part-cooling blower | 2 | ~USD 10 total (no listing checked) | General estimate |
| JST connectors, wire, heat shrink | assorted | ~USD 8 (from V1.0 BOM) | |
| Drag chain or cable sleeve | 1 | ~USD 8 (no listing checked) | General estimate |

## Stage 3: ABS readiness and filament

| Item | Qty | Price (unverified) | Note |
|---|---|---|---|
| Enclosure tent for Ender 5 Pro | 1 | No confirmed Ender 5 Pro price yet (snippets mixed in Ender 3 tents) | Several results were Ender 3 tents; check listed dimensions against the Ender 5 Pro frame. "Fireproof" is mostly marketing; only the Creality tent cites a test |
| ABS or ABS+ filament, 1 kg | 2 | eSun ABS+ 17.49 EUR/kg (~USD 19-20) | One retailer only; no brand comparison |

## Totals (rough, mixed currencies)

| Path | Approx USD |
|---|---|
| Stage 1 (base kit only, rails and fasteners unconfirmed) | 155 to 250 |
| Stage 2 | 45 to 55 |
| Stage 3 | 70 to 105 |
| **Whole path** | **about 270 to 410** |

The range is wide because rail inclusion and fastener cost are unknown. Pin them down first; they drive the total.

## Skipped to save money

- LDO dock kits, SKR 1.4 Turbo, second hotend, second extruder (all multi-tool only).
- New steppers (you have LDO 2504s).
- A stock-extruder Bowden build (it would be thrown away when you go direct drive).
- Rapido-class hotend, linear rails beyond the kit, PSU upgrade (revisit after the Stage 2 flow test).

## Open items before ordering

1. Exact Ender 5 Pro kit SKU, rail length (300 vs 330 mm) and whether rails and fasteners are included.
2. Hotend part number that fits your chosen mount.
3. PSU wattage label.
4. Sherpa Mini weight and motor label model.
