<div align="center">

**English** • [Русский](README.md)

</div>

# awesome-cctv-cameras

> Open database of 2,100+ CCTV / IP camera specs — machine-readable, CC0, no paywalls.

[![cameras](https://img.shields.io/badge/cameras-2101-blue?style=flat-square)](data/cameras.json)
[![brands](https://img.shields.io/badge/brands-71-green?style=flat-square)](cameras/)
[![license](https://img.shields.io/badge/license-CC0-lightgrey?style=flat-square)](LICENSE)
[![schema](https://img.shields.io/badge/schema-validated-brightgreen?style=flat-square)](schema/camera.schema.json)

Camera spec sheets are buried in vendor PDFs, paywalled databases (IPVM etc.), and inconsistent retailer pages. This repo normalises them into validated JSON — one file per camera, one big queryable array, one CSV for spreadsheets.

---

## Contents

- [Stats](#stats)
- [Browse](#browse)
- [Quick start](#quick-start)
- [Query examples](#query-examples)
- [Schema](#schema)
- [Brands](#brands)
- [Contributing](#contributing)
- [License](#license)

---

## Stats

| | |
|---|---|
| Cameras | **2,101** |
| Brands | **71** |
| Form factors | 11 |
| PoE wired | 1,279 |
| WiFi | 503 |
| Battery / wire-free | 180 |
| 4K / 8MP+ | 545 |
| With Frigate / HA configs | 1,507 |

---

## Browse

Live viewer → **[cctv-database.com](https://cctv-database.com)**

Offline: serve `docs/` with any static server and open `demo.html` (no build step needed).

```bash
cd docs && python3 -m http.server
```

---

## Quick start

```bash
git clone https://github.com/Mukller/awesome-cctv-cameras.git
cd awesome-cctv-cameras
npm install   # installs Ajv for schema validation only
npm run build # validates all JSON → writes data/cameras.json + data/cameras.csv
```

### Repo layout

```
awesome-cctv-cameras/
├── cameras/              # source of truth — one JSON per camera, grouped by brand
│   ├── hikvision/        # 209 cameras
│   ├── dahua/            # 156 cameras
│   ├── reolink/          # 114 cameras
│   └── …68 more brands
├── data/                 # GENERATED — do not edit by hand
│   ├── cameras.json      # all 2,101 cameras as one array
│   └── cameras.csv       # flattened for spreadsheets
├── schema/
│   └── camera.schema.json
├── scripts/
│   └── build.js
├── configs/              # Frigate / Home Assistant integration configs
└── docs/
    └── demo.html
```

---

## Query examples

```js
const cameras = require('./data/cameras.json');

// 4K PoE outdoor cameras
cameras.filter(c =>
  c.connectivity?.includes('poe') &&
  c.resolution.megapixels >= 8
);

// Color night vision
cameras.filter(c => c.night_vision?.type === 'color');

// No subscription fee
cameras.filter(c =>
  c.features?.some(f => f.toLowerCase().includes('no subscription'))
);

// UK market
cameras.filter(c => c.markets?.includes('UK'));
```

Or open `data/cameras.csv` in any spreadsheet.

---

## Schema

Each entry follows [`schema/camera.schema.json`](schema/camera.schema.json).

**Required fields:**
```json
{
  "id": "reolink-rlc-823a",
  "brand": "Reolink",
  "model": "RLC-823A",
  "type": "bullet",
  "resolution": { "megapixels": 8, "label": "4K UHD" }
}
```

**Common optional fields:**

| Field | Type | Example |
|---|---|---|
| `connectivity` | `string[]` | `["poe", "wifi"]` |
| `night_vision.type` | `string` | `"color"` / `"ir"` |
| `night_vision.range_m` | `number` | `30` |
| `power.method` | `string` | `"PoE (802.3af)"` |
| `ip_rating` | `string` | `"IP67"` |
| `audio.two_way` | `boolean` | `true` |
| `protocols` | `string[]` | `["onvif", "rtsp"]` |
| `markets` | `string[]` | `["UK", "EU"]` |
| `features` | `string[]` | `["no subscription"]` |
| `sources` | `string[]` | datasheet URLs |

---

## Brands

71 brands across every market segment:

| Brand | Cameras | Segment |
|---|---|---|
| Hikvision | 209 | Enterprise + consumer, global |
| Dahua | 156 | Enterprise + consumer, global |
| Bosch | 153 | Enterprise + thermal, EU/global |
| ACTi | 119 | Enterprise IP, NDAA |
| Reolink | 114 | Prosumer, no-subscription |
| EZVIZ | 87 | Consumer, global |
| ABUS | 85 | Consumer + professional, DE/AT/CH |
| Axis | 61 | Enterprise premium, global |
| Hi-Focus | 60 | Made-in-India, BIS certified |
| Kedacom | 58 | Enterprise, CN/global |
| IMOU | 56 | Consumer + prosumer, global |
| Tapo | 47 | Budget consumer, global |
| Eufy | 46 | No-subscription consumer |
| Hanwha | 45 | Enterprise AI, Korea/global |
| Lorex | 40 | Consumer NVR systems, CA/US |
| … | … | … |

Full list with counts in [`cameras/`](cameras/).

---

## Contributing

Three ways to add a camera:

| | Method | Best for |
|---|---|---|
| 🌐 | [Open an issue](../../issues/new?template=add-camera.yml) | Anyone — web form, no clone needed |
| 🧙 | `npm run add` | Guided CLI wizard, writes JSON for you |
| 🛠 | Edit JSON directly | Developers — see [CONTRIBUTING.md](CONTRIBUTING.md) |

```bash
git clone https://github.com/Mukller/awesome-cctv-cameras.git
cd awesome-cctv-cameras && npm install
npm run add   # wizard
npm run build # validate
```

Report a data error → [correction issue](../../issues/new?template=correction.yml)

---

## License

Data — [CC0 1.0](LICENSE) (public domain). Free to use, copy, redistribute with no restrictions.  
Trademarks and model names belong to their respective owners.

---

## Original author

Database originally created and maintained by **[ch-bas](https://github.com/ch-bas)** — [github.com/ch-bas/cctv-camera-database](https://github.com/ch-bas/cctv-camera-database).  
Data is CC0 — copied and redistributed freely as intended.
