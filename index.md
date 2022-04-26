## NBA Player Clustering and Game Win Prediction

<iframe width="560" height="315" src="https://www.youtube.com/embed/9ehn7Ot--Ds" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Intro/Background

The National Basketball Association (NBA) is a professional North American basketball league with 30 teams divided into 2 conferences - East and West.

There are several famous players, like Michael Jordan and Lebron James, and fans often debate over who is the Greatest Of All Time(G.O.A.T).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">LeBron: &quot;At that moment I was like I&#39;m the greatest basketball player people have ever seen.&quot; <br><br>Michael Jordan: <a href="https://t.co/mBmuP8d1H9">pic.twitter.com/mBmuP8d1H9</a></p>&mdash; NBA Memes (@NBAMemes) <a href="https://twitter.com/NBAMemes/status/1496001690549768197?ref_src=twsrc%5Etfw">February 22, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The players also play specific positions like Point Guard, Center, etc. With time the nature of how players play the game has also changed. For example the game now involves players shooting more 3 pointers. Thus rather than using traditional positions, it would be interesting to know how players relate to each other based on their statistics to aid in comparison.

Aside from this, betting on games is also an aspect which the league promotes like [NBABet](https://www.nba.com/nbabet), [FanDuel](https://www.fanduel.com/tnt), etc.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Charles Barkley guarantees Bucks win<br><br>Heat fans: <a href="https://t.co/18gk7CAQ3d">pic.twitter.com/18gk7CAQ3d</a></p>&mdash; NBA Memes (@NBAMemes) <a href="https://twitter.com/NBAMemes/status/1397024441368932352?ref_src=twsrc%5Etfw">May 25, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Often, as the season progresses, it's easy to spot which teams are going to be in the lead, and for games played by these high performing teams against lower ranking teams, the outcomes are predictable. The outcome of a game between two highly ranked teams though is generally difficult to predict and these games are the most exciting. Using data can help provide better insight for predicting game outcomes and possibly making money!

### Problem Definition

Given statistics for players and game history, roster per team, etc. for a game between two teams A and B this season, predict the outcome of the game.

Dataset description:

We have the following data available:
- Dataset with features for around 650 players participating in the NBA. Each player in the dataset has 26 features associated with them across multiple seasons and teams. These features include the basic information of the player like age, position on the court, etc. and much more complex features like the turnover rate, shooting percentage, etc.

Source: [2]
- Game history for teams over multiple seasons.

Source: [1]

### Data collection & Preprocessing

#### NBA Player Clustering

For the player clustering we considered the basketball stats from two different sources:
1. 2021-22 Season Advanced Stats: https://www.basketball-reference.com//leagues/NBA_2021_advanced.html
2. 2021-22 Season Stats: https://www.basketball-reference.com//leagues/NBA_2021_totals.html

The Season Stats contains simpler metrics like 3-pointers made, field-goals made, rebounds, blocks, etc. On further exploration we found that there are other advanced statistics like offensive rebound percentage, offensive win shares, etc. which we can use. Thus the combined features list is as follows:

![image](https://user-images.githubusercontent.com/23053768/161634743-c122b059-d762-4d63-85ef-4cc3bb34937c.png)

![image](https://user-images.githubusercontent.com/23053768/161634774-1345eb25-f275-4c87-8ea4-7cf6086e3ec6.png)

Now, the dataset wasn't perfect, so first we did some inital cleaning by removing rows which has invalid values such as -inf, inf or nan and gave us the following set.

![image](https://user-images.githubusercontent.com/23053768/161635342-7e4edd84-d420-459e-ab2a-125ea756fa2c.png)

In each season there are players who get traded i.e. they start the season with a certain team but mid-season they go to another team. Thus this dataset would contains 3 rows (for the older team, newer team and a total stat record) for the players as shown below

![image](https://user-images.githubusercontent.com/23053768/161635982-ec6d2ee0-a6e4-4850-a996-6164bc5f79ba.png)

Thus to reduce this to a single datapoint we just used the team for which the player has played to most games. The reason for this decision was that player perform differently in different team and total would not give a good estimate, also more no. of games a player plays the better the metric value represents the style of play.

![image](https://user-images.githubusercontent.com/23053768/161636264-37e9bb75-82ce-4720-9d0c-bdbe3b0c9c27.png)

Finally, we dropped the players which have < 25 games in this season because of the same reason mentioned before.

![image](https://user-images.githubusercontent.com/23053768/161636330-10b9e92d-b41e-4e3b-8c35-4900b794133d.png)

#### Game Prediction

For game data, we scraped data from these two sources:
1. https://www.basketball-reference.com/leagues/NBA_2021_games.html
2. https://www.basketball-reference.com/leagues/NBA_2021.html - Per Game Statistics

The game statistics contains list of games that took place in the 2020-2021 Season. They have some relevant information like dates, the teams involved, and their scores. Each month had a list of games that had to be merged together to consolidate all the data.

<img width="540" alt="Screenshot_1" src="https://user-images.githubusercontent.com/32405336/161855733-f2f826b9-5d08-4db2-8f7f-9b28220db966.png">

Some cleaning had to be done to this data as well. We excluded any NaN values. And removing any columns that would not provide any benefit to our model such as Attendance or Game Start Time. Finally, we added an Outcome column. This would be our labels for our supervised learning portion.

<img width="523" alt="Screenshot_2" src="https://user-images.githubusercontent.com/32405336/161855761-15c8dc4b-609b-47e0-803d-6de654c3ac4b.png">

There was some more information that we could merge into this dataset that could provide some insight on the outcomes. This came from the Per Game Stats that was pulled from the (2) link. Each team while on Home or Visitor standing had an average statistics across the team, similar to the advanced player stats such as FG (Field Goals) and 3P (3-Point Field Goals). We merged this data into our outcomes dataset based on Visitor or Home. Here's a sample of 5.

<img width="781" alt="Screenshot_3" src="https://user-images.githubusercontent.com/32405336/161855779-0987220c-3226-469a-a1ab-1680c1974b23.png">

We have plenty of features that we can feed into our neural network to get an understanding on the outcomes of games.

An interesting thing to notice is that there was a noteworthy advantage for Home teams. Home teams came out winning more on average compared to Visitors.

<img width="324" alt="Screenshot_4" src="https://user-images.githubusercontent.com/32405336/161855809-f4b9b5b2-be9f-40e6-91b2-ed1b0b321b3d.png">

We then realized that this won't be best way features for our supervised model, and rather than having features for the current game, average team statistics for the last 5 would provide more meaning and semantically would make sense as we use previous games for predicting game outcome. We also made an attempt to integrate player stats either in the raw form or some way to get meaning from clustering but couldn't integrate as features for game prediction.

We use the Basketball Reference API [6] to get game details for a season and then used the date to get box-scores and statistics for each team. But we found that the game details API would give incorrect dates which results in the team not being found when trying to get their box-score. This we manually extracted data for 3 seasons (2021-22, 2020-21 and 2019-20) and then gor each game used the API to get box scores for home and away teams. This was followed by aggregating the stats for the current home and away teams in a game over their last 5 games. Thus for each game across 3 seasons we now have the following features:

![image](https://user-images.githubusercontent.com/23053768/164947779-aa89c455-9bb7-4526-bdac-ba4a38e485fa.png)

We wanted to get some more information out of the data, specifically a metric to rank the teams amongst each other. A simple system for elo rating was used. The idea was to capture some complexity from the data, but ultimately keep it as general as possible to avoid overfitting. At the start of our dataset, all teams would begin with a standard elo of 1500. This was just a general number that we found would be the most useful. If a team won, they would win elo and the opposition would lose the same amount. It was important that the elo difference was the same, and the overall elo in the whole roster was the same. This would ensure that their rankings would be close to their actual hidden ranking. To capture a little more complexity. We made sure that if a team won by a huge margin then they would gain more elo. After analysing the data, we found that if the score difference at the end of the game was less than 10 points then that meant that the game was pretty close. In this case the winner would only gain 5 points in our elo system. If the score difference was between 10 and 20 then this was an average win, so the winner would receive 10 points. Finally a win with a score difference of +20 meant a huge victory, and this would come with 15 elo points to the winner. In all cases, the losing team would lose the same amount as mentioned before.

Here is some sample data from the beginning of our dataset to get a visualization of how the elo would change. Other columns were dropped only for clarity in this image.

<img width="912" alt="Screenshot_1" src="https://user-images.githubusercontent.com/32405336/165234034-9c509927-369b-43d1-9147-37f4b9aca249.png">

The elo system helps place teams over our entire dataset. We can consider that teams near 1500 elo are considered average. At the end, we compiled what elo each team had and, it was pretty accurate with how those teams actually performed over that time period. Specifically we found teams and their respective elos like Phoenix Suns 2220, Utah Jazz 2265, Milwaukee Bucks 2445 coming out on top. These teams performed well above average over that time frame. While on the other hand, teams like Orlando Magic 650, Detroit Pistons 705, Houston Rockets 850 performed pretty poorly over the same timeframe.

### Methods

In unsupervised methods:

- **Principal Component Analysis**: We implemented **Principal Component Analysis** (PCA) as a “preprocessing step”, which should help us reduce the dimensionality of features to use for clustering/classification. 

Principal Component Analysis is an unsupervised method to preprocess and reduce the dimensionality of high-dimensional datasets like the one we have. We compute the principal components of the data, and use them to perform a change of basis on the data. Principal components are a sequence of unit vectors where the i-th vector is along the direction of the line that fits the data best, while also being orthogonal to the  (i-1)-th vector. From this, we can infer that the first vector would be the vector along the direction of maximum variance in the data. For dimensionality reduction, we project each data point onto only the first few principal components, while preserving as much data variance as possible.

- **K-Means clustering**: We ran the **K-Means clustering** algorithm on our player data in order to see if we could identify meaningful patterns among players that might help classify them into useful roles/categories. K-means clustering is a method aimed to cluster n data-points into k clusters. The clustering is performed such that every data-point belongs to the cluster with the nearest mean (The mean of each cluster is used to represent the “cluster center”. Often, k random points from the data are used as the initial means). K-means clustering works to minimize the within-cluster variance. For this purpose, we leveraged the scikit-learn implementation of K-Means. Notably, this implementation does not use the classical EM-style algorithm for K-Means but rather the “Elkan” variation, which exploits the triangle inequality in order to converge more quickly. By default, this implementation assumes a goal of K=8 clusters, but we use the elbow method in order to find a more optimal number of clusters.

In supervised methods:
- Support Vector Machine
- Fully Connected Neural Network: We implemented Fully connected neural networks of different depths, and performed hyperparameter optimization to obtain best results for game result prediction. We used the library TensorFlow in Python to implement this. 
- Convolutional Neural Network (CNN): We implemented a 1-D convolutional neural network to perform game result prediction. The python library TensorFlow was used.

We will use the outcome of PCA for supervised methods, and analyze how it affects the outcome, by comparing with supervised outcomes without the use of PCA.

The supervised method implements the game winner prediction. We already have data around the teams playing each other and the team stats along with the labels which represents which team wins. We plan on using the output of our unsupervised learning to generate a few consolidated features for each of the players involved in the game and adding these features to our dataset to make more robust game predictions.

### Results and Discussion

#### PCA analysis

After applying PCA to our dataset, our original set of 45 features was reduced down to just 5 features, constituting a nearly 90% decrease in the number of features. These features retain 99% of the variance of the original data, and thus should be suitable for our further analyses.

![image](https://user-images.githubusercontent.com/39341245/161835399-e536a6b7-cc78-41f7-b878-47580445934a.png)

Part of the correlation matrix before PCA:

![image](https://user-images.githubusercontent.com/23053768/164111594-0570fe34-740c-42a5-a525-421e8d55807b.png)

Correlation matrix after PCA:

![image](https://user-images.githubusercontent.com/23053768/164111661-cca6ccad-8775-45ee-ae11-225c3e8aa594.png)

#### K-Means analysis

The dataset used for K-Means analysis has 5 features obtained after the feature reduction. The dataset was divided into clusters ranging from 4 to 12 clusters. The elbow method was then used to determine the best number of clusters which came out to be 7.  

![image](https://user-images.githubusercontent.com/25253566/161800360-b2929900-2db1-4967-aa93-5d3e62dda8ad.png)

Because the dataset has 5 features, the clusters cannot be represented on a graph. So for the purpose of demonstration, here are the clusters which just have 2 features out of the 5 plotted. These 2 features have been selected because they retain 94% of the variance of the dataset. This would pretty much give us a good idea around how the clustering has been done.

![image](https://user-images.githubusercontent.com/25253566/161800524-a3980b7c-a99b-4af7-8c4b-9db1eba09b33.png)
<img src = "https://user-images.githubusercontent.com/25253566/165320799-f744ed92-9316-48c5-a8aa-ffafd507e269.png" height = "200" width = "250">

Here is a pairplot of the clusters with each of the principle components pairwise:
![pairplot](https://user-images.githubusercontent.com/25253566/165343848-fb80fa9b-4bcf-42c4-8c13-8fbbad09eda0.png)

As we can see, a few of the clusters are really well defined (the blue, black, red, purple and yellow) while the remaining 2 are a bit scattered around. The popular metric scores around clustering for this data have been evaluated as follows:

Davies-Bouldin index: 0.781    

Silhouette coefficient: 0.415

Values closer to 0 imply better clustering in the case of Davies-Bouldin index. The current score reflects the clustering done in this manner. The silhouette score also tends a bit towards 0, which reflects that the clusters are not that well distanced from each other.

#### NBA Player Clustering Discussion

The first observation was that the players clustered together did not have the same "traditional" positions i.e each cluster has a mix of players in different positions. 

Now Cluster=5 pops out with only 21 players, this cluster in my opinion represents players with distinct performances in the season. Eg. Giannis won the finals MVP, Steph Curry was the scoring leader, Jokic won the regular season MVP, etc. [4]. Players like Bradley Beal, Luka Doncic, etc. were in the All NBA teams. Other players like Julius Randle, Russel Westbrook, etc. were in the MVP conversation [5].

As the TA pointed out correctly, there are some outliers when looking at players like RJ Barret and Zion Williamson who weren't in the discussion of top-cream of players that season. Possible reasons for this could be because of them starting in all games they played along with other features like effective field goal percentage being as high as the other players here but the exact reason doesn't seem to be clear. Also they could be the lower end of the spectrum here and despite of good overall performances, didn't stand out to be in this dicussion.

![image](https://user-images.githubusercontent.com/23053768/161846931-8a39c4d9-5453-43a2-bbe1-8ca7d29f75a4.png)

Cluster 2 could represent the next set of top-players with LeBron, Kyrie Irving, Joel Embid, etc. who made the All-NBA teams. In general the list consists of players that are considers as stars in the league.

![image](https://user-images.githubusercontent.com/23053768/161848812-dae16a3a-7f58-434d-a452-f49139fac5b0.png)

Cluster 6 consists of players that contribute a lot to the team like Lamelo Ball, Reggie Jackson but don't have a consistently high impact.  Notable names, which are superstars and in this category are Kevin Durant and James Harden, both of which had been sidelined in the season due to injuries and is  possible a reason of them being in this cluster rather than Cluster 5 or 2. 

![image](https://user-images.githubusercontent.com/23053768/161849499-beb42b34-834f-4a6f-9823-aafdc0540752.png)

Cluster 0 seems to consist of players that generally have low scoring performances, come off the bench and are considered as backups in the 2nd or 3rd unit of a team.

![image](https://user-images.githubusercontent.com/23053768/161850116-540d4fde-0081-45e3-a2dd-a0c81d5aa787.png)

Cluster 1 could be players that support the players in cluster 2, 5 and 6 with players like Lonzo Ball, Carmelo Anthony, Draymond Green, etc. 

![image](https://user-images.githubusercontent.com/23053768/161850376-f18c2242-ea32-4e02-b86d-69aadb642fff.png)

Cluster 3 and 4 seems to have players that generally don't put up big performances with Cluster 4 being the lower end of that spectrum.

![image](https://user-images.githubusercontent.com/23053768/161850746-1cf7e0c6-e980-4048-a272-bfe207c9c8ef.png)

![image](https://user-images.githubusercontent.com/23053768/161850722-962b633d-2529-4d18-8d74-48f3887288e6.png)

#### Fully Connected Neural Network



#### 1-D Convolutional Neural Network

The 1-D convolutional neural network used had two 1-D convolutional layers, with 32 and 64 filters each respectively. A kernel size of 3 was used with stride as 1. The two convolutional layers were followed by two fully connected layers, having output sizes 200 and 2 respectively. All layers except the last fully connected layer were followed Rectified Linear Unit as the activation function. The last fully connected layer used Softmax as the activation function for classification into one of 2 classes (team A wins vs Team B wins). The learning rate used was 1e-5, and Sparse Categorical Crossentropy loss was used (labels used were 0 and 1). Adam optimizer was used for training. The data was split into training and testing data according to an 80:20 ratio.

The best accuracy for the test dataset obtained was 61.42%. The following diagrams represent the accuracy and loss plots for the model over 50 epochs:

![image](https://user-images.githubusercontent.com/30110646/165359304-cae02d97-d996-4da6-a587-d3ef3c38b805.png)
![image](https://user-images.githubusercontent.com/30110646/165359349-396d155b-f9c2-476b-b8fd-46672f3344b8.png)

### Conclusions



### References

[1] https://www.basketball-reference.com/

[2] https://www.nbastuffer.com/

[3] https://towardsdatascience.com/predicting-the-outcome-of-nba-games-with-machine-learning-a810bb768f20

[4] https://wwhttps://github.com/jaebradley/basketball_reference_web_scraperw.basketball-reference.com/leagues/NBA_2021.html

[5] https://www.nba.com/news/nikola-jokic-wins-2020-21-kia-nba-most-valuable-player-award

[6] https://github.com/jaebradley/basketball_reference_web_scraper


### Proposed timeline and Responsibilities

| Dates       | Deliverable                     |
| ----------- | ------------------------------- |
| 3/3/2022    | Relevant Data Collection        |
| 3/10/2022   | Data Cleaning/Pruning           |
| 3/24/2022   | Data Analysis & Clustering      |
| 4/7/2022    | Clustering results and Start with Data curation for supervised method |
| 4/21/2022   | Supervised Method  & Project Video |
| 4/26/2022   | Final Project Report      |


| Member             | Responsiblity                           |
| ------------------ | --------------------------------------- |
| Adrian Thinnyun    | Unsupervised Methods, Supervised - SVM    |
| Brahmi Dwivedi     | Unsupervised Methods - PCA, Supervised - CNN          |
| Omkar Prabhu       | Unsupervised Data Curation, NBA Clustering implementation and discussion, supervised data curation |
| Rahul Shenoy       | Supervised Data Curation, Supervised Data Feature Engineering       |
| Yashodeep Mahapata | UnSupervised KMeans Analysis, NN  |

