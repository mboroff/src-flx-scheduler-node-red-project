nsrc-flex-scheduler
===================

Web-based scheduler/dashboard for remotely activating access to a set of shared amateur radio stations. Built with Node-RED and Node-RED Dashboard.

### About

This project provides a web dashboard that lets club members remotely "activate" one of several shared radio stations for a SmartLink session. Requesting a station sends an MQTT command to the remote site's controller, which powers up the station; a live countdown ("SmartLink will be available in N seconds") is shown on the dashboard while the station comes online, followed by a "SmartLink Available" status once it's ready to connect to.

### Stations

Each station has its own flow tab that publishes an activation command on `<Station>/send` and listens for status updates on `<Station>/recv` over MQTT:

- **MunEndfed** — Mundelein End-Fed antenna station
- **MunAV640** — Mundelein AV640 station
- **Skokie**
- **Northbrook**
- **Northfield** — currently marked "Not Available For Activation" (offline/not wired up yet)

For each station there are two dashboard variants:
- An **"All"** tab (e.g. `MunEndfed`, `Skokie`) intended for a combined/multi-station dashboard page.
- A **"Single"** tab (e.g. `Endfed Single`, `Skokie Single`) that presents the same activate button, status text, and countdown as a standalone widget, for embedding on its own page, along with a header showing the current date/time.

### Raspberry Pi Monitoring (V2 Raspberry monitoring)

A system health tab for the Raspberry Pi hosting Node-RED, showing:
- CPU temperature (`vcgencmd measure_temp`)
- CPU load (`top`)
- Memory usage (`free`)
- Disk usage (`df -h`)
- The Pi's local IP address(es)

These are displayed as dashboard gauges/text, refreshed on inject/interval.

### Dependencies

Defined in `package.json`:
- `@flowfuse/node-red-dashboard` / `node-red-dashboard` — the web dashboard UI
- `@colinl/node-red-dashboard-2-ui-gauge-classic` — classic-style gauges used on the Pi monitoring tab
- `node-red-contrib-string` — string parsing (e.g. extracting IP addresses)
- `node-red-contrib-unit-converter` — unit conversion used by the monitoring gauges

### Notes

The flow also contains a couple of empty/placeholder tabs ("Flow 1", "Flow 2") left over from development, which can be cleaned up or repurposed.

### Relationship to the club's PHP site

This repo is the Node-RED half of the North Shore Radio Club Flex-Cadre setup. Members don't come here directly — they log in through a separate PHP web site (login, account management, and the scheduling calendar/reservation grid) which is the actual front door on the Pi.

That site's `radio_control.php` page shows one button per radio; after confirming the current hour isn't already reserved by someone else (via its own reservation database), it opens this project's Node-RED dashboard in a new tab — that's the "Activate" / countdown / "SmartLink Available" flow described below. `NODE_RED_URL` in the PHP site's `config.php` points at this dashboard (e.g. `http://<pi-ip>:1880/dashboard/radio-activation`) and needs to stay in sync with whatever dashboard path is exposed here.

The PHP site also has its own super-admin-only "Pi Status" page with live temperature/CPU/memory/disk gauges and Reboot/Shutdown buttons — implemented independently in PHP (reading `/sys/class/thermal/thermal_zone0/temp` directly rather than shelling out), not by embedding the **V2 Raspberry monitoring** tab below. The two cover similar ground but aren't the same code; keep that in mind if one is changed and the other should match.

### Stations

Each station has its own flow tab that publishes an activation command on `<Station>/send` and listens for status updates on `<Station>/recv` over MQTT:

- **MunEndfed** — Mundelein End-Fed antenna station
- **MunAV640** — Mundelein AV640 station
- **Skokie**
- **Northbrook**
- **Northfield** — currently marked "Not Available For Activation" (offline/not wired up yet)

For each station there are two dashboard variants:
- An **"All"** tab (e.g. `MunEndfed`, `Skokie`) intended for a combined/multi-station dashboard page.
- A **"Single"** tab (e.g. `Endfed Single`, `Skokie Single`) that presents the same activate button, status text, and countdown as a standalone widget, for embedding on its own page, along with a header showing the current date/time.

### Raspberry Pi Monitoring (V2 Raspberry monitoring)

A system health tab for the Raspberry Pi hosting Node-RED, showing:
- CPU temperature (`vcgencmd measure_temp`)
- CPU load (`top`)
- Memory usage (`free`)
- Disk usage (`df -h`)
- The Pi's local IP address(es)

These are displayed as dashboard gauges/text, refreshed on inject/interval.

### Dependencies

Defined in `package.json`:
- `@flowfuse/node-red-dashboard` / `node-red-dashboard` — the web dashboard UI
- `@colinl/node-red-dashboard-2-ui-gauge-classic` — classic-style gauges used on the Pi monitoring tab
- `node-red-contrib-string` — string parsing (e.g. extracting IP addresses)
- `node-red-contrib-unit-converter` — unit conversion used by the monitoring gauges

### Notes

The flow also contains a couple of empty/placeholder tabs ("Flow 1", "Flow 2") left over from development, which can be cleaned up or repurposed.
