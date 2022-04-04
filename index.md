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

Source: https://www.nbastuffer.com/
- Game history for teams over multiple seasons.

Source: https://www.basketball-reference.com/

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

### Methods

WRITE ABOUT THINGS DONE FOR NOW

---- REMOVE OR EDIT THIS ----

In unsupervised methods:

We plan to implement **Principal Component Analysis** (PCA) as a “preprocessing step”, which should help us reduce the dimensionality of features to use for clustering/classification. We will also use **k-means clustering** to see if we can classify the data into meaningful clusters to identify hidden patterns.

---- REMOVE OR EDIT THIS ----


WRITE ABOUT THINGS TODO

A simple extension 

In supervised methods, we plan to implement:
- Support Vector Machine
- Fully Connected Neural Network 
- Convolutional Neural Network (CNN)

We will use the outcome of PCA for supervised methods, and analyze how it affects the outcome, by comparing with supervised outcomes without the use of PCA.

### Results and Discussion

WRITE MORE ABOUT THE RESULTS

We expect the outcome of PCA to compress the dataset, potentially making supervised methods faster, but it may compromise with the model’s performance. K-means clustering will give us insight into patterns in the dataset. We expect the supervised outcomes to be better without PCA, as PCA might lead to loss of information(depending on the extent of dimensionality reduction).

### References

- https://www.basketball-reference.com/
- https://www.nbastuffer.com/
- https://towardsdatascience.com/predicting-the-outcome-of-nba-games-with-machine-learning-a810bb768f20


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
| Adrian Thinnyun    | Predictional Analysis/Project Video    |
| Brahmi Dwivedi     | Data Collection/Data Cleaning           |
| Omkar Prabhu       | Unsupervised Data Curation and Inital Clustering method - PCA & KMeans |
| Rahul Shenoy       | Data Cleaning/Model Generation         |
| Yashodeep Mahapata | Project Report/Predictional Analysis   |

