import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
import scipy as sp
import unittest
from scipy import stats
from scipy.stats import chi2_contingency
from scipy.stats import chi2
from kneed import KneeLocator
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA
from matplotlib.colors import ListedColormap

skincareData= 'data/skincare_data.xlsx'    # Path to the dataset in the local directory

def readSpreadsheet(link):
  """Converts a hyperlink to a dataframe"""
  df = pd.read_excel(link)   #convert hyperlink to a dataframe
  return df

def cleanData(df):
  """This function cleans the datatset by dropping missing values and removing rows with specified strings."""
  #handling missing values
  df.dropna(inplace=True)
  #Remove rows with '#NAME?', 'No info', or 'Visit' in the 'Ingredients' column
  cleandf = df[~df['Ingredients'].str.contains('#NAME?|No info|Visit')]
  return cleandf

def statFinder(df):
  """This function creates statistics dataframe, uses chi squared test to check correlation and visualises the statistics"""
  statdf= pd.DataFrame(index=['Moisturizer', 'Cleanser', 'Treatment', 'Face Mask', 'Eye cream', 'Sun protect'], columns=['Highest rated', 'Mean price($)', 'Min price($)', 'Max price($)'])  #creating an empty dataframe with index as product type

  #Finding highest rated product(s) under each product type (outputting product and brand name)
  for product in statdf.index:
    productDf= df[df['Label'] == product]   #filtering df for product type
    maxR= productDf['Rank'].max()   # maximum rank for the product type
    maxRrows = productDf[productDf['Rank'] == maxR]  # filter rows with the highest rank
    maxRlist = []   #empty list to store the highest-rated products
    for index, row in maxRrows.iterrows():    # Iterate over each row containing maxR products
        name = f"{row['Brand']} - {row['Name']}"
        maxRlist.append(name) #add to list
    maxRproducts = ", ".join(maxRlist) #convert list to string
    statdf.loc[product, 'Highest rated'] = maxRproducts   #add to dataframe

    #Finding summary statistics for price under each product type
    summary = sp.stats.describe(productDf['Price'])
    statdf.loc[product, 'Mean price($)'] = summary.mean
    statdf.loc[product, 'Min price($)'] = summary.minmax[0]
    statdf.loc[product, 'Max price($)'] = summary.minmax[1]

  #Bar graph comparing mean price across all product types
  plt.figure(figsize=(10, 6))
  sns.barplot(x=statdf.index, y=statdf['Mean price($)'])
  plt.title('Mean Price Across Product Types')
  plt.xlabel('Product Type')
  plt.ylabel('Mean Price($)')
  plt.xticks(rotation=45)
  plt.show()

  # Using chi-squared test to check if the variables are correlated
  data= [df['Price'],df['Rank']]
  stat,p,dof,expected=chi2_contingency(data)
  if p < 0.05:
      print("Yes, price of a product and its rating are related")
  else:
      print("No, price of a product are its rating are not related")
  return statdf

def priceOutliers(df):
  """Identify high outliers in the price column (using z score) and plot a histogram."""
  # Plot a histogram of the price column
  plt.hist(df['Price'], bins=20, color='skyblue', edgecolor='black')
  plt.xlabel('Price')
  plt.ylabel('Frequency')
  plt.title('Histogram of Product Prices')
  plt.show()

  # Calculating z-scores
  z=(df['Price'] - df['Price'].mean()) / df['Price'].std()    #z score (not abs because only high prices)
  threshold = 5   #threshold for outliers
  # Identify the products with high prices
  highPrices =df[z > threshold]
  highList = list(highPrices['Brand'] + ' - ' + highPrices['Name'])
  return highList

def ingredientAnalyser(df):
    """Analyzes ingredients to find the most common active ones under each product type and for each skin type."""
    #dictionaries to store ingredient frequencies for product types and skin types
    productDict = {}
    skinDict = {}

    for index, row in df.iterrows():    #iterating over every row
        productType = row['Label']   # Extract product type and skin type
        skinTypeColumns = ['Combination', 'Dry', 'Normal', 'Oily', 'Sensitive']
        skinType = []
        for column in skinTypeColumns:
            if row[column] == 1:
                skinType.append(column)
        ingredients = row['Ingredients'].split(', ')  # Extract ingredients

        if productType not in productDict:    #ingredient frequencies for product type
            productDict[productType] = {}
        for ingredient in ingredients:
            if ingredient != 'Water' and ingredient != 'Aqua' and ingredient !='Glycerin' and ingredient !='Phenoxyethanol' and ingredient !='Butylene Glycol':
                productDict[productType][ingredient] = productDict[productType].get(ingredient, 0) + 1

        for item in skinType:   #ingredient frequencies for each skin type
            if item not in skinDict:
                skinDict[item] = {}
            for ingredient in ingredients:
                if ingredient != 'Water' and ingredient !='Aqua' and ingredient !='Glycerin' and ingredient !='Phenoxyethanol' and ingredient !='Butylene Glycol':
                    skinDict[item][ingredient] = skinDict[item].get(ingredient, 0) + 1

    #most common ingredients for each product type and skin type
    dict1 = {productType: max(ingredients, key=ingredients.get) for productType, ingredients in productDict.items()}
    dict2 = {skinType: max(ingredients, key=ingredients.get) for skinType, ingredients in skinDict.items()}
    return dict1, dict2

