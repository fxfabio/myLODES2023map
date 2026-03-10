# myLODES2023map# 🗽 New York LODES 2023 — Interactive Jobs Map

An interactive, self-contained web map exploring **where people live and work** across all 231,939 Census blocks in New York State, built from the U.S. Census Bureau's [LEHD Origin-Destination Employment Statistics (LODES)](https://lehd.ces.census.gov/data/) dataset.

👉 **[Live map →](https://fxfabio.github.io/myLODES2023map/)**

---

## Preview

![Map preview showing New York State colored by resident job density](preview.png)

*Each dot is a Census block representative point. Color encodes job count or density using the Viridis color scale.*

---

## What This Shows

The map visualizes two complementary datasets at the Census block level — the finest geographic unit in the U.S. Census:

| Layer | Full Name | What it measures |
|-------|-----------|-----------------|
| **RAC** | Residence Area Characteristics | Jobs counted by **where the worker lives** |
| **WAC** | Workplace Area Characteristics | Jobs counted by **where the job is located** |

> "RAC – Residence Area Characteristic data, jobs are totaled by home Census Block. WAC – Workplace Area Characteristic data, jobs are totaled by work Census Block."
> — [LODES Technical Document 8.4](https://lehd.ces.census.gov/data/lodes/LODES8/LODESTechDoc8.4.pdf), p. 1

Both use segment **S000** (all workers, no filter) and job type **JT00** (all jobs — private + federal).

### Variables available

| Variable | Description |
|----------|-------------|
| `rac_s000` | Count of resident-associated jobs per block |
| `wac_s000` | Count of workplace jobs per block |
| `rac_density_km2` | RAC jobs per km² of land area |
| `wac_density_km2` | WAC jobs per km² of land area |
| `log_ratio` | `log((WAC+1)/(RAC+1))` — imbalance index: positive = commercial area, negative = bedroom community |

### Color scale modes

| Mode | Description |
|------|-------------|
| `p1–p99` | Clips the bottom and top 1% — best default for skewed data |
| `p2–p98` | Tighter clip, more contrast in the dense middle range |
| `Full` | True min–max; one extreme block can wash out the rest |
| `Custom` | Set your own min and max values |

---

## Data Sources

| File | Description | Source |
|------|-------------|--------|
| `ny_rac_S000_JT00_2023.csv` | Residence area characteristics | [LODES 8.4](https://lehd.ces.census.gov/data/lodes/LODES8/) |
| `ny_wac_S000_JT00_2023.csv` | Workplace area characteristics | [LODES 8.4](https://lehd.ces.census.gov/data/lodes/LODES8/) |
| `ny_xwalk.csv` | Geography crosswalk (block lat/lon, land area) | [LODES 8.4](https://lehd.ces.census.gov/data/lodes/LODES8/) |

**Data year:** 2023 · **Geography vintage:** 2020 Census blocks · **Format version:** LODES 8.4

> Each point is placed at the block's internal representative point (`blklatdd` / `blklondd` from `ny_xwalk.csv`), which is guaranteed to lie inside the block boundary but is not a centroid.
> — [LODES Technical Document 8.4](https://lehd.ces.census.gov/data/lodes/LODES8/LODESTechDoc8.4.pdf), p. 11

---

## Features

- **231,939 points** rendered in real-time via [deck.gl](https://deck.gl) ScatterplotLayer
- **Interactive tooltip** with RAC/WAC counts, densities, and imbalance ratio
- **5 variables** switchable live
- **Custom color domain** — enter your own min/max to highlight specific ranges
- **Quick zoom** to NYC, Manhattan, Buffalo, Albany, Rochester
- **3 basemaps** — Dark, Light, Street (CartoDB)
- **Export** — download all data as CSV, GeoJSON, or TSV for use in QGIS, Python, R, Kepler.gl, etc.
- Fully **self-contained** — no server needed, works offline after first load

---

## How to Reproduce

Download the raw LODES files directly from the Census Bureau:

```bash
# Residence Area Characteristics
wget https://lehd.ces.census.gov/data/lodes/LODES8/ny/rac/ny_rac_S000_JT00_2023.csv.gz

# Workplace Area Characteristics
wget https://lehd.ces.census.gov/data/lodes/LODES8/ny/wac/ny_wac_S000_JT00_2023.csv.gz

# Geography crosswalk (block coordinates + land area)
wget https://lehd.ces.census.gov/data/lodes/LODES8/ny/ny_xwalk.csv.gz

# Decompress
gunzip *.gz
```

The map data was then prepared by joining RAC/WAC counts to the crosswalk on `h_geocode` / `w_geocode` → `tabblk2020`, keeping `blklondd`, `blklatdd`, and `aland` (land area in m²).

---

## Tech Stack

| Library | Version | Role |
|---------|---------|------|
| [MapLibre GL JS](https://maplibre.org/) | 4.7.1 | Base map rendering |
| [deck.gl](https://deck.gl/) | 9.0.27 | High-performance point layer |
| [CartoDB Basemaps](https://github.com/CartoDB/basemap-styles) | — | Tile layers (Dark Matter, Positron, Voyager) |

All JavaScript libraries are loaded from CDN. No build step required.

---

## Deploy Your Own

1. Fork this repo
2. Replace `index.html` with your own version (or use the one here)
3. Go to **Settings → Pages → Deploy from branch → main / root**
4. Your map is live at `https://YOUR-USERNAME.github.io/REPO-NAME/`

---

## Citation

> U.S. Census Bureau, Center for Economic Studies. *LEHD Origin-Destination Employment Statistics (LODES), Dataset Structure Format Version 8.4.* Washington, DC: U.S. Census Bureau, 2025. Available at: https://lehd.ces.census.gov/data/lodes/LODES8/LODESTechDoc8.4.pdf

---

## License

The LODES data is public domain (U.S. Government work). The map code in this repository is released under the [MIT License](LICENSE).
