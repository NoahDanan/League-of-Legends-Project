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
#### Filter for Teams
I first filtered out all the player stats by keeping only rows in the dataframe where 'position' was set to 'team.'
#### Correct Data Types
I then changed the appropriate data from strings to datetime objects so that I could use time-series analysis if needed.
#### Dropped Impossible Rows
I also looked through some of the data and dropped rows where data didn't make sense in the context of a LoL match, i.e. dropping any row where gold earned, total gold, and/or kills was less than zero.
#### Encoding Categorical Variables
Lastly, I changed the data types of the playoffs and result columns to boolean.
#### Cleaned Dataframe Head
| gameid                | datacompleteness   | url                                         | league   |   year | split   |   playoffs | date                |   game | side   | position   | teamname                      |   gamelength |   result |   kills |   deaths |   assists |   teamkills |   teamdeaths |   firstblood |   team kpm |   ckpm |   firsttower |   clouds |   oceans |     dpm |   damageshare |   wcpm |   totalgold |   earnedgold |   earned gpm |   earnedgoldshare |   goldspent |        gspd |    gpr |   total cs |     cspm |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |
|:----------------------|:-------------------|:--------------------------------------------|:---------|-------:|:--------|-----------:|:--------------------|-------:|:-------|:-----------|:------------------------------|-------------:|---------:|--------:|---------:|----------:|------------:|-------------:|-------------:|-----------:|-------:|-------------:|---------:|---------:|--------:|--------------:|-------:|------------:|-------------:|-------------:|------------------:|------------:|------------:|-------:|-----------:|---------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|
| ESPORTSTMNT01_2690210 | complete           | nan                                         | LCKC     |   2022 | Spring  |          0 | 2022-01-10 07:44:08 |      1 | Blue   | team       | Fredit BRION Challengers      |         1713 |        0 |       9 |       19 |        19 |           9 |           19 |            1 |     0.3152 | 0.9807 |            1 |        0 |        0 | 1981.09 |           nan | 1.7863 |       47070 |        28222 |      988.511 |               nan |       44570 | -0.0283123  |   0.94 |        nan |  29.4221 |      24806 |    28001 |      487 |          24699 |        29618 |          510 |            107 |        -1617 |          -23 |           5 |            10 |            6 |               6 |                18 |                5 |
| ESPORTSTMNT01_2690210 | complete           | nan                                         | LCKC     |   2022 | Spring  |          0 | 2022-01-10 07:44:08 |      1 | Red    | team       | Nongshim RedForce Challengers |         1713 |        1 |      19 |        9 |        62 |          19 |            9 |            0 |     0.6655 | 0.9807 |            0 |        0 |        0 | 2799.02 |           nan | 1.7863 |       52617 |        33769 |     1182.8   |               nan |       45850 |  0.0283123  |  -0.94 |        nan |  34.1856 |      24699 |    29618 |      510 |          24806 |        28001 |          487 |           -107 |         1617 |           23 |           6 |            18 |            5 |               5 |                10 |                6 |
| ESPORTSTMNT01_2690219 | complete           | nan                                         | LCKC     |   2022 | Spring  |          0 | 2022-01-10 08:38:24 |      1 | Blue   | team       | T1 Challengers                |         2114 |        0 |       3 |       16 |         7 |           3 |           16 |            0 |     0.0851 | 0.5393 |            0 |        0 |        0 | 1690.98 |           nan | 1.561  |       57629 |        34688 |      984.522 |               nan |       53945 | -0.207137   |  -3.23 |        nan |  34.3141 |      23522 |    28848 |      533 |          25285 |        29754 |          555 |          -1763 |         -906 |          -22 |           1 |             1 |            3 |               3 |                 3 |                1 |
| ESPORTSTMNT01_2690219 | complete           | nan                                         | LCKC     |   2022 | Spring  |          0 | 2022-01-10 08:38:24 |      1 | Red    | team       | Liiv SANDBOX Challengers      |         2114 |        1 |      16 |        3 |        39 |          16 |            3 |            1 |     0.4541 | 0.5393 |            1 |        1 |        0 | 2124.55 |           nan | 1.9868 |       71004 |        48063 |     1364.13  |               nan |       66410 |  0.207137   |   3.23 |        nan |  35.6764 |      25285 |    29754 |      555 |          23522 |        28848 |          533 |           1763 |          906 |           22 |           3 |             3 |            1 |               1 |                 1 |                3 |
| 8401-8401_game_1      | partial            | https://lpl.qq.com/es/stats.shtml?bmid=8401 | LPL      |   2022 | Spring  |          0 | 2022-01-10 09:24:26 |      1 | Blue   | team       | Oh My God                     |         1365 |        1 |      13 |        6 |        35 |          13 |            6 |            0 |     0.5714 | 0.8352 |          nan |      nan |      nan | 1762.02 |           nan | 1.4505 |       45468 |        30167 |     1326.02  |               nan |       36908 | -0.00586225 | nan    |        nan | nan      |        nan |      nan |      nan |            nan |          nan |          nan |            nan |          nan |          nan |         nan |           nan |          nan |             nan |               nan |              nan |

### Univariate Analysis

### Bivariate Analysis
### Interesting Aggregates
