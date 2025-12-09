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
- Data Used: 'viewer_interaction.db'; Link to download: https://my.luiss.it/pluginfile.php/87173/mod_folder/content/0/viewer_interactions.db?forcedownload=1
- Valuable website for viewing .db files (mainly for navigation and full visualization):
- https://inloop.github.io/sqlite-viewer/
- Algorithms used on data: KMeans++, Hierarchical Clustering, GMM (Gaussian Mixture Models), DBSCAN 
- Reproducability:
- Our main idea was to think about how this data could be used for a reccomendation system, so we wanted our machine learning algorithms to give valuable data towards the production of a reccomendation system.
- We used the algorithms listed above to gain valuable segrigated data based on the catigories we were using. We did this to gain the best infromation from the database, for example most active users (best for valuable opinions) and top movies (best for cold starts). 

## Experimental Design
Describe any experiments you conducted to demonstrate/validate the target contribution(s) of your project;

The first experiment of EDA that we did was generally looking how the data is structured:
- We loaded the data into a SQL viewer to look at how are data is structured.

First Interpretation:
- we realized that there are a lot of missing data within the database, so we created a diagnostics tab to check what percentages of missing data are in each column. From this we realized there was a large significance of data missing from the database. Dropping these values would result in a lot of data loss.

Calculations Section:
- Realizing that a lot of these missing values can be calculated, we created two sections to calculate and input the missing values. One section for `movie_statistics` table and one for the `user_statistics` table. These two tables contained the bulk of our data that we would be using in our models, so we mainly focused on recovering data for these two tables.

Mergering Section:
- After cleaning the database and inputing all the missing values wherever possible, we created a merge section to combine all the data into one table. This made the data easier to work with as it was all in one place. During the merge we dropped any row with missing values meaning that we could ensure that there were no missing values in any of the categories for each data entry. Having the data the data in this format means that it is also prepared ford for future use in a recommendation system.

Plotting/Statistical Evaluation Section:
- In this section we plotted the most valuable information from the dataset against each other. This allowed us to better visualize the data and gain insights on how metrics changed over time.

Model Considerations/Validity testing:

Experiments done on merged data `movie_features` and `user_features`

Varience and Standerd Deviation on user and movie features:
- Main purpose:
  - To identify which `user-level` and `movie-level` variables contain the most meaningful information for clustering. Because the user dataset is extremely large, we need to reduce the feature space to only the variables that meaningfully contribute to differences between users.
- Evaluation:
  - Through this method we were able to determine the most useful features in both movies and user features. We concluded for `user_features` that the most variate features are `user_total_ratings` and `activity_days`. Another varible that had a higher STD rating was `unique_movies` but we found that this data is heavily corilated to `user_total_ratings` as they are the same. We concluded for `movie_features` that the most variate feature was `movie_total_ratings` and that there was a high corrilation between `movie_total_ratings` and `unique_users` so we decided to drop `unique_users`.

Normal distribution test:
- Main purpose:
  - We needed to check this for the validity of GMM clustering model and also the overall performance of other clustering models like K-Means and Hierarcical clustering and DBSCAN
- Evaluation:
  - All the data from both movies and user features are not normally distributed, this automatically rules out the use of GMM clustering models and also tells us that K-means and Hierarchical clustering will also stuggle slightly from this fact.
 
Kernel Density Estimation Test (KDE):
- Main Purpose:
  - KDE reveals whether the data forms distinct dense regions (potential clusters) or whether it is one continuous mass with no meaningful separation.
- Evaluation:
  - We came to the conclusion from this test that the data is in a massive blob with very dense clusters on the x-axis. We assumed from the start that this would make it benefical for DBSCAN but we plotted the data points to showcase what these clusters of data would look like. We came to the conclusion that K-Means would end up working better than DBSCAN because of the fact that there is a lot of overlapping data. We also plotted the 'user_features' with the two most important features on a KDE graph and came to the conclusion that the data forms one single, long, pointed banana shape. This means we have a curved cluster. This shape means that for a clustering method like K-means that tries to group data in a spherical fasion it will not be able to preform well on this data.
 




- Calculations deviation and colinearity between features. We did this to find out the best features to train our model on, as we wouldn't use points that we highly corrilated, and we wanted to find the most significant features to avoid bloat.
- When picking the models that we wanted to use for our data, one of the tests that we carried out was checking if the features were linearly distributed. This is important because this factor can effect the effectivness of models and also it rulled out the use of GMM model.

Experiment 1: Optimal Cluster Number Determination

Main Purpose: Identify the ideal number of clusters that balances compactness and separation to reveal distinct viewer behaviors (e.g., casual vs. avid raters).
Baseline(s): Default K-Means with k=2 (minimal partitioning) and random initialization.
Evaluation Metric(s): Within-cluster sum of squares (WSS) via elbow method for compactness, and silhouette score (range -1 to 1) for cluster quality—chosen because it measures both cohesion and separation without requiring labels, ideal for unsupervised tasks.

Experiment 2: Algorithm Comparison

Main Purpose: Compare clustering algorithms to find the most interpretable model for user grouping, assessing robustness to noise and high dimensionality.
Baseline(s): K-Means with default parameters (e.g., k=3, Euclidean distance).
Evaluation Metric(s): Silhouette score for overall quality, and Davies-Bouldin index (lower is better) for minimal overlap—selected to quantify how well clusters represent natural groupings in viewer data.

Experiment 3: Dimensionality Reduction Impact

Main Purpose: Evaluate how PCA/TSNE affects clustering visualization and performance, ensuring low-dimensional projections preserve behavioral patterns.
Baseline(s): Clustering on raw scaled features without reduction.
Evaluation Metric(s): Silhouette score post-reduction, and visual inspection via 2D plots—used to confirm that reduced dimensions maintain cluster integrity for interpretability.

### Modeling:

We selected features based on EDA: for users, `user_total_ratings` and `activity_days` to capture engagement; for movies, `movie_avg_rating`, `movie_std_rating`, `movie_total_ratings`, `movie_min_rating`, and `movie_max_rating` to capture quality, variability, and popularity.

#### K-means ++

##### K-means ++ on movie data
We used K-means ++ on movie data as a means of clustering, this model was useful because of the large amount of data. We plotted the elbow method to help distinct the amount of clusters we want from the data. We calculated and fine tuned the silhouette score, to improve the model.

##### K-means ++ on user data

#### Dbscan

We were only able to use Dbscan with our smaller movie dataset, but when we tuned our `eps` and `min_samples` we noticed that we had way too many clusters. However, Plotting the data out showed that the data was well clustered and had a silhouette score of around 0.8. We decided to try to cluster these clusters in an effort to reduce the number of clusters. Using K-Means++, we were able to set the amount of final clusters we want.

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
 
 


• Main finding(s): report your final results and what you might
conclude from your work
• Include at least one placeholder figure and/or table for
communicating your findings
• All the figures containing results should be generated from the
code.

