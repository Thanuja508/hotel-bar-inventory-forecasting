# Hotel Bar Inventory Forecasting & Par-Level Recommendation System

## Project Overview

This project develops an end-to-end inventory forecasting and replenishment decision-support system for hotel bars.

The objective is to forecast item-level demand, recommend inventory par levels, and simulate inventory performance to help reduce stockout exposure while maintaining appropriate inventory levels.

## Business Problem

Hotel bars face two common inventory challenges:

- Stockouts of high-demand brands can affect customer service and bar operations.
- Overstocking slow-moving brands ties up working capital and storage space.

This project addresses these challenges by forecasting demand at the Bar + Brand level and using the forecasts to calculate inventory par levels.

## Dataset

The dataset contains **6,575 inventory movement records** across multiple hotel bars and brands.

Key fields include:

- Date Time Served
- Bar Name
- Alcohol Type
- Brand Name
- Opening Balance (ml)
- Purchase (ml)
- Consumed (ml)
- Closing Balance (ml)

## Approach

The project follows an end-to-end workflow:

1. Data quality validation
2. Timestamp conversion and preprocessing
3. Inventory conservation checks
4. Daily Bar + Brand demand aggregation
5. Zero-consumption day handling
6. Exploratory demand analysis
7. ABC analysis
8. Historical stockout audit
9. Demand intermittency analysis
10. Time-series feature engineering
11. Forecasting model comparison
12. Safety stock calculation
13. Dynamic par-level recommendation
14. Lead-time-aware inventory simulation
15. Sensitivity analysis
16. Stockout analysis
17. Business recommendations

## Forecasting Models

Three approaches were evaluated:

- 7-Day Rolling Mean Baseline
- Exponential Smoothing
- Random Forest Regression

A chronological train-test split was used to avoid future information leakage.

### Model Results

| Model | MAE (ml) | WAPE |
|---|---:|---:|
| 7-Day Rolling Mean | 95.83 | 160.21% |
| Exponential Smoothing | 95.52 | 159.68% |
| Random Forest | 96.32 | 161.02% |

Exponential Smoothing produced the lowest MAE and WAPE among the evaluated approaches. However, its improvement over the rolling-mean baseline was small at approximately 0.33%.

Demand was highly intermittent, with **83.48% of daily Bar + Brand observations having zero consumption**.

## Inventory Policy

The baseline inventory policy uses:

- Lead time: 2 days
- Target service level: 95%
- Z-score: 1.645

The recommended par level is calculated as:

**Par Level = Lead-Time Demand + Safety Stock**

Safety stock accounts for demand variability and supplier lead time.

## Simulation Results

A lead-time-aware inventory simulation was performed using historical test-period demand.

Key results:

- Total demand: 379,309.96 ml
- Fulfilled demand: 309,542.49 ml
- Stockout quantity: 69,767.47 ml
- Fill rate: 81.61%
- Stockout rate: 18.39%
- Stockout records: 427
- Average ending inventory: 344.71 ml

The 95% service level is an input to the safety-stock calculation and should not be interpreted as the achieved simulation fill rate.

## Business Recommendations

The analysis recommends:

- Forecast demand separately for each Bar + Brand combination.
- Use recommended par levels as initial replenishment targets.
- Monitor combinations with higher simulated stockout exposure.
- Use ABC analysis to prioritize high-consumption brands.
- Recalculate inventory policies regularly using recent demand.
- Incorporate operational data such as POS transactions, hotel occupancy, events, promotions, opening hours, and supplier lead times.

## Limitations

The current solution is an analytical prototype.

Important limitations include:

- Highly intermittent demand
- Limited external demand drivers
- Potential stockout censoring
- Simplified supplier lead-time assumptions
- No minimum-order or case-pack constraints
- No supplier delay variation
- Limited purchasing-cost information

## Future Improvements

A production version could evaluate:

- Croston-style forecasting
- TSB forecasting
- Gradient boosting models
- POS transaction-level data
- Stockout-event data
- Hotel occupancy
- Events and promotions
- Weather information
- Supplier lead-time history
- Minimum order quantities
- Case-pack constraints
- Purchasing costs

The solution could also be integrated with a hotel inventory dashboard to provide current inventory, forecast demand, recommended par levels, stockout risk, and replenishment recommendations.

## Project Files

```text
hotel-bar-inventory-forecasting/
│
├── notebooks/
│   └── Hotel_Bar_Inventory_Forecasting.ipynb
│
└── README.md
