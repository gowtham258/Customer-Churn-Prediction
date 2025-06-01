# Customer-Churn-Prediction
```markdown
# Telco Customer Churn Dashboard

A Dash‐based interactive dashboard for analyzing customer churn in a telecom dataset. This application:

- **Visualizes churn trends** by tenure group and over months of tenure  
- **Highlights high-risk segments** by contract type and payment method  
- **Displays model performance** metrics (classification report, confusion matrix, ROC curve) for a Random Forest churn‐prediction model  

---

## 📂 Repository Structure

```

.
├── app.py
├── WA\_Fn-UseC\_-Telco-Customer-Churn.csv
├── README.md
└── requirements.txt

````

- **app.py**  
  Main Dash application containing data loading, preprocessing, model training, and layout callbacks.  
- **WA_Fn-UseC_-Telco-Customer-Churn.csv**  
  Raw Telco customer churn dataset (publicly available).  
- **requirements.txt**  
  Python dependencies needed to run the dashboard.  
- **README.md**  
  This file: instructions, overview, and usage guidelines.

---

## 📝 Overview

The Telco Customer Churn Dashboard is a lightweight Dash application that:

1. **Loads & cleans** the Telco Churn CSV:  
   - Converts `TotalCharges` to numeric and imputes missing values  
   - Drops `customerID` and binarizes `Churn` (Yes→1, No→0)  
   - Creates a categorical `tenure_group` from the numeric `tenure` column  

2. **Preprocesses features** using a `ColumnTransformer`:  
   - Numeric columns (`tenure`, `MonthlyCharges`, `TotalCharges`) are median‐imputed and standardized  
   - All other columns are most‐frequent‐imputed and one-hot encoded  

3. **Trains a Random Forest classifier** (with 80/20 train/test split and upsampling of the minority class in training) to predict customer churn. The final model metrics include:  
   - **Classification report** (precision, recall, F1‐score, support)  
   - **Confusion matrix**  
   - **ROC curve** and **AUC**  

4. **Renders an interactive dashboard** with four main sections:  
   - **Churn Trends** (bar chart by tenure group; line chart over tenure months)  
   - **High-Risk Customer Groups** (bar chart by contract type; horizontal bar chart by payment method)  
   - **Model Accuracy & Performance** (Dash DataTable for classification report; heatmap for confusion matrix; ROC curve plot)  
   - **Filter controls** (multi-select dropdowns) to dynamically filter by `tenure_group` and `Contract`  

---

## ⚙️ Installation & Setup

1. **Clone or download** this repository to your local machine.

2. **Create a Python virtual environment** (recommended) and activate it.

   ```bash
   python3 -m venv venv
   source venv/bin/activate        # macOS/Linux
   # .\venv\Scripts\activate      # Windows PowerShell
````

3. **Install dependencies** using `requirements.txt`:

   ```bash
   pip install -r requirements.txt
   ```

   If `requirements.txt` isn’t provided, install manually:

   ```bash
   pip install dash pandas scikit-learn plotly
   ```

4. **Ensure the dataset file**
   The file `WA_Fn-UseC_-Telco-Customer-Churn.csv` should be in the same directory as `app.py`. This raw CSV is publicly available and contains customer demographic, account, and service information along with a `Churn` column.

---

## 🚀 Running the Dashboard

1. Open a terminal (with your virtual environment activated) in the project folder.

2. Run:

   ```bash
   python app.py
   ```

3. Once the server starts, open a web browser and navigate to:

   ```
   http://127.0.0.1:8050/
   ```

4. You will see the dashboard interface with:

   * Two dropdowns at the top (“Filter by Tenure Group” and “Filter by Contract Type”)
   * Four interactive Plotly charts below, updating automatically based on those filters
   * Static model‐performance tables/plots in the “Model Accuracy & Performance” section

---

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

