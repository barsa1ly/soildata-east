# soildata-east

A static copy of the USDA-NRCS **Soil Survey Geographic Database (SSURGO)**, fiscal year 2027 (published by NRCS on
1 October 2026), for **24 states and the District of Columbia**: Alabama, Connecticut, District of Columbia, Delaware, Florida, Georgia, Indiana, Kentucky, Massachusetts, Maryland, Maine, Michigan, Mississippi, North Carolina, New Hampshire, New Jersey, New York, Ohio, Pennsylvania, Rhode Island, South Carolina, Tennessee, Virginia, Vermont and West Virginia. SSURGO is public domain.

The copy of the 48 states and DC is split by region over four repositories with the same layout, so that each one is a
GitHub Pages site of less than 1 GB:

| Repository | States |
|---|---|
| [soildata](https://github.com/barsa1ly/soildata) | AR LA NM OK TX |
| [soildata-west](https://github.com/barsa1ly/soildata-west) | AZ CA CO ID MT NV OR UT WA WY |
| [soildata-central](https://github.com/barsa1ly/soildata-central) | IA IL KS MN MO ND NE SD WI |
| [soildata-east](https://github.com/barsa1ly/soildata-east) | AL CT DC DE FL GA IN KY MA MD ME MI MS NC NH NJ NY OH PA RI SC TN VA VT WV |

The [SoilGuard](https://barsa1ly.github.io/soilguard/) app reads it as a **backup**: it asks USDA's Soil Data Access
(SDA) first, and reads these files only when SDA does not answer. The copy holds what the app asks SDA for (its
`js/soil.js`, `batch()` and `tablesAt()`): the map unit at a point, and that map unit's rows of eight tables, with the
same columns, in the same order, and every value a string, as SDA's JSON gives them. The app's Basin tab also counts
the copy's cells inside a watershed (`backupIn()`) for its soil loss and runoff when SDA does not answer.

## States in this repository

| Code | State | Survey areas | Map units | Components | Horizons | 30 m tiles | 10 m pixels kept | Size (MB) | Tables from |
|---|---|---|---|---|---|---|---|---|---|
| AL | Alabama | 67 | 4,473 | 8,539 | 26,404 | 634 | 94.5% | 31.3 | state GeoPackage |
| CT | Connecticut | 2 | 633 | 3,668 | 15,934 | 84 | 92.5% | 6.4 | state GeoPackage |
| DC | District of Columbia | 1 | 129 | 426 | 1,238 | 5 | 93.4% | 0.3 | state GeoPackage |
| DE | Delaware | 3 | 407 | 1,768 | 6,164 | 44 | 95.7% | 2.1 | state GeoPackage |
| FL | Florida | 74 | 3,962 | 19,171 | 67,818 | 839 | 96.4% | 31.8 | state GeoPackage |
| GA | Georgia | 61 | 3,403 | 4,647 | 17,155 | 519 | 93.5% | 22.9 | state GeoPackage |
| IN | Indiana | 92 | 7,245 | 20,691 | 71,374 | 459 | 91.3% | 35.7 | state GeoPackage |
| KY | Kentucky | 84 | 4,914 | 22,229 | 42,309 | 516 | 92.7% | 31.6 | state GeoPackage |
| MA | Massachusetts | 19 | 2,082 | 8,545 | 33,999 | 155 | 94.8% | 10.5 | state GeoPackage |
| MD | Maryland | 25 | 2,807 | 10,035 | 36,565 | 183 | 93.6% | 12.9 | state GeoPackage |
| ME | Maine | 20 | 1,864 | 10,537 | 45,803 | 447 | 96.1% | 19.2 | state GeoPackage |
| MI | Michigan | 82 | 10,664 | 36,240 | 165,958 | 823 | 94.7% | 51.2 | state GeoPackage |
| MS | Mississippi | 82 | 4,089 | 10,721 | 37,006 | 600 | 94.6% | 29.1 | state GeoPackage |
| NC | North Carolina | 100 | 6,209 | 16,792 | 58,646 | 662 | 94.1% | 37.6 | state GeoPackage |
| NH | New Hampshire | 11 | 1,397 | 6,856 | 29,217 | 137 | 95.3% | 8.8 | state GeoPackage |
| NJ | New Jersey | 21 | 2,144 | 7,613 | 37,707 | 123 | 95.3% | 8.6 | state GeoPackage |
| NY | New York | 65 | 8,979 | 47,989 | 102,934 | 656 | 94.2% | 41.3 | state GeoPackage |
| OH | Ohio | 88 | 10,813 | 41,044 | 87,402 | 515 | 92.2% | 40.3 | state GeoPackage |
| PA | Pennsylvania | 67 | 7,261 | 24,393 | 97,661 | 558 | 93.2% | 39.9 | state GeoPackage |
| RI | Rhode Island | 1 | 170 | 814 | 3,612 | 30 | 94.9% | 1.4 | state GeoPackage |
| SC | South Carolina | 48 | 2,822 | 5,196 | 17,801 | 401 | 94.7% | 19.4 | state GeoPackage |
| TN | Tennessee | 94 | 6,870 | 14,605 | 42,674 | 544 | 92.4% | 35.8 | state GeoPackage |
| VA | Virginia | 108 | 8,534 | 13,401 | 44,095 | 548 | 92.8% | 34.7 | state GeoPackage |
| VT | Vermont | 14 | 1,632 | 8,193 | 32,870 | 137 | 94.6% | 9.8 | state GeoPackage |
| WV | West Virginia | 45 | 3,260 | 4,940 | 19,456 | 335 | 93.9% | 17.1 | state GeoPackage |

580 MB in all. *10 m pixels kept*: the share of the state's 10 m pixels whose map unit is the one their 30 m
cell keeps (see the raster below).

## Layout

```
<ST>/meta.json              source, fiscal year, survey dates, counts, build date, tool
<ST>/mukeys.json            [null, mukey, mukey, ...]: raster index -> mukey (index 0 = no map unit)
<ST>/mu/<k>.json.gz         map unit records, gzipped, k = mukey % 1024 (1024 files)
<ST>/r30/index.json         the 30 m raster's tile grid
<ST>/r30/<row>/<col>.bin    raster tiles, 512 x 512 cells (a tile with no map unit is not written)
```

The tools that build it are in the [soildata](https://github.com/barsa1ly/soildata/tree/main/tools) repository.

### Map unit records (`<ST>/mu/<k>.json.gz`, gzip: inflate, then parse as JSON)

```
{"<mukey>": {"mukey": 391100, "mu": {...}, "comps": [...], "hz": [...], "areas": [...],
             "eco": [...], "ylds": [...], "plants": [...], "restr": [...]}, ...}
```

The shape `tablesAt()` returns: `mukey` a number, `mu` one row object, the rest arrays of row objects whose keys are
the columns of the app's SELECTs with the table prefix dropped (`mu.mukey` is `mukey`, `tg.texture` is `texture`).

| key | rows | order |
|---|---|---|
| `mu` | `mapunit` with its `legend`, the survey's date (`sacatalog.saverest`) and `muaggatt` | one row |
| `comps` | `component` | `comppct_r` descending, ties by `cokey` |
| `hz` | `chorizon` with its representative texture group (`chtexturegrp`, `rvindicator = 'Yes'`), every component's | `cokey`, then `hzdept_r` |
| `areas` | `muaoverlap` with `laoverlap` (counties, MLRA, USLE climate and rainfall factor areas, ...) | `areaovacres` descending |
| `eco`, `ylds`, `plants`, `restr` | `coecoclass`, `cocropyld`, `coeplants`, `corestrictions` | `cokey`, then the table's own key |

Values are strings the way SDA writes them: whole numbers without `.0` (`"9"`, `"85"`), other numbers as written
(`"0.3"`, `"617.3"`), text as it is (`".37"`, `"Loamy fine sand"`), the survey date as `"9/3/2026 1:37:01 PM"`;
`null` where there is no value. Two horizon columns the apps ask SDA for are not in NRCS's download and are `null`
for every horizon: `fragvoltot_r` (total rock fragment volume) and `esp_r` (exchangeable sodium percentage).

### Map unit raster (`<ST>/r30/`)

`index.json`:

```
{"crs": "EPSG:5070", "res": 30, "tile": 512, "x0": ..., "y0": ..., "rows": ..., "cols": ...,
 "have": ["0/8", "0/9", ...], "format": "...", "resample": "mode"}
```

* The grid is NAD83 / Conus Albers (EPSG:5070, equal area, as NRCS's raster), 30 m cells, in tiles of 512 x 512 cells
  (15,360 m) whose origin (`x0`, `y0`: the left edge of tile column 0 and the top edge of tile row 0) is a multiple of
  15,360 m, so every state's grid lines up with the others'. `have` lists the tiles written, as `"row/col"`.
* Each `.bin` holds 512 x 512 unsigned 16-bit little-endian integers, row by row from the north-west corner,
  compressed with zlib (what `DecompressionStream("deflate")` reads). A value is an index into the state's
  `mukeys.json`; 0 means no map unit.
* **Why 30 m.** NRCS maps the soils at 10 m (`muraster_10m_<ST>.tif`). At 10 m the 48 states would take about 4 GB,
  more than four free GitHub Pages sites hold; at 30 m they take about 1.6 GB, in four sites of less than 1 GB. Each
  30 m cell takes the 9 pixels whose centres lie in it and keeps the map unit that covers most of them (pixels with
  no map unit do not count, so a cell is empty only when all 9 are; a tie goes to the centre pixel's map unit). The
  map unit at a point can therefore differ from NRCS's near the edge of a map unit, and map units narrower than a
  cell (strips along streams, for example) can be missed: see *10 m pixels kept* above.

## Looking up a point

1. Project longitude and latitude to EPSG:5070 (GRS 1980; standard parallels 29.5° N and 45.5° N; origin 23° N,
   96° W; no false easting or northing). Coordinates in WGS 84 can be used as they are (NAD83 differs by about a metre).
2. The tile is column `floor((x - x0) / (res * tile))`, row `floor((y0 - y) / (res * tile))`; if `"row/col"` is not in
   `have`, there is no map unit at the point.
3. The cell is column `floor((x - x0) / res)` and row `floor((y0 - y) / res)`; inside the tile it is at
   `(row mod 512) * 512 + (col mod 512)`, two bytes each.
4. `mukeys.json[index]` is the mukey (`null` for index 0), and `mu/<mukey % 1024>.json.gz` (gzip) holds its record under
   `"<mukey>"`.

In JavaScript (`base` is the state folder, such as `https://barsa1ly.github.io/soildata-east/AL`):

```js
// longitude, latitude (degrees) -> EPSG:5070 x, y (m): Snyder (1987), Map Projections: A Working Manual, eqs. 14-1 to 14-6
function albers5070(lon, lat) {
  const a = 6378137, f = 1 / 298.257222101, e2 = f * (2 - f), e = Math.sqrt(e2), r = Math.PI / 180;
  const m = p => Math.cos(p) / Math.sqrt(1 - e2 * Math.sin(p) ** 2);
  const q = p => { const s = Math.sin(p); return (1 - e2) * (s / (1 - e2 * s * s) - Math.log((1 - e * s) / (1 + e * s)) / (2 * e)); };
  const p1 = 29.5 * r, p2 = 45.5 * r, n = (m(p1) ** 2 - m(p2) ** 2) / (q(p2) - q(p1)), C = m(p1) ** 2 + n * q(p1);
  const rho = a * Math.sqrt(C - n * q(lat * r)) / n, rho0 = a * Math.sqrt(C - n * q(23 * r)) / n, t = n * (lon + 96) * r;
  return [rho * Math.sin(t), rho0 - rho * Math.cos(t)];
}

// the record at a point ({ mukey, mu, comps, hz, areas, eco, ylds, plants, restr }), or null where there is no map unit
async function soilAt(base, lat, lon) {
  const get = async p => { const res = await fetch(`${base}/${p}`); if (!res.ok) throw new Error(`${p}: HTTP ${res.status}`); return res; };
  const ix = await (await get("r30/index.json")).json();
  const [x, y] = albers5070(lon, lat);
  const col = Math.floor((x - ix.x0) / ix.res), row = Math.floor((ix.y0 - y) / ix.res);
  const tr = Math.floor(row / ix.tile), tc = Math.floor(col / ix.tile);
  if (!ix.have.includes(`${tr}/${tc}`)) return null;
  const body = (await get(`r30/${tr}/${tc}.bin`)).body.pipeThrough(new DecompressionStream("deflate"));
  const tile = new DataView(await new Response(body).arrayBuffer());
  const i = tile.getUint16(((row - tr * ix.tile) * ix.tile + (col - tc * ix.tile)) * 2, true);
  const mukey = (await (await get("mukeys.json")).json())[i];
  return mukey ? JSON.parse(await new Response((await get(`mu/${mukey % 1024}.json.gz`)).body.pipeThrough(new DecompressionStream("gzip"))).text())[mukey] || null : null;
}
```

`index.json` and `mukeys.json` are worth keeping once loaded; a point then costs one tile (tens of kB) and one record
file (tens of kB gzipped). In Python: `python tools/lookup_point.py AL <lat> <lon> --root <this repository's folder>`
(`tools/` of the soildata repository).

## Rebuilding

1. `tools/fetch_states.py` (soildata repository) lists NRCS's Box share, https://nrcs.app.box.com/v/soils, folder
   *SSURGO / SQLite SSURGO (Geopackages) / States*, downloads each state's `SQLite_SSURGO_<ST>.7z`, unpacks it with
   7-Zip (`SQLite_SSURGO_<ST>.gpkg` and `muraster_10m_<ST>.tif` with its `.ovr` and value table) and runs
   `tools/build_ssurgo.py <ST> --src <the unpacked folder> --out <this repository's folder>` (Python 3.9 or later,
   numpy and rasterio). It replaces `<ST>/mu` and `<ST>/r30` and rewrites `<ST>/mukeys.json` and `<ST>/meta.json`.
2. A state whose package has no usable GeoPackage takes its tables from the national CSV tables:
   `tools/state_from_csv.py <ST> --csv <unpacked csv_ssurgo folder> --out <folder>`, then
   `tools/build_ssurgo.py <ST> --src <the unpacked state folder> --tables <folder>/SQLite_SSURGO_<ST>.sqlite`.
3. `tools/make_readmes.py` writes this file again from the states' `meta.json`.

## Credit and disclaimer

Soil Survey Staff, Natural Resources Conservation Service, United States Department of Agriculture. Soil Survey
Geographic (SSURGO) Database, FY2027 (SQLite SSURGO state downloads and national CSV tables, published October 1,
2026). Available online at https://nrcs.app.box.com/v/soils. Public domain.

This is not an official NRCS product, and NRCS has not reviewed it. It is a converted copy: the tables are reshaped
and the map unit raster is coarsened from 10 m to 30 m, so the map unit found at a point can differ from the one in
NRCS's data, most often near the edge of a map unit. For official soil information use
[Web Soil Survey](https://websoilsurvey.nrcs.usda.gov/) or [Soil Data Access](https://sdmdataaccess.nrcs.usda.gov/).
