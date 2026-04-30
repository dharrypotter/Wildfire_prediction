
🔥 Project Overview: Wildfire Prediction


This project models wildfire spread as a time-to-event (survival analysis) problem, where the objective is to predict the probability that a wildfire reaches a critical zone within fixed time horizons (12h, 24h, 48h, 72h).

Unlike standard classification, this setup includes censored data, where some fires are not observed to reach the event within the observation window.

🧠 Problem Formulation

Explain simply:

Each wildfire = one sample
Target:
event = 1 → fire reached zone
event = 0 → censored (did not reach during observation)
We model:
time-to-event (T)
event indicator (E)

📊 Approach

Break into steps:

1. Feature Engineering
Distance-based risk metrics
Fire spread velocity features
Growth × direction alignment interactions
Log-transformed spatial distances

3. Survival Model
Model: GradientBoostingSurvivalAnalysis (GBSA)
Objective: estimate survival function S(t)

5. Prediction Strategy

Convert survival function → event probability:

𝑃(𝑇≤𝑡) = 1 − 𝑆(𝑡)
P(T≤t)=1−S(t)
Evaluate at fixed horizons: 12h, 24h, 48h, 72h

⚙️ Handling Censoring

This is important—most people forget to explain it.

Censored samples are cases where the fire has not yet reached the evacuation zone within the observation window. These samples are included in training but are treated differently in loss computation to avoid bias.

📈 Model Training Strategy
Stratified K-Fold CV
Ensemble across:
multiple seeds
multiple GBSA configurations
Final prediction = averaged survival probabilities
📉 Post-processing

Monotonicity enforcement:
ensures P(12h) ≤ P(24h) ≤ P(48h) ≤ P(72h)

🧪 Evaluation Metric

C-index (ranking quality)
Weighted Brier Score (probability accuracy)
Combined competition metric

🧠 Key Insights
Survival modeling captures temporal dynamics of wildfire spread
Censoring-aware training significantly improves realism
Monotonic probability constraints improve stability
Ensemble learning reduces variance in survival estimates
