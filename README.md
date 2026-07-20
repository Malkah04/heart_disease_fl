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
```mermaid
flowchart TD
    subgraph NodeA ["🏥 Hospital Node A"]
        DataA[("Private Local Data A")] --> PrepA["Local Preprocessing"]
        PrepA --> TrainA["Local PyTorch Training"]
        TrainA --> WeightsA["Local Weights A"]
    end

    subgraph NodeB ["🏥 Hospital Node B"]
        DataB[("Private Local Data B")] --> PrepB["Local Preprocessing"]
        PrepB --> TrainB["Local PyTorch Training"]
        TrainB --> WeightsB["Local Weights B"]
    end

    WeightsA --> FedAvg["⚡ Federated Averaging (FedAvg)"]
    WeightsB --> FedAvg

    FedAvg --> GlobalEval["🎯 Global Model Evaluation\nAccuracy: 84.47%"]

    style FedAvg fill:#f9f,stroke:#333,stroke-width:2px
    style GlobalEval fill:#bbf,stroke:#333,stroke-width:2px

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
* **Resolution:** Synchronizing feature normalization pipelines across test and training distributions boosted test accuracy straight to **84.47%**.

| Metric | Result |
| :--- | :--- |
| **Hospital A Local Accuracy** | ~84.95% |
| **Hospital B Local Accuracy** | ~86.02% |
| **Global Aggregated Accuracy** | **84.47%** |


