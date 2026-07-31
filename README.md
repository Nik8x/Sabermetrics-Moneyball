# Sabermetrics: Moneyball Revisited

1,232 MLB team-seasons (1962-2012), the classic Moneyball case study,
the 2002 Oakland A's used on-base percentage instead of batting average
to find undervalued players. This repo previously bundled several
smaller course-exercise notebooks alongside the Moneyball analysis;
those are kept in `_old/`, and this rebuild focuses on the Sabermetrics
data specifically, extended with a full decade of data the original
analysis never had.

The original notebook only looked at seasons through 2001, which made
sense for reconstructing what the A's knew at the time, but stops short
of a question the fuller dataset can actually answer: once the
Moneyball strategy became public, did the rest of the league catch on?
It also never held any data back for evaluation, its "prediction" for
the 2002 A's uses coefficients fit partly on data the team already had.

## Notebooks

1. `00_data_setup_eda.ipynb`: run-difference-vs-wins relationship,
   league-average OBP/SLG trend across the full 1962-2012 span.
2. `01_statistical_testing.ipynb`: does the OBP-over-SLG advantage
   shrink after 2002? (yes, a real 1.73x to 1.40x drop).
3. `02_feature_engineering_selection.ipynb`: an actual variance
   inflation factor on the batting-average multicollinearity the
   original noticed but only reasoned about qualitatively.
4. `03_model_training_evaluation.ipynb`: a genuine time-based
   train/test split, fit on 1962-2001, evaluated out-of-sample on
   2002-2012, unlike the original's in-sample-only check.
5. `04_clustering.ipynb`: KMeans and Gaussian mixture clustering of
   team-seasons by batting profile alone, checked against real wins
   and playoff outcomes afterward.

## Results

The OBP advantage over SLG shrank from 1.73x to 1.40x after 2002, a
real market inefficiency partially, not fully, arbitraged away once it
became public knowledge. A wins model fit only on data through 2001
still predicts 2002-2012 seasons to within about 3 wins on average out
of sample (versus a 9.7-win baseline), and batting profile alone sorts
team-seasons into real success tiers, playoff rate ranging from 0.4% to
37.5% across just 4 unsupervised clusters.

Full write-up with charts: `docs/index.html` (also published via GitHub
Pages).

## Future work

- Extend the market-efficiency check to individual player valuation
  (salary vs OBP/SLG contribution) rather than only team-season
  aggregates.
- Bring in pitching and defensive metrics beyond runs allowed for a
  fuller team-strength model.
- Try the same before/after market-efficiency analysis for a more
  recent sabermetric innovation (exit velocity, launch angle) to see if
  the same erosion pattern repeats.
