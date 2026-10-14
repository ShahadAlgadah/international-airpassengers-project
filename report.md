# International Air Passengers Forecasting
### Capstone Project — Final Report

**Submitted by:** Shahad Algadah  
**Academy:** @SDAIAAcademy  
**Date:** October 15, 2026

---

## 1. Executive Summary and Recommendation

**Recommendation: Adopt AutoARIMA as the preferred forecasting model, subject to improving its prediction interval calibration.**

Across eight rolling-origin validation windows, AutoARIMA achieved a Mean Absolute Scaled Error (MASE) of **0.6472**, compared with **1.3133** for the Seasonal Naive benchmark. This represents a **50.7% reduction in scaled forecast error**.

AutoARIMA also delivered better probabilistic forecasts. However, its nominal 80% prediction intervals covered only 69.79% of observations, highlighting the need for improved uncertainty estimation before operational deployment.

## 2. Data and Forecasting Approach

The analysis uses 144 monthly international airline passenger observations from January 1949 to December 1960, with no missing months.

Exploratory analysis identified strong long-term growth and recurring annual seasonality, with trend strength of 1.00 (rounded) and seasonal strength of 0.98. The seasonal fluctuations also increased as passenger volumes grew.

Two benchmark models, Naive and Seasonal Naive, were established before comparing models through AutoGluon. AutoARIMA achieved the lowest held-out test MASE (0.6081), making it a candidate for further evaluation rather than an immediate final selection.

The final comparison used **eight rolling-origin validation windows**, each forecasting 12 months ahead. Both AutoARIMA and Seasonal Naive were evaluated on identical periods without using future observations during fitting.

## 3. Model Performance and Prediction Reliability

| Metric | Seasonal Naive | AutoARIMA |
|---|---:|---:|
| MASE ↓ | 1.3133 | **0.6472** |
| RMSSE ↓ | 1.2456 | **0.6833** |
| Scaled CRPS ↓ | 0.0616 | **0.0339** |
| 80% Coverage | 51.04% | **69.79%** |
| Best Window MASE ↓ | 0.4116 | **0.3301** |
| Worst Window MASE ↓ | 1.9587 | **1.5938** |

AutoARIMA achieved lower forecasting errors and improved probabilistic forecast quality. Its scaled CRPS decreased by approximately 45% relative to Seasonal Naive.

The average width of its 80% prediction intervals was **44.08 thousand passengers**, compared with **77.76 thousand** for Seasonal Naive. AutoARIMA therefore achieved better empirical coverage despite using narrower intervals.

Nevertheless, its 69.79% coverage remained **10.21 percentage points below the nominal 80% level**. The intervals are consequently undercalibrated and should not yet be treated as fully reliable uncertainty estimates.

## 4. Forecast Errors and Model Limitations

Seasonal Naive exhibited predominantly positive residuals, indicating systematic underestimation of growing passenger demand. Its residual autocorrelation was approximately 0.74 at lag 1, revealing remaining short-term dependence that the benchmark failed to capture.

AutoARIMA improved forecasting performance but still showed limitations. Across the eight validation windows:

- **Mean Forecast Error:** +5.88 thousand passengers.
- **Mean Absolute Error:** 17.74 thousand passengers.

The positive mean error indicates some remaining underestimation. Errors also varied across validation periods, with the window ending in December 1958 showing persistent positive errors exceeding 60 thousand passengers at certain horizons.

These findings suggest that AutoARIMA captures the main temporal structure more effectively than Seasonal Naive, but some changing growth patterns remain difficult to predict. The out-of-sample error plots were interpreted descriptively; no formal residual independence test was applied.

## 5. Recommended Improvement and Conclusion

**Next improvement: Calibrate AutoARIMA's 80% prediction intervals using historical rolling-origin forecast errors.**

The proposed calibration should use only past forecast errors available at each validation cutoff to prevent data leakage. The adjusted intervals should then be tested on separate future periods, evaluating empirical coverage, interval width, MASE, and scaled CRPS.

The expected outcome is coverage closer to the intended 80% level without unnecessarily widening prediction intervals.

**Final decision:** AutoARIMA is the preferred model because it substantially improves point forecast accuracy and probabilistic forecast quality over Seasonal Naive. Its uncertainty estimates require further calibration and independent validation before operational use.

---

*All reported evaluation results were obtained from the project notebook and its rolling-origin cross-validation outputs.*
