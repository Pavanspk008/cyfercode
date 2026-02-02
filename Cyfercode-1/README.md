Final Project README / Summary
Project Overview

This project builds a production-ready machine learning system to classify SIEM security events as attack or benign.
The focus is on robust detection, interpretability, and operational reliability, rather than chasing complex models or inflated metrics.

The system is designed to generalize beyond known attack signatures and remain effective against unseen or zero-day behaviors.

Problem Statement

Traditional signature-based security systems perform well on known attacks but struggle with:
unseen attack variants,noisy or missing signatures,frequent rule maintenance.

The goal of this project was to:
detect attacks using behavioral and contextual signals,avoid over-reliance on predefined attack names and produce decisions that SOC teams can understand, tune, and trust

Approach & Modeling Strategy

1️⃣ Baseline V1 — Naive Model

Logistic Regression with raw alert metadata
Achieved perfect metrics
Feature importance revealed heavy reliance on alert signatures
❌ Identified as shortcut learning

2️⃣ Baseline V2-A — Signature-Reduced

Replaced raw attack names with abstracted behavioral flags
Improved generalization
Served as a feature engineering validation step

3️⃣ Baseline V2-B — Signature-Free (Final Model)

Removed all attack signatures
Relied purely on:
behavioral intensity (e.g., failed attempts)
contextual signals (IP type, protocol, severity)
Achieved near-perfect recall with realistic false positives
Demonstrated robust, behavior-driven learning

✅ Baseline V2-B was selected for production deployment

🤖 Final Model (Deployed)

Algorithm: Logistic Regression

Design: Signature-free, behavior-driven

Why Logistic Regression?

Transparent coefficients

SOC-friendly explanations

Stable probability outputs

Easy threshold control

Core Features Used
Behavioral Signals

failed_attempts

session_duration

bytes_sent

dst_port

Contextual Signals

src_ip_type

protocol

event_type

alert_severity

is_cloud_asset

No attack names
No rule-based labels
No signature leakage

Production Inference Flow

Receive a single SIEM event

Validate & convert to tabular format

Generate attack probability

Apply configurable SOC threshold

Return structured decision output

{
  "attack_probability": 0.99996,
  "threshold": 0.7,
  "prediction": "ATTACK",
  "model": "logistic_regression",
  "baseline": "V2-B"
}

Why not complex models?

Interpretability for SOC teams,
Easier monitoring and debugging,
Configurable thresholds without retraining.

Evaluation Strategy

Confusion matrix and class-wise metrics
Threshold analysis (security-focused, not accuracy-only)
Stress testing via signature removal
Feature importance via model coefficients

Production Inference Design

Inference Characteristics
Stateless, single-event inference
Same pipeline used for training and inference
Outputs probability score + final decision
Decision Logic
Model outputs attack_probability
SOC-controlled threshold determines final alert
Sensitivity can be adjusted without retraining

Monitoring & Drift Strategy

After deployment, the system monitors:

Prediction distribution drift

Key feature drift (severity, failed attempts, IP type)

Alert volume & SOC workload

Performance metrics when labels become available

This ensures early detection of silent failures.

🔄 Retraining Strategy

Triggered by:

Feature drift

Prediction drift

SOC feedback

Performance degradation

Offline validation before redeploy

Rollback-ready model versioning


Project Structure (Logical)
├── data/
│   └── raw/
├── notebooks/
│   ├── baseline_v1.ipynb
│   ├── baseline_v2.ipynb
│   └── production_inference_v2b.ipynb
├── models/
│   └── siem_lr_v2b_pipeline.joblib
└── README.md






