def ingredientEncoding(df):
  """Converts the top 80% of ingredients of each product into a numerical format using Ingredient Set Encoding"""
  ingCol = df['Ingredients']   # Extract the 'ingredients' column

  # Creating a list of unique ingredients
  uniqueIng = set()   # empty set to store unique ingredients
  for ingList in ingCol:
      length = int(0.8 * len(ingList.split(', ')))    # Calculate 80% of the length of the ingredient list
      selected = ingList.split(', ')[:length]
      for ing in selected:
        if '/' in ing:    # Check if the ingredient contains slashes
            uniqueIng.update(ing.split('/'))
        else:
            uniqueIng.add(ing)
  uniqueIngList = list(uniqueIng)   # Convert set to list

  #Ingredient Set Encoding
  binaryEnc = {}    #dictionary to store binary encoding for each product
  for index, ingList in ingCol.items():    #iterate over each ingredient list
      length = int(0.8 * len(ingList.split(', ')))
      productEnc = {}   #binary encoding for current product
      ingredients = ingList.split(', ')[:length]  # Consider only the first 80% of ingredients

      for ingredient in uniqueIngList:    # Set 1 for ingredients present in the product, 0 otherwise
          ingredient_parts = ingredient.split('/')    #split ingridients with multiple names
          present = False
          i = 0
          while i < len(ingredient_parts) and not present:
              if ingredient_parts[i] in ingredients:
                  present = True
              else:
                  i += 1
          if present:
              productEnc[ingredient] = 1
          else:
              productEnc[ingredient] = 0
      binaryEnc[index] = productEnc   # Update binary encoding dictionary for current product

  # add the binary encoding dictionary to original dataframe
  binarydf = pd.DataFrame.from_dict(binaryEnc, orient='index')
  encodeddf = pd.concat([df, binarydf], axis=1)
  return encodeddf

def Kfinder(encodeddf):
  """Finds the optimal k value to perform K means clustering on the ingredient data using the elbow method."""
  binary= encodeddf.iloc[:, 12:]    #extracting binary columns
  SSE = []    #to store SSE
  kmeans_kwargs = {"init": "random", "n_init": 10, "max_iter": 300, "random_state": 29}
  for k in range(1,11):   #iterate over different k values
    kmeans = KMeans(n_clusters = k, **kmeans_kwargs)
    kmeans.fit(binary)
    SSE.append(kmeans.inertia_)
  kValue = KneeLocator(range(1,11), SSE, curve = "convex", direction = "decreasing")    #Find the elbow point
  return kValue.elbow

def applyKMeansClustering(encodeddf, k):
  """Applies K mean clustering to group products into different categories based on ingredient composition"""
  binary= encodeddf.iloc[:, 12:]   #extracting binary columns
  kmeans = KMeans(init="random", n_clusters=k, n_init=10, max_iter=100, random_state=29)    #no. of clusters= k
  kmeans.fit(binary)    #fit k means model to data
  clusterLabels = kmeans.predict(binary)  # Assign each product to a cluster
  clusterCenters = kmeans.cluster_centers_   # Get cluster centres
  return clusterLabels, clusterCenters

def clusterVisualisation(encodeddf, clusterLabels):
  """ Visualizes the clustering results by reducing dimensionality using PCA and plotting clusters by colour in a 2D space."""
  #extracting binary columns
  binary= encodeddf.iloc[:, 12:]
  # Apply PCA to reduce dimensionality to 2 dimensions
  pca = PCA(n_components=2)
  reduced = pca.fit_transform(binary)
  #Colourmap
  cluster_cmap = ListedColormap(['red', 'blue', 'green', 'purple'] )
  # Plot
  plt.figure(figsize=(8, 6))
  scatter= plt.scatter(reduced[:, 0], reduced[:, 1], c=clusterLabels, cmap=cluster_cmap, edgecolor='white')
  # Generate handles and labels for legend
  handles, labels = scatter.legend_elements()
  # Create legend with handles and labels
  plt.legend(handles, [f'Cluster {label}' for label in set(clusterLabels)], title='Clusters')
  plt.title('Ingredient Clustering Results PCA Visualization')
  plt.xlabel('Principal Component 1')
  plt.ylabel('Principal Component 2')
  plt.grid(True)
  plt.show()
  return

