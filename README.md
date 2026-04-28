# Understanding Customer and Product Churn in Online Grocery Platforms

## 📌 Project Overview

This project presents a comprehensive data-driven analysis of churn in online grocery platforms, focusing on both **customer churn** and **product churn** as interconnected problems. Traditional approaches treat churn as a standalone customer-level prediction task. However, this project introduces a **unified behavioral framework** that connects customer activity with product-level dynamics. The core idea is that customer churn is often influenced by changes in product availability, demand, and engagement patterns.

Using large-scale transactional data from the Instacart dataset, this project analyzes behavioral signals such as recency, frequency, and reorder patterns to understand and predict churn.

> **Key Insight:** Churn is not a sudden event — it builds gradually through behavior.

👉 The main deliverable is:

**`main_notebook.ipynb`**

This notebook contains:
- Problem formulation
- Feature engineering
- Modeling
- Evaluation
- Business insights


## 🎥 Project Video - [Predicting Customer & Product Churn](https://www.youtube.com/watch?v=PAr57ze_g0w)


## ❓ Research Questions

Before the final churn-focused notebook, the project originally explored broader data mining research questions based on association rule mining and recommendation analysis.

These earlier research questions were useful because they helped establish the behavioral foundation for the final project.

### RQ1: Reliability of Discovered Patterns

This question studied whether co-purchase patterns discovered from Instacart baskets are stable under different mining settings.

It asked:

- How stable are association rules under different support thresholds?
- How does product-level representation compare with aisle-level representation?
- Are discovered rules reliable enough for downstream applications?

### RQ2: Temporal Variation in Co-Purchase Patterns

This question studied whether shopping behavior changes across time.

It asked:

- Do co-purchase patterns vary by day of week?
- Do co-purchase patterns vary by hour of day?
- Are temporal rule sets different from global rule sets?

### RQ3: Popularity vs Co-Occurrence vs Temporal Patterns in Prediction

This question compared different recommendation approaches.

It asked:

- How well does a popularity-based baseline predict future purchases?
- Do association rules improve prediction beyond popularity?
- Does sequential transition modeling improve recommendation coverage and usefulness?


## ❓Final Notebook Question

The final notebook focuses on the following main question:

> **Can behavioral patterns in transaction data be used to predict both customer churn and product churn in an online grocery platform?**

To answer this, the notebook explores three supporting areas.

### 1. Customer Churn Prediction

This section studies whether customer-level behavioral features can predict whether a customer is likely to become inactive.

It focuses on questions such as:

- Which customer behaviors are most related to churn?
- Do features like order frequency, recency, basket size, and reorder rate help identify high-risk customers?
- How does the churn definition affect model performance?

### 2. Product Churn Detection

This section studies whether product-level behavioral features can identify products that are losing demand.

It focuses on questions such as:

- Can historical purchase behavior detect products with declining demand?
- Which product signals are most useful for identifying churn-prone products?
- How do repeat purchase behavior and product popularity relate to product churn?

### 3. Cross-Behavioral Business Insights

This section connects customer churn and product churn to generate business-level insights.

It focuses on questions such as:

- Are customers exposed to more churn-prone products more likely to churn?
- Does weak repeat behavior in purchased products increase customer churn risk?
- Are some aisles or departments more churn-prone than others?
- Are loyal customers anchored by sticky, highly reordered products?



## 📂 Data


### Dataset Source

This project uses the **Instacart Market Basket Analysis dataset**, which is publicly available on Kaggle.

Dataset link: https://www.kaggle.com/competitions/instacart-market-basket-analysis/data

The dataset contains anonymized transactional data from an online grocery platform, including customer orders, product information, aisle information, department information, and reorder behavior.

### Dataset Scale

The dataset includes approximately:

- 3.4 million orders
- 32 million prior order-product interactions
- 200,000+ users
- 49,000+ products
- Product, aisle, and department metadata

