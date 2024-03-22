# League of Legends Project
Data science project for my DSC 80 class at UCSD. It incorporates almost all the aspects of the data science life cycle that we learned in class. My particular project was done using the League of Legends dataset from Oracle's Elixir public match data.

By: Noah Danan

## Introduction
### Dataset
The dataset used comes from competitive esports matches of League of Legends (LoL). It is hands down the most popular multiplayer online battle arena (MOBA) game since its release in 2009. Specifically, I will be focussing on games from the two regions of LCK (League of Champions Korea) and the LEC (League of Legends European Championship), both of whom are widely regarded as the best regions. After being converted into a pandas dataframe, the dataset contains 148,992 rows with 131 columns. These will both decrease significantly as the data is cleaned and filtered for only the relevant data. 
### Centered Question
Does the model's performance, as measured by precision, vary between the LCK and LEC leagues, and what does this indicate about fairness in predictive modeling within esports analytics?
### Significance
Competitive Integrity: Fairness in predictive analytics ensures that insights derived from data do not inherently favor one group over another, maintaining the competitive integrity of the esports environment.
Strategic Development: Teams and organizations can leverage fair and accurate models to develop strategies, scout talent, and analyze opponents, promoting a higher level of play and innovation within the game.
Fan Engagement: Fans and viewers often engage with statistical analyses and predictions as part of their viewing experience. Ensuring the accuracy and fairness of these predictions enhances the engagement and trust of the community.
### Relevant Columns
league
: Identifies the league of the match (LCK or LEC), indicating the regional competition.

result
: The match outcome (win or loss), serving as the target variable for predictive modeling.

firsttower
: Indicates whether the team secured the first tower of the game, a binary metric reflecting early-game objectives.

golddiffat15
: The gold difference between the teams at 15 minutes, serving as a numerical indicator of early-game economic advantage.

totalgold
: Total gold earned by the team throughout the match, representing overall economic resources.

kills
: The total number of kills secured by the team, reflecting the team's aggressiveness and success in combat.

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
#### Filter for Teams (Applied Starting with Hypothesis Test)
I first filtered out all the player stats by keeping only rows in the dataframe where 'position' was set to 'team.'
#### Correct Data Types
I then changed the appropriate data from strings to datetime objects so that I could use time-series analysis if needed.
#### Dropped Impossible Rows
I also looked through some of the data and dropped rows where data didn't make sense in the context of a LoL match, i.e. dropping any row where gold earned, total gold, and/or kills was less than zero.
#### Encoding Categorical Variables
Lastly, I changed the data types of the playoffs and result columns to boolean.
#### Cleaned Dataframe Head
| gameid                | datacompleteness   | url                                         | league   |   year | playoffs   | date                |   game | position   | result   |   kills |   deaths |   assists |   firstblood |   firsttower |   oceans |     dpm |   totalgold |   earnedgold |   goldspent |   goldat15 |   golddiffat15 |   killsat15 |   assistsat15 |   deathsat15 |
|:----------------------|:-------------------|:--------------------------------------------|:---------|-------:|:-----------|:--------------------|-------:|:-----------|:---------|--------:|---------:|----------:|-------------:|-------------:|---------:|--------:|------------:|-------------:|------------:|-----------:|---------------:|------------:|--------------:|-------------:|
| ESPORTSTMNT01_2690210 | complete           | nan                                         | LCKC     |   2022 | False      | 2022-01-10 07:44:08 |      1 | team       | False    |       9 |       19 |        19 |            1 |            1 |        0 | 1981.09 |       47070 |        28222 |       44570 |      24806 |            107 |           5 |            10 |            6 |
| ESPORTSTMNT01_2690210 | complete           | nan                                         | LCKC     |   2022 | False      | 2022-01-10 07:44:08 |      1 | team       | True     |      19 |        9 |        62 |            0 |            0 |        0 | 2799.02 |       52617 |        33769 |       45850 |      24699 |           -107 |           6 |            18 |            5 |
| ESPORTSTMNT01_2690219 | complete           | nan                                         | LCKC     |   2022 | False      | 2022-01-10 08:38:24 |      1 | team       | False    |       3 |       16 |         7 |            0 |            0 |        0 | 1690.98 |       57629 |        34688 |       53945 |      23522 |          -1763 |           1 |             1 |            3 |
| ESPORTSTMNT01_2690219 | complete           | nan                                         | LCKC     |   2022 | False      | 2022-01-10 08:38:24 |      1 | team       | True     |      16 |        3 |        39 |            1 |            1 |        0 | 2124.55 |       71004 |        48063 |       66410 |      25285 |           1763 |           3 |             3 |            1 |
| 8401-8401_game_1      | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 | False      | 2022-01-10 09:24:26 |      1 | team       | True     |      13 |        6 |        35 |            0 |          nan |      nan | 1762.02 |       45468 |        30167 |       36908 |        nan |            nan |         nan |           nan |          nan |
### Univariate Analysis

### Bivariate Analysis
### Interesting Aggregates