def fungalAcneTriggers(df):
  """Compares a pre-defined list of fungal acne triggers to create a fungal acne trigger list for every product in df"""
  #list of fungal acne triggers
  FAtriggers = ["acetylated glycol stearate", "ascorbyl palmitate", "coconut oil", "decyl oleate",
  "ethylhexyl palmitate", "galactomyces", "glycerides citrate", "glycerine", "glyceryl laurate",
  "glyceryl monostearate", "glyceryl oleate", "glyceryl stearate", "glycol distearate",
  "hexyl laurate", "hydrogenated coco-glycerides", "hydrogenated castor oil",
  "hydrogenated palm glycerides", "isopropyl palmitate", "isopropyl myristate", "lecithin",
  "lactic acid", "mango butter", "methyl glucose sesquistearate", "monostearate",
  "PEG-7 glyceryl cocoate", "PEG-8 laurate", "PEG-10 isostearate", "PEG-20 glyceryl triisostearate",
  "PEG-30 dipolyhydroxystearate", "PEG-35 castor oil", "PEG-40 stearate", "PEG-40 castor oil",
  "PEG-90 glyceryl isostearate", "PEG-100 stearate", "PEG-glyceryl stearate", "polyglyceryl isostearate",
  "polyglyceryl-3 diisostearate", "polysorbate-20", "polysorbate-40", "polysorbate-60", "polysorbate-80",
  "retinyl palmitate", "shea butter", "sodium cocoyl isethionate", "sodium methyl cocoyl taurate",
  "sorbitan laurate", "sorbitan trioleate", "sucrose cocoate", "trihydroxystearin"]

  #new df to store triggers
  triggerDf= df.copy()
  triggerDf['Fungal Acne Triggers'] = None

  for index, row in triggerDf.iterrows():    # Iterate over each row in the DataFrame
    ingredients = row['Ingredients'].lower().split(', ')    # Convert the ingredients to lowercase and split them
    triggerList = []  # List to store fungal acne triggers

    # Check each ingredient in the product's ingredients list against fungal acne triggers
    for item in ingredients:
        if '/' in item:  # If the ingredient contains slashes
            subparts = item.split('/')
            for subpart in subparts:
                if subpart.strip() in FAtriggers:
                    triggerList.append(subpart.strip())
        else:  # No slashes
            if item.strip() in FAtriggers:
                triggerList.append(item.strip())
    triggerDf.at[index, 'Fungal Acne Triggers'] = triggerList     # Update the original DataFrame with the fungal acne triggers for the current product
  return triggerDf

def recommendationSystem(encodeddf, productName, k):
  """Creates a dataframe with FA triggers in asceding order of price of similarly formulated products with the same product type and skin suitability as inputted product"""
  # Step 1: Apply K means clustering to group products into different categories based on ingredient composition
  clusterLabels, _ = applyKMeansClustering(encodeddf, k)

  # Step 2: Filter products with the same cluster label as the input product
  product = encodeddf[encodeddf['Name'] == productName].copy()
  index = product.index[0]  # Get the index of the product
  label = clusterLabels[index]  # Get the cluster label of the product
  sameCluster = encodeddf[clusterLabels == label].copy()    # Filter products with the same cluster label

  # Step 3: Narrow down the products based on product type and skin suitability
  productType = product['Label']
  suitability = product[['Combination', 'Dry', 'Normal', 'Oily', 'Sensitive']]
  narrowedDown = sameCluster[(sameCluster['Label'] == productType.iloc[0]) &
   (sameCluster[['Combination', 'Dry', 'Normal', 'Oily', 'Sensitive']].eq(suitability.iloc[0])).all(axis=1)]

  # Step 4: Run fungal acne triggers function for the products
  triggerDf= fungalAcneTriggers(narrowedDown)

  #Step 5: Creating an output df with label, brand, name, price, rank, and fungal acne triggers
  outputColumns = ['Label', 'Brand', 'Name', 'Price', 'Rank', 'Fungal Acne Triggers']
  finalDf = triggerDf[outputColumns].copy()

  # Step 6:Sort dataframe by price in ascending order
  finalDf.sort_values(by='Price', ascending=True, inplace=True)
  return finalDf

def main():
  df= readSpreadsheet(skincareData)   #reading data
  cleandf= cleanData(df)              #cleaning data
  print(cleandf['Price'].dtype)

  #statistics
  print('Statistics:')
  statdf= statFinder(cleandf)
  print(statdf)                       #outputting stat dataframe, chi-sqauared test result and mean price bar plot
  statdf.to_excel("output/output.xlsx", index=True)    #outputting stat df to spreadsheet
  highList= priceOutliers(cleandf)
  print('Extremely high priced products:', highList)    #output products with extremely high prices
  dict1, dict2= ingredientAnalyser(cleandf)   #most common ingredients according to skintype and producttype
  print('Most common ingredients according to product type:',dict1)
  print('Most common ingredients according to skin type:', dict2)

  #Recommendation system
  encodeddf= ingredientEncoding(cleandf)
  Kvalue= Kfinder(encodeddf)
  clusterLabels, clusterCenters= applyKMeansClustering(encodeddf, Kvalue)
  print(clusterVisualisation(encodeddf, clusterLabels))   #plot of similar products by ingredient list clustered together
  productName = input("Enter a product name:")    #asking user for product name to use recommendation system
  finalDf= recommendationSystem(encodeddf, productName, Kvalue)
  print(finalDf)   #outputting the df from recommendation system
  finalDf.to_excel("output/output1.xlsx", index=False)    #outputting final df to spreadsheet

if __name__ == "__main__":
  main()
