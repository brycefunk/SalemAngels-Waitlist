# Salem Angels Waitlist Map

An interactive map built with [MapLibre GL JS](https://maplibre.org/) and [MapTiler](https://www.maptiler.com/) that visualizes **Salem Angels’ waitlist families**.  
The map pulls live data from a **Google Sheet** (published as CSV), places each family at an approximate location (neighborhood centroids + jitter for privacy), and displays details in branded popups.  

This project can also be reused by **other Angels chapters** as a template.

---

## ✨ Features
- **Live Data** — families are stored in a Google Sheet, not hard-coded.
- **Privacy First** — locations use neighborhood centroids + random jitter (no real addresses).
- **Program Icons** — different icons for *LoveBox* and *Dare to Dream*.
- **Urgency Highlighting** — colored halo around pins (red = high, orange = medium, green = low).
- **Custom Branding** — Montserrat font, styled popup background, supports Salem Angels palette.
- **Embeddable** — hosted via GitHub Pages, easily embedded in Flipcause or other sites with `<iframe>`.

---

## 📂 Project Structure
```
SalemAngels-Waitlist/
│
├── index.html                # Main map page
├── data/
│   └── area_centroids_salem.json   # Neighborhood → [lng,lat] centroids
├── assets/
│   └── icons/
│       ├── lovebox-pin.png
│       └── daretodream-pin.png
└── README.md                 # This file
```

---

## 📝 Data Source (Google Sheet)
The map pulls data from a **published Google Sheet** in CSV format.  

### Required headers:
```
id,display_name,program,summary,city,area,needs,urgency,status,lat,lon
```

### Column meanings:
- **id**: Unique row ID (any string).
- **display_name**: Label for popup (e.g., “Family A”).
- **program**: `LoveBox` or `Dare to Dream`.
- **summary**: Short description (quotes required if commas are inside).
- **city**: City name (for display only).
- **area**: Neighborhood name (must match a key in `area_centroids_salem.json`).
- **needs**: Semicolon-separated list (`meals;transport;playdates`).
- **urgency**: `high`, `medium`, or `low`.
- **status**: Current family status (e.g., `waitlist`).
- **lat, lon** *(optional)*: Override coordinates for public facilities. If empty, uses `area` centroid + jitter.

### Publish instructions:
1. In Google Sheets: **File → Share → Publish to web**.  
2. Choose **CSV** format for the sheet tab.  
3. Copy the link and paste into `SHEET_CSV_URL` in `index.html`.

---

## 🗺️ Map Configuration
In `index.html`:

```js
const MAPTILER_KEY = "YOUR_MAPTILER_KEY";
const SHEET_CSV_URL = "YOUR_PUBLISHED_CSV_URL";
```

- [Get a MapTiler key](https://cloud.maptiler.com/maps/) (free tier works).
- Restrict key to your domain in MapTiler dashboard for security.
- Change map style by replacing `basic` with another MapTiler style (`outdoor`, `topo`, `bright`, `satellite`, etc.).

---

## 🔒 Privacy and Security
- **Addresses are never stored.** Families are mapped by area centroid + random jitter (300–600m).  
- **Sensitive data stays in Google Sheets.** Only display-safe fields are published to the map.  
- **GitHub Repo Access**:  
  - Keep your repo **private** if this is your production site.  
  - Use branch protection on `main` so only approved changes can deploy.  
- **Sharing with other chapters**:  
  - Create a **template repo** with placeholders (fake Sheet URL + MapTiler key).  
  - Other chapters can click **“Use this template”** → they get their own copy.  
  - This prevents anyone from editing your live map.

---

## 🌐 Deployment
1. Push changes to GitHub.  
2. In repo settings → **Pages**, set branch = `main` (or `gh-pages` if you prefer).  
3. GitHub will serve the site at:  
   ```
   https://<your-username>.github.io/SalemAngels-Waitlist/
   ```

---

## 📦 Embedding
Embed the map in Flipcause (or any site) using an iframe:

```html
<div style="position:relative;padding-top:56.25%;width:100%;max-width:1000px;margin:0 auto;">
  <iframe
    src="https://<your-username>.github.io/SalemAngels-Waitlist/"
    style="position:absolute;top:0;left:0;width:100%;height:100%;border:0;"
    loading="lazy"
    referrerpolicy="no-referrer-when-downgrade"
    allowfullscreen>
  </iframe>
</div>
```

---

## ⚙️ Development Workflow
```bash
# clone the repo
git clone https://github.com/<username>/SalemAngels-Waitlist.git
cd SalemAngels-Waitlist

# open in VS Code
code .

# check repo status
git status

# stage & commit changes
git add .
git commit -m "update popup styling"

# push to GitHub
git push origin main
```

If you see “dubious ownership” errors on Windows (working from a network share), run:
```bash
git config --global --add safe.directory "<path-to-your-repo>"
```

---

## ✅ To-Do / Future Enhancements
- Add **filters** (checkboxes to toggle LoveBox / Dare to Dream, urgency levels).  
- Add clustering if there are many pins in one area.  
- Add auto-refresh interval for near-real-time updates.  
- Add brand-matching popup theme (colors from Salem Angels palette).  
