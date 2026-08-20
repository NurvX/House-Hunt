# House Hunt

A set of macOS **Quick Actions / Services** (built with Automator) for speeding up house-hunting research on macOS. Each workflow takes a selected address (plain text, selected anywhere on the Mac — Finder, browser, Notes, etc.) and opens it in a relevant map, listing, or search tool via the right-click **Services** menu.

## What's included

| Workflow | Services menu label | What it does |
|---|---|---|
| `AIO_Buyers_Search.workflow` | AIO_Buyers_Search | Runs a local web server serving `viewer.html` and opens a combined LINZ/Suncalc map viewer for the selected address. |
| `Open in Offline LINZ CRoSL Map.workflow` | Open in Offline LINZ CRoSL Map | Same local-server approach, opens `viewer.html` pointed at the LINZ CRoSL (Certificate of Title / property) map for the address. |
| `Open in LINZ CRoSL Map.workflow` | LINZ | Opens the selected address directly in the LINZ CRoSL ArcGIS web map viewer (online). |
| `Open in Suncalc.workflow` | Open in Suncalc | Geocodes the selected address (via Esri/ArcGIS) and opens it in [suncalc.org](https://www.suncalc.org) to check sun position/shading for the property. |
| `Directions to Britomart.workflow` | Directions to Britomart | Opens Google Maps directions from the selected address to Britomart, Auckland. |
| `Search on SOLD TradeMe.workflow` | Search on TradeMe | Opens the selected address on TradeMe Property Insights (sold price history). |

`viewer.html` is the map viewer page used by the two "offline"/local-server workflows above — it's served locally via `python3 -m http.server` from this folder.

## Requirements

- macOS with Automator-compatible Services (macOS 10.6+)
- `python3` available on `PATH` (used by the local-server workflows to serve `viewer.html`)
- Internet access for the map/search services (LINZ, TradeMe, Google Maps, Suncalc, Esri geocoder)

## Installation — where files go on macOS

macOS loads Services from either:

- **Per-user:** `~/Library/Services/` (i.e. `/Users/<your-username>/Library/Services/`)
- **System-wide (all users):** `/Library/Services/` (requires admin rights)

To install:

1. Clone or download this repo.
2. Copy (or move) each `*.workflow` folder into `~/Library/Services/`.
3. Copy `viewer.html` into the **same folder** the local-server workflows point to — by default that's also `~/Library/Services/` (the two "offline"/local-server workflows serve whatever folder they're configured with via a `PROJECT_DIR` variable baked into the workflow; edit this in Automator if you place things elsewhere).
4. Log out/in, or run `killall pboard` / relaunch Finder, so macOS picks up the new Services (usually near-instant, but a relaunch of Finder/the frontmost app forces a refresh).
5. Select an address (as plain text) anywhere on your Mac, then go to the app menu → **Services**, or right-click the selection → **Services**, and choose the workflow you want.

You can also open a workflow in **Automator.app** (double-click it) to inspect or edit it — e.g. to change the `PROJECT_DIR` path, default region/suburb assumptions, or destination address for the Britomart directions workflow.

## Notes

- These workflows call out to third-party services (LINZ, TradeMe, Google Maps, Suncalc, Esri) — behavior depends on those services staying available and their URL formats staying stable.
- The local-server workflows start a background `python3 -m http.server` process on first use; it's left running to serve subsequent requests quickly.
