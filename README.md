
# Deep Learning for State-of-Charge Estimation in Grid-Scale Battery Storage

This repository contains the implementation and experiments  on cross‑chemistry transfer learning for **state‑of‑charge (SOC) estimation** in grid‑scale battery energy storage systems (BESS).

## Overview

Accurate SOC estimation is essential for safe and profitable operation of BESS. However, battery behaviour changes with chemistry, ageing, and operating conditions. Traditional model‑based methods drift and need costly re‑calibration; data‑driven methods usually require large amounts of labelled data from every new installation.

This work proposes a **CNN‑LSTM deep learning framework** that estimates SOC from voltage, current, and their 2‑hour rolling averages. The main contribution is a **cross‑chemistry transfer learning strategy**: a model pre‑trained on lead‑acid and lithium‑manganese‑oxide (LMO) battery units is adapted to a lithium‑titanate‑oxide (LTO) unit using only a few days of target‑domain data.

## Dataset

All experiments use the public **[M5BAT](https://m5bat.de/)** grid‑scale battery dataset. The system consists of ten units across five battery technologies, connected to the German 10 kV grid. The primary unit studied is Batt10, an LTO unit operating in frequency containment reserve (FCR) mode.

## Models

Three architectures are implemented in TensorFlow/Keras:

- **CNN** – two Conv1D layers with residual connection
- **LSTM** – single LSTM layer with self‑attention
- **CNN‑LSTM (Dual)** – parallel CNN and LSTM branches with a learned gating mechanism

A classical particle filter with a Thévenin equivalent circuit model is also included as a model‑based baseline.

## Key Results

| Experiment | Model | MAE (%) | RMSE (%) |
|------------|-------|---------|----------|
| Direct training on LTO (21 days) | CNN‑LSTM | **1.20 ± 0.03** | **1.72 ± 0.04** |
| | LSTM | 1.34 ± 0.08 | 1.78 ± 0.05 |
| | CNN | 1.34 ± 0.13 | 1.78 ± 0.11 |
| Transfer learning (5 days of LTO data) | CNN‑LSTM | 1.87 | 2.41 |
| Transfer learning (15 days of LTO data) | CNN‑LSTM | 1.36 | 1.76 |
| Particle filter (literature params) | – | 9.38 | – |

*Direct training results are averaged over 5 random seeds.*



