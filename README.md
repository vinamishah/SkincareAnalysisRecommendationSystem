**Project Title**: Skincare Analysis and Recommendation System

**Goal**:
The goal of the Skincare Product Analysis and Recommendation System is to leverage data-driven insights to provide consumers with the capability to make informed choices regarding cosmetic products. Personally, I am always on the lookout for more affordable and fungal acne-safe alternatives to products, which are also suitable for my skin type. By analysing skincare data and employing machine learning techniques, the Skincare Product Analysis and Recommendation System will do exactly this. First, it will provide the user with some skincare-related data including highly rated products, average prices of skincare products, extremely highly-priced products, frequently used ingredients for different products and skin types, and whether product characteristics like price and rating are correlated. Next, when the user inputs a certain product name, the recommendation system will output other products with similar formulations in the same overarching product category (e.g. Sunscreen, moisturizer etc ) suitable for the same skin type in ascending order of price with fungal acne triggers. This will help users find fungal acne-safe and more affordable alternatives to products with similar formulations and suitability. 

Function 	Description 
readSpreadsheet	Converts the hyperlink of the spreadsheet containing the data into a dataframe
cleanData	This function cleans the dataframe by dropping missing values and removing rows with '#NAME?', 'No info', or 'Visit' in the 'Ingredients' column. 
statFinder	This function:
1.Creates statistics dataframe where the index is product type the columns are Highest rated product name under each product type, mean price of each product type, max price of each product type, and min price of each product type.  
2.Outputs a bar graph comparing mean price across each product type 
3.Uses chi squared test to check if rating and price are correlated. 
priceOutliers	Identifies and outputs a list of names of products which have extremely high prices (using z score to find outliers). Also creates a histogram to visualise price distribution. 
ingredientAnalyser	Creates 2 dictionaries containing most common ingredients under each product type and each skin type. Considers only the active ingredients for both the cases(excludes water/aqua, glycerin, Phenoxyethanol and Butylene Glycol).
ingredientEncoding	Converts the top 80% of ingredient lists of each product into a numerical format using Binary Encoding. Adds these binary columns to the input dataframe. 
Kfinder	Finds the optimal k (number of clusters) value to perform K means clustering on the ingredient data using the elbow method.
applyKMeansClustering	Applies K mean clustering to identify clusters of products with similar ingredient lists. Outputs arrays containing cluster labels for each product and cluster centres. 
clusterVisualisation
	Visualizes the clustering results by reducing dimensionality using PCA and plotting clusters by colour in a 2D space. 
fungalAcneTriggers	Compares a pre-defined list of fungal acne triggers to create a fungal acne trigger list for every product and adds this column to dataframe. 
recommendationSystem	Takes in a product name from user and creates a dataframe of products with similar ingredient compositions, and the same product type and skin suitability in ascending order of price. A fungal acne trigger list for each of these products is present too. 
main 	Runs all the functions to output all the desired information
![image](https://github.com/user-attachments/assets/7dd362e9-1334-4c9c-ac2d-339cfc654f59)


**Program Overview and Requirements**:
In terms of the structure of the program, first I convert the data into a dataframe and clean the data set. Then, I carry out statistical analysis including finding highly rated products, pricing, ingredient frequency and correlation. Then, I convert the ingredient lists of every product into binary data and to carry out K means clustering. Lastly, I create a recommendation system that takes a product input from the user and outputs similar products with their fungal acne triggers (if any).  

Computation is mainly incorporated through machine learning.  Ingredient lists of multiple skincare products are converted into a numerical format using Ingredient Set Encoding. Each product's ingredients are represented as a set of binary columns. Then, K-means clustering is applied to group products into similar categories based on ingredient composition. The optimal k value is found using the elbow method. Then, to visualise this multi-dimensional data, the dimensionality is reduced using Principal Component Analysis. Statistics is incorporated by finding descriptive statistics of price under each product type and finding the highest rated products under each product. Also, by using the chi-squared test to check for correlation. Moreover, ingredient frequency analysis is carried out under each product and skin type. Lastly, high-price outliers are also found. Visualisation is incorporated by creating a bar graph comparing the mean price across each product type. Also, a histogram of the prices is created to visualise the price distribution. Lastly, ingredient cluster distribution is also visualised by plotting clusters by colour. 

**Data**: The dataset is obtained from Kaggle: https://www.kaggle.com/datasets/kingabzpro/cosmetics-datasets?resource=download. The dataset contains information about product type, brand, name, price, rank, ingredients, and binary skin type suitability. The dataset is also included in the local directory. 


**Assumptions and simplifications**:
Firstly, different ingredient names for the same ingredient in different lists are disregarded. In frequency analysis, while excluding inactive ingredients like water and glycerin it is assumed they are listed in the format the function expects. Some lists use slashes for alternative names; I split these into separate ingredients during encoding. Though this may double-count ingredients for specific products, it doesn't affect clustering results since the ingredients with alternate names would also be double-counted in other lists. The fungal acne trigger list in the code isn't exhaustive, and products may contain triggers not on the list. Lastly, the recommendation system assumes users input product names exactly as listed in the database; spelling errors or shortened names are not considered.

**Future directions**:
In the future, frequent ingredient combinations for each product and skin type could also be evaluated. Ingredient distribution could also be studied. Additionally, the recommendation system could be enhanced by integrating factors like user preferences and ratings. Additional datasets or external sources containing ingredient safety ratings or consumer reviews could also be leveraged.

**How to run**:
Install the requirements: pip install -r requirements.txt
Run the script: python skincare_analysis_recommendation.py
The code may take about a minute to run. It will ask to input a product name for which Crème de la Mer may be used as an example.

