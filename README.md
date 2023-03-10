# League of Legends Analysis
## Introduction
League of Legends is a online video game developed by Riot Games. This dataset, composed of 149232 rows and 123 columns, holds match information such as game ID, player names, date, and much more for several leagues in 2022. Using this data, we want to find an answer to the question, "For the LCK CL league, which role typically performs better in the team: Bot Lanes (ADCs) or Mid Lanes?" To answer this question, we want to focus on the kills and deaths columns. These columns describe the number of kills and deaths a specific player had in a specific game. We will be utilizing the kills and deaths columns as a measurement to determine which role performs "better" by calculating their K/D (kill/death) ratio and comparing this ratio. With this analysis, we will be able to determine which roles in the LCK CL are stronger and offer insights to where they might be able to improve in the future. 

Plus, throughout this project, Greg will teach Eric about League. 

## Cleaning and EDA (Exploratory Data Analysis)

### Data Cleaning
Since our question revolved around the kill/death ratio between bots and mids for LCK CL, we had to first query and filter the dataset to keep only the rows whose "league" column was equal to LCK CL and save the DataFrame to a variable called "lck_cl". Next, we examined the kills and deaths columns in lck_cl to see if there were any NaN values. Fortunately, there were no null values in either of the kills or deaths columns in lck_cl which made it a lot easier for us to compute the kill/death ratio. 

Afterwards, to group the data into the bots and mids categories, we grouped the lck_cl by the gameid, side, position because we wanted to record each time the mid and bot position was played. gameid keeps track of the specific game, side keeps track of the blue or red side, and position keeps track of the position (mid, bot, etc.). This means that for each game, there should be two mids and two tops since there is one position played per side. 

With the data all grouped properly, we then needed to only keep the columns we needed, which were the kills and deaths columns, as well as create two new DataFrames, one for just the bot rows and one for just the mid ros. To compute the kill/death ratio, we could simply divide the two kills and deaths columns of each dataframe. However, we discovered an issue with this because it is possible for a cetain position to have 0 deaths in a game, which would then lead to division by zero. 44But, we used our knowledge that to calculate the kill/death ratio, if someone dies zero times, then we calculate their kill/death ratio as if they died once. For example, if the mid on the blue side got 7 kills and died 0 times in a certain game, then we would say that their kill/death ratio was 7/1 = 7.0. We created a function to apply to the deaths columns that would turn any 0 to 1, which would allow for us to correctly calculate the kill/death ratio. Afterwards, we created a column to contain the kill/death ratios in both the bots DataFrame and mids DataFrame. We then merged these two DataFrames to create one master DataFrame which held the kill/death ratios for the bot and mid on each side. 

Finally, we wanted to calculate the difference between the two, so we decided to do mid kill/death ratio - bot kill/death ratio. 

The first few rows of our combined DataFrame are shown down below:


|                             index |   botskd |   midskd |   midminusbotdiff |
|:----------------------------------|---------:|---------:|------------------:|
| ('ESPORTSTMNT01_2690210', 'Blue') |      0.5 |      1   |               0.5 |
| ('ESPORTSTMNT01_2690210', 'Red')  |      4   |      2   |              -2   |
| ('ESPORTSTMNT01_2690219', 'Blue') |      0   |      0.5 |               0.5 |
| ('ESPORTSTMNT01_2690219', 'Red')  |      3   |      5   |               2   |
| ('ESPORTSTMNT01_2690227', 'Blue') |      1.5 |      5   |               3.5 |


### Univariate Analysis

<iframe src="assets/kdratiosdiff.html" width=800 height=600 frameBorder=0></iframe>
The plot presented above representing the difference between mid and bot lane kill death ratios is skewed to the left, meaning that we see more extreme values on the left side. What this means is that there are instances where the bot has a much higher kill/death ratio than the mid's kill/death ratio. And in general, it looks like bots have a higher kill/death ratio than mids. 


### Bivariate Analysis
<iframe src="assets/killsvsdeaths-bots.html" width=800 height=600 frameBorder=0></iframe>
When looking at this, we saw that the top 3 highest kill/death ratios were those who died fewer times. This makes sense because the denominator will change the ratio by a lot more, but also shows us a snowball effect, that as you get more kills, it's easier for you to get more kills in the feature. It can also relate to those who are more skilled, as those who are better at the game will get more skills and die less frequently. 

