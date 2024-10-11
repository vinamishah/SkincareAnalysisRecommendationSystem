**Project Title**: Skincare Analysis and Recommendation System

**Goal**:
The goal of the Skincare Product Analysis and Recommendation System is to leverage data-driven insights to provide consumers with the capability to make informed choices regarding cosmetic products. Personally, I am always on the lookout for more affordable and fungal acne-safe alternatives to products, which are also suitable for my skin type. By analysing skincare data and employing machine learning techniques, the Skincare Product Analysis and Recommendation System will do exactly this. First, it will provide the user with some skincare-related data including highly rated products, average prices of skincare products, extremely highly-priced products, frequently used ingredients for different products and skin types, and whether product characteristics like price and rating are correlated. Next, when the user inputs a certain product name, the recommendation system will output other products with similar formulations in the same overarching product category (e.g. Sunscreen, moisturizer etc ) suitable for the same skin type in ascending order of price with fungal acne triggers. This will help users find fungal acne-safe and more affordable alternatives to products with similar formulations and suitability. 

**Program Overview and Requirements**:
In terms of the structure of the program, first I convert the data into a dataframe and clean the data set. Then, I carry out statistical analysis including finding highly rated products, pricing, ingredient frequency and correlation. Then, I convert the ingredient lists of every product into binary data and to carry out K means clustering. Lastly, I create a recommendation system that takes a product input from the user and outputs similar products with their fungal acne triggers (if any).  

Computation is mainly incorporated through machine learning.  Ingredient lists of multiple skincare products are converted into a numerical format using Ingredient Set Encoding. Each product's ingredients are represented as a set of binary columns. Then, K-means clustering is applied to group products into similar categories based on ingredient composition. The optimal k value is found using the elbow method. Then, to visualise this multi-dimensional data, the dimensionality is reduced using Principal Component Analysis. Statistics is incorporated by finding descriptive statistics of price under each product type and finding the highest rated products under each product. Also, by using the chi-squared test to check for correlation. Moreover, ingredient frequency analysis is carried out under each product and skin type. Lastly, high-price outliers are also found. Visualisation is incorporated by creating a bar graph comparing the mean price across each product type. Also, a histogram of the prices is created to visualise the price distribution. Lastly, ingredient cluster distribution is also visualised by plotting clusters by colour. 

**Data**: The dataset is obtained from Kaggle: https://www.kaggle.com/datasets/kingabzpro/cosmetics-datasets?resource=download. The dataset contains information about product type, brand, name, price, rank, ingredients, and binary skin type suitability. The dataset is also included in the local directory. 

**Key Results**:

Chi-squared test result: Yes, price of a product and its rating are related. 

Statistics: 

<img width="662" alt="image" src="https://github.com/user-attachments/assets/88861076-1f43-4944-b8a6-a6f92fca7029">

<img width="500" alt="image" src="https://github.com/user-attachments/assets/7bb3e205-e346-4b12-a2b9-b671c4a55f22">


<img width="500" alt="image" src="https://github.com/user-attachments/assets/e1f31088-2521-4c64-ba75-57f6dd6a52b2">



Extremely high-priced products: ['LA MER - Little Miss Miracle Limited-Edition Crème de la Mer', 'FRESH - Crème Ancienne®', 'LA MER - The Concentrate', 'LA MER - The Regenerating Serum', 'BIOEFFECT - 30 Day Treatment', 'SHISEIDO - Future Solution LX Intensive Firming Contour Serum']

Most common ingredients according to product type: {'Moisturizer': 'Dimethicone', 'Cleanser': 'Citric Acid', 'Treatment': 'Sodium Hyaluronate', 'Face Mask': 'Xanthan Gum', 'Eye cream': 'Dimethicone', 'Sun protect': 'Dimethicone'}

Most common ingredients according to skin type: {'Combination': 'Ethylhexylglycerin', 'Dry': 'Ethylhexylglycerin', 'Normal': 'Caprylyl Glycol', 'Oily': 'Caprylyl Glycol', 'Sensitive': 'Caprylyl Glycol'} 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/e6d8b1c6-44ce-4807-8cdc-6b916e46b914">

Recommendation system (example input: Multi-Active Day Cream SPF 20 - All Skin Types):

<img width="575" alt="image" src="https://github.com/user-attachments/assets/9200029c-25d1-4901-8816-120cd3d521c1">

**Assumptions and simplifications**:
Firstly, different ingredient names for the same ingredient in different lists are disregarded. In frequency analysis, while excluding inactive ingredients like water and glycerin it is assumed they are listed in the format the function expects. Some lists use slashes for alternative names; I split these into separate ingredients during encoding. Though this may double-count ingredients for specific products, it doesn't affect clustering results since the ingredients with alternate names would also be double-counted in other lists. The fungal acne trigger list in the code isn't exhaustive, and products may contain triggers not on the list. Lastly, the recommendation system assumes users input product names exactly as listed in the database; spelling errors or shortened names are not considered.

**Future directions**:
In the future, frequent ingredient combinations for each product and skin type could also be evaluated. Ingredient distribution could also be studied. Additionally, the recommendation system could be enhanced by integrating factors like user preferences and ratings. Additional datasets or external sources containing ingredient safety ratings or consumer reviews could also be leveraged.

**How to run**:
Install the requirements: pip install -r requirements.txt
Run the script: python skincare_analysis_recommendation.py
The code may take about a minute to run. It will ask to input a product name for which Crème de la Mer may be used as an example.
