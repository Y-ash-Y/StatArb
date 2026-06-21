# Statistical Arbitrage — EE798Z

A causal, adaptive, honestly-evaluated pairs-trading study on **real** market data.
Rebuilds a naive pairs-trading pipeline (headline "Sharpe 4.40 on synthetic data")
into a methodology that survives quantitative scrutiny, follows the evidence to an
honest negative result, diagnoses *why* (universal decointegration), and then acts
on that diagnosis with adaptive walk-forward selection on a structural ETF universe.

**Headline finding:** there is no reliable out-of-sample edge in liquid US large-caps
(all 12 selected pairs decointegrate across the 2022 regime shift). Structurally
cointegrated ETF pairs *do* mean-revert gross of costs (Sharpe +0.28, 81%
bootstrap-positive), but the edge is capturable only below **≈5 bps/leg** — the
binding constraint is execution cost relative to spread width, not signal
sophistication.

---

## File layout

### Engine (Python)
| File | Purpose |
|------|---------|
| `statarb.py`   | Core causal engine: sector universe, FDR cointegration screen, rolling-OLS + Kalman β, daily mark-to-market backtest, multi-pair portfolio, block-bootstrap CIs |
| `statarb10.py` | Adaptive layer: walk-forward re-selection, decointegration kill-switch, slippage, ETF structural-pairs universe, Sortino/Calmar metric suite |

### Pipeline (reproduce the results)
| File | Purpose |
|------|---------|
| `run_study10.py`      | Runs the full study: walk-forward on equities vs ETFs, cost break-even, ablations → `study10_results.json` |
| `build_notebook10.py` | Generates the narrated notebook `statarb_10.ipynb` |

### Notebook (narrated, pre-executed with outputs)
| File | Purpose |
|------|---------|
| `statarb_10.ipynb` | The study end-to-end with 3 figures and all numbers |

### Report
| File | Purpose |
|------|---------|
| `Statistical_Arbitrage_Report.tex` | LaTeX source (7 sections, 6 figures, 5 tables) |
| `Statistical_Arbitrage_Report.pdf` | Compiled report |
| `report_figures/`                  | Figures referenced by the `.tex` |

### Caches & artifacts (auto-generated, safe to delete)
| File | Purpose |
|------|---------|
| `prices_cache.pkl`     | Downloaded equity prices (73 names) |
| `etf_cache.pkl`        | Downloaded ETF prices (30 names) |
| `study10_results.json` | Numeric results |

### `archive/` — original work this study supersedes
- `final.ipynb` — original kitchen-sink notebook (synthetic data, lookahead bias)
- `Statistical_Arbitrage_Final_Report.pdf` — original report (Sharpe 4.40 claim)
- `figures/` — original figures

---

## Setup

Requires Python 3.11 (the system `python3` may be 3.14, which lacks wheels for the
scientific stack). A virtualenv `.venv` is used throughout.

```bash
cd /Users/yashy/Downloads/EE798Z
python3.11 -m venv .venv
.venv/bin/python -m pip install --upgrade pip
.venv/bin/python -m pip install numpy pandas scipy statsmodels scikit-learn \
    matplotlib yfinance nbformat nbconvert ipykernel
```

The first run downloads real prices from Yahoo Finance and caches them to
`*.pkl`; subsequent runs are offline and fast. Delete the `.pkl` files to
re-download.

---

## Reproduce

**Numbers only:**
```bash
.venv/bin/python run_study10.py    # a few minutes; the equities walk-forward is the slow part
```

**Rebuild + re-execute the notebook:**
```bash
.venv/bin/python build_notebook10.py
.venv/bin/jupyter nbconvert --to notebook --execute --inplace \
    --ExecutePreprocessor.timeout=900 statarb_10.ipynb
```

**Rebuild the report PDF** (needs `report_figures/` alongside the `.tex`):
```bash
tectonic Statistical_Arbitrage_Report.tex      # single-binary engine, or:
pdflatex Statistical_Arbitrage_Report.tex      # run twice for cross-references
```

---

## Key results at a glance

| Study | Result |
|-------|--------|
| Static selection, equities OOS | Sharpe **−0.44** (no edge); 12/12 pairs decointegrate |
| Walk-forward + kill-switch, equities | Sharpe **+0.09**, maxDD −1.0% (drag removed, still no edge) |
| Selection survival (next quarter) | single stocks **9%** vs structural ETFs **48%** |
| ETF structural pairs, gross | Sharpe **+0.28**, 81% bootstrap-positive |
| ETF structural pairs, net @10 bps/leg | Sharpe **−0.32** |
| Cost break-even | **≈5 bps/leg** (capturable at institutional execution only) |
| Best config (Kalman, ETFs, 1 bp/leg) | Sharpe **0.46**, Sortino 0.41, Calmar 0.42, P(Sharpe>0)=94% |

**The deliverable is the methodology and its defensibility — not an inflated
performance number.**
