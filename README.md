# Introduction to AI System Operations

**Session Time:** 120 minutes

---

## Table of Contents
1. Module Overview
2. Key Components of AI-Enabled Systems
3. The Lifecycle of AI Models
4. Operational Practices for Reliability
5. Lab Reference: System Landscape Mapping
6. Tools and Resources

---

## 1. Module Overview

Learn the fundamentals of maintaining and managing AI-enabled systems in production. Transitioning a machine learning model from a static Jupyter notebook to a live, reliable system requires a significant paradigm shift. In this module, you will understand how data pipelines, models, and applications interact continuously across the entire system lifecycle.

---

## 2. Key Components of AI-Enabled Systems

**Learning Objective:** Describe the key components of AI-enabled systems and their interdependencies.

An operational AI system is a complex, interconnected web of infrastructure, data pipelines, and software engineering components.  Understanding these interdependencies is critical for ensuring system reliability and scalability.

**Core Components:**
* **Data Ingestion & Pipelines:** The mechanisms that extract, transform, and load (ETL) streaming or batch data into a format the model can consume.
* **Feature Store:** A centralized repository for storing documented, curated, and access-controlled features used for both training and low-latency serving.
* **Model Registry:** A version control system specifically for machine learning models, tracking metadata, hyperparameters, and artifacts.
* **Serving Infrastructure:** The compute environments (e.g., Kubernetes clusters, serverless functions) that host the model and expose it via REST or gRPC endpoints.
* **Monitoring & Logging:** Telemetry systems that track hardware metrics (CPU, memory) and ML-specific metrics (prediction distributions, latency).

**Practical Example: Dynamic Pricing Engine**
Consider a dynamic pricing engine for a ride-sharing application. 
* The **Data Pipeline** processes live weather and traffic data. 
* The **Feature Store** provides historical rider demand profiles. 
* The **Serving Infrastructure** hosts the pricing model, which must return a fare prediction in under 100 milliseconds. 
If the data pipeline lags, the model serves pricing based on stale traffic data, leading to a poor user experience and lost revenue, even if the model's internal logic is mathematically perfect.

---

## 3. The Lifecycle of AI Models

**Learning Objective:** Explain the lifecycle of AI models from development to production.

The machine learning workflow requires continuous iteration. A typical ML workflow consists of the following stages: Problem Definition & Business Understanding, Data Collection & Preprocessing, Exploratory Data Analysis (EDA), Feature Engineering, Model Selection & Training, Evaluation & Validation, and Deployment & Monitoring. 

Unlike traditional software, an AI model's lifecycle does not end at deployment; it enters a continuous loop of serving, monitoring, and retraining to adapt to real-world changes.

```mermaid
graph TD;
    A[Problem Definition & Data Collection] --> B[Exploratory Data Analysis & Feature Engineering];
    B --> C[Model Selection & Training];
    C --> D[Evaluation & Validation];
    D --> E[Deployment & Serving];
    E --> F[Continuous Monitoring];
    F -->|Performance Degradation Detected| A;

```

**Practical Example: Fraud Detection System**
A model is trained on historical transaction data and evaluated against a holdout dataset to ensure high precision and recall. Once deployed, it begins scoring live credit card transactions. However, fraudsters constantly change their tactics. The lifecycle must include a feedback loop where newly identified fraudulent patterns are fed back into the data collection phase for continuous retraining, ensuring the model does not become obsolete.

---

## 4. Operational Practices for Reliability

**Learning Objective:** Analyse how operational practices maintain performance and reliability in deployed AI systems.

Deployed models degrade over time because the real world changes.  Operational practices in MLOps focus on detecting this degradation and automating the remediation process to maintain reliability.

**Key Operational Practices:**

* **Data Drift Monitoring:** Tracking shifts in the statistical distribution of incoming production data compared to the original training data.
* **Concept Drift Monitoring:** Tracking changes in the relationship between input features and the target variable (e.g., a sudden economic shift changes purchasing behavior).
* **Continuous Training (CT):** Automating the pipeline to retrain the model when performance degrades or when new data thresholds are met.
* **Shadow Deployment:** Running a new model version alongside the current production model, logging its predictions without returning them to the user, to safely evaluate real-world performance.

**Technical Suggestion: Detecting Data Drift**
Below is a simplified Python approach using a Kolmogorov-Smirnov (KS) test to detect if the distribution of a specific feature has drifted significantly from the training baseline.

```python
from scipy.stats import ks_2samp
import numpy as np

# Simulated reference data (from training) and current data (from production)
reference_feature_data = np.random.normal(loc=0.0, scale=1.0, size=1000)
production_feature_data = np.random.normal(loc=0.5, scale=1.0, size=1000) # Simulating a shift

# Perform KS test to compare distributions
statistic, p_value = ks_2samp(reference_feature_data, production_feature_data)

# Analyze the p-value against a threshold (e.g., 0.05)
if p_value < 0.05:
    print(f"ALERT: Significant Data Drift Detected! (p-value: {p_value:.4f})")
    # Trigger alerting or retraining pipeline here
else:
    print(f"System Normal: No significant drift detected. (p-value: {p_value:.4f})")

```

---

## 5. Lab Reference: System Landscape Mapping

**Lab Objective:** Visualise how data, models, and services connect within an operational AI system. This lab helps learners understand dependencies and the flow between components in a live environment.

*Note: Please refer to the dedicated Lab Guide for full instructions, architectural constraints, and deliverables.*

---

## 6. Tools and Resources

The following tools will be utilized in this module to map and understand operational environments:

* **GitHub:** For version control of infrastructure and model configurations.
* **Draw.io (free online):** For creating system landscape diagrams and architecture workflows.
* **AWS Free Tier:** For exploring real-world cloud components (e.g., S3 for data lakes, SageMaker for model registries).