### Kaggle Setup

Because the dataset is large, it is not included directly in this repository. To reproduce this project, download the dataset from Kaggle and place it in the expected local directory.

Create the dataset folder:

    mkdir -p data/instacart

Install the Kaggle package:

    pip install kaggle

Create the Kaggle configuration folder:

    mkdir -p ~/.kaggle

Place your Kaggle API key file here:

    ~/.kaggle/kaggle.json

Download the dataset:

    kaggle competitions download -c instacart-market-basket-analysis

Unzip the dataset into the project data folder:

    unzip instacart-market-basket-analysis.zip -d data/instacart

### Expected Directory Structure

After downloading and extracting the dataset, the folder should look like this:

    data/
    └── instacart/
        ├── orders.csv
        ├── order_products__prior.csv
        ├── order_products__train.csv
        ├── products.csv
        ├── aisles.csv
        └── departments.csv

### Data Usage in This Project

The notebook reads the dataset directly from the local `data/instacart/` directory. All preprocessing, feature engineering, churn label creation, modeling, and analysis are handled inside the notebook workflow.

No dataset files are committed to GitHub because of size constraints.


---

## ⚙️ Data Processing & Feature Engineering

The project converts raw transactional data into structured behavioral features that can be used for analysis and modeling. Feature engineering is performed entirely within the notebook to ensure reproducibility.

### Customer-Level Features
Customer features summarize each user’s purchasing behavior over time. These features are designed to capture engagement, consistency, and purchasing habits.Key features include:
- Total number of orders- Average basket size
- Total number of unique products purchased
- Reorder ratio (fraction of previously purchased items reordered)
- Days since last order (recency)
- Average time gap between orders
- Order frequency and activity patterns

These features are used to model customer churn and identify at-risk users.

### Product-Level Features
Product features capture demand patterns and repeat purchase behavior for each product.Key features include:
- Total purchase count- Number of unique customers purchasing the product
- Reorder count and reorder ratio
- Product popularity trends over time
- Demand consistency across time windows
- Aisle and department-level groupingThese features are used to detect products that are losing demand (product churn).

### Temporal Features
Time-based features are used to capture when purchasing behavior occurs. Key features include:
- Orders by day of week- Orders by hour of day
- Average basket size by time
- Reorder behavior across time windows
- Time since prior order
 
These features help analyze how behavior varies across different temporal contexts.

### Cross-Behavioral Features
To connect customer churn and product churn, additional features are created to capture the interaction between customers and the types of products they engage with.Examples include:
- Exposure to low-reorder or weak-demand products
- Ratio of weak products in a customer’s purchase history
- Interaction with highly reordered (sticky) products
- Category-level exposure (aisle/department behavior)

These features enable the unified analysis of churn across customers and products.

### Feature Design RationaleThe feature engineering process is guided by three key principles:
- Behavioral signals (recency, frequency, reorder patterns) are strong indicators of engagement
- Aggregation at multiple levels (customer, product, category) captures different perspectives
- Temporal context is important for understanding changes in behavior over time

All features are designed to be interpretable and aligned with real-world business signals, making the final analysis both practical and explainable.


## 🤖 Modeling Approach

The project models churn at two levels: customer churn and product churn, and then combines them for unified insights.

### Customer Churn

- Framed as a classification problem  
- Uses behavioral features (recency, frequency, basket size, reorder ratio)  
- Random Forest models capture non-linear relationships  
- Multiple churn definitions (low, medium, high) ensure robustness  

### Product Churn

- Identifies products with declining demand over time  
- Uses a time-based split:
  - Historical window (X) → feature creation  
  - Future window (Y) → demand change  
- Labels products as churn-prone if future activity drops significantly  


### Combined Analysis

- Links product behavior with customer churn  
- Exposure to weak products increases churn risk  
- Sticky (high-reorder) products improve retention  
- Category-level patterns highlight systemic risk  

