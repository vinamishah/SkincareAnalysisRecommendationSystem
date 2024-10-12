# Skincare Analysis and Recommendation System

## Problem
Consumers often struggle to find skincare products that meet their specific needs, such as affordability, suitability for their skin type, and safety from harmful ingredients like fungal acne triggers. While there are well-known, highly-rated products available, these often come with high price tags, may contain ingredients that aren't safe for fungal acne, or aren't suitable for certain skin types. This makes it difficult for consumers to find affordable and safe alternatives that work for them.

## Solution
The Skincare Product Analysis and Recommendation System leverages data-driven insights and machine learning to help consumers make informed choices about skincare products. The system provides:

Data Insights: The system offers valuable information on
- Highly-rated products
- Average prices of skincare products
- Extremely high-priced products
- Frequently used ingredients for different products and skin types
- Whether characteristics like price and rating are correlated

These insights help consumers better understand the skincare market. They can see how much they should typically pay for different types of skincare products, identify the best and most expensive products in each category, and determine whether higher prices are truly linked to better ratings. Additionally, it provides guidance on which ingredients to look for in different products based on their specific skin type.

Product Recommendations: Users can input the name of a product, and the system will recommend similar products with similar formulations, sorted by price and suitable for the same skin type. The recommendations also include alerts for any fungal acne triggers, making it easier for users to find affordable, safe alternatives.

This helps consumers discover more affordable, fungal acne-safe products with similar formulations to the expensive, well-known products they might be familiar with.

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
