# EU capitals dashboard: sustainable vacation ranking

Ranks the 27 EU capitals for a trip **next week from Le Mans** by weighing the
7-day weather forecast ([Open-Meteo](https://open-meteo.com), no API key needed)
50/50 against the round-trip flight CO₂.

- `data/capitals.csv`: static reference table (coordinates)
- `scripts/fetch_and_score.py`: fetch, normalize, score, write `docs/data/rankings.json`
- `.github/workflows/update-data.yml`: runs every Monday 06:00 UTC and commits the result
- `docs/index.html`: GitHub Pages dashboard (recommendation, ranking bar chart, trade-off scatter)

To change the origin city, edit `ORIGIN_NAME`, `ORIGIN_LAT` and `ORIGIN_LON` at the top
of the script. The page reads the origin name from `rankings.json`.
