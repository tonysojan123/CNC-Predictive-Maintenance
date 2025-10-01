Project Overview
This project addresses the critical challenge of unplanned downtime in manufacturing by developing a machine learning model to predict equipment failure. The model forecasts the Remaining Useful Life (RUL) of a CNC machine's cutting tool, enabling a shift from reactive maintenance to a proactive, data-driven strategy.

By analyzing time-series sensor data, the model learns to identify subtle patterns of degradation, providing an actionable estimate of how many operational cycles a tool has left before a likely failure. This proof-of-concept demonstrates a robust, end-to-end data science workflow, from raw data processing to honest model evaluation.

Key Features
True RUL Engineering: Transformed a classification-style dataset into a regression problem by intelligently segmenting the data into "tool lifecycles" and calculating a true RUL in "cycles-to-failure."

Time-Series Feature Engineering: Constructed historically-aware features, including rolling window statistics (mean, standard deviation) and lag features, to provide the model with crucial temporal context.

Physics-Informed Features: Developed interaction features based on domain knowledge (e.g., Power and Temperature Difference) to capture the physical drivers of machine failure.

Robust Validation: Implemented a chronological train-test split based on tool lifecycles to prevent data leakage and ensure the model's performance reflects a realistic forecasting scenario.

Methodology
The project followed a rigorous, multi-stage methodology:

Lifecycle Segmentation: The continuous dataset was first segmented into distinct tool lifecycles. A new lifecycle was identified each time the Tool wear [min] value reset, indicating a tool replacement.

RUL Calculation: The target variable, RUL_cycles, was engineered by working backward from the first failure event within each lifecycle. This provided a true, leakage-free measure of the time-to-failure at every data point.

Feature Engineering: A rich feature set was created to capture the machine's operational dynamics:

Rolling Statistics: 10-period rolling means and standard deviations for key sensors (Temperature, Torque, Rotational Speed, etc.).

Lag Features: The value of each sensor from the previous time step (_lag1).

Interaction Features: Power_W (calculated from torque and rotational speed in rad/s) and Temp_diff.

Modeling & Evaluation:

A Random Forest Regressor was trained on the engineered features to predict RUL_cycles.

The data was split chronologically, using the first 80% of lifecycles for training and the final 20% for testing.

The model's performance was evaluated using standard regression metrics, yielding realistic and actionable results.

Results & Performance
The final model provides an honest and reliable baseline for RUL prediction.

Mean Absolute Error (MAE): 14.19 cycles

Root Mean Squared Error (RMSE): 19.35 cycles

R-squared (R²): 0.2537

Interpretation: The MAE of ~14 cycles indicates that, on average, the model's prediction is off by only 14 operational cycles. The R² score reflects a realistic performance on a complex problem, free from the data leakage that can artificially inflate scores.

The feature importance analysis revealed that Tool wear [min], Temp_diff, and the rolling means of various sensors were the most influential predictors, confirming that the model successfully learned the underlying physical patterns of machine degradation.

How to Use
**Clone the repository:**
git clone https://github.com/tonysojan123/CNC-Predictive-Maintenance.git

Install dependencies:

pip install -r requirements.txt

Run the analysis:
Open and run the Jupyter Notebook (predictive_maintenance.ipynb) to see the full data processing, modeling, and evaluation workflow.

Dataset
This project uses the AI4I 2020 Predictive Maintenance Dataset, which is a synthetic dataset modeled after real-world CNC milling machine data.

Source: Kaggle /(https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset)

Content: 10,000 data points with sensor readings including air temperature, process temperature, rotational speed, torque, and tool wear.

