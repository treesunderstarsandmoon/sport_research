# Sports Forecasting Portfolio

**Baiqing Chen**

Two projects in probabilistic sports forecasting. The football project tests whether team attack and defence improve goal forecasts over historical averages. The NBA project estimates pre-game scoring distributions and checks whether prediction intervals cover actual scores at the expected rate. Both evaluate forecasts on later games using chronological splits.

## Football | Goal forecasting

This Sporting Risk workshop exercise begins by comparing two probability forecasts using log loss and the Brier score. It then fits an independent Poisson model for home and away goals using team attack, opponent defence and home advantage. The model is trained on the earliest 80% of matches and compared with a baseline that uses separate historical averages for home and away goals.

| Test metric | Home goals | Away goals |
| --- | ---: | ---: |
| Baseline negative log-likelihood | 1.6805 | 1.5528 |
| Team model negative log-likelihood | 1.6391 | 1.5082 |
| Reduction in negative log-likelihood | 2.46% | 2.87% |

The two goal distributions could be used to price markets such as match result and total goals. The evaluation measures forecast quality on historical matches. A follow-up would test a Dixon–Coles adjustment for dependence in low-scoring outcomes.

[Football notebook](football/football_goal_forecasting.ipynb)

## NBA | Pre-game score forecasting

Separate histogram-based gradient boosting models predict home and away points from 41 features covering recent form, pace, shooting efficiency and scheduling. Team statistics are lagged before rolling averages are computed so each game's features use prior results. The chronological split assigns 70% of games to training, 15% to validation and 15% to testing. Validation residuals estimate the spread of each Gaussian scoring distribution, and the final point models are refitted on the combined training and validation sets.

| Test metric | Home points | Away points |
| --- | ---: | ---: |
| Mean absolute error (points) | 10.23 | 9.77 |
| Root mean squared error (points) | 13.10 | 12.60 |
| Gaussian negative log-likelihood | 3.998 | 3.954 |
| Observed coverage of 80% intervals | 77.8% | 79.3% |
| Average interval width (points) | 30.92 | 31.13 |

The model estimates each team's scoring distribution separately and uses a fixed residual variance for each side. A joint model could account for score correlation when forecasting totals or margins.

[NBA notebook](nba/nba_score_forecasting.ipynb) · [Methodology report (LaTeX source)](nba/methodology.tex)

## Viewing and running the notebooks

The notebooks include saved outputs for the results above. To rerun them, install the dependencies, place the corresponding workbook beside its notebook and run the cells from top to bottom with the notebook's folder as the working directory.

```bash
python -m pip install jupyter numpy pandas scikit-learn scipy openpyxl
```

| Notebook folder | Required workbook |
| --- | --- |
| `football/` | `football_modelling.xlsx` |
| `nba/` | `nba_dataset.xlsx` |

The source workbooks are absent from this repository. To build the NBA report, run `pdflatex methodology.tex` from `nba/` with a LaTeX distribution installed.
