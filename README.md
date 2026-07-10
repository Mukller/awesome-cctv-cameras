<div align="center">

[English](README_EN.md) • **Русский**

</div>

# awesome-cctv-cameras

> Открытая база данных 2100+ IP-камер и камер видеонаблюдения — машиночитаемый формат, CC0, без пейволлов.

[![cameras](https://img.shields.io/badge/камер-2101-blue?style=flat-square)](data/cameras.json)
[![brands](https://img.shields.io/badge/брендов-71-green?style=flat-square)](cameras/)
[![license](https://img.shields.io/badge/лицензия-CC0-lightgrey?style=flat-square)](LICENSE)
[![schema](https://img.shields.io/badge/схема-validated-brightgreen?style=flat-square)](schema/camera.schema.json)

Спецификации камер разбросаны по PDF-каталогам производителей, пейволлным базам (IPVM и др.) и страницам ретейлеров в несовместимых форматах. Этот репозиторий сводит их в валидированный JSON — один файл на камеру, один общий массив для запросов, CSV для таблиц.

---

## Содержание

- [Статистика](#статистика)
- [Просмотр онлайн](#просмотр-онлайн)
- [Быстрый старт](#быстрый-старт)
- [Примеры запросов](#примеры-запросов)
- [Схема данных](#схема-данных)
- [Бренды](#бренды)
- [Участие в проекте](#участие-в-проекте)
- [Лицензия](#лицензия)

---

## Статистика

| | |
|---|---|
| Камеры | **2 101** |
| Бренды | **71** |
| Форм-факторы | 11 |
| PoE (проводные) | 1 279 |
| WiFi | 503 |
| Аккумуляторные / беспроводные | 180 |
| 4K / 8MP+ | 545 |
| С конфигами Frigate / HA | 1 507 |

---

## Просмотр онлайн

Веб-интерфейс → **[cctv-database.com](https://cctv-database.com)**

Локально: запустить `docs/` через любой статический сервер и открыть `demo.html` (сборка не нужна).

```bash
cd docs && python3 -m http.server
```

---

## Быстрый старт

```bash
git clone https://github.com/Mukller/awesome-cctv-cameras.git
cd awesome-cctv-cameras
npm install   # только Ajv для валидации схемы
npm run build # валидирует JSON → пишет data/cameras.json + data/cameras.csv
```

### Структура репозитория

```
awesome-cctv-cameras/
├── cameras/              # источник истины — один JSON на камеру, по брендам
│   ├── hikvision/        # 209 камер
│   ├── dahua/            # 156 камер
│   ├── reolink/          # 114 камер
│   └── …68 брендов
├── data/                 # ГЕНЕРИРУЕТСЯ — не редактировать вручную
│   ├── cameras.json      # все 2 101 камера одним массивом
│   └── cameras.csv       # плоская таблица для Excel/Google Sheets
├── schema/
│   └── camera.schema.json
├── scripts/
│   └── build.js
├── configs/              # конфиги для Frigate / Home Assistant
└── docs/
    └── demo.html
```

---

## Примеры запросов

```js
const cameras = require('./data/cameras.json');

// 4K PoE уличные камеры
cameras.filter(c =>
  c.connectivity?.includes('poe') &&
  c.resolution.megapixels >= 8
);

// Цветное ночное видение
cameras.filter(c => c.night_vision?.type === 'color');

// Без абонентской платы
cameras.filter(c =>
  c.features?.some(f => f.toLowerCase().includes('no subscription'))
);

// Рынок UK
cameras.filter(c => c.markets?.includes('UK'));
```

Или открыть `data/cameras.csv` в любой таблице.

---

## Схема данных

Каждая запись соответствует [`schema/camera.schema.json`](schema/camera.schema.json).

**Обязательные поля:**
```json
{
  "id": "reolink-rlc-823a",
  "brand": "Reolink",
  "model": "RLC-823A",
  "type": "bullet",
  "resolution": { "megapixels": 8, "label": "4K UHD" }
}
```

**Основные опциональные поля:**

| Поле | Тип | Пример |
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
| `sources` | `string[]` | ссылки на даташиты |

---

## Бренды

71 бренд во всех сегментах рынка:

| Бренд | Камер | Сегмент |
|---|---|---|
| Hikvision | 209 | Enterprise + потребительский, global |
| Dahua | 156 | Enterprise + потребительский, global |
| Bosch | 153 | Enterprise + тепловизоры, EU/global |
| ACTi | 119 | Enterprise IP, NDAA |
| Reolink | 114 | Prosumer, без подписки |
| EZVIZ | 87 | Потребительский, global |
| ABUS | 85 | Потребительский + профессиональный, DE/AT/CH |
| Axis | 61 | Enterprise premium, global |
| Hi-Focus | 60 | Производство в Индии, BIS |
| Kedacom | 58 | Enterprise, CN/global |
| IMOU | 56 | Потребительский + prosumer, global |
| Tapo | 47 | Бюджетный, global |
| Eufy | 46 | Без подписки, потребительский |
| Hanwha | 45 | Enterprise AI, Korea/global |
| Lorex | 40 | Потребительские NVR-системы, CA/US |
| … | … | … |

Полный список с количеством камер — в [`cameras/`](cameras/).

---

## Участие в проекте

Три способа добавить камеру:

| | Способ | Подходит для |
|---|---|---|
| 🌐 | [Открыть issue](../../issues/new?template=add-camera.yml) | Любой — веб-форма, клонировать не нужно |
| 🧙 | `npm run add` | CLI-мастер, создаёт JSON автоматически |
| 🛠 | Редактировать JSON напрямую | Разработчики — см. [CONTRIBUTING.md](CONTRIBUTING.md) |

```bash
git clone https://github.com/Mukller/awesome-cctv-cameras.git
cd awesome-cctv-cameras && npm install
npm run add   # мастер добавления
npm run build # валидация
```

Нашли ошибку в данных → [issue с исправлением](../../issues/new?template=correction.yml)

---

## Лицензия

Данные — [CC0 1.0](LICENSE) (общественное достояние). Свободно использовать, копировать, распространять без ограничений.  
Товарные знаки и названия моделей принадлежат их владельцам.

---

## Автор оригинала

База данных создана и поддерживается **[ch-bas](https://github.com/ch-bas)** — [github.com/ch-bas/cctv-camera-database](https://github.com/ch-bas/cctv-camera-database).  
Данные CC0 — скопировано и распространяется свободно согласно намерению автора.
