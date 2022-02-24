## NBA Player Clustering and Game Win Prediction

### Intro/Background

The National Basketball Association (NBA) is a professional North American basketball league with 30 teams divided into 2 conferences - East and West.

There are several famous players like Michael Jordan, Lebron James, etc. and fans often debate over who is the Greatest Of All Time(G.O.A.T).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">LeBron: &quot;At that moment I was like I&#39;m the greatest basketball player people have ever seen.&quot; <br><br>Michael Jordan: <a href="https://t.co/mBmuP8d1H9">pic.twitter.com/mBmuP8d1H9</a></p>&mdash; NBA Memes (@NBAMemes) <a href="https://twitter.com/NBAMemes/status/1496001690549768197?ref_src=twsrc%5Etfw">February 22, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The players also play specific positions like Point Guard, Center, etc. With time the nature of how players play the game also changed. For example the game now involves players shooting more 3 pointers. Thus rather than using traditional positions, it would be interesting to know how players relate to each other based on their statistics to aid in comparison.

Aside from this betting on games is also an aspect which the league also promotes like [NBABet](https://www.nba.com/nbabet), [FanDuel](https://www.fanduel.com/tnt), etc.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Charles Barkley guarantees Bucks win<br><br>Heat fans: <a href="https://t.co/18gk7CAQ3d">pic.twitter.com/18gk7CAQ3d</a></p>&mdash; NBA Memes (@NBAMemes) <a href="https://twitter.com/NBAMemes/status/1397024441368932352?ref_src=twsrc%5Etfw">May 25, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Often, as the season progresses, it's easy to spot which teams are going to be in the lead, and for games played by these high performing teams against lower ranked teams, the outcomes are predictable. The outcome of a game between two highly ranked teams though is generally difficult to predict and these games are the most exciting. Using data can help provide better insight for predicting game outcomes and possibly making money!

### Problem Definition

Given statistics for players and game history, roster per team, etc. for a game between two teams A and B this season, predict the outcome of the game.

Dataset description:

We have the following data available:
- Dataset with features for around 650 players participating in the NBA. Each player in the dataset has 26 features associated with them across multiple seasons and teams. These features include the basic information of the player like age, position on the court, etc. and much more complex features like the turnover rate, shooting percentage, etc.

Source: 
- Game history for teams over multiple seasons.

Source: 

### Methods

We are planning to employ both supervised and unsupervised approaches for our problem. 

In unsupervised methods:

We plan to implement **Principal Component Analysis** (PCA) as a “preprocessing step”, which should help us reduce the dimensionality of features to use for clustering/classification. We will also use **k-means clustering** to see if we can classify the data into meaningful clusters to identify hidden patterns.

In supervised methods, we plan to implement:
- Support Vector Machine
- Fully Connected Neural Network 
- Convolutional Neural Network (CNN)

We will use the outcome of PCA for supervised methods, and analyze how it affects the outcome, by comparing with supervised outcomes without the use of PCA.

### Potential results and Discussion

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
| 3/24/2022   | Data Analysis/Model Generation |
| 4/7/2022    | Predicational Analysis on Model |
| 4/21/2022   | Project Video                   |
| 4/26/2022   | Final Project Report Video      |


| Member             | Responsiblity                           |
| ------------------ | --------------------------------------- |
| Adrian Thinnyun    | Predictional Analysis/Project Video    |
| Brahmi Dwivedi     | Data Collection/Data Cleaning           |
| Omkar Prabhu       | Predictional Analysis/Model Generation |
| Rahul Shenoy       | Data Cleaning/Model Generation         |
| Yashodeep Mahapata | Project Report/Predictional Analysis   |

