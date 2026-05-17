# Minimalistic Weather Card

A compact, single-row Lovelace card for Home Assistant, designed for the Section view layout. It shows the current weather condition, temperature, high/low forecast, and optional sensor data (rain, wind) — all in a clean two-row grid that aligns neatly with other cards.

<!-- Screenshots go here -->

## Features

- Colorful weather icons matching the native HA weather card
- Current temperature and today's high/low (via the `weather/subscribe_forecast` API)
- Optional rain, wind speed, and wind direction sensors
- Tap, hold, and double-tap actions
- Full UI editor support
- Supports multiple languages (English, Danish, German, French, Spanish)

## Installation

### Manual

1. Copy `www/mini-weather-card/minimalistic-weather-card.js` to your HA `config/www/mini-weather-card/` folder.
2. Add it as a Lovelace resource:
   - Go to **Settings → Dashboards → Resources** (three-dot menu)
   - Click **Add Resource**
   - URL: `/local/mini-weather-card/minimalistic-weather-card.js`
   - Type: **JavaScript module**
3. Reload the browser.

### HACS

1. Open HACS and go to **Frontend**.
2. Click the three-dot menu → **Custom repositories**.
3. Add this repository URL and select category **Lovelace**.
4. Install **Minimalistic Weather Card** and reload the browser.

## Configuration

### Minimal

```yaml
type: custom:minimalistic-weather-card
entity: weather.home
```

### Full example

```yaml
type: custom:minimalistic-weather-card
entity: weather.home
name: Outside
rain_entity: sensor.rain_today
wind_speed_entity: sensor.wind_speed
wind_bearing_entity: sensor.wind_bearing
tap_action:
  action: more-info
hold_action:
  action: none
double_tap_action:
  action: none
```

## Options

| Option | Type | Default | Description |
|---|---|---|---|
| `entity` | string | **required** | A `weather.*` entity |
| `name` | string | Entity friendly name | Label shown in the top-left |
| `rain_entity` | string | — | Sensor entity for today's rain accumulation |
| `wind_speed_entity` | string | — | Sensor entity for wind speed |
| `wind_bearing_entity` | string | — | Sensor entity for wind bearing in degrees (0–360) |
| `tap_action` | action | `more-info` | Action on single tap |
| `hold_action` | action | `none` | Action on long press (≥ 500 ms) |
| `double_tap_action` | action | `none` | Action on double tap |

### Action options

| Key | Description |
|---|---|
| `action: more-info` | Opens the More Info dialog for the weather entity |
| `action: navigate` | Navigates to `navigation_path` |
| `action: url` | Opens `url_path` in a new tab |
| `action: none` | Does nothing |

```yaml
tap_action:
  action: navigate
  navigation_path: /lovelace/weather

hold_action:
  action: url
  url_path: https://yr.no
```

## Layout

```
┌─────────────────────────────────────────────────────┐
│  ⛅  OUTSIDE              2.6 mm        13°C        │
│       Partly cloudy       ▼ S · 21 km/h  13° / 11°  │
└─────────────────────────────────────────────────────┘
```

- **Top row:** name, optional rain, current temperature
- **Bottom row:** condition, optional wind direction + speed, high/low forecast

## Development

The dev environment uses a Home Assistant instance served by `scripts/develop`. The card JS is served from `config/www/` (symlinked to `www/`) and registered in `config/.storage/lovelace_resources`.

```bash
# Start Home Assistant
scripts/develop
```

After editing `www/mini-weather-card/minimalistic-weather-card.js`, bump the `?v=N` cache-busting parameter in `config/.storage/lovelace_resources` and restart HA to force browsers to reload the new file.

## License

MIT
