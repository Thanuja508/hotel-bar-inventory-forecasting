# Video Walkthrough – Hotel Bar Inventory Forecasting

## 0:00–0:45 — Problem

Hi, I'm Thanuja.

This project focuses on Hotel Bar Inventory Forecasting and Par-Level Recommendation.

The main business problem is balancing two inventory challenges across multiple hotel bars.

First, stockouts of high-demand items can affect customer satisfaction and interrupt bar operations.

Second, overstocking slow-moving items ties up working capital and storage space.

The goal of this project is to forecast demand at the Bar and Brand level, recommend appropriate inventory par levels, and simulate how the inventory policy performs using historical demand.

## 0:45–1:45 — Approach & Modeling

The dataset contains 6,575 inventory movement records across multiple bars and brands.

I first converted the timestamp into a daily date and validated the inventory conservation equation:

Opening Balance plus Purchase minus Consumption should equal Closing Balance.

I then aggregated consumption at the Bar and Brand level and created complete daily time series so that zero-consumption days were also represented.

For exploratory analysis, I performed ABC analysis, analyzed demand by day of the week, and examined demand intermittency.

One important finding was that 83.48 percent of daily Bar-Brand observations had zero consumption, showing that the demand is highly intermittent.

For forecasting, I compared three approaches: a 7-day rolling mean baseline, Exponential Smoothing, and Random Forest.

I used a chronological train-test split to avoid using future information during training.

Exponential Smoothing achieved the lowest MAE of 95.52 millilitres, compared with 95.83 for the baseline and 96.32 for Random Forest.

However, the improvement over the baseline was small, at approximately 0.33 percent.

## 1:45–3:00 — Inventory Logic & Simulation

Next, I converted the demand forecast into an inventory policy.

I assumed a 2-day supplier lead time and a 95 percent target service level, which corresponds to a Z-score of 1.645.

The par level is calculated as forecasted demand during lead time plus safety stock.

Safety stock accounts for demand variability and supplier lead time.

I also performed sensitivity analysis by changing the service level and lead time. The results showed that recommended par levels increase when either the service level or lead time increases.

Finally, I built a lead-time-aware inventory simulation.

The simulation fulfills daily demand, tracks pending replenishment orders, receives orders after the two-day lead time, and replenishes inventory toward the recommended par level.

On the historical test period, the simulation had approximately 379,310 millilitres of total demand.

309,542 millilitres were fulfilled, resulting in a fill rate of 81.61 percent and a stockout rate of 18.39 percent.

There were 427 stockout records, with average ending inventory of approximately 344.71 millilitres.

## 3:00–4:00 — Business Impact & Scalability

From an operational perspective, this system can help hotel managers make inventory decisions at the individual Bar and Brand level instead of using one inventory target for all locations.

The recommendations include using the calculated par levels as initial replenishment targets, monitoring combinations with higher simulated stockout exposure, and using ABC analysis to prioritize high-consumption brands.

For example, the simulation identified Brown's Bar with Coors, Thomas's Bar with Yellow Tail, and Taylor's Bar with Smirnoff as combinations with relatively high simulated stockout quantities.

For production deployment, the system could be connected to hotel POS and inventory systems and updated regularly.

Additional information such as hotel occupancy, events, promotions, opening hours, actual stockout events, and supplier lead-time history could improve the forecasts.

Because the demand is highly intermittent and the forecasting improvement over the baseline was small, I would treat this as a decision-support prototype that requires further validation before production deployment.

Overall, this project provides an end-to-end framework from demand analysis and forecasting to par-level recommendation and inventory simulation.

Thank you.
