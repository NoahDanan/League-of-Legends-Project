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
#### NMAR Analysis Graph
<iframe
  src="assets/golddiffat15_on_url.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
#### Dependency on 'league' (NMAR Example):
P-value: 0.0 (or extremely close to zero)
Interpretation: There's a statistically significant association between the missingness of 'url' data and the 'league' category. This suggests that the likelihood of 'url' data being missing is not uniform across leagues and could vary significantly from one league to another. This might reflect differences in data collection practices, the availability of resources, or the priority given to online content in different leagues.
#### Dependency on 'oceans' (MAR Example):
P-value: 0.9010854527992864
Interpretation: Unlike 'league' and 'year', the association between 'url' missingness and the 'oceans' variable is not statistically significant, given the high p-value. This suggests that the presence or absence of 'url' data does not depend on the 'oceans' variable within your dataset. The missingness of 'url' data appears to be independent of the 'oceans' column, indicating no systemic pattern linking these two.

## Hypothesis Testing
Null Hypothesis: There is no difference in the mean golddiffat15 (will test other stats as well) between teams from the LCK and LEC regions, suggesting similar skill levels.
Alternative Hypothesis: There is a difference in the mean golddiffat15 between teams from the LCK and LEC regions, suggesting differing skill levels.

For my testing, I used a permutation test with a difference in means as the statistic. There is also an assumed significance level of 0.05 which is typically standard. 

### Conclusion for golddiffat15
Given the p-value of 1.0, we fail to reject the null hypothesis. This means that there is no statistical evidence to suggest a difference in the mean golddiffat15 between teams from the LCK and LEC regions based on the data analyzed. It's important to note that this does not prove the null hypothesis is true; rather, it indicates that, according to this specific measure of early game performance, the skill levels between these two regions cannot be distinguished.

### Conclusion for DPM (Damage Per Minute)
Given the p-value of 0.0, we reject the null hypothesis that there is no difference in the mean dpm between teams from the LCK and LEC regions. There is statistically significant evidence to suggest a difference, implying that the skill levels or gameplay strategies related to damage output may differ between these regions. The negative observed difference in means further suggests that LEC teams, on average, have a higher dpm compared to LCK teams.

### Conclusion for deathsat15
Given the p-value of 0.0001, we reject the null hypothesis that there is no difference in the mean deathsat15 between teams from the LCK and LEC regions. There is statistically significant evidence to suggest a difference in early game fatalities between these regions, implying that the gameplay strategies, risk tolerance, or player behaviors might differ significantly between LCK and LEC teams. Specifically, the negative observed difference suggests that LEC teams, on average, manage to secure safer early game conditions leading to fewer deaths compared to LCK teams.

## Framing a Prediction Problem
### Problem Identification
Objective: To predict the outcome of a LoL game (win or lose) for a team based on early-game metrics and possibly other relevant features.
Type: Binary classification
Repsonse Variable: 'result' column as it determines who won the match (most simple determinant of skill)
Metric: F1 score (balance between recall and accuracy)

## Baseline Model
### Description
The model constructed is a logistic regression classifier, designed to predict the outcome of a match (win or lose) based on early game performance indicators in League of Legends matches. The logistic regression model is suitable for binary classification tasks like this, where the target variable result is binary (1 for win, 0 for lose).
### Features in Model
golddiffat15 (Quantitative)
: This feature represents the gold difference between the two teams at 15 minutes into the game. It is a continuous variable, providing a numeric measure of the early game advantage (or disadvantage) in terms of in-game economy. Since it is a numeric variable, it was standardized using StandardScaler to have a mean of 0 and a standard deviation of 1. Standardization helps in improving the model's convergence during training, especially for logistic regression, by ensuring all numeric features contribute equally to the model's decision process.
firstblood (Nominal)
: This binary categorical feature indicates whether the team secured the first kill of the match. Although it technically has two levels (True or False), it does not have an inherent order, making it a nominal variable. It was encoded using OneHotEncoder, which transforms this categorical variable into a format that can be provided to machine learning algorithms. One-hot encoding is essential for including categorical data in the model, as it converts categorical data into a numeric form without implying any ordinal relationship between categories.
### Performance
The performance of the model is evaluated using the F1 Score, which is a harmonic mean of precision and recall. The F1 Score for this model is approximately 0.714, indicating a balance between precision and recall. This score suggests that the model is reasonably effective in identifying wins based on the provided features but leaves room for improvement. Although the model has a moderately good F1 Score, it has too few features and hasn't been optimized enough to reach an ideal F1 Score.
### Graph
<iframe
  src="assets/baselinegraph.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
