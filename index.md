## NBA Player Clustering and Game Win Prediction

<iframe width="560" height="315" src="https://www.youtube.com/embed/9ehn7Ot--Ds" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Intro/Background

The National Basketball Association (NBA) is a professional North American basketball league with 30 teams divided into 2 conferences - East and West.

There are several famous players, like Michael Jordan and Lebron James, and fans often debate over who is the Greatest Of All Time (G.O.A.T).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">LeBron: &quot;At that moment I was like I&#39;m the greatest basketball player people have ever seen.&quot; <br><br>Michael Jordan: <a href="https://t.co/mBmuP8d1H9">pic.twitter.com/mBmuP8d1H9</a></p>&mdash; NBA Memes (@NBAMemes) <a href="https://twitter.com/NBAMemes/status/1496001690549768197?ref_src=twsrc%5Etfw">February 22, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The players also play specific positions like Point Guard, Center, etc. With time the nature of how players play the game has also changed. For example, the game now involves players shooting more 3 pointers. Thus rather than using traditional positions, it would be interesting to know how players relate to each other based on their statistics to aid in comparison.

Aside from this, betting on games is also an aspect which the league promotes through sites like [NBABet](https://www.nba.com/nbabet), [FanDuel](https://www.fanduel.com/tnt), etc.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Charles Barkley guarantees Bucks win<br><br>Heat fans: <a href="https://t.co/18gk7CAQ3d">pic.twitter.com/18gk7CAQ3d</a></p>&mdash; NBA Memes (@NBAMemes) <a href="https://twitter.com/NBAMemes/status/1397024441368932352?ref_src=twsrc%5Etfw">May 25, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Often, as the season progresses, it's easy to spot which teams are going to be in the lead, and for games played by these high performing teams against lower ranking teams, the outcomes are predictable. The outcome of a game between two highly ranked teams though is generally difficult to predict, and these games are the most exciting. Using data can help provide better insight for predicting game outcomes and possibly making money!

### Problem Definition

Given statistics for players and game history, roster per team, etc. for a game between two teams A and B this season, predict the outcome of the game.

Dataset description:

We have the following data available:
- Dataset with features for around 650 players participating in the NBA. Each player in the dataset has 26 features associated with them across multiple seasons and teams. These features include the basic information of the player like age, position on the court, etc. and much more complex features like the turnover rate, shooting percentage, etc.

Source: [2]
- Game history for teams over multiple seasons.

Source: [1]

### Data Collection & Preprocessing

#### NBA Player Clustering

For the player clustering, we considered the basketball stats from two different sources:
1. 2021-22 Season Advanced Stats: https://www.basketball-reference.com//leagues/NBA_2021_advanced.html
2. 2021-22 Season Stats: https://www.basketball-reference.com//leagues/NBA_2021_totals.html

The Season Stats contain simpler metrics like 3-pointers made, field-goals made, rebounds, blocks, etc. On further exploration we found that there are other advanced statistics like offensive rebound percentage, offensive win shares, etc. which we can use. Thus the combined features list is as follows:

![image](https://user-images.githubusercontent.com/23053768/161634743-c122b059-d762-4d63-85ef-4cc3bb34937c.png)

![image](https://user-images.githubusercontent.com/23053768/161634774-1345eb25-f275-4c87-8ea4-7cf6086e3ec6.png)

Now, the dataset wasn't perfect, so first we did some inital cleaning by removing rows which have invalid values such as -inf, inf or nan and obtained the following set.

![image](https://user-images.githubusercontent.com/23053768/161635342-7e4edd84-d420-459e-ab2a-125ea756fa2c.png)

In each season there are players who get traded i.e. they start the season with a certain team but mid-season they go to another team. Thus this dataset would contain 3 rows (for the older team, newer team and a total stat record) for the players as shown below

![image](https://user-images.githubusercontent.com/23053768/161635982-ec6d2ee0-a6e4-4850-a996-6164bc5f79ba.png)

Thus to reduce this to a single datapoint we just used the team for which the player has played the most games. The reason for this decision was that players perform differently in different teams and the total would not give a good estimate, also the more games a player plays the better the metric represents the player's true style.

![image](https://user-images.githubusercontent.com/23053768/161636264-37e9bb75-82ce-4720-9d0c-bdbe3b0c9c27.png)

Finally, we dropped the players which have < 25 games in this season for the same reason mentioned before.

![image](https://user-images.githubusercontent.com/23053768/161636330-10b9e92d-b41e-4e3b-8c35-4900b794133d.png)

#### Game Prediction

For game data, we scraped data from these two sources:
1. https://www.basketball-reference.com/leagues/NBA_2021_games.html
2. https://www.basketball-reference.com/leagues/NBA_2021.html - Per Game Statistics

The game statistics contains a list of games that took place in the 2020-2021 Season. They have some relevant information like dates, the teams involved, and their scores. Each month had a list of games that had to be merged together to consolidate all the data.

<img width="540" alt="Screenshot_1" src="https://user-images.githubusercontent.com/32405336/161855733-f2f826b9-5d08-4db2-8f7f-9b28220db966.png">

Some cleaning had to be done to this data as well. We excluded any NaN values and removed any columns that would not provide any benefit to our model such as Attendance or Game Start Time. Finally, we added an Outcome column. This would be the labels for our supervised learning portion.

<img width="523" alt="Screenshot_2" src="https://user-images.githubusercontent.com/32405336/161855761-15c8dc4b-609b-47e0-803d-6de654c3ac4b.png">

There was some more information that we could merge into this dataset that could provide some insight on the outcomes. This came from the Per Game Stats that was pulled from the (2) link. Each team while on Home or Visitor standing had an average statistics value across the team, similar to the advanced player stats such as FG (Field Goals) and 3P (3-Point Field Goals). We merged this data into our outcomes dataset based on Visitor or Home. Here's a sample of 5.

<img width="781" alt="Screenshot_3" src="https://user-images.githubusercontent.com/32405336/161855779-0987220c-3226-469a-a1ab-1680c1974b23.png">

We have plenty of features that we can feed into our neural network to get an understanding on the outcomes of games.

An interesting thing to notice is that there was a noteworthy advantage for Home teams. Home teams came out winning more on average compared to Visitors.

<img width="324" alt="Screenshot_4" src="https://user-images.githubusercontent.com/32405336/161855809-f4b9b5b2-be9f-40e6-91b2-ed1b0b321b3d.png">

We then realized that these wouldn't be the best features for our supervised model, and rather than using features for the current game, average team statistics for the last 5 would provide more meaning and would make sense as we use previous games for predicting game outcome. We also made an attempt to integrate player stats either through the raw data or clustering but couldn't manage to utilize them for game prediction.

We use the Basketball Reference API [6] to get game details for a season and then used the date to get box-scores and statistics for each team. But we found that the game details API would give incorrect dates which results in the team not being found when trying to get their box-score. Thus we manually extracted data for 3 seasons (2021-22, 2020-21 and 2019-20) and then for each game we used the API to get box scores for home and away teams. This was followed by aggregating the stats for the current home and away teams in a game over their last 5 games. Thus for each game across 3 seasons we now have the following features:

![image](https://user-images.githubusercontent.com/23053768/164947779-aa89c455-9bb7-4526-bdac-ba4a38e485fa.png)

We wanted to get some more information out of the data, specifically a metric to rank the teams amongst each other. A simple system for Elo rating was used. The idea was to capture some complexity from the data, but ultimately keep it as general as possible to avoid overfitting. At the start of our dataset, all teams would begin with a standard Elo of 1500. This was just a general number that we found would be the most useful. If a team won, they would win elo and the opposition would lose the same amount. It was important that the Elo difference was the same, and that the overall Elo across the whole roster would remain the same. This would ensure that their rankings would be close to their actual hidden ranking. To capture a little more complexity, we made sure that if a team won by a huge margin then they would gain more Elo. After analysing the data, we found that if the score difference at the end of the game was less than 10 points then that meant that the game was pretty close. In this case the winner would only gain 5 points in our Elo system. If the score difference was between 10 and 20 then this was an average win, so the winner would receive 10 points. Finally a win with a score difference of >20 meant a huge victory, and this would come with 15 Elo points to the winner. In all cases, the losing team would lose the same amount as mentioned before.

Here is some sample data from the beginning of our dataset to get a visualization of how the elo would change. Other columns were dropped only for clarity in this image.

<img width="912" alt="Screenshot_1" src="https://user-images.githubusercontent.com/32405336/165234034-9c509927-369b-43d1-9147-37f4b9aca249.png">

The Elo system helps place teams over our entire dataset. We can consider that teams near 1500 Elo are considered average. At the end, we compiled what Elo each team had, and it was pretty accurate with how those teams actually performed over that time period. Specifically we found teams like the Phoenix Suns (2220), the Utah Jazz (2265), and the Milwaukee Bucks (2445) coming out on top. These teams performed well above average over that time frame. While on the other hand, teams like Orlando Magic (650), Detroit Pistons (705), and Houston Rockets (850) performed pretty poorly over the same timeframe.

Thus, finally we used the stats for home and away teams in last five games along with elo_before rating as the input features to our supervised methods. We dropped the home_points, away_points, after_elo and as they would directly correspond to the outcome.

### Methods

In unsupervised methods:

- **Principal Component Analysis**: We implemented **Principal Component Analysis** (PCA) as a “preprocessing step”, which should help us reduce the dimensionality of features to use for clustering/classification. 

Principal Component Analysis is an unsupervised method to preprocess and reduce the dimensionality of high-dimensional datasets like the one we have. We compute the principal components of the data, and use them to perform a change of basis on the data. Principal components are a sequence of unit vectors where the i-th vector is along the direction of the line that fits the data best, while also being orthogonal to the  (i-1)-th vector. From this, we can infer that the first vector would be the vector along the direction of maximum variance in the data. For dimensionality reduction, we project each data point onto only the first few principal components, while preserving as much data variance as possible.

- **K-Means clustering**: We ran the **K-Means clustering** algorithm on our player data in order to see if we could identify meaningful patterns among players that might help classify them into useful roles/categories. K-means clustering is a method aimed to cluster n data-points into k clusters. The clustering is performed such that every data-point belongs to the cluster with the nearest mean (The mean of each cluster is used to represent the “cluster center”. Often, k random points from the data are used as the initial means). K-means clustering works to minimize the within-cluster variance. For this purpose, we leveraged the scikit-learn implementation of K-Means. Notably, this implementation does not use the classical EM-style algorithm for K-Means but rather the “Elkan” variation, which exploits the triangle inequality in order to converge more quickly. By default, this implementation assumes a goal of K=8 clusters, but we use the elbow method in order to find a more optimal number of clusters.

In supervised methods:
- **Support Vector Classifier**: We implemented a **Support Vector Classifier** (SVC) and performed a grid search over various hyperparameters including kernel and regularization level to achieve the best game prediction performance. Both the SVC itself as well as the grid search were implemented in Python using the scikit-learn library.
- **Fully Connected Neural Network**: We implemented **Fully Connected Neural Networks** of different depths (number of layers) and performed hyperparameter optimization to obtain best results for game result prediction. We used the library TensorFlow in Python to implement this. Fully Connected Neural Networks consist of multiple layers of neurons, where each neuron in one layer is connected to every other neuron in the next layer and the previous layer.
- **1-D Convolutional Neural Network (CNN)**: We implemented a **1-D Convolutional Neural Network** to perform game result prediction. The python library TensorFlow was used. In 1-D convolution, the convolution operation is performed between the vector and filter (number of filters is a hyperparameter), resulting in an output vector which has as many channels as the number of filters used. The size of the filter is also a hyperparameter. We use 1-D convolution as our input consists of 1-D vectors (as opposed to 2-D or 3-D images for 2-D convolution).

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

As we can see, a few of the clusters are really well defined (the blue, black, red, purple and yellow) while the remaining 2 are a bit scattered around. The values for our clustering based on some popular metrics are as follows:

Davies-Bouldin index: 0.781    

Silhouette coefficient: 0.415

Values closer to 0 imply better clustering in the case of Davies-Bouldin index. The current score reflects the clustering done in this manner. The silhouette score also tends a bit towards 0, which reflects that the clusters are not that well distanced from each other.

#### NBA Player Clustering Discussion

The first observation was that the players clustered together did not have the same "traditional" positions i.e each cluster has a mix of players in different positions. 

Now Cluster 5 stands out with only 21 players, this cluster in my opinion represents players with distinct performances in the season. Eg. Giannis won the finals MVP, Steph Curry was the scoring leader, Jokic won the regular season MVP, etc. [4]. Players like Bradley Beal, Luka Doncic, etc. were in the All NBA teams. Other players like Julius Randle, Russel Westbrook, etc. were in the MVP conversation [5].

As pointed out to us by a TA, there are some outliers when looking at players like RJ Barret and Zion Williamson who weren't being considered MVP players that season. Possible reasons for this could be because of them starting in all games they played along with other features like effective field goal percentage being as high as the other players here but the exact reason doesn't seem to be clear. Also they could be the lower end of the spectrum here and despite good overall performances simply didn't stand out enough to be in this dicussion.

![image](https://user-images.githubusercontent.com/23053768/161846931-8a39c4d9-5453-43a2-bbe1-8ca7d29f75a4.png)

Cluster 2 could represent the next set of top-players with LeBron, Kyrie Irving, Joel Embid, etc. who made the All-NBA teams. In general the list consists of players that are considered stars in the league.

![image](https://user-images.githubusercontent.com/23053768/161848812-dae16a3a-7f58-434d-a452-f49139fac5b0.png)

Cluster 6 consists of players that contribute a lot to the team like Lamelo Ball and Reggie Jackson but don't have a consistently high impact.  Notable names, which are superstars and in this category are Kevin Durant and James Harden, both of which had been sidelined in the season due to injuries, which might be the reason for them being in this cluster rather than Cluster 5 or 2. 

![image](https://user-images.githubusercontent.com/23053768/161849499-beb42b34-834f-4a6f-9823-aafdc0540752.png)

Cluster 0 seems to consist of players that generally have low scoring performances, come off the bench, and are considered as backups in the 2nd or 3rd unit of a team.

![image](https://user-images.githubusercontent.com/23053768/161850116-540d4fde-0081-45e3-a2dd-a0c81d5aa787.png)

Cluster 1 could be players that support the players in cluster 2, 5 and 6 with players like Lonzo Ball, Carmelo Anthony, Draymond Green, etc. 

![image](https://user-images.githubusercontent.com/23053768/161850376-f18c2242-ea32-4e02-b86d-69aadb642fff.png)

Cluster 3 and 4 seem to have players that generally don't put up big performances with Cluster 4 being the lower end of that spectrum.

![image](https://user-images.githubusercontent.com/23053768/161850746-1cf7e0c6-e980-4048-a272-bfe207c9c8ef.png)

![image](https://user-images.githubusercontent.com/23053768/161850722-962b633d-2529-4d18-8d74-48f3887288e6.png)

For the following three methods, in order to preprocess the input correctly, non-numerical features (e.g. team names) are converted to one-hot vector representations (30 unique teams in total = size 30 vector), and dates are divided into 3 features: day, month and year.

#### Support Vector Classifier

Three different parameters were considered in the grid search parameter optimization: the kernel function used in the algorithm (linear, polynomial, or gaussian rbf), the value of the regularization parameter C (0.03, 0.1, 0.3, 1, 3), and the degree of the kernel function if polynomial (2, 3, 4). The grid search revealed that the best choice of parameters was an SVC using a linear kernel function and a C of 0.1. Since no polynomial kernel function was used, the degree is irrelevant. The data was split into training and testing data according to an 80:20 ratio.

The SVC fitted using these parameters achieved an accuracy of 60.00% on the test dataset.

#### Fully Connected Neural Network

The fully connected neural network used consisted of 5 fully connected layers, with output sizes 100, 200, 100, 50 and 2 respectively. All but the final layer used a Rectified Linear Unit as the activation function. The final layer used Softmax as the activation function for classification into one of 2 classes (team A wins vs Team B wins, labelled 0/1). Dropout regularization was used before the last fully connected layer with dropout probability 0.5. An Adam optimizer with a sparse categorical cross entropy loss function was used for training, with a learning rate of 1e-5. The data was split into training and testing data according to an 80:20 ratio.

The final accuracy obtained for the test data for this model was 60.84%. The following diagrams represent the accuracy and loss plots for the model over 50 epochs:

![image](https://user-images.githubusercontent.com/30110646/165373585-1828b179-8af0-44ca-89e7-e27d6c764e70.png)
![image](https://user-images.githubusercontent.com/30110646/165373594-64686338-17c4-4371-a698-74c31f4a8872.png)

#### 1-D Convolutional Neural Network

The 1-D convolutional neural network used had two 1-D convolutional layers, with 32 and 64 filters each respectively. A kernel size of 3 was used with stride 1. The two convolutional layers were followed by two fully connected layers, having output sizes 200 and 2 respectively. All layers except the last fully connected layer were followed by a Rectified Linear Unit as the activation function. The last fully connected layer used Softmax as the activation function for classification into one of 2 classes (team A wins vs Team B wins). The learning rate used was 1e-5, and a sparse categorical cross entropy loss function was used (labels used were 0 and 1). An Adam optimizer was used for training. The data was split into training and testing data according to an 80:20 ratio.

The best accuracy for the test dataset obtained was 61.42%. The following diagrams represent the accuracy and loss plots for the model over 50 epochs:

![image](https://user-images.githubusercontent.com/30110646/165359304-cae02d97-d996-4da6-a587-d3ef3c38b805.png)
![image](https://user-images.githubusercontent.com/30110646/165359349-396d155b-f9c2-476b-b8fd-46672f3344b8.png)

### Conclusions

Using player statistics for a particular season we discovered meaningful clusters with KMeans (after using PCA) which can be an alternative to comparing players by their basketball positions. This could potentially be extended to clustering players using their stats across multiple seasons and viewing how the clusters change. Also, including players from different era's in the NBA like 80-90's with Shaq, Jordan, etc. with current players like LeBron, Durant, etc. to see how they are clustered would be an interesting extension.

For supervised methods, both FCNN and 1-D CNN resulted in slightly higher than 60% accuracy for the test dataset. 1-D CNN performed slightly better than FCNN, but not by a huge margin. The SVC performed slightly worse than either of the neural networks at exactly 60% test accuracy, but again not by a huge margin. While this accuracy is better than a completely random prediction, there is scope for improvement. One of the areas for improvement can be feature engineering- manipulating raw features such that they will help extract more meaningful patterns for game outcome prediction.  Currently, we have summarized last 5 games of each team by averaging raw features for those 5 games, and are using these as features to represent that team. Probably features that contain richer information over a longer time period than 5 games would lead to better outcomes.

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
| 4/21/2022   | Supervised Method & Project Video |
| 4/26/2022   | Final Project Report      |


| Member             | Responsiblity                           |
| ------------------ | --------------------------------------- |
| Adrian Thinnyun    | Unsupervised Methods, Supervised - SVC, Project Videos    |
| Brahmi Dwivedi     | Unsupervised Methods - PCA, Supervised - CNN          |
| Omkar Prabhu       | Unsupervised Data Curation, NBA Clustering implementation and discussion, supervised data curation |
| Rahul Shenoy       | Supervised Data Curation, Supervised Data Feature Engineering       |
| Yashodeep Mahapata | Unsupervised KMeans Analysis, NN  |

