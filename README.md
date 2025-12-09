# ROM_AD_Project_321481

**Team Members**
- Ryder Wood Captian: 321481
- Marco Malliani: 324971
- Giacomo Mazzarella: 321931

## Introduction
The goal of this project is to identify user and movie relationships using clustering models. We start by by calculating as many missing values as possible and removing any unusable data. Once our data is clean, we look at the movie and user statistics seperately. We use various clustering models to explore relationships and uncover patterns. We then plot our clustered data to visually analyze the relationships, and look at each cluster individually to better understand what criteria the algorithm is clustering by.

## Methods
- Python 3.11
- Libraries: pandas, numpy, matplotlib, seaborn, scikit-learn, datetime, scipy, sqlite3
- Install all Libraries (run in terminal) :`pip install -r requirements.txt`
- Data Used: 'viewer_interaction.db'; Link to download: https://my.luiss.it/pluginfile.php/87173/mod_folder/content/0/viewer_interactions.db?forcedownload=1
- Valuable website for viewing .db files (mainly for navigation and full visualization):
- https://inloop.github.io/sqlite-viewer/
- Algorithms used on data: KMeans++, Hierarchical Clustering, GMM (Gaussian Mixture Models), DBSCAN 
- Reproducability:
- Our main idea was to think about how this data could be used for a reccomendation system, so we wanted our machine learning algorithms to give valuable data towards the production of a reccomendation system.
- We used the algorithms listed above to gain valuable segrigated data based on the catigories we were using. We did this to gain the best infromation from the database, for example most active users (best for valuable opinions) and top movies (best for cold starts). 

**Design Choices:**

- Use of `dfs` storing data frame in a dictionary with the key being the table name and the value being the data frame
- Reason:
  - It was the way we wanted to manage and view the data. It also helps when using features from the pandas libraries.
- Use of `DF_MAP`
- DF_MAP is the lookup table that links each global DataFrame name to its actual entry inside the dfs dictionary. It is filled by add_new_df function whenever a new dataframe is registered.
- Reason:
  - This allowed us to create/store global variables later on in the code. This was important because when we were mergering datasets we wouldn't have to create new hard coded global variables. 

- We created an asortment of helper methods to help clean the code:
- they consist of:
  - `add_new_df`
  - `sync_dataframe`
  - `search_by_parameter`
  - `manual_std`
  - `covert_string_to_date`
  - `elbow_method`
  - `compute_silhouette_score`
  - `inspect_movie_cluster`
  - `plot_clusters_pca_2d`
  - `plot_dbscan_pca_2d`

## Experimental Design
Describe any experiments you conducted to demonstrate/validate the target contribution(s) of your project;

The first experiment of EDA that we did was generally looking how the data is structured:
- We loaded the data into a SQL viewer to look at how are data is structured.

First Interpretation:
- we realized that there are a lot of missing data within the database, so we created a diagnostics tab throughout first to check what percentages of missing data are in each column. From this we realized there was a large significance of data missing from the database. Dropping these values would result in a lot of data loss.

**Main EDA:**
Calculations Section:
- Realizing that a lot of these missing values can be calculated, we created two sections to calculate and input the missing values. One section for `movie_statistics` table and one for the `user_statistics` table. These two tables contained the bulk of our data that we would be using in our models, so we mainly focused on recovering data for these two tables.

Movie Statistics Section:
- Purpose:
  - Calculate the missing values in the `movie_statistics` table
- Result:
  - Missing data was successfully added into the table where it was missing. We accounted for all missing values and for data that was missing that could not be calculated was later removed in the merging process.
- Included Calculations:
  - Missing `std_rating`
  - Missing `total_ratings`
  - Missing `avg_ratings`
  - Missing `min_ratings, max_ratings`
  - Missing `unique_users`
 
User Statistics Section:
- Purpose:
  - Calculate the missing values in the `user_statistics` table
- Result:
  - Missing data was successfully added into the table where it was missing. We accounted for all missing values and for data that was missing that could not be calculated was later removed in the merging process.
- Included Calculations:
  - Missing `std_rating`
  - Missing `total_ratings`
  - Missing `avg_ratings`
  - Missing `min_ratings, max_ratings`
  - Missing `unique_movies`
  - Missing `activity_days`

Mergering Section:

- After cleaning the database and inputing all the missing values wherever possible, we created a merge section to combine all the data into one table. This made the data easier to work with as it was all in one place. During the merge we dropped any row with missing values meaning that we could ensure that there were no missing values in any of the categories for each data entry. Having the data the data in this format means that it is also prepared ford for future use in a recommendation system.

Model Considerations/Validity testing:

Experiments done on merged data `movie_features` and `user_features`

Varience and Standerd Deviation on user and movie features:
- Main purpose:
  - To identify which `user-level` and `movie-level` variables contain the most meaningful information for clustering. Because the user dataset is extremely large, we need to reduce the feature space to only the variables that meaningfully contribute to differences between users.
