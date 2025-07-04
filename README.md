# ParkSense: Dynamic Pricing for Urban Parking
## 📄 Project Overview
**ParkSense** is an intelligent, data-driven dynamic pricing system designed for urban parking lots. This capstone project, developed as part of the Summer Analytics 2025 program by IIT Guwahati, addresses the inefficiencies of static parking prices (like overcrowding or underutilization) by dynamically adjusting parking rates in real-time.
The system processes streaming data from multiple parking spaces, taking into account various parameters including occupancy, queue length, traffic conditions, special events, and vehicle types. It implements **three** progressively complex pricing models, culminating in a competitive pricing strategy, and visualizes real-time price fluctuations using interactive dashboards.

## 🎯 Key Objectives
- Develop a robust dynamic pricing engine for multiple urban parking spaces.
- Implement pricing models from scratch using fundamental data science libraries.
- Utilize a real-time data streaming framework for continuous price updates.
- Provide interactive visualizations to demonstrate pricing behavior and justification.
  
## 🛠️ Tech Stack
This project leverages a powerful combination of Python libraries and tools for data processing, modeling, and visualization:
- **Python**: The core programming language.
- **NumPy**: Fundamental package for numerical computing, used for efficient array operations and linear algebra (e.g., Normal Equation for linear regression).
- **Pandas**: Essential for data manipulation, cleaning, and analysis of historical parking data.
- **Pathway**: A high-performance Python framework for building real-time data streaming applications. Used for ingesting, processing, and transforming data streams.
- **Bokeh**: An interactive visualization library for creating dynamic and real-time plots that render in web browsers. Used for visualizing live price updates across parking lots.
- **Panel**: A library for creating custom interactive dashboards and web applications, seamlessly integrating with Bokeh plots to present the real-time insights.
- **Google Colab**: The development environment used for writing and executing the Python code.

## 📐 Architecture Diagram
The following diagram illustrates the high-level architecture and data flow of the ParkSense system:

```
graph TD
    A[Historical Dataset - dataset.csv] --> B(Data Preprocessing - Pandas/NumPy);
    B --> C(Simulated Data Stream - parking_data_stream.csv);
    C --> D[Pathway Data Ingestion - pw.demo.replay_csv];
    D --> E{Pathway Real-time Processing};
    E -- Current Data (pw.this) --> F[Pricing Models:];
    F --> F1(Model 1: Baseline Linear);
    F --> F2(Model 2: Demand-Based - Learned Coefficients);
    F --> F3(Model 3: Competitive - Simplified);
    E -- All Parking Data --> F3;
    F1 & F2 & F3 --> G(Combined Prices Stream);
    G --> H[Real-time Visualization - Bokeh/Panel];
    H --> I[Interactive Dashboard in Browser];
```

## 🚀 Project Architecture and Workflow
The ParkSense system operates as a real-time data pipeline, designed to continuously calculate and update parking prices.
1) **Data Ingestion and Preprocessing**:
   - The project begins with a dataset.csv file containing historical parking data (occupancy, capacity, traffic, special days, vehicle types, etc.).
   - This raw data is first loaded and preprocessed using Pandas and NumPy. This involves:
     - Combining date and time columns into a single Timestamp.
     - Sorting data chronologically.
     - Encoding categorical features (like VehicleType and TrafficConditionNearby) into numerical representations.
     - Normalizing numerical features to a consistent scale (0-1) to ensure fair contribution in models.
     - Defining a Demand_Proxy from existing features to serve as the target variable for model training.
      
2) **Model Training (Offline)**:
   - Before real-time processing, the coefficients for Model 2 (Demand-Based Price Function) are determined.
   - A Linear Regression model is implemented from scratch using NumPy's Normal Equation method. This model is trained on the preprocessed historical data to learn the impact of factors like Occupancy Rate, Queue Length, Traffic Condition, Special Day, and Vehicle Type on the Demand_Proxy. The learned coefficients (alpha_occ, beta_queue, gamma_traffic, delta_special, epsilon_vehicle) are then used by the real-time pricing engine.
     
3) **Pathway Real-time Data Stream**:
   - The preprocessed historical data is saved into a parking_data_stream.csv.
   - Pathway is then used to simulate a real-time data stream from this CSV file using pw.demo.replay_csv. This function replays the data at a controlled rate, mimicking a live data feed.
   - A Pathway Schema is defined to ensure the incoming data is correctly structured and typed.
     
4) **Dynamic Pricing Engine (Pathway Pipeline)**:
   - As data flows through the Pathway pipeline, each incoming record (representing the current state of a parking lot) is passed through three distinct pricing models:
     - Model 1 (Baseline Linear Model): A simple model where price adjusts linearly with occupancy rate based on a fixed alpha parameter.
     - Model 2 (Demand-Based Price Function): Calculates a demand score using the learned coefficients from the offline training step. This demand score is then normalized and used to dynamically adjust the BASE_PRICE.
     - Model 3 (Competitive Pricing Model): Builds upon Model 2's price. It includes a simplified logic to adjust prices based on the current lot's occupancy and a hypothetical average competitor price. (Note: A full implementation would involve real-time competitive price fetching via Pathway joins).
   - All pricing models ensure that the final price remains within predefined minimum and maximum bounds to prevent erratic fluctuations.
   - The results from all three models are combined into a single combined_prices Pathway table.
     
5) **Real-time Visualization**:
   - The combined_prices stream is connected to Bokeh for interactive, real-time visualizations.
   - For each unique parking spot, a dedicated Bokeh plot is generated, displaying the price trends from all three models over time.
   - Panel is used to arrange and serve these Bokeh plots as an interactive dashboard, allowing users to observe the dynamic pricing behavior as the simulated data streams in.
     
6) **Pipeline Execution**:
   - The entire Pathway pipeline is executed using pw.run(), which continuously processes the simulated data stream and updates the Bokeh/Panel dashboard in real-time.

## 📂 Repository Contents
- **ParkSense.ipynb**: The main Google Colab notebook containing all the Python code for data preprocessing, model training, Pathway pipeline definition, and Bokeh/Panel visualizations.
- **dataset.csv**: The raw historical parking data used for the project.
- **parking_data_stream.csv**: The preprocessed CSV file used by Pathway for simulating the real-time data stream.
- **README.md**
- **Project_Report.pdf**: A detailed report explaining the project, models, assumptions, and results.








