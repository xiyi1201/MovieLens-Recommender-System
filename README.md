# MovieLens Recommender System

## Table of Contents
- [Project Description](#project-description)
- [Dataset Overview](#dataset-overview)
- [Code](#Code)
- [Conclusion](#Conclusion)

## Project Description
This is a project about designing and building a movie recommendation engine using the MovieLens dataset. I implemented collaborative filtering (user-based, item-based), matrix factorization techniques, and a hybrid recommender system. 

## [Dataset Overview](https://grouplens.org/datasets/movielens/100k/)
This data set consists of:
	* 100,000 ratings (1-5) from 943 users on 1682 movies. 
	* Each user has rated at least 20 movies. 
        * Simple demographic info for the users (age, gender, occupation, zip)

The data was collected through the MovieLens web site (movielens.umn.edu) during the seven-month period from September 19th, 1997 through April 22nd, 1998. This data has been cleaned up - users who had less than 20 ratings or did not have complete demographic information were removed from this data set.

## Code Structure

### **EDA**: 
Goal:
1. Perform exploratory data analysis to understand the structure and characteristics of the dataset.
2. Visualize key statistics such as movie ratings distribution, user preferences, etc.
3. Explore relationships between variables (e.g., user ratings, movie genres).

Key takeaways: 
1. total 19 generes
2. total 1664 unique items (movies)
3. the average rating per user is centered between 3.0 and 4.5 
![Distribution_of_ratings](/Users/Cecilia/MovieLens-Recommender-System/Distribution_of_ratings.png)
![Ratings_by_gender](/Users/Cecilia/MovieLens-Recommender-System/Ratings_by_gender.png)
![Ratings_by_age](/Users/Cecilia/MovieLens-Recommender-System/Ratings_by_age.png)
4. If we assume that the number of ratings correlates with the popularity or viewership of the movies, then movies with a rating of 3 are the most popular or most viewed, followed closely by those rated 4 and then 5. Movies rated 1 and 2 are less popular or have been viewed less often.
5. There is not too much correlation between genres and ratings.

### **Building the Recommendation Engine**:
Goal:
1. Implement two collaborative filters: user-based and item-based.
2. Implement two different techniques matrix factorization based collaborative filters.
3. Split the dataset into training and testing sets.
4. Train the recommendation engine on the training set.
5. Generate movie recommendations for users based on their historical ratings or preferences.
6. Evaluate the performance of the recommendation engine using appropriate metrics.

- **User-based Collaborative Filtering (UB-CF)**
    - `compute_user_similarity`: Compute user similarity using **Pearson** correlation coefficient
    - `recommend_user_based`: Generate recommendations for a user by specifying n similar users to compare

- **Item-based Collaborative Filtering (IB-CF)**
    - `compute_item_similarity`: Compute item-item similarity using **cosine** similarity
    - `recommend_item_based`: Get recommendations for a user based on item-item similarity

- **Model-based Collaborative Filtering**
Factorization increases the information density of the sparse User-Item matrix.
1. **Non-negative Matrix Factorization (NMF)**
    - `recommend_items_nmf`: Recommend top N items for a specific user using NMF
2. **Alternating Least Squares (ALS)**
    - `recommend_items_als`: Recommend top N items for a specific user using ALS

### **Algorithms**
- `SVD()`, `KNNBasic()`, `NMF()`, `SlopeOne()`, `CoClustering()`
- ![alg_result](/Users/Cecilia/MovieLens-Recommender-System/alg_result.png)

### **Fine-tuning and Optimization**
- `hybrid_recommendation`: Hybrid recommendation that combines SVD and KNNBasic.
    - param user_id: The user ID for whom the recommendation is made.
    - param item_id: The item ID to predict the rating for.
    - param switch_threshold: The threshold for switching between SVD and KNNBasic.
    - return: The estimated rating.
- ![Architecture](/Users/Cecilia/MovieLens-Recommender-System/Arch.png)

### **Handling Cold Starts**
(For users with no or very little history (cold start problem), consider the following strategies)

- Content-Based Filtering: Use user or item metadata to make initial recommendations. For new users, ask for their preferences on certain items or categories at sign-up to bootstrap the recommendation process.
- Popularity-Based Recommendations: Recommend popular items or trending items to new users as an initial strategy, given no user-specific data is available.
- Hybrid Approaches: Combine collaborative filtering with content-based or popularity-based methods to generate initial recommendations for new users.

### Insights: 

I will design a Hybrid Recommender that combines the strengths of SVD and KNNBasic. 
1. Layered Approach: Use SVD to generate a base set of recommendations and then refine these recommendations using KNNBasic based on similarity measures.
2. Feature Combination: Combine features learned by SVD and KNNBasic into a single model, and then train a meta-learner or use a simple heuristic to select or rank recommendations.
3. Switching: Use SVD by default but switch to KNNBasic for certain users or items based on specific criteria, like the sparsity of user-item interactions.

## Conclusion

The hybrid recommender system successfully combines the predictive power of SVD with the personalized touch of KNNBasic, offering a robust solution capable of addressing common challenges in recommendation systems, such as accuracy and the cold start problem. By fine-tuning model parameters and employing a strategic combination of algorithms, the system demonstrates improved performance and flexibility in generating user-specific recommendations. Further enhancements could include exploring more sophisticated feature combination techniques and expanding the evaluation framework to include additional metrics and real-world testing scenarios.


