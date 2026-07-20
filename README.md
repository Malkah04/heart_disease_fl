# 🛡️ Privacy-Preserving Heart Disease Classification using Federated Learning & PySyft

A decentralized, privacy-preserving machine learning pipeline built with **PyTorch** and the latest **PySyft** framework. This project demonstrates how to collaboratively train a global model across isolated hospital nodes (simulating real-world decentralized entities) without transferring raw, sensitive medical data.

---

## 📌 Features

* **Data Privacy First:** Raw clinical data remains strictly inside local hospital nodes.
* **Modern PySyft Integration:** Built using PySyft's secure remote execution functions (`@sy.syft_function`).
* **Robust Preprocessing Pipeline:** Isolated local feature scaling (`StandardScaler`) carefully structured to prevent data leakage and domain shifts.
* **Overfitting Prevention:** Local early stopping mechanism coupled with stratified train/validation splitting.
* **Aggregated Model Performance:** Achieved **84.47% Global Test Accuracy** via federated parameter aggregation (**FedAvg**).

---

## 🏗️ System Architecture & Workflow


1. **Local Node Execution:** Each hospital runs an isolated training job locally using `@sy.syft_function`.
2. **Feature Normalization:** Data is scaled independently using `StandardScaler` on local training partitions.
3. **Weight Sharing:** Only the local model parameters (`state_dict`) are exposed and collected.
4. **Global Aggregation:** Weights are averaged across nodes to form the updated Global Model.

---

## 🛠️ Tech Stack

* **Frameworks:** [PyTorch](https://pytorch.org/), [PySyft](https://github.com/OpenMined/PySyft) (OpenMined)
* **Data Processing & Analytics:** `scikit-learn`, `pandas`, `numpy`
* **Architecture Style:** Decentralized / Federated Learning (PPML)

---

## 📊 Key Results & Insights

* **Initial Baseline Challenge:** Uncalibrated feature distributions between training and testing environments initially dropped accuracy to ~51%.
* **Resolution:** Synchronizing feature normalization pipelines across test and training distributions boosted test accuracy straight to **84.47%**

## 🏗️ System Architecture & Workflow

```mermaid
graph TD
    subgraph Hospital_A [Hospital Node A]
        DataA[(Private Local Data)] --> PrepA[Local Preprocessing]
        PrepA --> TrainA[Local PyTorch Training]
        TrainA --> WeightA[Local Weights]
    end

    subgraph Hospital_B [Hospital Node B]
        DataB[(Private Local Data)] --> PrepB[Local Preprocessing]
        PrepB --> TrainB[Local PyTorch Training]
        TrainB --> WeightB[Local Weights]
    end

    WeightA --> FedAvg[Central Data Scientist / FedAvg]
    WeightB --> FedAvg

    FedAvg --> GlobalModel[Global Model Evaluation]
    GlobalModel --> Result["🎯 Accuracy: 84.47%"]

    style Hospital_A fill:#1f2937,stroke:#3b82f6,color:#fff
    style Hospital_B fill:#1f2937,stroke:#3b82f6,color:#fff
    style FedAvg fill:#111827,stroke:#10b981,color:#fff
    style Result fill:#064e3b,stroke:#10b981,color:#fff

| Metric | Result |
| :--- | :--- |
| **Hospital A Local Accuracy** | ~84.95% |
| **Hospital B Local Accuracy** | ~86.02% |
| **Global Aggregated Accuracy** | **84.47%** |