- Evaluation:
  - Through this method we were able to determine the most useful features in both movies and user features. We concluded for `user_features` that the most variate features are `user_total_ratings` and `activity_days`. Another varible that had a higher STD rating was `unique_movies` but we found that this data is heavily corilated to `user_total_ratings` as they are the same. We concluded for `movie_features` that the most variate feature was `movie_total_ratings` and that there was a high corrilation between `movie_total_ratings` and `unique_users` so we decided to drop `unique_users`.

Normal distribution test:
**add Figure**
- Main purpose:
  - We needed to check this for the validity of GMM clustering model and also the overall performance of other clustering models like K-Means and Hierarcical clustering and DBSCAN
- Evaluation:
  - All the data from both movies and user features are not normally distributed, this automatically rules out the use of GMM clustering models and also tells us that K-means and Hierarchical clustering will also stuggle slightly from this fact.
 
Kernel Density Estimation Test (KDE):
**add figure**
- Main Purpose:
  - KDE reveals whether the data forms distinct dense regions (potential clusters) or whether it is one continuous mass with no meaningful separation.
- Evaluation:
  - We came to the conclusion from this test that the data is in a massive blob with very dense clusters on the x-axis. We assumed from the start that this would make it benefical for DBSCAN but we plotted the data points to showcase what these clusters of data would look like. We came to the conclusion that K-Means would end up working better than DBSCAN because of the fact that there is a lot of overlapping data. We also plotted the 'user_features' with the two most important features on a KDE graph and came to the conclusion that the data forms one single, long, pointed banana shape. This means we have a curved cluster. This shape means that for a clustering method like K-means that tries to group data in a spherical fasion it will not be able to preform well on this data.

KNN Statistics test:
- Main purpose:
  - Checks whether the dataset has natural cluster structure by analyzing how close each point is to its neighbors. If real clusters exist, points inside a cluster should have similar and small neighbor distances, while points between clusters should have larger distances.
- Evaluation:
  - User Features Analysis Mean of mean distances: 0.027 Std of mean distances: 0.205, These statistics indicate a highly variable density in the user feature space. The low overall mean suggests that, on average, users are closely packed in dense regions. However, the standard deviation is disproportionately (high) pointing to diversity in the data. This means that there is a presence of dense clusters alongside sparser regions or outliers. These variations validate the use of density-based clustering like DBSCAN over uniform-assumption methods like K-Means.
  - Movie Features Analysis
  - Mean of mean distances: 0.150, Std of mean distances: 0.207. For movies, the mean is higher than for users, indicating generally sparser packed movies. The standard deviation is comparable in absolute terms but lower relative to the mean, suggesting more uniform density overall compared to users. This implies fewer extreme variations in local neighborhoods, with movies forming somewhat consistent groups. This means algoritms like K-means, and hierarchical clustering could work well here.
 
Distance Distribution Test (only on movie features - user features were too expensive):
**add figure**
- Main purpose:
  - Helps to understand how all points in the dataset relate to each other in terms of distance. It checks whether the dataset contains distinct separation gaps (which would indicate clusters) or if distances form one continuous distribution (which suggests no clear cluster structure).
- Evaluation:
  - The resulting distribution is extremely narrow and heavily concentrated between 0 and ~6–8, with an abrupt drop-off and almost no distances beyond ~15–20. This is a classic signature of the **curse of dimensionality**: in high-dimensional spaces, even randomly distributed points appear surprisingly close to each other under Euclidean distance. The lack of spread in distances dramatically reduces the meaningfulness of distance-based methods like K-Means, hierarchical clustering with average/ward linkage, and especially DBSCAN.
 
Hopkins Test:
- Main purpose:
   - Is there actually any meaningful clustering structure in my data, or is it just random uniform noise
- Evaluation:
   - Our scores for both user and movie features were above 0.9 which means that out data has: Very strong clustering tendency making it perfect for clustering.
 
Cumulative Explained Variance Test:
**add figure**
- Main purpose:
  - To understand how much of the dataset’s total variance can be captured with a reduced number of principal components. This helps evaluate whether the data is high-dimensional with complex structure, or whether most variation lies in just a few directions (which often indicates redundancy or low intrinsic dimensionality).
- Evaluation:
  - We concluded from this test that it is not dimensionally complex as it shows in the number majority of our data can be explained at 3 dimensions (principle components). 
  - Most fo the data is located in the first 3 dimensions:
  - Explained variance ratio: [0.492 0.298 0.193 0.014 0.003]
  - Cumulative: [0.492 0.79  0.984 0.997 1.   ]
  - The cumulative varience ratio shows us that at 3 dimensions majority of out data is showcased (0.984 = 98.4%). The reason this is important because it means for algorithms like K-means it will be able to handle this data more easily.


Experiment 1: Optimal Cluster Number Determination

