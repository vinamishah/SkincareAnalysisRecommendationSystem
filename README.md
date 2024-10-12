# Skincare Analysis and Recommendation System

## Goal
The goal of the Skincare Product Analysis and Recommendation System is to leverage data-driven insights to help consumers make informed choices regarding cosmetic products. 

First, by analyzing skincare data and employing machine learning techniques, this system provides insights into:
- Highly-rated products
- Average prices of skincare products
- Frequently used ingredients for different product and skin types
- Whether product characteristics like price and rating are correlated

Next, when the user inputs a product name, the system outputs similar products with similar formulations, sorted by price, and flags potential fungal acne triggers. This will help users find affordable, fungal acne-safe alternatives.

Next, when the user inputs a product name, the recommendation system outputs products with similar formulations in the same overarching product category (e.g., sunscreen, moisturizer, etc) suitable for the same skin type. The products are also sorted by price and have fungal acne trigger alerts. This will help users find affordable, fungal acne-safe alternatives.

## Program Overview and Requirements
The program structure includes the following steps:
1. Convert the data into a dataframe and clean it.
2. Perform statistical analysis (e.g., product ratings, pricing, and ingredient frequency).
3. Convert ingredient lists into binary data and apply K-means clustering to group similar products.
4. Build a recommendation system that suggests products based on ingredient similarity.

### Key Machine Learning Techniques:
- **Ingredient Set Encoding**: Converts ingredients into binary columns for numerical analysis.
- **K-Means Clustering**: Groups products based on ingredient composition(optimal k value is found using the elbow method).
- **Principal Component Analysis (PCA)**: Reduces data dimensionality for visualization.

## Data
- **Source**: The dataset is obtained from Kaggle: [Cosmetics Datasets](https://www.kaggle.com/datasets/kingabzpro/cosmetics-datasets?resource=download).
- The dataset includes information about product type, brand, name, price, rank, ingredients, and binary skin type suitability.

## Key Results

- **Chi-Squared Test Result**: Yes, the price of a product and its rating are correlated.

- **Statistics**: 

<img width="662" alt="image" src="https://github.com/user-attachments/assets/88861076-1f43-4944-b8a6-a6f92fca7029">

<img width="500" alt="image" src="https://github.com/user-attachments/assets/7bb3e205-e346-4b12-a2b9-b671c4a55f22">


<img width="500" alt="image" src="https://github.com/user-attachments/assets/e1f31088-2521-4c64-ba75-57f6dd6a52b2">


- **Extremely high-priced products**: ['LA MER - Little Miss Miracle Limited-Edition Crème de la Mer', 'FRESH - Crème Ancienne®', 'LA MER - The Concentrate', 'LA MER - The Regenerating Serum', 'BIOEFFECT - 30 Day Treatment', 'SHISEIDO - Future Solution LX Intensive Firming Contour Serum']

- **Most common ingredients according to product type**: {'Moisturizer': 'Dimethicone', 'Cleanser': 'Citric Acid', 'Treatment': 'Sodium Hyaluronate', 'Face Mask': 'Xanthan Gum', 'Eye cream': 'Dimethicone', 'Sun protect': 'Dimethicone'}

- **Most common ingredients according to skin type**: {'Combination': 'Ethylhexylglycerin', 'Dry': 'Ethylhexylglycerin', 'Normal': 'Caprylyl Glycol', 'Oily': 'Caprylyl Glycol', 'Sensitive': 'Caprylyl Glycol'} 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/e6d8b1c6-44ce-4807-8cdc-6b916e46b914">

- **Recommendation system** (example input: Multi-Active Day Cream SPF 20 - All Skin Types):

<img width="575" alt="image" src="https://github.com/user-attachments/assets/9200029c-25d1-4901-8816-120cd3d521c1">

## Assumptions and Simplifications
- Ingredient names with slashes (e.g., alternative names) are split and double-counted in some cases.
- Inactive ingredients like water and glycerin are excluded from ingredient frequency analysis.
- The fungal acne trigger list is not exhaustive.
- Users must input product names exactly as listed in the dataset (no spelling errors or variations are accounted for).

## Future Directions
- Explore frequent ingredient combinations for different products and skin types.
- Incorporate user preferences or product ratings into the recommendation system.
- Leverage additional datasets for ingredient safety ratings or consumer reviews.

## How to Run
1. **Install the requirements**:
    ```bash
    pip install -r requirements.txt
    ```
2. **Run the script**:
    ```bash
    python skincare_system.py
    ```
3. The code will take about a minute to run. When prompted, enter a product name (e.g., `Crème de la Mer`) for the recommendation system.
