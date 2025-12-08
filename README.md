# ROM_AD_Project_321481

**Team Members**
Ryder Wood Captian: 321481
Marco Malliani: 324971
Giacomo Mazzarella: 321931

## Introduction
Our project is to clean and analize data from the viewer_interactions database. We clean the data by calculating missing data and also explore patterns and valuable statistics on the data. we primarily looked at the movie and user statistics to uncover patterns. The main goal of identify user/movie segments via clustering, explore rating biases, and visualize movie clusters by popularity and quality.

## Methods
- Python 3.11
- Libraries: pandas, numpy, matplotlib, seaborn, scikit-learn
- Data Used: 'viewer_interaction.db'; Link to download: https://my.luiss.it/pluginfile.php/87173/mod_folder/content/0/viewer_interactions.db?forcedownload=1
- Algorithms used on data: KMeans++, Hierarchical Clustering, GMM (Gaussian Mixture Models), DBSCAN 
- Reproducability:

## Experimental Design
Describe any experiments you conducted to demonstrate/validate the target contribution(s) of your project;

We selected features based on EDA: for users, `user_total_ratings` and `activity_days` to capture engagement; for movies, `movie_avg_rating`, `movie_std_rating`, `movie_total_ratings`, `year_of_release` to capture quality, variability, and popularity.
indicate the following for each experiment:
• The main purpose: 1-2 sentence high-level explanation
• Baseline(s): describe the method(s) that you used to compare
your work to
• Evaluation Metrics(s): which ones did you use and why?
