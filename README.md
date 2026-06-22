# CTS-Production

Web application suite for **City Technical Solutions (CTS)** — a Malaysian IoT device manufacturer. Deployed on GitHub Pages, backed by Firebase Firestore. Covers worker attendance, fleet energy monitoring, inventory, and device documentation for operations in Malaysia, Singapore, Australia, and Hong Kong.

---

## Pages

| File | Title | Purpose |
|------|-------|---------|
| `index.html` | Operations Status Page | Login portal (Worker / Admin roles), customer banner carousel, project status tiles |
| `dashboard.html` | CTS Production — Dashboard | Worker attendance: clock-in/out (up to 4 sessions), shift management, OT/rest day logic, PDF/CSV/WhatsApp export |
| `admin.html` | CTS Production — Admin Panel | Worker management, PIN management, Activity Log, Manuals tab, App Reference |
| `energy_dashboard.html` | Energy Eye · Live Dashboard | Live energy data from Azure Functions API; kW/kWh charting, SG & AU region support, TNB tariff tooltips |
| `inventory.html` | CTS Production — Inventory | Stock item tracking, movement history, PDF reports |
| `manual.html` | CTS Production — Device Manuals | In-app viewer for Energy Eye and DoD installation PDFs |
| `teraterm_dashboard.html` | CTS Energy Eye — TeraTerm Log Dashboard | Parses TeraTerm serial logs; RTC-anchored timestamps, peak/off-peak kWh charts, TNB tariff, PWA |

---

## Tech Stack

- **Frontend:** Vanilla HTML/CSS/JS — single-file pages, no build step
- **Backend:** Firebase Firestore (project: `cts-production-7069c`)
- **Charts:** Chart.js 4.4, jsPDF + jspdf-autotable
- **Energy API:** Azure Functions (`energy-eye-functions-prod`)
- **Holiday data:**
  - SG / HK / AU — [date.nager.at](https://date.nager.at) API (auto-updating)
  - MY — `holidays_my.json` in this repo (manually maintained, with embedded fallback)
- **Deployment:** GitHub Pages (access restricted to `*.github.io` and `localhost`)

---

## Deployment

All pages are static files deployed via GitHub Pages. No build or server required.

Access is gated at the JS level — pages reject requests from non-`github.io` / non-`localhost` hosts.

> ⚠️ **Security note:** Firestore rules are currently set to `if true` (open read/write). Role gating is enforced client-side via `sessionStorage`. Before exposing publicly, deploy restrictive Firestore security rules via the Firebase Console and consider a Cloudflare Worker proxy for the Azure Functions API key.

---

## Holiday Data (Malaysia)

`holidays_my.json` contains Malaysian federal + KL state public holidays for 2026–2027. To add a new year:

1. Open `holidays_my.json` in the GitHub web editor.
2. Append a new key under `"years"` using the format:
   ```json
   "2028": [
     { "date": "2028-01-01", "name": "New Year" }
   ]
   ```
3. Commit and push. The dashboard picks it up on next refresh.

---

## Shift Configuration

`dashboard.html` supports three shift modes:

| Mode | Hours |
|------|-------|
| Day Shift A | 08:00 – 17:00 |
| Day Shift B | 09:00 – 18:00 |
| Flex | Configurable |

Attendance grace period: **1–5 min** = On Time · **6–15 min** = Late (minor).

---

## Energy Eye Device

The `EnergyEye_1NCE.bin` firmware file and `manual-ee.pdf` / `manual-dod.pdf` are included in this repo. The device is deployed across Singapore and Australia via City FM, using 1NCE SIM (LTE-M roaming).

Data structure from the Azure API: 12 interval buckets per frame · `kW = kWh × 12` · `DateTime = timestamp + (bucketNum − 1) × 5 min`.

---

## Repository Structure

```
CTS-Production/
├── index.html               # Login / Operations Status
├── dashboard.html           # Worker Attendance
├── admin.html               # Admin Panel
├── energy_dashboard.html    # Energy Eye Live Dashboard
├── inventory.html           # Inventory Management
├── manual.html              # Device Manuals viewer
├── teraterm_dashboard.html  # TeraTerm Log Dashboard (PWA)
├── holidays_my.json         # Malaysia public holidays (editable)
├── manual-ee.pdf            # Energy Eye installation guide
├── manual-dod.pdf           # DoD installation guide
└── EnergyEye_1NCE.bin       # Device firmware
```

---

*Designed in Malaysia · deployed in SG · AU · HK · since 2024*
