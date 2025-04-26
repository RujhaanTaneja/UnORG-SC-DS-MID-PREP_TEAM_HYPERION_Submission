# Supply Chain and Data Science: UnORG Mid-Prep (Team Hyperion)

---

## Problem Statement and Overview

UnORG operates a B2B grocery supply chain serving a diverse customer base, including local vendors, sweet shops, hotels, and restaurants.  
The key challenges include:

- **Highly volatile demand**, influenced by seasonality, tourism, and perishability.
- **Wide SKU diversity**, with 600+ SKUs varying in shelf life and order patterns.
- **Logistical bottlenecks**, especially in semi-urban and rural last-mile delivery.

---

## 🎯 Key Deliverables

- **Daily Customer Order Prediction**: Predict whether a customer will place an order over the next 14 days.
- **SKU-level Demand Forecasting**: Forecast which SKU and what quantity each customer will order.
- **Inventory Planning & Fulfillment**: Forecast inventory requirements to minimize stockouts and overstocking.

---

## Solution Approach and Architecture

### 1. Customer Order Prediction

- Feature Engineering: Created 20+ features like average order gap, total orders, order consistency score, and customer segmentation (clustered into regulars, seldom buyers, and few-timers).
- Clustering: Used unsupervised clustering to segment customers based on purchase behavior.
- LSTM-based Prediction Model: For regular customers (Cluster 2), a Clustering + LSTM hybrid was deployed to predict daily ordering probabilities across a 14-day horizon.

---

### 2. SKU Demand Forecasting

- **Model**: **LightGBM Regressor** trained on daily SKU-customer order quantities.
- **Temporal Engineering**: Careful design of calendar features, training, validation, and test windows to capture seasonality (e.g., post-New Year surges).

---

### 3. Inventory Planning and Fulfillment

- **Model**: **Periodic Review (P-System)** + **Mixed-Integer Linear Programming (MILP)** using **PuLP**.
- **Strategy**:
  - Forecast demand per SKU at the warehouse level.
  - Calculate safety stock using demand standard deviation and 95% service level z-scores.
  - Derive **Order Quantity** = Target Inventory - Current Inventory.

> **Note** Prediction of rare events (customers placing orders on a specific day) is a naturally low-recall, high-precision problem, which the team handled carefully by prioritizing actionable segments (Cluster 2 customers)

---

## Future Scope for Improvements

- **Stacked Ensemble Models**: Blend LSTM outputs with LightGBM/XGBoost for meta-predictions.
- **Dynamic Demand Signals**: Incorporate real-time data like promotions, web trends, and PoS signals.
- **Advanced Customer Segmentation**: Refine clustering using **Customer Lifetime Value (CLV)** for better inventory targeting.

---

