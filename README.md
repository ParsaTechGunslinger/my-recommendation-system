
# Recommendation System

This project implements a recommendation system to suggest products to users based on their past interactions.

## Features

- **Generate Recommendations**: Provides product recommendations for users.
- **User-Product Matrix**: Creates a matrix of user ratings for products.
- **Popular Products**: Identifies and ranks products based on user ratings.

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/your-username/my-recommendation-system.git
   cd my-recommendation-system

2. **pip install -r requirements.txt**

   ```bash
   pip install -r requirements.txt

## Usage

1. **Ensure your dataset is available:**

Make sure your dataset (ratings_Electronics.csv) is in the correct path or update the path in the script accordingly.

2. **Run the Recommendation Script:**
   ```bash
      import numpy as np
import pandas as pd

    # Load the dataset
data = pd.read_csv("D:/ML & AI & Python &DL projects/ratings_Electronics (1).csv")

    # Rename columns for clarity
data.columns = ['user_id', 'product_id', 'ratings', 'timestamp']

    # Take a 10% sample of the data
df = data[:int(len(data) * .1)]

    # Filter users with at least 50 ratings
counts = df['user_id'].value_counts()
data = df[df['user_id'].isin(counts[counts >= 50].index)]

    # Calculate the mean ratings for each product
data.groupby('product_id')['ratings'].mean().sort_values(ascending=False)

    # Create the user-product matrix
final_ratings = data.pivot(index='user_id', columns='product_id', values='ratings').fillna(0)

    # Calculate the density of the matrix
num_of_ratings = np.count_nonzero(final_ratings)
possible_ratings = final_ratings.shape[0] * final_ratings.shape[1]
density = (num_of_ratings / possible_ratings) * 100

    # Transpose the matrix
final_ratings_T = final_ratings.transpose()

    # Group and rank products based on user interactions
grouped = data.groupby('product_id').agg({'user_id': 'count'}).reset_index()
grouped.rename(columns={'user_id': 'score'}, inplace=True)
training_data = grouped.sort_values(['score', 'product_id'], ascending=[0, 1])
training_data['Rank'] = training_data['score'].rank(ascending=0, method='first')
recommendations = training_data.head()

    # Function to generate recommendations for a user
def recommend(id):
    recommend_products = recommendations.copy()
    recommend_products['user_id'] = id
    column = recommend_products.columns.tolist()
    column = column[-1:] + column[:-1]
    recommend_products = recommend_products[column]
    return recommend_products

    # Example usage:
    print(recommend(11))

   
  

