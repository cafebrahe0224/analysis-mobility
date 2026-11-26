# Urban Mobility and Economic Productivity in Latin American Cities

Exploratory analysis of how urban congestion relates to city-level economic output, combining the TomTom Traffic Index with OECD city indicators for 2024.

## Question

Is congestion associated with GDP per capita across Latin American cities? The answer is meant to inform where transport investment could matter most.

## Data

| Source | Content | Size |
|---|---|---|
| TomTom Traffic Index | Congestion metrics per city over time (`jams_delay`, `traffic_index_live`, travel times) | ~1.0M records |
| OECD Cities | GDP per capita, unemployment, PM2.5, population | 30 records |

The raw files are not included in this repository. Place them in `data/`.

## Method

1. Standardize column names to `snake_case` and parse timestamps.
2. Convert text-formatted economic indicators (decimal commas, thousands separators, `%`) to floats.
3. Filter to a single year and average the traffic metrics per city.
4. Inner-join with the economic indicators on `city` and `year`, keeping only cities present in both sources.
5. Export the merged dataset and generate summary figures.

## Usage

```bash
pip install -r requirements.txt
python mobility_economy.py \
    --traffic data/tomtom_traffic.csv \
    --economy data/oecd_city_economy.csv \
    --year 2024 \
    --output-dir outputs
```

Outputs: `ladb_mobility_economy_2024_clean.csv`, three figures (`.png`), and a correlation table printed to the console.

## Findings (2024, 15 cities)

- No clear relationship between GDP per capita and congestion: the correlation with `jams_delay` is about 0.28 and about 0.06 with `traffic_index_live`.
- `jams_delay` tracks city size far more closely than economic output (correlation with population of about 0.88). It is a cumulative delay, so it grows with the size of the road network and not only with poor traffic flow.
- Cases go in both directions: high GDP with heavy congestion (Mexico City), high GDP with light congestion (Montevideo, Brasília), and mid-range GDP with heavy congestion (Bogotá, Lima).

## Limitations

- Only 15 cities remain after the join, 9 of them Brazilian. A correlation of 0.28 on this sample is statistically indistinguishable from zero.
- Santiago's GDP per capita looks like a data-entry error and affects the correlation (0.33 without it).
- Correlation between congestion and GDP does not support conclusions about the return on transport investment.

## Project structure

```
.
├── mobility_economy.py
├── requirements.txt
├── data/        # raw inputs (not tracked)
└── outputs/     # generated dataset and figures
```
