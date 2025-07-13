# Personalized E-Commerce Product Recommender System

## Overview

This project implements a personalized product recommendation system for an e-commerce platform, leveraging a **real-world product ratings dataset sourced from Amazon**. The primary goal is to enhance user experience and engagement by intelligently suggesting relevant products based on historical user interactions.

In highly competitive e-commerce environments, personalized recommendations are crucial for improving discoverability, driving sales, and fostering customer loyalty. This system addresses this need by identifying patterns in user preferences to deliver tailored product suggestions.

## Dataset

The system utilizes a comprehensive dataset from Amazon, containing explicit user ratings for various products. Key columns include:
* `product_id`
* `product_name`
* `category`
* `discounted_price`
* `actual_price`
* `discount_percentage`
* `rating`
* `user_id`
* `user_name`
* `img_link`
* `product_link`

A critical characteristic of this dataset is its **extreme sparsity (99.91%)** in the user-item interaction matrix, meaning very few users have rated a small fraction of the available products. This presents a significant challenge for collaborative filtering models.

## Methodology

The development of the recommender system followed a structured approach:

1.  **Data Preprocessing & Cleaning:**
    * Handling of missing values, particularly for the `rating` column (imputed with the mean).
    * Standardization of column names and data types.
    * **User Name Normalization:** Cleaned the `user_name` column to extract only the first name, addressing concatenated names and ensuring consistency.
    * Preparation of data into a format suitable for the `Surprise` library.

2.  **Exploratory Data Analysis (EDA):**
    * Visualized key aspects of the dataset using various plots (Bar Charts, Histograms, Scatter Plots, Heatmap, Pie Chart).
    * **Key Insights from EDA:**
        * Identified top-rated products and most popular categories (e.g., Electronics, Computers&Accessories).
        * Analyzed the distribution of ratings, revealing generally high customer satisfaction.
        * Explored price and discount distributions across products.
        * Investigated relationships between ratings, prices, and discounts, finding no strong linear correlation between discount and rating.
        * Identified top active users, showing a skewed distribution of user engagement.
        * Visualized the proportional distribution of products across different categories.
        * Examined correlations between numerical features.

3.  **Collaborative Filtering Model (SVD):**
    * The **Singular Value Decomposition (SVD)** algorithm was chosen as the core collaborative filtering model. SVD is a matrix factorization technique that decomposes the user-item interaction matrix to uncover latent features representing user preferences and item characteristics.
    * The model was trained using explicit user ratings.

4.  **Hyperparameter Tuning:**
    * `GridSearchCV` from the `Surprise` library was employed to perform extensive hyperparameter tuning for the SVD model. This process optimized parameters such as `n_factors`, `n_epochs`, `lr_all`, and `reg_all` to minimize prediction error.

5.  **Model Scope Decision:**
    * Given the extreme data sparsity and the project's scope, a decision was made to focus solely on the **pure collaborative filtering (SVD) approach**, without incorporating content-based (NLP) features. This highlights the inherent challenges and limitations of such models in highly sparse environments.

## Results and Performance

The tuned SVD model's performance on the test set is as follows:

* **RMSE (Root Mean Squared Error):** 0.2497
* **MAE (Mean Absolute Error):** 0.1834
* **R2 Score (Coefficient of Determination):** 0.1236

**Observations:**

* The low RMSE and MAE indicate that the model makes relatively accurate predictions for the ratings it can learn from. On average, predictions are off by approximately 0.25 stars.
* The $R^2$ score, while positive, remains low. This is a direct consequence of the **99.91% data sparsity**. Pure collaborative filtering models struggle to explain a large proportion of the variance in ratings when most of the user-item interactions are unobserved.
* The model effectively identifies patterns from existing user-item interactions and can generate personalized recommendations based on these learned preferences. However, it inherently faces the **cold-start problem** for new users (with no rating history) and new products (with no ratings).

## Key Features & Output

The system demonstrates its functionality by:

* Randomly selecting a user from the dataset.
* Predicting ratings for products that the selected user has not yet rated.
* Displaying the top 6 recommended products in a clear, user-friendly format, including:
    * Product Name
    * Estimated Rating
    * A direct link to buy the product on Amazon.

## How to Run the System

To set up and run this recommender system:

### Prerequisites

* Python 3.x
* Jupyter Notebook or Google Colab environment
