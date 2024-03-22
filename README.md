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
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>gameid</th>
      <th>league</th>
      <th>result</th>
      <th>kills</th>
      <th>totalgold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>ESPORTSTMNT01_2690210</td>
      <td>LCKC</td>
      <td>False</td>
      <td>9</td>
      <td>47070</td>
    </tr>
    <tr>
      <td>ESPORTSTMNT01_2690210</td>
      <td>LCKC</td>
      <td>True</td>
      <td>19</td>
      <td>52617</td>
    </tr>
    <tr>
      <td>ESPORTSTMNT01_2690219</td>
      <td>LCKC</td>
      <td>False</td>
      <td>3</td>
      <td>57629</td>
    </tr>
    <tr>
      <td>ESPORTSTMNT01_2690219</td>
      <td>LCKC</td>
      <td>True</td>
      <td>16</td>
      <td>71004</td>
    </tr>
    <tr>
      <td>8401-8401_game_1</td>
      <td>LPL</td>
      <td>True</td>
      <td>13</td>
      <td>45468</td>
    </tr>
  </tbody>
</table>
### Univariate Analysis

<iframe
  src="assets/golddiffat15.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/killsat15.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
As we can see in both of the above graphs, for golddiffat15, the data is normally distributed for both winners and loser with the losers being shifted more towards the left end and the winners being shifted more towards the right end. The graph for killsat15 has a moderate right skew with the kill counts trailing a bit farther for the winners than the losers. This means that on average, the winners of each match tend to have more kills at the 15 minutes mark. 

### Bivariate Analysis

<iframe
  src="assets/bigolddiffat15.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/bikillvsdeathat15.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Looking at the boxplot above we can see that for the losers (false) the plot is similar to the winners (true), except it is shifted down and the tail end of the majority of the outliers are skewed towards the x-axis where as it is the opposite case for the winners box plot.

### Interesting Aggregates
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">assists</th>
      <th colspan="2" halign="left">deaths</th>
      <th colspan="2" halign="left">golddiffat15</th>
      <th colspan="2" halign="left">kills</th>
    </tr>
    <tr>
      <th>year</th>
      <th>2022</th>
      <th>2023</th>
      <th>2022</th>
      <th>2023</th>
      <th>2022</th>
      <th>2023</th>
      <th>2022</th>
      <th>2023</th>
    </tr>
    <tr>
      <th>league</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CBLOL</th>
      <td>33.86</td>
      <td>0.00</td>
      <td>14.26</td>
      <td>0.00</td>
      <td>0</td>
      <td>0</td>
      <td>14.24</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>CBLOLA</th>
      <td>34.84</td>
      <td>0.00</td>
      <td>14.60</td>
      <td>0.00</td>
      <td>0</td>
      <td>0</td>
      <td>14.58</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>CDF</th>
      <td>39.19</td>
      <td>0.00</td>
      <td>18.64</td>
      <td>0.00</td>
      <td>0</td>
      <td>0</td>
      <td>18.61</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>CT</th>
      <td>33.77</td>
      <td>0.00</td>
      <td>16.27</td>
      <td>0.00</td>
      <td>0</td>
      <td>0</td>
      <td>16.17</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>DCup</th>
      <td>32.49</td>
      <td>0.00</td>
      <td>14.68</td>
      <td>0.00</td>
      <td>0</td>
      <td>0</td>
      <td>14.64</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>

From the data above we are able to see what level these different leagues tend to perform given their main stats. For the most part, there aren't any outliears even when looking at the full table.

## Asssessment of Missingness
### NMAR Analysis
For the Not Missing as Random (NMAR) analysis, upon examining the missingness of all the columns I believe the URL column's missingness is NMAR mostly because it was missing at a rate just shy of 85% which caught my attention. I then procede to test for missingness dependency on a few columns one of which being URL as seen below.
### Missingness Dependency