### Design Principles

- Interpretable  
- Flexible (multiple churn definitions)  
- Scalable  
- Aligned with real-world business use cases  

## 📊 Evaluation Metrics

The performance of the churn models is evaluated using standard classification metrics. These metrics provide a balanced view of model effectiveness across different aspects of prediction quality.

### Metrics Used

- Accuracy  
  Measures the overall correctness of the model predictions.

- Precision  
  Measures how many predicted churn cases are actually correct. This is important when false positives are costly.

- Recall  
  Measures how many actual churn cases are successfully identified. This is important when missing churn cases is costly.

- F1 Score  
  Provides a balance between precision and recall, especially useful when the classes are imbalanced.

- ROC-AUC (Receiver Operating Characteristic - Area Under Curve)  
  Evaluates model performance across all classification thresholds. This is a key metric in this project because it provides a threshold-independent view of model quality.



### Evaluation Strategy

- Models are evaluated separately for each churn definition (low, medium, high sensitivity)
- Performance is compared across these definitions to understand robustness
- Metrics are analyzed together rather than relying on a single number
- ROC-AUC is used as the primary metric for overall comparison


### Why These Metrics

Churn prediction is typically an imbalanced classification problem, where non-churn cases may dominate the dataset. Because of this:

- Accuracy alone can be misleading
- Precision and recall provide better insight into model behavior
- F1 score helps balance trade-offs
- ROC-AUC provides a holistic view of separability

This combination ensures that the evaluation reflects both predictive accuracy and practical usefulness.


## 📈 Key Results & Insights


The final analysis shows that churn is strongly driven by behavioral patterns rather than isolated events. By modeling both customer and product behavior, the project uncovers several important insights.

### Customer Behavior Insights

- Customers with declining engagement are more likely to churn  
- Recency is one of the strongest indicators of churn risk  
- Lower order frequency and smaller basket sizes are associated with higher churn probability  
- Reduced reorder behavior signals weakening customer loyalty  

These patterns indicate that churn can often be detected early through gradual behavioral changes.


### Product Behavior Insights

- Products with low reorder rates are more likely to experience demand decline  
- A small subset of products accounts for a large portion of total purchases (long-tail effect)  
- Stable, frequently reordered products show consistent demand over time  
- Some categories naturally exhibit weaker repeat behavior than others  

These findings help identify products that may require intervention, such as promotions or repositioning.


### Cross-Behavioral Insights

- Customers exposed to weaker or low-reorder products tend to have higher churn risk  
- Interaction with highly reordered (sticky) products improves customer retention  
- Product experience plays a significant role in shaping customer behavior  
- Certain departments and categories show concentrated churn risk  

This confirms that customer churn and product churn are interconnected rather than independent problems.

---

### Overall Takeaway

The combined analysis supports a key conclusion:

Churn is not random. It is driven by measurable behavioral signals and can be predicted using transaction data.

By linking customer activity with product demand patterns, the project provides a more complete understanding of churn in online grocery platforms.

## 💼 Business Impact

The insights from this project can be directly applied to improve decision-making in online grocery platforms. By identifying early signals of churn at both customer and product levels, businesses can take proactive steps to reduce revenue loss and improve user retention.

### Customer Retention

- Identify high-risk customers early based on behavioral signals  
- Enable targeted interventions such as personalized offers or reminders  
- Improve engagement strategies by focusing on declining activity patterns  


### Product Strategy

- Detect products that are losing demand before they become inactive  
- Support promotion and bundling decisions for weak-performing products  
- Improve assortment planning and inventory allocation  


### Category-Level Insights

- Identify departments or aisles with higher churn risk  
- Understand which categories drive consistent engagement  
- Support strategic decisions around product placement and visibility  


### Recommendation Systems

- Move beyond simple popularity-based recommendations  
- Incorporate behavioral signals such as reorder patterns and recency  
- Prioritize products that contribute to long-term customer retention  


