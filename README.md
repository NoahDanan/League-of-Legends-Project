# League-of-Legends-Project
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
