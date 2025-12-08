# ROM_AD_Project_321481

**Team Members**
- Ryder Wood Captian: 321481
- Marco Malliani: 324971
- Giacomo Mazzarella: 321931

## Introduction
Our project is to clean and analize data from the viewer_interactions database. We clean the data by calculating missing data and also explore patterns and valuable statistics on the data. we primarily looked at the movie and user statistics to uncover patterns. The main goal of identify user/movie segments via clustering, explore rating biases, and visualize movie clusters by popularity and quality.

## Methods
- Python 3.11
- Libraries: pandas, numpy, matplotlib, seaborn, scikit-learn
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
- we did this through viewing the data base to understand what we are looking at.

- First Interpretation:
- we realized that there are a lot of missing data within the database, so we created a diagnostics tab to check what percentages of missing data are in each column. From this we realized there was a large significance of data missing from the database, too much to drop the missing values.

- Calculations Section:
- We created two sections to calculate and input the missing values and input them back into the database. One section for `movie_statistics` table and one for the `user_statistics` table. These were realistically the most important tables in terms for the creation of statistical analysis.

- Mergering Section:
- After cleaning the database and inputing all the missing values we created a merge section to combine all the data into one table. This benefited us as it allowed for all the data to be in one table for us to access. Also for future use of out project for the creation of a recommendation system having all the data in one table is beneficiary.

- Plotting/Statistical Evaluation Section:
- In this section we plotted the most valuable information from the dataset. Like `top_movies`, most engagement for the year of release of movies, and overall rating bias. This is valuable information for context on the data and also recommendation in the future. It showcases the best movies that have to have high interactions, and the year of release with the most engagement.

- Modeling:

We selected features based on EDA: for users, `user_total_ratings` and `activity_days` to capture engagement; for movies, `movie_avg_rating`, `movie_std_rating`, `movie_total_ratings`, `year_of_release` to capture quality, variability, and popularity.
indicate the following for each experiment:
• The main purpose: 1-2 sentence high-level explanation
• Baseline(s): describe the method(s) that you used to compare
your work to
• Evaluation Metrics(s): which ones did you use and why?
