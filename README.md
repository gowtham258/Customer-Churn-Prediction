## 📊 Features & Screenshots

### 1. Churn Trends

* **Bar Chart**: Displays churn rate (percentage of customers who left) by `tenure_group` (0–12, 13–24, 25–48, 49–60, 60+ months).
* **Line Chart**: Plots churn rate across each month of raw `tenure` (0–72+ months), illustrating how churn probability changes over time.

### 2. High-Risk Customer Groups

* **Churn by Contract Type**: Shows churn rates for “Month-to-month”, “One year”, and “Two year” contract customers.
* **Churn by Payment Method**: Horizontal bar chart comparing churn rates across payment methods (e.g., Electronic Check, Mailed Check, Bank Transfer, Credit Card).

### 3. Model Accuracy & Performance

* **Classification Report**: Dash DataTable listing precision, recall, F1-score, and support for “No Churn” vs. “Churn”, plus macro/weighted averages.
* **Confusion Matrix**: 2×2 heatmap annotated with true positives, false positives, true negatives, and false negatives.
* **ROC Curve**: Line plot of True Positive Rate vs. False Positive Rate, with AUC displayed in the title.

---

## 🛠️ Code Structure & Key Functions

* **`load_and_preprocess()`**

  * Reads the raw CSV
  * Converts and imputes `TotalCharges`
  * Drops `customerID`
  * Binarizes `Churn`
  * Creates `tenure_group` categories

* **`build_preprocessor()`**

  * Defines a `ColumnTransformer` that:

    * Median-imputes & standardizes numeric features (`tenure`, `MonthlyCharges`, `TotalCharges`)
    * Mode-imputes & one-hot encodes all other categorical columns

* **`train_model()`**

  * Splits into train/test (80/20, stratified by `Churn`)
  * Upsamples minority class (Churn=1) in training set
  * Fits `RandomForestClassifier(n_estimators=100)`
  * Returns model, test data, predictions, classification report dict, confusion matrix, ROC curve (FPR, TPR), and AUC

* **Dash Layout & Callbacks**

  * Two multi-select dropdowns: id=`tenure-filter` and id=`contract-filter`
  * Four `dcc.Graph` components (IDs:
    `churn-by-tenure-group`, `churn-over-tenure-months`, `churn-by-contract`, `churn-by-payment-method`)
  * One callback—`update_charts`—listening on both dropdowns:

    * Filters the raw DataFrame (`df_raw`) by selected tenure groups and contract types
    * Recomputes and returns four Plotly figures for sections 1 & 2

---

## 📋 Dependencies

Listed in `requirements.txt` (or install manually):

* `dash>=2.0.0`
* `pandas>=1.3.0`
* `numpy>=1.21.0`
* `scikit-learn>=1.2.0`
* `plotly>=5.0.0`

Example `requirements.txt`:

```
dash
pandas
numpy
scikit-learn
plotly
```

---

## 🤝 Contributing

1. Fork this repository.
2. Create a new branch (`git checkout -b feature/YourFeature`).
3. Make your changes, commit (`git commit -m "Add new feature"`), and push (`git push origin feature/YourFeature`).
4. Open a pull request explaining your updates.

---
checkout visualisation : https://drive.google.com/drive/folders/19vWYgGac3aJ7NGqRp0HH1AuiCfTUxjTj?usp=sharing
