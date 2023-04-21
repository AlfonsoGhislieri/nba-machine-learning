# Redefining Player Positions in the NBA

## Introduction
The National Basketball League (NBA), originally created in 1949, is one of the most recognizable basketball leagues in the world, with 30 teams and players from all over the world playing in it. Throughout the years the NBA has seen numerous changes, the biggest being the introduction of the three-point line in 1979, encouraging teams to shoot from further away. Today’s “modern era” of basketball, having started in 2013, is a completely different game than before. The problem remains that even though the NBA has completely changed, the positions players are assigned have remained the same. Draymond Green, listed as a PF, is a perfect example. He should in theory mostly be winning rebounds and spending time around the basket. However, season after season he posts a very high number of assists, something you would expect from a PG. The NBA has become filled with extremely versatile players and multiple roles are being merged together. It has become apparent that the old positions no longer provide an accurate way to label players. 

Thus, this research aims to redefine the positions in the NBA using player performance statistics from the modern era. It is hypothesised that these newly created positions will be filled with a variety of different traditional positions. This is because players with similar performance statistics are expected to be grouped together, irrespective of their originally assigned position.

## Data Set
The data set that was chosen for this project was per 100 possession NBA player data from 1947-present. The data set was downloaded from Kaggle.com and is made up from data scraped from the very reputable basketball-reference.com. Link to the data set: here.

It was chosen because it provides a large amount of player performance statistics for both offensive and defensive phases of basketball. With 25412 total rows, it includes the vast majority of all player stats since 1947. Per 100 possession data was chosen because it provides data that is not heavily influenced by the amount of time played by a player throughout a season or game. The data set has 36 features and is mostly made up of continuous floating point numerical data, with the majority representing the different average statistics of a player’s performance for every 100 times they have the ball during a season (eg: assists, turnovers, 3 point shots attempted). These are the features that will be the focus of this analysis. The other features are made up of categorial string values which are of no interest as they only provide additional descriptive data (eg: team, player, league).


![image](https://user-images.githubusercontent.com/652368/233578925-0c1a908a-ac65-4a13-8424-7e74c3588fdf.png)

## Pre-processing

**1.1** Removing data outside of modern era
The first step was removing any data for players from any season before 2012-2013.

**1.2** Column legibility
All columns that included _per_100_poss were shortened to just include the relevant statistic. For example: blk_per_100_poss was shortened to just blk. All columns that included _percent had the word replaced with a % sign.

**1.3** Unreliable data
The next step was to remove players that had unreliable data or missing data due to them not having played enough games or minutes during a season. 
Dropped players that played less than 30 games, out of the maximum 82.
Dropped players that played less than 1⁄4 of the possible game time for the season (984 min). 

**1.4** Irrelevant features
Firstly, all features that were not performance statistics were dropped as these are not relevant for the scope of this research. This includes features like: player_id, player, team (tm), positions (pos), age and season. Secondly, games (g), games started (gs) and minutes played (mp) were also dropped as they do not give insight into the role of a player on the court, but simply if the player is a starter or not.

**1.5** Null values
110 data points missing values for x3p%, these were players that simply took no 3 point shots, and the Null values were replaced with 0.

**1.6** Scaling and dimension reduction
All the data was then scaled between 0 and 1. This was done since the range of values of many of the features was different. Thus, scaling was used to reduce variance and distance between the various data points, in order to avoid any one feature having more impact simply because the scale was broader.

Finally, PCA was used to reduce the dimensions. This was done before clustering in order to both improve the performance of the clustering algorithms and reduce any noise. The number of dimensions chosen was 13, keeping the first 13 components still captured around 99% of the variability in the data.

## Determining Number of Clusters 

Next K-means and GMM were selected as the two algorithms that would be used, it was necessary to determine the optimal number of clusters for each. This was done using the silhouette and elbow method.

![image](https://user-images.githubusercontent.com/652368/233579960-dffb4aed-9338-4b50-96f3-b14bca068aa7.png)

![image](https://user-images.githubusercontent.com/652368/233580033-300788bc-0a59-43c8-a187-96cf2f42a4fd.png)

Only using 2 or 3 clusters would be extremely limiting to the creation of any new significant NBA positions. So for K-means 7 was selected as the optimal number of clusters (score at 7 is very similar to that of 4), while for GMM only 4 (there is no good choice after that).

## Comparing Models

Testing the learning accuracy of both algorithms was not possible as these are unsupervised algorithms. However, by reducing the dimensions of the dataset to just 2 features using PCA the clusters were plotted in 2D in order to compare the results of both models visually.

![image](https://user-images.githubusercontent.com/652368/233580777-ac738018-1b2d-4e15-be11-d32d275e050a.png)

When comparing the results of both clustering models it is apparent that the divisions between the clusters are different. The GMM clusters have a lot more overlap, with the clusters not being as clearly divided, while K-means clusters are more well defined. A limitation of K-means is that it assigns a data point to the cluster with the nearest centroid, without accounting for uncertainty like GMM. Consequently, data points that are in between 2 clusters may have been misclassified. On the other hand, GMM clustering is based on probability which accounts for overlap, so data points in between clusters can be assumed to have been more accurately assigned. Moreover, the K-means clusters are limited to being spherical, while GMM cluster shapes are more flexible (Sharma, 2023). Therefore, in theory the GMM model should have produced more accurate results than K-means. Yet, K-means seems to have produced results that are more interesting for further analysis. 

It is important to note that during preprocessing the data was scaled rather than normalised. This process affected the range but not the actual shape of the data, and may have impacted GMM’s ability to correctly identify clusters. Since GMM uses Gaussian Distribution to determine the probability that a data point belongs to a cluster, without normalisation, it is likely that GMM was not able to fit the data accurately (Sharma, 2023). Therefore, the results from the K-means model were chosen for further analysis as they are most likely to have actually produced the most accurate clusters. 

## Results

In order to better understand each of the clusters that was produced by the K-means model, the number of traditional positions that were present in each cluster was analysed and then tabulated below.

![image](https://user-images.githubusercontent.com/652368/233581595-0702f08a-941b-4bda-9d39-08a893a159a8.png)

While every cluster had one position that made up a larger percentage of the total population, a significant spread of the traditional NBA positions can be seen across the different clusters. Clusters 0, 1 and 6 stand out as having a larger spread of different positions. Whereas clusters 2, 4 and 5 have a higher concentration of just two positions.

## Defining new NBA positions

The means of all the features were calculated for each different cluster and saved in a new data frame. These values were then compared in order to better understand which performance statistics differentiated each cluster. The results of the analysis can be seen in the table below.



