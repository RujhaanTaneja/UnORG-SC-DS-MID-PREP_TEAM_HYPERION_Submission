# Supply Chain and Data Science: UnORG Mid-Prep (Team Hyperion)

## Problem Statement and Overview

UnORG operates a B2B grocery supply chain serving diverse customers like local vendors, sweet shops, hotels, and restaurants. The challenge stems from:

- Highly volatile demand driven by seasonality, tourism, and perishability.
- Wide SKU diversity, with 600+ SKUs varying in shelf life and order patterns.
- Logistical bottlenecks in semi-urban and rural last-mile delivery.
  

## Key Deliverables:

1. Daily Customer Order Prediction: Predict whether a customer will place an order for the next 14 days.
2. SKU-level Demand Forecasting: Predict which SKU and what quantity each customer will order.
3. Inventory Planning & Fulfillment: Forecast inventory needs to minimize stockouts and overstocking.
   

## Solution Approach and Architecture

### Customer Order Prediction
- Feature Engineering: Created 20+ features like average order gap, total orders, order consistency score, and customer segmentation (clustered into regulars, seldom buyers, and few-timers).
- Clustering: Used unsupervised clustering to segment customers based on purchase behavior.
- LSTM-based Prediction Model: For regular customers (Cluster 2), a Clustering + LSTM hybrid was deployed to predict daily ordering probabilities across a 14-day horizon.

### SKU Demand Forecasting

- Model: LightGBM Regressor trained to predict daily demand for each SKU-customer pair.
- Temporal Features: Reference dates and windows (train/validation/test splits) were carefully engineered to capture order seasonality and spikes (e.g., post-New Year surges).

## Inventory Planning and Fulfillment 
Model: A Periodic Review (P-System) combined with Mixed-Integer Linear Programming (MILP) using PuLP in Python.

### Inventory Strategy:

- Forecast daily demand per SKU at warehouse level.
- Calculate safety stock using standard deviation of demand and z-scores for a 95% service level.
- Derive order quantity = Target Inventory - Current Inventory (simulated if real-time data unavailable).

Note: Prediction of rare events (customers placing orders on a specific day) is a naturally low-recall, high-precision problem, which the team handled carefully by prioritizing actionable segments (Cluster 2 customers)


## Future Scope for Improvements

Stacking Ensemble Models:

1. Combine LSTM outputs with LightGBM or XGBoost for customer-level meta-predictions.
2. Integrate signals like point-of-sale data, promotions, and web search trends to adjust forecasts dynamically.
3. Further refine clusters based on CLV to prioritize inventory and delivery resources.

