# Real-Time Dynamic Pricing for Parking Lots Using Occupancy, Demand, and Competitor Intelligence
### Summer Analytics 2025 – Capstone Project by Bliss Machado

> Real-time competitive and contextual pricing system for parking lots using historical patterns, traffic, vehicle type, and GPS-aware competition.

---

## 📌 A Brief Overview of the Project

This project builds a **smart dynamic pricing engine** for 14 real-world parking lots using timestamped data.

The system calculates parking prices in real-time based on:
- 📊 Occupancy trends
- 🚦 Nearby traffic conditions
- 🗓 Special day events
- 🚗 Vehicle type (car, truck, bike)
- 📍 GPS-based pricing from nearby competitors

Three increasingly intelligent models were built:
1. **Model 1** – Baseline linear pricing based on occupancy  
2. **Model 2** – Demand-based pricing using multiple features  
3. **Model 3** – Competitive GPS-aware pricing using Haversine formula

The solution includes:
- Real-time visualizations using Bokeh  
- Pricing output for each parking lot  
- A final PDF report explaining the logic

---

## 🧰 Tech Stack Used

| Tool / Library     | Purpose                                    |
|--------------------|--------------------------------------------|
| Python 3.x         | Core logic and model implementation        |
| Pandas             | Data preprocessing                         |
| NumPy              | Numerical calculations                     |
| Bokeh              | Interactive visualizations                 |
| FPDF               | Auto-generating submission-ready reports   |
| Mermaid.js         | Architecture diagrams                      |
| Google Colab       | Development and simulation environment     |

---

## 📊 Architecture Diagram (Mermaid)

![Architecture Diagram]![Architecture Diagram SA _ Mermaid Chart-2025-07-06-050519](https://github.com/user-attachments/assets/fb6cb6bc-1124-4e30-8d17-d6e2ca2f4e43)
# Architecture Diagram Explanation

This document explains the key components and flow of the Smart Parking Price Optimization System as represented in the architecture diagram.
# 🏗️ Architecture Stages

### 1. Data Ingestion

- **Source**: Raw CSV containing timestamped records for 14 parking lots.
- **Fields**: Occupancy, Capacity, Vehicle Type, Traffic, Queue Length, Special Day Indicator, Location (Latitude & Longitude), Timestamps.

---

### 2. Preprocessing

Key transformations before modeling:
- **Timestamp Parsing**: Combined `LastUpdatedDate` and `LastUpdatedTime` to form a single `Timestamp` column.
- **Feature Mapping**:
  - `TrafficConditionNearby`: low → 0.2, avg → 0.5, high → 1.0
  - `VehicleType`: bike → 0.5, car → 1.0, truck → 1.5, cycle → 0.3
- **Sorting**: Data sorted chronologically per `ParkingLotID`.

---

### 3. Model 1 – Linear Pricing

A simple pricing baseline:
- Price increases linearly with occupancy ratio.
- Formula:
  ```
  Price(t+1) = Price(t) + α * (Occupancy / Capacity)
  ```
- Acts as a reference for evaluating more intelligent models.

---

### 4. Model 2 – Demand-Based Pricing

This model introduces contextual demand:
- Demand Score calculated using:
  - Occupancy
  - Queue Length
  - Traffic Conditions
  - Special Day
  - Vehicle Type
- Formula:
  ```
  Price = BasePrice * (1 + λ * NormalizedDemand)
  ```
- Normalized and clipped to remain between 0.5× and 2× base price.

---

### 5. Model 3 – GPS-Aware Competitive Pricing

Adds location-based intelligence:
- Calculates **Haversine distance** between parking lots.
- Identifies nearby lots (within 0.5 km).
- Compares local prices and adjusts accordingly:
  - Your price > competitor avg → reduce by 15%
  - Your price < competitor avg → increase by 15%

---

### 6. Visualization & Reporting

- **Bokeh Line Chart**: Tracks Model 2 vs Model 3 price evolution over time.
- **Bokeh Bar Chart**: Compares your lot vs nearby competitors at a timestamp.
- **FPDF Report**: Final automated PDF with all formulas, visuals, and results.

---

# Assumptions & Visual Insights

## ✅ Key Assumptions

- Data is timestamped and synchronized across all lots.
- Traffic and vehicle type significantly impact demand.
- Competitor pricing is relevant only within a 0.5 km radius.
- All pricing adjustments are smooth and bounded.

## 📈 Visual Insights

## 📊 Model 2 vs Model 3: Pricing Over Time

This line graph shows how competitive pricing (Model 3) fluctuates more responsively to nearby conditions, while demand-based pricing (Model 2) maintains smoother variation.

![Pricing Over Time]![Screenshot 2025-07-06 104210](https://github.com/user-attachments/assets/05bf294b-c8b6-47ab-9d30-6ab608d02c93)


---

## 📊 Competitor Prices vs Your Lot

This bar chart compares your demand-based and competitive prices against nearby competitor pricing at a specific timestamp.

![Competitor Bar Chart]![Screenshot 2025-07-06 104248](https://github.com/user-attachments/assets/bf167760-eb14-4c90-bfb8-6ca851593931)



This architecture ensures a modular, scalable, and intelligent pricing pipeline adaptable to real-time extensions.




