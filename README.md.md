# Privacy-Preserving Enhanced EnLeM for Phishing Detection

## Overview

This project extends **EnLeM (Ensemble Learning-Based Model to Detect Phishing Websites)** into a privacy-preserving phishing-detection framework.

The system classifies websites as **legitimate** or **phishing** using an ensemble of:

- Decision Tree
- Random Forest
- K-Nearest Neighbour (KNN)

The project compares the original EnLeM approach with feature selection, autoencoder-based feature compression, and federated-learning variants.

## Motivation

Phishing websites attempt to steal passwords, banking details, and other sensitive information. A centralized machine-learning system may require collecting user or organizational data in one location, creating privacy risks.

This project addresses that limitation by using **federated learning**, where clients train locally and share model updates or predictions instead of sharing raw data.

## Models evaluated

### 1. Original EnLeM

Decision Tree, Random Forest, and KNN are combined using hard voting. The majority prediction is used as the final classification.

### 2. EnLeM with Mutual Information

Mutual Information selects the most informative features before training the EnLeM classifiers. This reduces irrelevant and redundant input features.

### 3. Autoencoder + EnLeM

An autoencoder learns a compact latent representation of the website features. The encoded features are then passed to the EnLeM ensemble.

### 4. Federated Autoencoder + Local EnLeM

The training data is divided among simulated clients. Each client trains an autoencoder locally. The server aggregates autoencoder weights using weighted federated averaging. Each client then trains a local EnLeM model, and client predictions are combined by majority voting.

### 5. Federated Neural Classifier

As an additional federated baseline, a small neural-network classifier is trained locally on each client. Its weights are aggregated using federated averaging.

## Dataset

The experiments use the **UCI Phishing Websites dataset**:

- 11,055 instances
- 30 website features
- Binary target: legitimate or phishing

The dataset is loaded in Colab using `ucimlrepo`:

```python
from ucimlrepo import fetch_ucirepo

dataset = fetch_ucirepo(id=327)
X = dataset.data.features
y = dataset.data.targets
```

The target is converted to:

- `0`: legitimate
- `1`: phishing

## Project workflow

```text
UCI phishing dataset
        |
Cleaning and scaling
        |
Train/validation/test split
        |
        +--> Original EnLeM
        |
        +--> Mutual Information + EnLeM
        |
        +--> Autoencoder + EnLeM
        |
        +--> Federated Autoencoder + local EnLeM
        |
        +--> Federated neural classifier
        |
Performance comparison
```

## Requirements

The notebook is designed for Google Colab. Install the dependencies with:

```bash
pip install ucimlrepo tensorflow scikit-learn pandas numpy matplotlib seaborn
```

Main libraries:

- Python 3
- TensorFlow/Keras
- scikit-learn
- pandas
- NumPy
- Matplotlib
- Seaborn
- ucimlrepo

## Running the project

1. Open the project notebook in Google Colab.
2. Run the dependency-installation cell.
3. Download and inspect the UCI dataset.
4. Clean, encode, scale, and split the data.
5. Train and evaluate the original EnLeM model.
6. Train and evaluate the Mutual Information version.
7. Train the autoencoder and evaluate Autoencoder + EnLeM.
8. Divide the training data into simulated clients.
9. Run federated autoencoder training.
10. Train local EnLeM models and aggregate client predictions.
11. Train and evaluate the federated neural classifier.
12. Compare all models using the final metrics table and plots.

## Evaluation metrics

The project reports:

- Accuracy
- Precision
- Recall
- F1-score
- Confusion matrix

Recall is especially important because a false negative represents a phishing website incorrectly classified as legitimate. F1-score is used as the main balanced comparison metric.

The project also records or can be extended to record:

- Training time
- Prediction time
- Number of federated rounds
- Number of clients
- Communication cost
- False-positive rate

## Privacy and federated-learning scope

The federated experiment is a simulation running on one Colab machine. It represents multiple clients by partitioning the training set into separate local datasets. Raw client data is not sent to the aggregation step; only model weights or predictions are combined.

This implementation demonstrates the federated-learning workflow but does not provide formal differential privacy, secure aggregation, or encrypted communication. Those mechanisms can be added in a production deployment using frameworks such as Flower, TensorFlow Federated, or PySyft.

## Important implementation note

Decision Trees, Random Forests, and KNN do not have directly compatible parameters for standard neural-network federated averaging. Therefore, the project uses:

- federated averaging for the autoencoder and neural classifier;
- local EnLeM models with aggregated predictions for the ensemble-based federated experiment.

This distinction should be reported clearly in the project documentation and presentation.

## Expected contribution

The project evaluates whether privacy-preserving distributed training and compact feature representation can improve or maintain the accuracy, efficiency, scalability, and generalization of the original EnLeM phishing detector.

## Reproducibility

For comparable results, use:

- fixed random seed: `42`;
- stratified train/validation/test splits;
- preprocessing fitted only on the training data;
- the same test set for every model;
- identical evaluation metrics for every experiment.

## Authors

- Bongu Dhanush — 23102A041025
- Avalasingu Harsha Vardhan — 23102A041016
- Annepu Bhavyasri — 23102A041014
- Budda Teja — 23102A041027

## Guide

Dr. C. Madhusudhana Rao  
Professor, Department of Computer Science and Engineering  
Mohan Babu University, Tirupati

## Academic project

This project is submitted in partial fulfillment of the requirements for the award of the degree of Bachelor of Technology in Computer Science and Engineering at Mohan Babu University, Tirupati, for the academic year 2026–2027.
