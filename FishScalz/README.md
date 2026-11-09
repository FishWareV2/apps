# FishScalz

Estimate a fish's weight from its length, species and relative weight (Wr). Live page: https://fishwarev2.github.io/test-page/FishScalz/

## How to use

1. **Pick a species.** Four tabs (Salmonids, Suckers, Warmwater, Minnows et al.) each have their own dropdown.
2. **Enter a length** in inches or millimetres. The label shows which length the equation expects (TL, FL or BL). Changing the length unit converts the number you typed.
3. **Choose weight units**: English (lb / oz) or Metric (g / kg). Length and weight units are set separately.
4. **Move the Wr slider** (50–150). The marker at **93** is labelled "average", and the slider starts there.

The results box shows the estimated weight, the standard weight at that length, and the slope and intercept (plus a quadratic term for species that have one). The citation for the equation appears below them. A warning appears if the length is outside the range the equation was built for.

## How it calculates

Weight = standard weight (Ws) × Wr ÷ 100.

Ws comes from `standard_weight_equations_English.csv`, with length in inches and weight in pounds:

```
log10 Ws(lb) = int + slope × log10 L(in) + quad × (log10 L(in))²
```

The quadratic term is only used when the CSV has a value for it (for example Blue Sucker). Metric inputs and outputs are converted to and from inches and pounds (1 in = 25.4 mm, 1 lb = 453.59237 g).

## Data: `standard_weight_equations_English.csv`

This file replaced the species table that was copied from JakeOMatic. It holds 77 equations. The page reads these columns: `species`, `group`, `measure`, `method`, `min.len`, `max.len`, `int`, `slope`, `quad`, `source`, `comment`.

- **Column 2 (`group`) decides the tab.** Values are `Salmonid`, `Sucker`, `Warmwater` and `Minnows et al.`. Trailing spaces are ignored.
- To add or change a species, edit the CSV. No code change is needed.
- The CSV has no species code column, so the page doesn't show one. Add a code column if you want codes back.

### Things to check in the data

- **River Carpsucker, Shorthead Redhorse and Bigmouth Buffalo** are in the Minnows et al. group because the CSV puts them there, not under Suckers.
- **Length limits**: `min.len` and `max.len` are treated as inches, except values of 50 or more, which are treated as mm. This matters for Longear Sunfish (80–180) and Lake Chubsucker (80). Please check those two rows.
- **Northern Pikeminnow (original)** has a comment in the CSV saying its equation has been revised and should not be used. The comment shows under the dropdown when it's selected.
- **Wr average of 93** is a fixed marker on the slider. It is not read from the CSV.

## Running locally

The page loads the CSV with `fetch`, so opening `index.html` directly from disk won't work (the page shows a message saying so). Serve the repository instead:

```bash
python3 -m http.server 8000
# then open http://localhost:8000/FishScalz/
```

## Files

| File | Role |
|---|---|
| `index.html` | The whole page: layout, CSV parsing and the weight calculation |
| `standard_weight_equations_English.csv` | The standard-weight equations and their groups |
