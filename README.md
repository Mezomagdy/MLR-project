# Energy Consumption Prediction - Multiple Linear Regression Analysis

## 📋 Project Overview

This project implements **Multiple Linear Regression** from scratch using gradient descent optimization to predict energy consumption based on building characteristics. The analysis explores six different feature combinations and hyperparameter tuning (learning rates and epochs) to identify the optimal model configuration.

### Dataset
- **Source**: `assignment1dataset.csv`
- **Size**: 1,002 samples
- **Target Variable**: Energy Consumption (kWh)
- **Features**: Square Footage, Number of Occupants, Appliances Used, Average Temperature

---

## 🔧 Model Architecture

The project implements Multiple Linear Regression using the following mathematical framework:

### Cost Function (Mean Squared Error)
```
MSE(m₁, m₂, ..., b) = (1/2n) Σᵢ (ŷ - y)² = (1/2n) Σᵢ (m₁x₁ + m₂x₂ + ... + b - y)²
```

### Gradient Descent Updates
```
∂E/∂mᵢ = (1/n) Σᵢ (mᵢxᵢ + b - yᵢ)xᵢ
∂E/∂b = (1/n) Σᵢ (mᵢxᵢ + b - yᵢ)

W = W - α * ∇E  (Parameter update rule)
```

Where:
- **W** = Weight vector [b, m₁, m₂, ..., mₖ]
- **α** = Learning rate
- **n** = Number of samples
- **k** = Number of features

### Prediction
```
ŷ = [1, X₁, X₂, ..., Xₖ] · [b, m₁, m₂, ..., mₖ]ᵀ
```

---

## 📊 Data Preprocessing

### Steps Implemented:

1. **Data Loading**
   - Load CSV data as pandas DataFrame
   - Convert all columns to float type for numerical operations

2. **Feature Engineering**
   - Created derived feature: `Appliances by Space` = Square Footage × Appliances Used
   - This captures the interaction between building size and appliance count

3. **Feature Normalization** (Min-Max Scaling)
   ```python
   X_normalized = (X - X_min) / (X_max - X_min)
   ```
   - Scales all features to [0, 1] range
   - Improves gradient descent convergence
   - Ensures fair feature importance comparison

4. **Bias Term Addition**
   - Prepend a column of ones to feature matrix
   - Enables the intercept parameter in linear regression

5. **Array Conversion**
   - Convert pandas DataFrames to NumPy arrays for efficient computation

### Features Used:
- **Index 0**: Bias term (always 1)
- **Index 1**: Square Footage
- **Index 2**: Number of Occupants
- **Index 3**: Appliances Used
- **Index 4**: Average Temperature
- **Index 5**: Appliances by Space (derived)

---

## 🤖 Model Variants Trained

Six different models were trained using various feature combinations:

| Model Name | Features | Feature Indices | Purpose |
|-----------|----------|-----------------|---------|
| Square Footage | Bias + Square Footage | [0, 1] | Single feature baseline |
| Occupants | Bias + Number of Occupants | [0, 2] | Single feature baseline |
| Appliances | Bias + Appliances Used | [0, 3] | Single feature baseline |
| Appliances per Space | Bias + Derived Feature | [0, 5] | Engineered feature evaluation |
| SqFt & ApS | Bias + Square Footage + Appliances per Space | [0, 1, 5] | Two-feature model |
| All Four Features | Bias + All 4 original features + Derived | [0, 1, 2, 3, 5] | Comprehensive model |

---

## ⚙️ Hyperparameter Tuning

### Learning Rates Tested
```python
[0.1, 0.05, 0.04, 0.03, 0.02, 0.01, 0.001]
```
- Fixed epochs: 1000

### Epochs Tested
```python
[100, 500, 1000, 2000, 5000]
```
- Fixed learning rate: 0.01

### Results
- **Performance Metric**: Mean Squared Error (MSE)
- **Visualization**: Plots show the impact of learning rates and epochs on model performance
- **Best Configuration**: Identified for each model variant

---

## 📁 Project Structure

```
JupyterProject1/
├── models.ipynb                 # Main Jupyter notebook with full implementation
├── README.md                    # This file
├── data/
│   └── assignment1dataset.csv   # Input dataset (1,002 samples)
└── models/                      # (Empty) Directory for saving trained models
```

---

## 🚀 How to Run the Code

### Prerequisites

Ensure you have Python 3.7+ with the following packages:

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

Or install from requirements:

```bash
pip install -r requirements.txt
```

### Running the Notebook

1. **Open Jupyter Notebook**
   ```bash
   jupyter notebook models.ipynb
   ```

2. **Execute All Cells**
   - Use `Cell` → `Run All` in the menu, or
   - Press `Ctrl+A` then `Ctrl+Enter`

3. **View Results**
   - Summary tables displaying MSE for each model
   - Learning rate vs. MSE comparison plots
   - Epochs vs. MSE comparison plots
   - Scatter plots with best-fit lines/predictions for each model

### Expected Output

The notebook produces:
1. **Data Exploration**: Pairplot before and after normalization
2. **Model Performance Tables**: Summary of MSE values across all configurations
3. **Hyperparameter Analysis Plots**: 
   - Epochs impact on convergence
   - Learning rate impact on final MSE
4. **Best Model Visualizations**: Predictions vs. actual values with best-fit lines

---

## 📈 Key Findings

### Model Performance Insights
- Each model variant shows different performance characteristics
- Learning rate affects convergence speed and final error
- Epochs determine whether the model fully converges
- Feature engineering (Appliances per Space) may improve predictions
- Multi-feature models generally achieve lower MSE than single-feature models

### Hyperparameter Sensitivity
- **Too high learning rate**: Model may diverge (loss increases)
- **Too low learning rate**: Slow convergence, may not reach optimal point
- **Insufficient epochs**: Model hasn't fully trained
- **Excessive epochs**: Diminishing returns after convergence

---

## 💻 Implementation Details

### Core Functions

**Prediction**
```python
def predict(X, weights):
    return np.dot(X, weights)
```

**Mean Squared Error**
```python
def MSE(weights, X, Y):
    predictions = predict(X, weights)
    errors = predictions - Y
    return (1 / (2 * len(Y))) * np.sum(errors ** 2)
```

**Gradient Descent**
```python
def gradient_descent(X, Y, learning_rate=0.01, epochs=1000):
    n = len(Y)
    weights = np.zeros(X.shape[1])
    for epoch in range(epochs):
        predictions = predict(X, weights)
        error = predictions - Y
        gradient = (1 / n) * np.dot(X.T, error)
        weights = weights - learning_rate * gradient
    return weights, MSE(weights, X, Y)
```

**Model Class**
```python
class Model:
    def __init__(self, name, feature_indices, learning_rates, epochs):
        self.name = name
        self.feature_indices = feature_indices
        self.learning_rates = learning_rates
        self.epochs = epochs
        self.learning_rates_vs_MSE = []  # (lr, weights, mse)
        self.epochs_vs_MSE = []          # (epochs, weights, mse)
```

---

## 📝 Notes

- All features are **normalized** to [0, 1] range before training
- **Bias term** is always 1 and is not normalized
- Models are trained on the **entire dataset** (no train/test split)
- **MSE is calculated on training data** - not a final generalization metric
- For production use, consider: cross-validation, test set evaluation, and regularization

---

## 👤 Author
Generated for educational purposes in machine learning fundamentals.

## 📅 Date
March 2026

---

## 📚 References

- Multiple Linear Regression with Gradient Descent
- Feature Normalization (Min-Max Scaling)
- Hyperparameter Tuning Best Practices
