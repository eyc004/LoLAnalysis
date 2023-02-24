# League of Legends Analysis
## Introduction
League of Legends is a online video game developed by Riot Games. This dataset, composed of 149232 rows and 123 columns, holds match information such as game ID, player names, date, and much more for several leagues in 2022. Using this data, we want to find an answer to the question, "For the LCK CL league, which role typically performs better in the team: Bot Lanes (ADCs) or Mid Lanes?" To answer this question, we want to focus on the kills and deaths columns. These columns describe the number of kills and deaths a specific player had in a specific game. We will be utilizing the kills and deaths columns as a measurement to determine which role performs "better" by calculating their K/D (kill/death) ratio and comparing this ratio. With this analysis, we will be able to determine which roles in the LCK CL are stronger and offer insights to where they might be able to improve in the future. 

Plus, throughout this project, Greg will teach Eric about League. 

## Cleaning and EDA (Exploratory Data Analysis)
Since our question revolved around the kill/death ratio between bots and mids for LCK CL, we had to first query and filter the dataset to keep only the rows whose "league" column was equal to LCK CL and save the DataFrame to a variable called "lck_cl". Next, we examined the kills and deaths columns in lck_cl to see if there were any NaN values. Fortunately, there were no null values in either of the kills or deaths columsn in lck_cl which made it a lot easier for us to compute the kill/death ratio.
 
<iframe src="assets/killsvsdeaths-bots.html" width=800 height=600 frameBorder=0></iframe>



## Assessment of Missingness
## Hypothesis
