# WattWatch

Campus buildings waste a lot of electricity on lights, fans and ACs running in empty classrooms and labs, and nobody can see where the energy goes. WattWatch is a live dashboard that tracks energy use per building and room, catches waste automatically, predicts tomorrow's load and nudges people to save.

![Explore view](docs/screenshots/explore.png)

## Run it

It is a single page with no build step.

- Open `index.html` in any browser, or
- turn on GitHub Pages for this repository (Settings → Pages → Deploy from branch → `main` / root) and open the link it gives you.

All readings come from a built-in simulator, so the page works without any hardware.

## What it does

| Page | What you see |
|---|---|
| **Explore** | Every room as a card with its live status, cost per hour and a drawing that shows lights, fans, AC and people as they are right now, next to a campus map coloured by load |
| **Room** | Live power, today's cost by hour, occupancy from the PIR sensor and timetable, and a **Turn off power** button that opens the relay |
| **Alerts** | Rooms empty with power on for 15 minutes, with a preview of the Telegram message the building in-charge receives |
| **Insights** | Cost by building, peak hours, tomorrow's forecast and unusual readings |
| **Planner** | "What if" sliders for switch-off rules, AC temperature and sensor coverage, with ₹ saved, CO₂ avoided and payback period |
| **Hostels** | Weekly hostel leaderboard ranked by savings against each hostel's own baseline |

### Make it your campus

Open the menu under your initials (top right):

- **Manage rooms / Add a room**: add, edit or remove classrooms, labs and offices, including their lights, fans, ACs and timetable.
- **Site and campus names**: rename the site, add your name or team, your college and the six buildings.

Changes are saved in your browser. Clear the site's data to go back to the sample campus.

### Presenter controls

Press **D** (or use the button in the corner) to speed up time (1×, 10×, 60×), trigger scenarios such as a room left on, a lab running overnight or a faulty AC, and switch on automatic shutoff.

## Hardware plan

```
[ESP32 + SCT-013 current clamp + PIR] → MQTT (HiveMQ Cloud) → [FastAPI + InfluxDB] → [Dashboard]
                                                                    ↓
                                                  [Prophet forecast + Isolation Forest anomalies]
                                                                    ↓
                                                        [Telegram alerts · relay shutoff]
```

About ₹800 of hardware per room. PIR sensors report presence only. There are no cameras.

The simulator lives in the page's script and exposes the same room state a real MQTT feed would fill, so live sensor data can replace it room by room.

## Pitch deck

[`docs/WattWatch-Pitch.pptx`](docs/WattWatch-Pitch.pptx): 7 slides with speaker notes.

| Room page | Alerts |
|---|---|
| ![Room page](docs/screenshots/room.png) | ![Alerts](docs/screenshots/alerts.png) |

Figures on the site and in the deck come from the simulation (₹9 per kWh, 0.72 kg CO₂ per kWh). Replace them with your campus's real bills before presenting.
