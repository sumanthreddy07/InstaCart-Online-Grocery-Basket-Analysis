# 🛒 Instacart Online Grocery Basket Analysis

## 📖 Overview

This project's current objective is it performs a comprehensive Exploratory Data Analysis (EDA) on the Instacart Market Basket Analysis dataset.

The objective is to understand customer purchasing behavior, basket composition, reorder dynamics, and category-level trends before applying advanced data mining or machine learning techniques.

The current implementation focuses on:

- Order-level analysis  
- Basket behavior  
- Product popularity  
- Reorder patterns  
- Category-level insights  
- User purchasing behavior  

The analysis is designed to be reproducible, structured, and scalable.

---

## 📂 Dataset

This project uses the **Instacart Market Basket Analysis** dataset (Kaggle).

You must manually download the dataset and place the following files inside a local folder:


- `orders.csv`
- `order_products__prior.csv`
- `order_products__train.csv`
- `products.csv`
- `aisles.csv`
- `departments.csv`

**Dataset Scale:**
- 3.4M+ orders  
- 32M+ prior line-items  
- 206K+ users  
- 49K+ products  

---

## 🔎 Exploratory Data Analysis

The first notebook focuses on comprehensive EDA, including:

### Data Validation & Sanity Checks
- Verified data types and memory optimization
- Checked missing values (only expected missing values in `days_since_prior_order`)
- Confirmed order/user/product uniqueness

### Basket-Level Analysis
- Basket size distribution
- Unique product count per order
- Reorder ratio within baskets
- Distribution clipping for outlier handling

### Temporal Behavior
- Orders by day of week
- Orders by hour of day
- Average basket size by hour
- Average reorder ratio by hour
- Distribution of days since prior order

### Product Popularity & Concentration
- Top products by purchase frequency
- Cumulative coverage curve (long-tail analysis)
- Percentage of products needed to cover 50%, 80%, 90%, 95% of transactions

### 7️User-Level Analysis
- Orders per user distribution
- Average basket size per user
- Average reorder ratio per user
- User segmentation by order count

---

## 📊 Key Initial Insights

- Shopping baskets are right-skewed; most contain fewer than 15 items.
- A small fraction of products accounts for the majority of purchases (strong long-tail effect).
- Dairy and produce show the highest reorder rates, suggesting staple behavior.
- Pantry and personal care categories have lower reorder rates.
- Morning hours show elevated reorder ratios, suggesting habitual shopping.
- Organic products have a slightly higher average reorder rate than non-organic products.

---

## ⚙️ How to Run

1. Clone the repository:
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```
2. Place the dataset files inside:
```bash
./instacart/
```

3. Open the notebook and ensure:
```bash
DATA_DIR = "./instacart"
```

4. Run all cells.

## 🧠 Next Steps  

### Planned Future Extensions  

- Frequent itemset mining (Apriori / FP-Growth)  
- Association rule generation (confidence, lift analysis)  
- Sequential pattern mining (time-aware purchasing behavior)  
- Predictive modeling for reorder probability  


---

## 📁 Repository Structure  

├── notebooks/
│ └── instacart_eda.ipynb
├── instacart/ (dataset folder - not included in repo)
├── README.md
└── requirements.txt

---

## 🏗️ Tech Stack  

- Python  
- Pandas  
- NumPy  
- Matplotlib  
- Seaborn  


---

## 👤 Author  

**Venkata Sumanth Reddy Kota**  
Master’s Student – Computer Science  
Texas A&M University

---

## 📌 Note  

This project is being actively updated as part of a course progression.  
Future commits will include additional analyses, modeling approaches, and performance comparisons.