### Interesting Aggregates
|                                   |   blue k/d ratio |   red k/d ratio |   red_blue_kd_diff |
|:----------------------------------|-----------------:|----------------:|-------------------:|
| ('ESPORTSTMNT01_2690210', 'bot')  |         0.5      |         4       |            3.5     |
| ('ESPORTSTMNT01_2690210', 'jng')  |         0.4      |         4       |            3.6     |
| ('ESPORTSTMNT01_2690210', 'mid')  |         1        |         2       |            1       |
| ('ESPORTSTMNT01_2690210', 'sup')  |         0.2      |         0       |           -0.2     |
| ('ESPORTSTMNT01_2690210', 'team') |         0.473684 |         2.11111 |            1.63743 |
<iframe src="assets/pos-side-kd-diff.html" width=800 height=600 frameBorder=0></iframe>
We decided to look at the different between the groupings of the sides and teams and their distribution of differences between the kill/death ratios. One thing that Greg pointed out was that generally, people felt like being on the blue side was better. However, with this plot, we found that they were all roughly distributed, so we ruled that out. 

## Assessment of Missingness

### NMAR Analysis
We do not believe that there is a column that is NMAR in our dataset. Much of the League of Legends data from 2022 that we were given appeared to be missing by design MD because they chunked each data column by game and had overall for the team (in that specific game) after each row of players (10 players). One such example is in the ???damageshare??? column where each player was given a proportion corresponding to the damage they dealt relative to their team. The row for team total (in this column) is left empty for all of our data points. This additional data point would not tell us anything useful because it does not quite make sense to take the proportion of the total. It would not tell us anything useful. In addition, many of the columns that were not missing by design appeared to be fairly complete in their data, but to check for whether or not the data were missing at random (MAR) and missing completely at random (MCAR) we conducted two permutation tests. 

### Missingness Dependency
For the columns that missingness do depend on each other, our null hypothesis was there was not an association between ???league??? and the missingness of the column ???cspm??? (creep score per minute), and any variation would be due to random chance. The alternative hypothesis is that there is an association between ???league??? and the missingness of ???cspm???, and it is not due to random chance.
<iframe src="assets/missingnessdepends.html" width=800 height=600 frameBorder=0></iframe>
After doing 500 repetitions of permutation tests and calculating the total variation distance, we can support our alternative hypothesis that there is an association between the ???league??? and the missingness of ???cspm??? within the League of Legends dataset. With a p-value of 0.0, we reject the null hypothesis in favor of the alternative hypothesis. 

For the columns whose missingness does not depend on each other, our null hypothesis was there was not an association between ???cspm??? and the missingness of the column ???split???. The alternative hypothesis is that there is an association between ???cspm??? and the missingness of ???split???.
<iframe src="assets/missingnessdoesnotdepend.html" width=800 height=600 frameBorder=0></iframe>
After doing 500 repetitions of permutation tests and calculating the total variation distance, we determined we do not have significant enough evidence to support the alternative hypothesis that there is an association between the column ???cspm??? and the missingness in the column of ???split???. With a p-value of 1.0, we failed to reject the null hypothesis. 


## Hypothesis
Going back to our original question, "For the LCK CL league, which role typically performs better in the team: Bot Lanes (ADCs) or Mid Lanes?", our null hypothesis is that there is no significant difference between the kill/death ratios between Bot Lanes and Mid Lanes, and any difference is due to random chance. Our alternative hypothesis is that there Bot Lanes have a higher kill/death ratio than Mid Lanes, and this is not simply due to random chance. We chose to use these to carry out our work on supporting or rejecting our hypotheses because we needed a metric (kill/death ratio) to determine what constituted "performing" better. As we have the quantitative data, we want to use a permutation test to determine if the kill death ratios for both positions look like they were drawn from the same population, and any difference we see is just due to random chance. 

We will be using permutation tests to compute the mean difference between the Bot Kill/Death Ratio and Mid Kill/Death Ratio, which will be our test statistic. We are using a significance level of 0.05. 
<iframe src="assets/hypothesisplot.html" width=800 height=600 frameBorder=0></iframe>
After doing 1000 repetitions of permutation tests and calculating the mean difference between the Bot Kill/Death Ratio and Mid Kill/Death Ratio (our test statistic), we determined that we have evidence to reject the null hypothesis in favor of the alternative hypothesis as we calculated a p-value of 0.0. 