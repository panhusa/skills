---
name: ac-setup-generator
description: Generates an Assetto Corsa setup .ini file for a given car and track. Uses car class, track profile, and known stock values. Output goes to /tmp/ for the user to copy to their PC.
---

# AC Setup Generator

Generate two files to `/tmp/`:
- `/tmp/ac_setup_<car>_<track>.ini` — the setup file
- `/tmp/ac_setup_<car>_<track>.md` — human-readable docs with delta-from-stock table and PC destination path

User copies `.ini` to `Documents\Assetto Corsa\setups\<car>\<track>\` on the Windows PC.

---

## Step 1 — Collect inputs

If not provided, ask for:
- Car name (folder under `/opt/ac_server/content/cars/`)
- Track name (folder under `/opt/ac_server/content/tracks/`)

```bash
ls /opt/ac_server/content/cars/<car>/
ls /opt/ac_server/content/tracks/<track>/
```

---

## Step 2 — Read car data

```bash
cat /opt/ac_server/content/cars/<car>/data/setup.ini 2>/dev/null
```

**If setup.ini exists:** use MIN/MAX/STEP to compute values:
- `SHOW_CLICKS=0` → VALUE = real value (PSI, degrees, N/mm)
- `SHOW_CLICKS=1` → VALUE = clicks from MIN: `round((target - MIN) / STEP)`
- `SHOW_CLICKS=2` → VALUE = clicks from 0: `round((target - MIN) / STEP)`

**If missing** (data.acd — encrypted):
1. Check Known Cars below.
2. If not listed — ask the user for their stock setup. **Never generate abstract values.** Always use real AC values from an actual .ini file.

---

## Step 3 — Identify class and track profile

### Car classes

| Name pattern                               | Class     |
| ------------------------------------------ | --------- |
| `dallara_*`, `tatuus*`, `blckbox_tatuus*`  | FORMULA   |
| `*_gt3`, `mercedes_sls_gt3`                | GT3       |
| `*_gt4`                                    | GT4       |
| `*_gt1`, `ks_maserati_mc12*`               | GT1       |
| `urd_*`, `ks_praga_r1`                     | PROTOTYPE |
| `btcc_*`, `pm3dm_*`, `ks_mercedes_190*`    | BTCC      |
| `ks_mazda_mx5*`, `abarth500*`              | CUP       |
| anything else                              | ROAD      |

### Track profiles

| Track                 | DF       | Surface | Notes                                   |
| --------------------- | -------- | ------- | --------------------------------------- |
| `monza`               | LOW      | smooth  | min drag                                |
| `ks_red_bull_ring`    | LOW-MED  | smooth  |                                         |
| `spa`                 | MED      | smooth  | baseline                                |
| `ks_silverstone`      | MED      | smooth  |                                         |
| `imola`               | MED      | bumpy   | springs−5%, arb−5%, packer+3, press+1  |
| `mugello`             | MED-HIGH | smooth  |                                         |
| `ks_brands_hatch`     | HIGH     | smooth  |                                         |
| `rt_suzuka`           | HIGH     | smooth  |                                         |
| `monaco`              | HIGH     | bumpy   | max DF, springs−10%, press+1           |
| `ks_nordschleife`     | MED      | bumpy   | springs−20%, arb−15%, rh+3, press+1   |
| `ks_laguna_seca`      | MED-HIGH | bumpy   | springs−10%, press+1                   |
| hillclimb (e.g. pikes_peak) | MAX | smooth | max DF, diff_power max, fuel=1        |
| `drift`               | LOW      | varies  | min DF, diff_power=80%                  |
| `prague`              | MED-HIGH | bumpy   | springs−10%, press+1                   |
| unknown               | MED      | smooth  | use baseline                            |

---

## Step 4 — Generate values

### Known Car: tatuusfa1 / blckbox_tatuusfa1_halo

**Notes:**
- Front suspension key: `HF` (both wheels), rear: `LR` / `RR` separately
- CAMBER: tenths of a degree (−31 = −3.1°)
- WING: mm, range **0–17**
- DAMP\_\*: SHOW\_CLICKS=2 (clicks from 0)
- ARB: SHOW\_CLICKS=2 (clicks from 0)
- SPRING\_RATE: N/mm (SHOW\_CLICKS=0)
- ROD\_LENGTH: mod-specific unit (SHOW\_CLICKS=0)
- PACKER\_RANGE: mm (SHOW\_CLICKS=0)
- PRESSURES: PSI (SHOW\_CLICKS=0)
- DIFF\_POWER / DIFF\_COAST: % (SHOW\_CLICKS=0)

**Target values by track profile:**

| Parameter             | LOW (monza) | MED (spa) | HIGH (brands) | MAX (hillclimb) |
| --------------------- | ----------- | --------- | ------------- | --------------- |
| WING\_1               | 2           | 6         | 13            | 15              |
| WING\_2               | 2           | 6         | 13            | 15              |
| ARB\_FRONT            | 20          | 30        | 42            | 45              |
| ARB\_REAR             | 0           | 0         | 8             | 10              |
| CAMBER\_LF/RF         | −30         | −31       | −33           | −34             |
| CAMBER\_LR/RR         | −21         | −22       | −24           | −24             |
| TOE\_OUT F            | 5           | 5         | 4             | 3               |
| TOE\_OUT R            | 6           | 8         | 11            | 12              |
| SPRING\_RATE\_HF      | 75          | 80        | 85            | 85              |
| SPRING\_RATE\_LR/RR   | 88          | 94        | 98            | 98              |
| ROD\_LENGTH\_HF       | 23          | 20        | 18            | 17              |
| ROD\_LENGTH\_LR/RR    | 20          | 17        | 15            | 15              |
| PACKER\_RANGE F       | 33          | 30        | 27            | 28              |
| PACKER\_RANGE R       | 62          | 59        | 56            | 55              |
| DAMP\_BUMP            | 21          | 23        | 20            | 20              |
| DAMP\_FAST\_BUMP      | 29          | 32        | 28            | 28              |
| DAMP\_REBOUND         | 13          | 15        | 18            | 18              |
| DAMP\_FAST\_REBOUND   | 13          | 14        | 12            | 12              |
| DIFF\_POWER           | 10          | 10        | 35            | 45              |
| DIFF\_COAST           | 90          | 90        | 75            | 70              |
| FRONT\_BIAS           | 53          | 53        | 55            | 55              |
| PRESSURE F (PSI)      | 21          | 19        | 19            | 19              |
| PRESSURE R (PSI)      | 19          | 17        | 17            | 17              |
| FUEL (L)              | 40          | 40        | 40            | 1               |

For bumpy surface: SPRING\_RATE −5, PACKER\_RANGE +3, PRESSURE +1, DAMP\_BUMP −3.

**Unchanged from stock:** TYRES=1, BRAKE\_POWER\_MULT=100, INTERNAL\_GEAR\_2-7 (1/2/4/7/10/14), \_\_EXT\_PATCH VERSION=0.2.11

---

### Known Car: dallara_sf14_2017

setup.ini available on the server — read it to confirm ranges:
```bash
cat /opt/ac_server/content/cars/dallara_sf14_2017/data/setup.ini
```

Full parameter ranges:

| Parameter           | MIN    | MAX    | STEP | SHOW\_CLICKS | Unit          |
| ------------------- | ------ | ------ | ---- | ------------ | ------------- |
| WING\_1 / WING\_5   | 10     | 25     | 1    | 1            | clicks        |
| WING\_2             | 5      | 45     | 1    | 0            | degrees       |
| PRESSURE\_*         | 9      | 30     | 1    | 0            | PSI           |
| SPRING\_RATE\_*     | 60     | 250    | 1    | 0            | N/mm          |
| ROD\_LENGTH\_*      | 50     | 500    | 10   | 0            | mm            |
| ARB\_FRONT          | 20000  | 120000 | 5000 | 2            | N/m → clicks  |
| ARB\_REAR           | 2500   | 100000 | 2500 | 2            | N/m → clicks  |
| PACKER\_RANGE F     | 10     | 75     | 1    | 0            | mm            |
| PACKER\_RANGE R     | 15     | 80     | 1    | 0            | mm            |
| CAMBER\_*           | −5     | −1     | 1    | 0            | degrees (int) |
| TOE\_OUT\_*         | −70/80 | 80/110 | 1   | 2            | clicks        |
| DAMP\_BUMP          | 2812   | 7813   | 333  | 2            | N/(m/s)→clicks|
| DAMP\_FAST\_BUMP    | 1253   | 3353   | 140  | 2            | clicks        |
| DAMP\_REBOUND       | 4572   | 17792  | 748  | 2            | clicks        |
| DAMP\_FAST\_REBOUND | 2340   | 8075   | 249  | 2            | clicks        |
| DIFF\_POWER         | 10     | 80     | 1    | 0            | %             |
| DIFF\_COAST         | 10     | 70     | 1    | 0            | %             |
| DIFF\_PRELOAD       | 0      | 200    | 1    | 0            | Nm            |
| FUEL                | 0      | 70     | 1    | 0            | L             |
| FRONT\_BIAS         | 45     | 70     | 1    | 0            | %             |
| ENGINE\_LIMITER     | 95     | 100    | 1    | 1            | clicks        |

Target values by profile (computed from MIN/MAX):

| Parameter         | LOW (monza) | MED (spa) | HIGH (brands) |
| ----------------- | ----------- | --------- | ------------- |
| WING\_1/5 (clicks)| 0           | 7         | 12            |
| WING\_2 (°)       | 10          | 25        | 38            |
| PRESSURE F (PSI)  | 22          | 20        | 19            |
| PRESSURE R (PSI)  | 20          | 18        | 17            |
| SPRING\_RATE F    | 100         | 130       | 160           |
| SPRING\_RATE R    | 90          | 120       | 150           |
| ROD\_LENGTH F     | 150         | 130       | 110           |
| ROD\_LENGTH R     | 130         | 110       | 90            |
| ARB\_FRONT        | 4           | 10        | 16            |
| ARB\_REAR         | 2           | 8         | 16            |
| CAMBER F (°)      | −3          | −3        | −4            |
| CAMBER R (°)      | −2          | −2        | −3            |
| TOE\_OUT F        | 0           | 0         | 5             |
| TOE\_OUT R        | 10          | 15        | 20            |
| DAMP\_BUMP        | 4           | 7         | 10            |
| DAMP\_FAST\_BUMP  | 4           | 7         | 10            |
| DAMP\_REBOUND     | 4           | 7         | 10            |
| DAMP\_FAST\_REB   | 4           | 7         | 10            |
| DIFF\_POWER (%)   | 20          | 30        | 50            |
| DIFF\_COAST (%)   | 20          | 30        | 50            |
| DIFF\_PRELOAD     | 20          | 50        | 80            |
| FRONT\_BIAS (%)   | 53          | 55        | 58            |
| FUEL (L)          | 40          | 50        | 60            |

---

### Classes without known stock (GT3, GT4, GT1, PROTOTYPE, BTCC, CUP)

If user doesn't provide a stock setup, use the tables below as a starting point. Note in the .md that these are estimates.

#### GT3

| Parameter            | LOW DF | MED DF | HIGH DF |
| -------------------- | ------ | ------ | ------- |
| PRESSURE F (PSI)     | 27     | 26     | 25      |
| PRESSURE R (PSI)     | 28     | 27     | 26      |
| CAMBER F (°)         | −3.0   | −3.2   | −3.4    |
| CAMBER R (°)         | −1.5   | −1.5   | −1.7    |
| ARB\_FRONT (clicks)  | 3      | 4      | 5       |
| ARB\_REAR (clicks)   | 2      | 3      | 4       |
| SPRING\_RATE F (clk) | 4      | 5      | 6       |
| SPRING\_RATE R (clk) | 3      | 4      | 5       |
| ROD\_LENGTH F (mm)   | 65     | 62     | 58      |
| ROD\_LENGTH R (mm)   | 70     | 67     | 63      |
| FRONT\_BIAS (%)      | 55     | 56     | 58      |
| FUEL (L)             | 90     | 90     | 90      |

#### GT4

| Parameter         | LOW DF | MED DF | HIGH DF |
| ----------------- | ------ | ------ | ------- |
| PRESSURE F (PSI)  | 28     | 27     | 26      |
| PRESSURE R (PSI)  | 29     | 28     | 27      |
| CAMBER F (°)      | −2.5   | −2.8   | −3.0    |
| CAMBER R (°)      | −1.2   | −1.4   | −1.6    |
| FRONT\_BIAS (%)   | 55     | 56     | 57      |
| FUEL (L)          | 60     | 60     | 60      |

#### BTCC / Touring

| Parameter        | Value |
| ---------------- | ----- |
| PRESSURE F (PSI) | 30    |
| PRESSURE R (PSI) | 28    |
| CAMBER F (°)     | −2.5  |
| CAMBER R (°)     | −1.0  |
| FRONT\_BIAS (%)  | 56    |
| FUEL (L)         | 60    |

Bumpy surface: PRESSURE +2.

#### CUP / Road

| Parameter        | Value |
| ---------------- | ----- |
| PRESSURE F (PSI) | 32    |
| PRESSURE R (PSI) | 30    |
| CAMBER F (°)     | −2.0  |
| CAMBER R (°)     | −1.0  |
| FRONT\_BIAS (%)  | 55    |
| FUEL (L)         | 40    |

---

## Step 5 — Write files

### .ini format

```ini
[PARAMETER_NAME]
VALUE=<value>

```

File: `/tmp/ac_setup_<car>_<track>.ini`

Include ALL parameters the car has (from stock or setup.ini). Do not skip INTERNAL\_GEAR, TYRES, CAR, \_\_EXT\_PATCH if they were in the stock setup.

### .md format

```markdown
# <Car> — <Track> (<surface>)
Generated: <date> | Profile: <DF level>

## Delta from stock
| Parameter | Stock | This setup | Reason |
...

## Destination (PC)
Documents\Assetto Corsa\setups\<car>\<track_folder>\<name>.ini
```

---

## Step 6 — Show and iterate

1. Display .ini content as a code block
2. Show PC destination path
3. Explain 2–3 key decisions (wings, diff, bias) in context of the track

Ask: "Want to change anything? (e.g. 'stiffer rear', 'more diff', 'less fuel')"
Accept natural language, convert to values, regenerate both files.
