# econ3916-lab14-ols-diagnostics
Lab 14 Submission

# OLS Diagnostics: When the Standard Error Is Lying

## Objective
I tested when an OLS coefficient can be trusted but its standard error cannot, using simulations, residual diagnostics, influence measures and a real housing dataset.

## Methodology
- Simulated data whose error spread grows with x, then fitted the same regression with classical and with robust (HC3) standard errors.
- Ran 400 simulated samples and counted how often each 95% interval contained the true slope, reporting a Monte Carlo standard error for each coverage rate.
- Read four residual-vs-fitted plots (well behaved, heteroskedastic, missing curvature, an outlier) beside Breusch-Pagan and Jarque-Bera tests.
- Measured leverage, standardised residuals and Cook's distance for the outlier panel, then deleted the most influential row out of 600 and refitted.
- Regressed California Housing median value on income, house age, rooms and occupancy, and compared classical and HC3 standard errors.
- Wrote a function, `hc3_se()`, that returns the HC3 standard error for any term, and used it to refit without the 965 districts at the 5.0 ceiling.

## Key Findings
- **Same estimate, different uncertainty:** both fits gave a slope of 2.0147, but the standard error was 0.0264 classical and 0.0290 robust. Heteroskedasticity left the coefficient alone and changed only the standard error.
- **Coverage:** the classical 95% interval covered the true slope in 91.8% of samples (Monte Carlo SE 1.4%), a shortfall of more than two Monte Carlo SEs. The robust interval covered it in 93.5% (Monte Carlo SE 1.2%), which is within simulation noise of 95%.
- **Tests vs plots:** each problem showed up clearly in its residual plot, but a formal test only catches the pattern it is built for. Breusch-Pagan missed the missing curvature, because that problem is in the residuals' mean, not their spread.
- **One row out of 600:** deleting the outlier moved the slope only 0.58 of its own standard error, but the standard error fell 33%. A low-leverage point with a huge residual mainly damages the standard error.
- **Real data:** in California Housing, the HC3 standard error on AveRooms was 5.02 times the classical one.
- **The ceiling:** the HC3 standard error on AveRooms was 0.0121 with all districts and 0.0082 without the 965 at the ceiling. A smaller standard error is not a reason to drop them. Those values are censored, not wrong, so I report both fits and explain the difference.
