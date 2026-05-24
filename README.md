Here is a complete, well-structured, and scannable `README.md` file designed for your GitHub repository based on the provided technology stack, project code, and dataset details.

---

# # Adidas US Sales Data Analytics & Machine Learning Application

An interactive web-based dashboard and API powered by **Flask** and **Scikit-Learn** that performs descriptive analytics and predictive machine learning tasks using the **Adidas US Sales Datasets.xlsx**.

The application utilizes three specialized machine learning architectures (Linear Regression, Decision Tree, and Random Forest) to analyze sales distribution trends, predict transaction types, and estimate retail demand volumes.

---

## ## Project Architecture & Machine Learning Pipelines

The system ingests raw transaction data, structures pipeline dependencies, and trains three individual estimators:

```
[ Adidas US Sales Datasets.xlsx ]
                │
                ├──> Model 1: Log-Linear Regression ───> Predicts Total Sales ($)
                ├──> Model 2: Decision Tree Classifier ─> Predicts Sales Method (Online/In-store/Outlet)
                └──> Model 3: Random Forest Regressor ──> Predicts Units Sold (Volume)

```

### ### Estimator Breakdown

#### **1. Total Sales Predictor (Linear Regression)**

* **Target Variable ($y$):** `Log_Total_Sales` (Log-transformed `Total Sales` to compress variance and stabilize skewness).
* **Preprocessing:** Categorical columns are transformed via **Frequency Encoding** (mapping categories to their data-wide occurrence probability). Numeric variables are standardized using `StandardScaler`. Target outlier mitigation is handled via a `0.01` to `0.99` quantile clip.
* **Evaluation Metrics:** Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and the Coefficient of Determination ($R^2$).

#### **2. Sales Method Classifier (Decision Tree)**

* **Target Variable ($y$):** `Sales_Method_Encoded` (`In-store: 0`, `Outlet: 1`, `Online: 2`).
* **Splitting Criterion:** `Entropy` (Information Gain maximization).
* **Preprocessing:** Independent features are vectorized using `LabelEncoder`. Stratified train-test splits are performed to avoid class imbalance biases.
* **Evaluation Metrics:** Classification Accuracy and Confusion Matrix distribution.

#### **3. Demand Volume Regressor (Random Forest)**

* **Target Variable ($y$):** `Units Sold` (Continuous numeric value).
* **Feature Engineering:** Extracts cyclical temporal variables from `Invoice Date`, outputting explicit features for `Year`, `Month`, and `Weekday`.
* **Estimator Setup:** Ensembled forest consisting of 200 individual base estimators (`n_estimators=200`) executed across all core processing threads (`n_jobs=-1`).
* **Evaluation Metrics:** $R^2$, MAE, MSE, RMSE, and a ranked index of Gini Feature Importance.

---

## ## Repository File Structure

```text
├── app.py                      # Flask Server Core & ML Training Execution
├── requirements.txt            # Package Dependencies
├── Adidas US Sales Datasets.xlsx# Source Data File Verbatim
├── templates/
│   └── index.html              # Frontend UI Layout Dashboard
└── README.md                   # Repository Documentation

```

---

## ## API Endpoints

The Flask background thread exposes the following transactional JSON REST interfaces:

| Method | Endpoint | Description |
| --- | --- | --- |
| **GET** | `/` | Serves the interactive user dashboard UI. |
| **GET** | `/api/sample-data` | Returns the first 10 raw historical database rows. |
| **GET** | `/api/model-info/<model_id>` | Returns model metadata, features, and historical evaluation metrics. |
| **GET** | `/api/visualization/<model_id>` | Generates matplotlib/seaborn evaluation plots as Base64 image strings. |
| **POST** | `/api/predict/<model_id>` | Accepts feature data payloads and returns live model inferences. |

---

## ## Core Technologies & Dependencies

This system was constructed using the specifications contained within `requirements.txt`:

* **Backend Framework:** `Flask (3.0.0)`
* **Data Structures & Arrays:** `pandas (2.0.3)`, `numpy (1.24.3)`
* **Data Science & Modeling:** `scikit-learn (1.3.0)`
* **Graphic Plot Rendering Engine:** `matplotlib (3.7.2)`, `seaborn (0.12.2)`
* **Excel Workbook Parsing:** `openpyxl (3.1.2)`

---

## ## Installation & Getting Started

Follow these operational steps to build your local deployment environment:

### ### 1. Environment Isolation and Package Restoration

Clone this repository to your disk architecture, navigate inside the working root, and initialize an isolated Python environment:

```bash
# Clone the repository
git clone <your-github-repo-url>
cd <repo-folder-name>

# Initialize the environment wrapper
python -m venv venv

# Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install exact dependency manifests
pip install -r requirements.txt

```

### ### 2. Launching the Server Backend

Execute the app initialization routine. This process reads **Adidas US Sales Datasets.xlsx**, fits all three mathematical modeling structures, and launches the HTTP loop:

```bash
python app.py

```

Upon successful startup, access the UI loop via your web browser:

> **Local Web Interface Address:** `http://localhost:5000`

---

## ## Model Performance & Interpretations

* **Model 1 (Linear Regression):** Demonstrates an exceptionally high structural correlation ($R^2 \approx 0.986$) due to the mathematical relationship between price, units sold, and total revenue.
* **Model 2 (Decision Tree):** Exhibits solid classification capabilities, using geographical patterns and revenue parameters to correctly identify fulfillment methods.
* **Model 3 (Random Forest):** Operates with minimal mean absolute error values, allowing supply chain managers to optimize warehouse stocking volumes by predicting retail demand accurately.
