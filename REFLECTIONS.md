# REFLECTIONS (TD1)

## 1. Before building: weather vs CO₂
I'd lean slightly towards weather (around 60/40). A low-CO₂ trip to a cold, rainy city
isn't much of a vacation, but I wouldn't ignore CO₂: a big extra distance for slightly
better weather isn't worth it.

## 2. Stage 1: acquisition reliability
Each separate connection is another chance to fail: another TLS handshake, another
request that a shared GitHub Actions IP can get throttled or rate-limited on. With 26
calls in a row, the odds that *at least one* fails stack up, and once the API starts
throttling, every later call fails too. One batched request is one point of failure,
not 26.

## 3. Stage 1: provenance of the CO₂ factor
I'd replace the rounded 0.15 with the current year's official factor (e.g. the latest
DEFRA/ADEME short-haul figure), cite the source and year in the code and on the page,
and probably split short-haul/long-haul or add the radiative-forcing uplift. I'd also
check whether a train is realistic for close destinations (Brussels, Paris), since flying
there doesn't make sense.

## 4. Stage 2: relative vs absolute
Adding a very cold, rainy 27th capital wouldn't change a mediocre capital's real
temperature, but it **would** raise its score, because the new city becomes the new
"worst" and stretches the 0-1 scale. So the score is relative to the list, not a fact
about the city: a KPI is a construct built on top of a measurement and depends on
the comparison set.

## 5. Stage 3: prediction (written before running)
**Madrid.** It's mid-distance from Paris (~1,050 km), so its CO₂ score stays decent,
and in late October it should be warm and dry compared to northern capitals.

## 6. Stage 6: was the prediction right?
Yes. From Paris (run on 2026-10-21) the top was **Madrid** (final 0.755: weather 0.80,
CO₂ 0.71), then Lisbon (0.728) and Rome (0.671). Brussels has the best CO₂ score (1.0) but
weather of 0.23, so it only came 5th. What surprised me was that Lisbon, despite better
weather, loses to Madrid because of the extra ~400 km. With a 50/50 formula, distance
pulls hard.

## 7. Stage 6: monitoring
The "Data last updated …" timestamp at the bottom of the page. If it's more than a week
old, the pipeline has stopped. It's a weak signal on its own because it's small and
grey, and visitors have to know what to look for. I'd add a visible warning banner when
`generated_at` is older than 8 days, plus a failure notification on the workflow.

## 8. Stage 7: changing the origin (Le Mans)
- `scripts/fetch_and_score.py`: replaced `PARIS_LAT/PARIS_LON` with
  `ORIGIN_NAME/ORIGIN_LAT/ORIGIN_LON` (Le Mans: 48.0061, 0.1996), used `ORIGIN_NAME` in
  the exclusion filter, and wrote `origin` into `rankings.json`. That's where the origin
  is actually used in the calculation.
- `docs/index.html`: removed the hardcoded "Paris" and read the origin name from the
  JSON, so the page can never say a different city than the data. Also made "all 26
  destinations" dynamic, because Le Mans isn't a capital, so there are now 27
  destinations (Paris included).

The "best" destination is **relative to the starting point**. From Le Mans, Madrid still
wins (0.778), but **Paris** jumps to 3rd (185 km, CO₂ score 1.0), and Dublin enters the
top 6 while Brussels and Prague drop out.
