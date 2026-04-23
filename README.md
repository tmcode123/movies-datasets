# 🎬 Movies Box Office Analysis

Exploratory analysis and predictive modelling of box office performance across 3,800+ films (1980–2016), using two public datasets: IMDB-sourced movie metadata and the TMDB 5000 movies dataset.

---

## What this project does

Most movie dashboards stop at "which genre earns the most." This project goes further:

- **Cleans and audits** two raw datasets, logging every dropped row with a reason
- **Answers four analytical questions** with data before touching any model
- **Builds and compares four models** to predict gross box office from budget, genre, rating, and audience engagement signals
- **Draws conclusions** — not just charts — about what actually drives box office success

---

## What we found

**Horror is the best risk-adjusted bet in Hollywood.**
With a median ROI of 2.2× and relatively low production budgets (~$10M), Horror outperforms Action on a per-dollar basis. Action films dominate by volume but sit *below* the break-even median ROI of 1.0×.

**Budget matters less than you'd think.**
The correlation between log(budget) and log(gross) is 0.67 — meaningful, but far from deterministic. The strongest single predictor in our model is `num_voted_users` (audience engagement), which captures cultural momentum that production spend cannot manufacture.

**Gradient Boosting beats linear models by a wide margin.**
Linear regression achieves CV-R² = 0.54. Gradient Boosting hits 0.68, with a median absolute error of $23M — confirming that the relationship between budget, genre, and gross is genuinely non-linear.

**Blockbuster economics shifted post-2000.**
Median real gross rose steadily through the 1990s, plateaued 2000–2010, then jumped sharply — consistent with franchise consolidation (MCU, Disney) inflating the top of the distribution while mid-budget films largely disappeared.

### Model comparison

| Model | R² (test) | CV-R² | MAE |
|---|---|---|---|
| Gradient Boosting | 0.696 | 0.684 | $23.3M |
| Random Forest | 0.672 | 0.668 | $23.8M |
| Ridge Regression | 0.577 | 0.536 | $46.8M |
| Linear Regression | 0.576 | 0.536 | $47.1M |

### Limitations

- Gross figures are US domestic only — international revenue can 2–4× the total
- Budgets are self-reported and exclude marketing spend (~50% of production cost)
- IMDB scores reflect retrospective audience opinion, not pre-release expectations
- Sample ends at 2016; streaming-era economics are not represented

---

## Project structure

```
.
├── data/
│   ├── movie_metadata.csv              # Raw — IMDB-sourced (5,043 films)
│   ├── tmdb_5000_movies.csv            # Raw — TMDB API (4,803 films)
│   └── movies_genres_summary.csv       # Pre-aggregated summary (used by MDapp.py)
│
├── 01_data_cleaning.py                 # ETL: raw → cleaned, with quality report
├── 02_modelling.py                     # Feature engineering, model comparison, diagnostics
├── movies_analysis.ipynb               # Full narrative notebook (EDA + modelling)
└── MDapp.py                            # Streamlit dashboard (interactive visualisation)
```

---

## How to run it

### Requirements

```bash
pip install pandas numpy matplotlib seaborn scikit-learn streamlit altair
```

### Step 1 — Clean the data

```bash
python 01_data_cleaning.py
```

Produces `data/movies_clean.csv`, `data/tmdb_clean.csv`, and `data/data_quality_report.csv`.

### Step 2 — Run the models

```bash
python 02_modelling.py
```

Produces model comparison results, feature importances, genre ROI summary, and a diagnostics plot — all saved to `data/`.

### Step 3 — Explore the notebook

```bash
jupyter notebook movies_analysis.ipynb
```

Walks through the full analysis: data cleaning rationale → four exploratory questions → model comparison → findings and limitations.

### Step 4 — Launch the dashboard

```bash
streamlit run MDapp.py
```

Interactive genre/year/metric explorer. Requires `data/movies_genres_summary.csv`.

---

## Data sources

- [IMDB Movie Metadata](https://www.kaggle.com/datasets/carolzhangdc/imdb-5000-movie-dataset) — Kaggle
- [TMDB 5000 Movies](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata) — Kaggle / The Movie Database