### Operational Benefits

- Reduce churn-related revenue loss  
- Improve efficiency in marketing and promotions  
- Enable data-driven decision making across teams  


### Key Business Takeaway

Churn is not only predictable but also actionable. By leveraging behavioral data, businesses can:

- Detect churn early  
- Understand its underlying causes  
- Take targeted actions to prevent it  

This makes churn analysis a critical component of modern online retail systems.


## 🛠️ How to Reproduce

This project is designed to be fully reproducible using a standard Python environment or Google Colab.

### 1. Clone the Repository

    git clone https://github.com/<your-username>/<your-repo>.git
    cd <your-repo>

### 2. Install Dependencies

    pip install -r requirements.txt

### 3. Download the Dataset

Download the Instacart dataset from Kaggle:

    https://www.kaggle.com/competitions/instacart-market-basket-analysis/data

Follow the setup instructions in the **Data** section to place the dataset inside:

    data/instacart/

### 4. Run the Main Notebook

Open and execute:

    main_notebook.ipynb

### Recommended Execution Flow

Run the notebook cells in the following order:

1. Load dataset and validate data  
2. Perform preprocessing and cleaning  
3. Generate customer-level features  
4. Generate product-level features  
5. Define churn labels  
6. Train customer churn models  
7. Analyze product demand decline  
8. Evaluate model performance  
9. Generate business insights  

## Environment

This project was developed using Google Colab.

Recommended environment:

    Python 3.11


## Key Dependencies

The main libraries used in this project include:

    pandas
    numpy
    matplotlib
    seaborn
    scikit-learn
    mlxtend

The complete list of dependencies is available in:

    requirements.txt

## Repository Structure

The repository is organized to keep the final deliverable, supporting notebooks, data instructions, and environment files easy to locate.

    .
    ├── README.md
    ├── requirements.txt
    ├── main_notebook.ipynb
    ├── checkpoints/
    │   ├── checkpoint_1.ipynb
    │   └── checkpoint_2.ipynb
    ├── notebooks/
    │   ├── Exploratory Data Analysis.ipynb
    │   └── RQ Formation.ipynb
    ├── data/
    │   └── instacart/
    └── assets/

### File Descriptions

- `README.md`: Project overview, setup instructions, reproduction steps, and results summary.
- `requirements.txt`: Python dependencies required to run the project.
- `main_notebook.ipynb`: Final curated notebook and the main file to review.
- `checkpoints/`: Earlier project checkpoint notebooks showing project progression.
- `notebooks/`: Supporting exploratory notebooks and research question formation work.
- `data/instacart/`: Local dataset folder. The dataset is not committed to GitHub because of size constraints.
- `assets/`: Optional folder for figures, plots, presentation images, or other supporting visuals.

### Notes

- Start with `main_notebook.ipynb`.
- Keep the Instacart dataset inside `data/instacart/`.
- Ensure `requirements.txt` is placed at the root of the repository.

## Future Work

- Apply time-series and sequential models to better capture behavioral trends  
- Refine churn definitions beyond simple thresholds  
- Incorporate additional features such as pricing, promotions, and product availabil-ity  
- Extend the framework to support real-time churn detection  
- Integrate insights with recommendation and inventory systems  
- Validate results on other datasets and through real-world testing

## Results Summary

This project shows that churn in online grocery platforms is driven by behavioral patterns that can be identified using transaction data. Customer churn is strongly associated with declining engagement, including lower order frequency, reduced basket size, and weaker reorder behavior. Product churn is characterized by declining demand and low repeat purchase rates.

A key finding is that customer churn and product churn are interconnected. Customers who interact more with low-performing or weak-demand products are more likely to churn, while highly reordered (sticky) products contribute to retention. Overall, the project demonstrates that churn is predictable and can be analyzed through behavioral signals, enabling more informed decisions around customer retention and product strategy.