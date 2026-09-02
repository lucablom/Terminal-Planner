# Terminal Planner

Plan and validate Beckhoff Bus Terminal configurations — current budget, power contacts and build width — in a single HTML file that runs offline.

> **Unofficial tool.** This is a self-developed, independent project. It is not affiliated with, authorised by, or endorsed by Beckhoff Automation GmbH & Co. KG. Beckhoff®, EtherCAT®, TwinCAT®, TwinSAFE® and XFC® are registered trademarks of Beckhoff Automation GmbH. Product data is reproduced here for engineering convenience only.
>
> **The output is not a substitute for your own verification.** Always check the finished configuration against the current Beckhoff documentation before ordering or building.

---

## What it does

You enter the terminals in mounting order. The tool looks each one up, tracks the running current budget, checks the power contact chain, and draws the rail.

- **Current budget** — E-bus and K-bus residual current after every module, resetting at each supply
- **Power contacts** — validates the `+`, `−` and `PE` chain: potential supply, forwarding, separation and bus termination
- **Rails** — split a configuration across several DIN rails; the potential chain restarts at each break while the bus budget carries on
- **Build width** — cumulative width per rail and for the whole configuration
- **Ex i segments** — the ELX rules for intrinsically safe terminals (see below)
- **Rail view** — modules drawn to scale, colour-coded by signal type, with power contact rails and a residual current band underneath

## Getting started

1. Download `terminal-planner.html`
2. Open it in a browser

That is the whole installation. There is no server, no build step and no network access — the file contains the interface, the logic and the full product catalogue. It works on an air-gapped machine.

## Features

**Configuration**

- Search 1598 articles by product name or description
- Add, duplicate (`Shift`-click for several copies), reorder and delete rows
- Paste a whole list at once, `3x KL1404` style
- Rail dividers with editable names
- Errors listed in plain language, click to jump to the offending row

**Catalogue**

- All articles grouped by Beckhoff series — KL1xxx, EL3xxx, CX8xxx and so on
- Search across names and descriptions
- Add any article straight to the configuration

**Edit articles**

- Add your own articles, or edit the built-in ones
- Rename and create catalogue groups
- Set card colour and signal-type strip colour per article
- Save the whole tool, with your additions baked in, as a new version-numbered HTML file

**Output**

- PDF report with status, key figures, rail views, the full table and a parts list
- CSV export, JSON project files
- Your own logo in the header and in the report
- Swedish, English and German; light and dark mode

## How the check works

The core arithmetic reproduces the behaviour of Beckhoff's own configuration spreadsheet, including its handling of empty cells, so results match cell for cell.

On top of that:

| Check | Rule |
|---|---|
| Current budget | Residual must not go negative on either bus |
| Bus start | A potential separation cannot be the first module on a rail |
| Forwarding | Potential forwarding needs potential above it on the same contact |
| Separation | Nothing may forward potential below a separation |
| Termination | Every rail must end with an end or bus extension terminal |
| Rail break | The rail above a divider must be terminated before the break |

### Ex i (ELX) segments

The ELX series has its own arrangement rules, taken from the Beckhoff ELX9xxx operating manual:

- ELX signal terminals may only be mounted after an ELX9560 power supply terminal
- Only ELX terminals may sit inside an ELX segment
- Each additional ELX9560 needs an ELX9410 directly in front of it
- An ELX9410 must not sit directly after an ELX9560, nor directly before an ELX signal terminal
- A segment must end with an ELX9012, an EK1110, or two ELX9410 in succession

ELX terminals are drawn with the blue housing they actually have, and carry only two power contacts — `+24 V Ex` and `0 V Ex`, no PE.

## Data sources

| Source | Covers |
|---|---|
| Beckhoff configuration spreadsheet | 1570 articles: current, width, power contacts, descriptions |
| Beckhoff ESI device description files | The ELX series, current values as used by TwinCAT |
| Beckhoff operating manuals and product pages | ELX widths, power contacts and segment rules |

Product data ages. If a terminal is missing or a value looks wrong, correct it under **Edit articles** and save a new version of the file.

## Known limitations

- **ELX widths** — eight are verified against the manuals; the remaining twenty are assumed to be 12 mm. ELX2008 and ELX3158 are the most likely to be wrong. Width affects the build width total only, never the current budget.
- **Missing articles** — ELX2792, ELX3632 and ELX6233 are newer than the ESI file used and are not in the catalogue.
- **No network topology** — the tool models rails, not EtherCAT links. Cable redundancy, junctions and ring topologies are out of scope.
- **No persistence** — nothing is stored between sessions. Use *Save .json* to keep a configuration.
- **No ordering data** — no prices, availability or lifecycle status.

## Version

Current: **1.9**

The version number lives in the header. When you save a new HTML file from the *Edit articles* tab you set the next number yourself, so a file always states which build it is.

## Contributing

Corrections to article data are the most useful contribution — especially widths and power contacts, and especially with a source. ESI files and current Beckhoff configuration spreadsheets are both good inputs.

## Licence

MIT


Note that the product data in this repository originates from Beckhoff's published material. Your own code and the product data are two different things, and redistributing the data publicly is worth a moment's thought before the repository goes public.
