# International Air Passengers Forecasting
## Capstone Project — Final Report

**Submitted by:** Shahad Algadah  
**Academy:** @SDAIAAcademy  
**Project:** Time Series Forecasting  
**Date:** October 15, 2026

---

## 1. Executive Summary and Recommendation

**Recommendation: Select AutoARIMA as the preferred forecasting model, subject to improving the calibration of its prediction intervals.**

Across eight rolling-origin validation windows, AutoARIMA achieved a Mean Absolute Scaled Error (MASE) of **0.6472**, compared with **1.3133** for Seasonal Naive, representing approximately a **50.7% reduction in scaled forecasting error**.

AutoARIMA also provided better probabilistic forecasts, with narrower prediction intervals and higher empirical coverage. However, its 80% prediction intervals covered only 69.79% of actual observations. Therefore, additional calibration is recommended before operational use.

## 2. Data Overview and Exploration

The analysis uses the classical Box and Jenkins international airline passenger dataset, containing 144 monthly observations from January 1949 to December 1960. Passenger counts are reported in thousands, with no missing months.

The series exhibits sustained long-term growth and recurring annual seasonality. Seasonal fluctuations also increase in magnitude as passenger volumes rise.

STL decomposition confirmed these characteristics:

- **Trend strength:** 1.00 (rounded)
- **Seasonal strength:** 0.98

The autocorrelation analysis showed strong dependence across consecutive months, with notable positive correlations at lags 12 and 24. These findings support evaluating forecasting approaches that model both growth and annual seasonality.

## 3. Forecasting Models and Evaluation

Two simple benchmarks, Naive and Seasonal Naive, were examined before evaluating more advanced models through AutoGluon.

AutoGluon compared Naive, Seasonal Naive, AutoETS, AutoARIMA, and Theta, and generated a WeightedEnsemble. AutoARIMA achieved the best held-out test MASE of 0.6081. However, this single test period was not treated as sufficient evidence for final model selection.

Theta experienced a forecasting failure and used Seasonal Naive as a fallback. Its displayed result was therefore not considered independent evidence of Theta's forecasting ability.

For the final comparison, AutoARIMA and Seasonal Naive were evaluated using the same eight rolling-origin windows, each forecasting 12 months ahead with a 12-month step. Each model was fitted using only the information available at its respective training cutoff.

### Cross-Validation Results

| Evaluation Metric | Seasonal Naive | AutoARIMA |
|---|---:|---:|
| MASE | 1.3133 | **0.6472** |
| RMSSE | 1.2456 | **0.6833** |
| Scaled CRPS | 0.0616 | **0.0339** |
| 80% Coverage | 51.04% | **69.79%** |
| Minimum Window MASE | 0.4116 | **0.3301** |
| Maximum Window MASE | 1.9587 | **1.5938** |

AutoARIMA outperformed Seasonal Naive in aggregate point accuracy, squared-error performance, and probabilistic forecast quality. Its scaled CRPS was approximately 45% lower.

The lower minimum and maximum window-level MASE values also support AutoARIMA, although they do not demonstrate that it wins in every individual validation window.

## 4. Prediction Intervals and Reliability

The average width of the nominal 80% prediction intervals was:

| Interval Statistic | Seasonal Naive | AutoARIMA |
|---|---:|---:|
| Mean Width | 77.76 thousand | 44.08 thousand |
| Minimum Width | 65.89 thousand | 18.18 thousand |
| Maximum Width | 88.55 thousand | 76.15 thousand |

AutoARIMA generated narrower intervals while obtaining better empirical coverage than Seasonal Naive. This suggests more informative uncertainty estimates from AutoARIMA relative to the benchmark.

Nevertheless, both models undercovered the observed passenger counts. AutoARIMA achieved 69.79% coverage against the nominal 80% target, a shortfall of 10.21 percentage points.

**Reliability assessment:** AutoARIMA is preferable, but its prediction intervals are not yet sufficiently calibrated for decisions requiring reliable uncertainty estimates.

## 5. Residual and Forecast Error Analysis

The Seasonal Naive benchmark produced predominantly positive historical residuals, indicating systematic underestimation as passenger demand increased.

Its residual autocorrelation was strong at short lags, approximately 0.74 at lag 1, while lag 12 was close to zero. This indicates that the seasonal benchmark captured much of the annual repetition but left growth-related and short-term structure unexplained.

For AutoARIMA, out-of-sample errors from the eight rolling-origin windows showed:

- **Mean Error:** +5.88 thousand passengers.
- **Mean Absolute Error:** 17.74 thousand passengers.

The positive mean error suggests some remaining underestimation. Forecast errors also varied across validation windows. The window ending in December 1958 showed persistent positive errors, with some monthly errors exceeding 60 thousand passengers.

Although AutoARIMA substantially improved forecasting performance, these remaining errors indicate difficulty adapting to some changes in passenger demand. The plotted forecast errors provide descriptive evidence of this limitation; no formal independence test was applied to AutoARIMA residuals.

## 6. Recommended Next Improvement

**Proposed improvement: Calibrate AutoARIMA's prediction intervals using historical rolling-origin forecast errors.**

The next iteration should use a strictly chronological calibration procedure to adjust uncertainty intervals, with calibration data kept separate from the observations being evaluated.

The objective is to bring empirical 80% coverage closer to its nominal target while avoiding unnecessarily wide prediction intervals.

The revised approach should be assessed using future or separately reserved rolling-origin windows, comparing coverage, interval width, MASE, and scaled CRPS against the current AutoARIMA results.

## 7. Conclusion

AutoARIMA is the recommended forecasting model because it consistently demonstrates stronger aggregate results than Seasonal Naive across the reported evaluation metrics. Its MASE of 0.6472 and scaled CRPS of 0.0339 provide evidence of improved point and probabilistic forecasting performance.

However, its 69.79% empirical coverage indicates that uncertainty estimation remains imperfect. The model is a strong candidate for further use, provided that interval calibration is improved and independently validated before operational deployment.

---

*All reported model performance and interval statistics were obtained from the project notebook and its rolling-origin evaluation outputs.*
