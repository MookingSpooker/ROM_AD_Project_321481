# ROM_AD_Project_321481

**Team Members**  
Ryder Wood — Captain: 321481  
Marco Malliani: 324971  
Giacomo Mazzarella: 321931  

## 1. Introduction
The goal of this project is to identify user and movie relationships using clustering models. We start by calculating as many missing values as possible and removing unusable data. Once our data is clean, we analyze movie-level and user-level statistics separately and apply unsupervised clustering methods to uncover patterns in viewer behavior and movie performance.

Our broader motivation is to understand how this dataset could support future recommendation systems.

## 2. Methods
### 2.1 Environment & Tools
- Python 3.11  
- Libraries: pandas, numpy, matplotlib, seaborn, scikit-learn, datetime, scipy, sqlite3  
- Install: `pip install -r requirements.txt`

Dataset: `viewer_interactions.db`  
Viewer tool: https://inloop.github.io/sqlite-viewer/

### 2.2 Features Used
**Movie features:**  
`movie_total_ratings`, `movie_avg_rating`, `movie_std_rating`, `movie_min_rating`, `movie_max_rating`, `year_of_release`

**User features:**  
`user_total_ratings`, `activity_days`

### 2.3 Preprocessing
Recovered missing values, merged tables, dropped irrecoverable rows, scaled all numeric features.

### 2.4 Helper Functions
(add_new_df, sync_dataframe, search_by_parameter, manual_std, convert_string_to_date, elbow_method, compute_silhouette_score, inspect_movie_cluster, plot_clusters_pca_2d, plot_dbscan_pca_2d)

### 2.5 Models Used
K-Means++, Agglomerative, GMM, DBSCAN

## 3. Experimental Design
### 3.1 EDA Experiment
Purpose: understand data structure.  
Findings: large missing values; most recoverable.

### 3.2 Variance & Correlation Experiment
Purpose: identify informative features.  
Findings: selected two user features; removed correlated variables.

### 3.3 Normality Test
Purpose: verify GMM assumptions.  
Findings: data not Gaussian → GMM invalid.

### 3.4 KDE Experiment
Purpose: detect density regions.  
Findings: movies form one blob; users form curved shape → K-means limited.

### 3.5 KNN Statistics
Users: high density variation → DBSCAN possible.  
Movies: uniform → K-means effective.

### 3.6 Pairwise Distance Distribution
Findings: curse of dimensionality limits distance-based clustering.

### 3.7 Hopkins Statistic
H > 0.9 → strong clustering tendency.

### 3.8 PCA Explained Variance
98.4% variance in 3 components.

### 3.9 Modeling Experiments
Experiment 1: Choosing k  
Experiment 2: Comparing algorithms

## 4. Results
### 4.1 K-Means++ (Movies)
k = 4  
Cluster descriptions included.  
Silhouette ≈ 0.41

### 4.2 K-Means++ (Users)
Silhouette ≈ 0.35

### 4.3 DBSCAN
Silhouette ≈ 0.80 on dense core (misleading)

### 4.4 Agglomerative
Slightly below K-Means.

### 4.5 GMM
Unstable → rejected.

### 4.6 Comparison Table
| Model | Dataset | Silhouette | Strengths | Weaknesses |
|-------|---------|------------|-----------|-------------|
| K-Means++ | Movies | ~0.41 | interpretability | spherical assumption |
| K-Means++ | Users | ~0.35 | simple | elongated shape |
| DBSCAN | Movies | ~0.80* | finds dense cores | discards noise |
| Agglomerative | Movies | ~0.38 | hierarchy | noise-sensitive |
| GMM | Movies | N/A | probabilistic | invalid assumptions |

### 4.7 Plots
(Figure placeholders: rating distribution, avg rating by year, top movies, PCA movie clusters, PCA user clusters)

## 5. Conclusions
### Summary
Reconstructed missing data, merged statistics, applied clustering, extracted insights on viewer behavior and movie performance. K-Means++ best for movies; DBSCAN misleading; GMM invalid.

### Future Work
Suggested: nonlinear clustering, using genres and timestamps, dynamic profiles, hybrid recommendations.