Main Purpose: Identify the ideal number of clusters that balances compactness and separation to reveal distinct viewer behaviors (e.g., casual vs. avid raters).
Baseline: Default K-Means with k=2 and random initialization (keep constant/Use the same `random_state`).
Evaluation Metric(s): elbow method for compactness, and silhouette score (range -1 to 1) for cluster quality—chosen because it measures both cohesion and separation without requiring labels.
Experiment 2: Algorithm Comparison

Experiment 2: Algorithm Comparison

Main Purpose: Compare clustering algorithms to find the most interpretable model for user grouping, assessing robustness to noise and high dimensionality.
Baseline: K-Means with default parameters (use of elbow method later to find optimal).
Evaluation Metrics: Silhouette score for overall quality

### Modeling:

We selected features based on EDA: for users, `user_total_ratings` and `activity_days` to capture engagement; for movies, `movie_avg_rating`, `movie_std_rating`, `movie_total_ratings`, `movie_min_rating`, and `movie_max_rating` to capture quality, variability, and popularity.

#### K-means ++

##### K-means ++ on movie data
We used K-means ++ on movie data as a means of clustering, this model was useful because of the large amount of data. We plotted the elbow method to help distinct the amount of clusters we want from the data. We calculated and fine tuned the silhouette score, to improve the model.

##### K-means ++ on user data

#### Dbscan

We were only able to use Dbscan with our smaller movie dataset, but when we tuned our `eps` and `min_samples` we noticed that we had way too many clusters. However, Plotting the data out showed that the data was well clustered and had a silhouette score of around 0.8. We decided to try to cluster these clusters in an effort to reduce the number of clusters. Using K-Means++, we were able to set the amount of final clusters we want.

Performance of DBSCAN:
- reasons for it being the best option based on testing.
- the KDE map, hopkins statistic and pairwise distribution all indicate that the dataset as a whole does not contain clear cluster structure. (the point density forms a single continuos mass with no seperated peaks) however DBSCAN produces a high silhoutte score by discarding a large part of the data as noise, and cluster only the dense core regions. We launched evaluating test on this sub-set independently where even K-means achives a high silhoutte score (around 0.84) confirming that DBSCAN is not discovering true clusters, but rather exploiting density artifacts in the data. 

#### Higherarcical Clustering

indicate the following for each experiment:
• The main purpose: 1-2 sentence high-level explanation
• Baseline(s): describe the method(s) that you used to compare
your work to
• Evaluation Metrics(s): which ones did you use and why?

Results – Describe the following:

- Statistical Findings:



- Model Findings:

#### K-means ++ 

- On movie data, We found through the elbow method that the most optimal amount of clusters was 4. These clusters from looking at them were mainly spit based on `total_ratings` and `average_ratings`. We concluded this in every cluster (cluster number based on index)
  - Cluster 0: contains unpopular movies (low `total_ratings`) with high `average_ratings` with a total of 3248 elements in that cluster
  - Cluster 1: contains unpopular movies (low `total_ratings`) with lower `average_ratings` with a total of 4434 elements in that cluster
  - CLuster 2: contains top selling movies (highest `total_ratings`) with High `average_ratings` with a total of 19 elements in that cluster
  - Cluster 3: contains average movies (medium to high `total_ratings`) with average `average_ratings` with a total of 3028 elements in that cluster

- Explanation for the smallest cluster
   - These 19 data points are the most interacted top seller movies. The reason that they are in there own cluster is because there `total_ratings` and `average_ratings` are significantly higher than the rest of the other movies. We also show case the data without the cluster to see if it changes an of the shapes of the other clusters but it does not. We decided to keep this cluster in because we think that these 19 movies are valuable data points. As an example if this decoded audience is used for a recommendation system then these 19 moivies are perfect for cold start or a recommendation that has a very low likely hood of not being valid.
 
 

Results – Describe the following:
• Main finding(s): report your final results and what you might
conclude from your work

Plotting/Statistical Evaluation Section:
- In this section we plotted the most valuable information from the dataset against each other. This allowed us to better visualize the data and gain insights on how metrics changed over time.

Data we found:
**add plot**
- This is a plot of the rating distributions
- We can see that the rating bias is overly positive, likely due to the fact that this was done on a streaming platform

**add plot**
- This plot shows the average ratings over time, it showcases that the movies released in the 2000s have the most engagment, and we can clearly see that majority of the ratings over the years seem to be inbetween 2 and 4

**add plot**
- This plot shows the top 10 most popular movies with a minimum of 1,000 ratings to avoid movies with low total ratings but high scores.
- This data is important because it shows the best of the best movies that could be used in a recomendation system if this project is used for that.

need to add the analysis on the clustering models


[Section 5] Conclusions – List some concluding remarks. In particular:
• Summarize in one paragraph the take-away point from your
work.
• Include one paragraph to explain what questions may not be
fully answered by your work as well as natural next steps for this
direction of future work

