# Hotel Bar Inventory Forecasting & Par-Level Recommendation System

## 1. Business Problem

Hotel bars need to maintain enough inventory to meet customer demand while avoiding excessive stock levels.

Two key challenges are:

- Stockouts of high-demand brands can affect customer service and bar operations.
- Overstocking slow-moving brands can tie up working capital and storage space.

This project develops an inventory decision-support system that forecasts demand at the Bar + Brand level, recommends inventory par levels, and simulates inventory performance.

---

## 2. Data & Preprocessing

The dataset contains 6,575 inventory movement records across multiple hotel bars and brands.

The main fields include:

- Date Time Served
- Bar Name
- Alcohol Type
- Brand Name
- Opening Balance
- Purchase
- Consumed
- Closing Balance

The timestamp was converted into a daily date.

Inventory consistency was checked using:

**Closing Balance = Opening Balance + Purchase - Consumption**

6,434 records were consistent within 0.1 ml, while 141 records showed small inconsistencies. These records were retained because the differences may represent rounding, spillage, breakage, or manual inventory adjustments.

Consumption was aggregated by Date, Bar, and Brand.

Zero-consumption days were explicitly included to create complete daily time series.

---

## 3. Exploratory Analysis

ABC analysis was performed to identify brands contributing the largest share of total consumption.

Demand was also analyzed by day of the week and across Bar + Brand combinations.

A major finding was that **83.48% of daily Bar + Brand observations had zero consumption**.

This indicates highly intermittent demand and is an important consideration when selecting forecasting methods.

A historical stockout audit was also performed using:

**Closing Balance = 0 AND Consumption > 0**

This audit describes historical inventory records and is separate from the later simulation-based stockout analysis.

---

## 4. Forecasting Approach

Three forecasting approaches were evaluated:

1. 7-Day Rolling Mean Baseline
2. Exponential Smoothing
3. Random Forest Regression

A chronological train-test split was used to avoid future information leakage.

The forecasting features included:

- Lag 1
- Lag 7
- Lag 14
- 7-Day Rolling Mean
- 7-Day Rolling Standard Deviation
- Day of Week
- Weekend indicator

### Model Results

| Model | MAE (ml) | WAPE |
|---|---:|---:|
| 7-Day Rolling Mean | 95.83 | 160.21% |
| Exponential Smoothing | 95.52 | 159.68% |
| Random Forest | 96.32 | 161.02% |

Exponential Smoothing produced the lowest MAE and WAPE among the evaluated approaches.

However, the improvement over the rolling-mean baseline was small at approximately 0.33%.

The high WAPE values should be interpreted in the context of highly intermittent demand and the large number of zero-consumption observations.

---

## 5. Inventory Policy

The demand forecast was converted into a dynamic inventory policy.

The baseline assumptions were:

- Lead time: 2 days
- Target service level: 95%
- Z-score: 1.645

The recommended par level is calculated as:

**Par Level = Lead-Time Demand + Safety Stock**

Safety stock is calculated using demand variability and lead time.

Sensitivity analysis was performed across different lead times and service levels.

The results showed that recommended par levels increase as either lead time or the target service level increases.

---

## 6. Inventory Simulation

A lead-time-aware inventory simulation was implemented using historical test-period demand.

The simulation:

1. Starts with available inventory.
2. Fulfills daily demand when inventory is available.
3. Records stockout quantity when demand exceeds inventory.
4. Tracks pending replenishment orders.
5. Receives replenishment after the assumed lead time.
6. Replenishes inventory toward the recommended par level.

### Simulation Results

| Metric | Result |
|---|---:|
| Total Demand | 379,309.96 ml |
| Fulfilled Demand | 309,542.49 ml |
| Stockout Quantity | 69,767.47 ml |
| Fill Rate | 81.61% |
| Stockout Rate | 18.39% |
| Stockout Records | 427 |
| Average Ending Inventory | 344.71 ml |

The 95% service level is an input to the safety-stock calculation. It should not be interpreted as the achieved simulation fill rate.

---

## 7. Business Recommendations

The analysis supports the following operational recommendations:

- Forecast demand separately for each Bar + Brand combination.
- Use recommended par levels as initial replenishment targets.
- Monitor combinations with higher simulated stockout exposure.
- Use ABC analysis to prioritize high-consumption brands.
- Recalculate inventory policies regularly using recent demand.
- Incorporate operational information such as POS transactions, hotel occupancy, events, promotions, opening hours, and supplier lead times.

The simulation identified combinations such as Brown's Bar with Coors, Thomas's Bar with Yellow Tail, and Taylor's Bar with Smirnoff as examples of relatively high simulated stockout exposure.

These are simulation results rather than observations of historical stockouts.

---

## 8. Limitations

The current solution is an analytical prototype.

Important limitations include:

- Highly intermittent demand
- Limited external demand drivers
- Potential stockout censoring
- Simplified supplier lead-time assumptions
- No minimum-order or case-pack constraints
- No supplier delay variation
- Limited purchasing-cost information

Because the demand is highly intermittent and the forecasting improvement over the baseline was small, further validation would be required before production deployment.

---

## 9. Future Improvements

A production implementation could evaluate:

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

The system could also be integrated with a hotel inventory dashboard showing current inventory, forecast demand, recommended par levels, stockout risk, and replenishment recommendations.

---

## 10. Conclusion

This project provides an end-to-end framework for hotel bar demand forecasting and inventory planning.

The workflow combines:

**Data Quality → Demand Analysis → Forecasting → Safety Stock → Par-Level Recommendation → Inventory Simulation**

The results provide a baseline for inventory decision support while highlighting the challenges of highly intermittent demand.

Further improvement would require richer operational data, intermittent-demand forecasting methods, and more detailed supplier and purchasing constraints.
