# Report-of-2022-LOL-games

Eric Chen, Ruijia Xiao

## Introduction

The dataset of the League of Legends 2022 Competitive Matches records all the matches took place in Season 2022 from Year 2022 to 2023. It records the statistics of competitions from 49 leagues, 595 teams. In our report, we study the relationship between team KDR, the ratio between team’s total kills and team’s total death, and playoffs. This is a meaningful question to ask because playoffs are the most popular contests between the strongest teams within each leagues, and players are so eager to win in order to get to tickets to the World Championship. In that case, the performances of each team matter a lot. There are 149400 rows in the datasets, and we have selected out 5 columns “datacompleteness”, “teamname”, “playoffs”, “teamkills” , “teamdeaths”.

## Cleaning and EDA

### Data cleaning

Firstly, we clean the teamname column by setting the Nan value to 'MISSING'. Then, we find out that the playoffs column is not bool, so we convert it to boolean to make it reasonable. And our exploration question requires KDR of the teams in each matches, which are not included in the dataset, so we made a new column of KDR. In this process, the problem is that some teams have 0 death in one match, which can cause the original formula: teamkills/teamdeath have infinity. So we did some research and find out that when calculating KDR, people use 1 death to represent 0 death, so we defined a new function that convert 0 to 1 death when calculating KDR. At last, we grab the column we want from the dataset: 'datacompleteness','teamname','playoffs', 'teamkills','teamdeaths','KDR'.

Here is the head of the dataset after our cleaning:

|    | datacompleteness | teamname                      | playoffs | teamkills | teamdeaths | KDR    |
| -- | :--------------: | :---------------------------: | :------: | :-------: | :--------: | :----: |
| 10 | complete         | Fredit BRION Challengers      | False    |9          |19          |0.473684|
| 11 | complete         | Nongshim RedForce Challengers | False    |19         |9           |2.111111|
| 22 | complete         | T1 Challengers                | False    |3          |16          |0.187500|
| 23 | complete         | Liiv SANDBOX Challengers      | False    |16         |3           |5.333333|
| 34 | complete         | Oh My God                     | False    |13         |6           |2.166667|

### Univariate Analysis

<iframe src="assets/G1.html" width=800 height=600 frameBorder=0></iframe>

The Teamkills distribution is skewed to right. Most values are concentrated within the interval from 5 to 30, and have a mean and median between 10 to 20.

<iframe src="assets/G2.html" width=800 height=600 frameBorder=0></iframe>


The Teamdeaths distribution looks very similar to Teamkills distribution. Most values are concentrated within the interval from 5 to 30, and have a mean and median between 10 to 20.

<iframe src="assets/G3.html" width=800 height=600 frameBorder=0></iframe>

This pie plot shows that the distribution of playoffs in all matches, around 17.1% of the matches are playoffs, and the rest 82.9% are non-playoff matches.

<iframe src="assets/G4.html" width=800 height=600 frameBorder=0></iframe>

The KDR (the ratio between Teamkills and Teamdeaths) distribution is skewed to right. Most teams’ KDR are between 0 to 5. 

### Bivariate Analysis

<iframe src="assets/G5.html" width=800 height=600 frameBorder=0></iframe>

This scatter plot shows the relationship between Teamkills and Teamdeaths. Most values are between 0 to 40.


<iframe src="assets/G6.html" width=800 height=600 frameBorder=0></iframe>

This scatter plot shows the relationship between KDR and playoffs. 

### Interesting Aggregates

| playoffs| False    | True     |
| KDR     | 1.731432 | 1.710219 |

This is the pivot table of the mean KDR grouped by playoffs.

## Assessment of Missingness

### NMAR Analysis

Among the column we selected, we believe there is no column that has a NMAR missingness type. In those selected columns, teamname is the only column has missing values. After we analyzing the match time, we tend to believe that it is unlikely to be the same team that has lost the data because the time between two lost data is too close. And When diving into the other columns, we found out that all of the missing teams came from PRM league, which is not a famous league in the world. So we guess the possible reason why it is missing is because that league does not have a complete and well-preserved database that might cause the lost of teamname data. Therefore, there is no NMAR missingness type in the selected colums.

### Missingness Dependency

Among the column we selected, teamname is the only column has missing values. Firsty, after analyzing the dataframe, we can determine that this missing type cannot be missing by design. Then, based on the illustration above, we know that this column is not NMAR. So for the next step, we want to check if it is MAR. To check this hypothesis, among the columns we selected, we present the question that if the teamname is depend on playoff. So the null hypothesis we have is "The missing of teamname is not depend on the playoffs", and the Alt hypothesis is "The missing of teamname is depend on the playoffs". After we performing our permutation testing, the p value we got is 0.0, which is smaller 0.05. Therefore, we reject the null hypothesis and we tend to believe that its missingness type is MAR.

<iframe src="assets/G7.html" width=800 height=600 frameBorder=0></iframe>

This histogram plot shows the distribution of playoff when teamname is missing.


<iframe src="assets/G8.html" width=800 height=600 frameBorder=0></iframe>

This histogram plot shows the distribution of playoff when teamname is not missing .


<iframe src="assets/G10.html" width=800 height=600 frameBorder=0></iframe>

This histogram plot shows the distribution of difference in mean in permutation test.

The observed result is 0.17122654774818208.

And the p-value in this permutation test we have is 0.0, so we rejct the null hypothesis.

## Hypothesis Testing

In this part of the report, we want to test on the exploration question we have: does the KDR affected by playoffs? 

The null hypothesis: there is no relationship between KDR and playoffs. 
The alternate hypothesis: in playoffs teams tend to have different KDR than those in non-playoff matches. 

Firstly, we compute the difference in absolute mean KDR between the games in playoffs and the game not in playoffs. Tnen we perform a permutation test that loops 1000 times to shuffle the KDR and find the absolute mean to check if the new result is larger than the observed one. 

After testing, we found p-value is 0.57, which is larger than 0.05, so that we fail to reject our null hypothesis. Therefore, we tend to believe that there is no relationship between KDR and playoffs.

For the choosing of the significant value, 0.05 and 0.01 are both reasonable choice. That is because in this test, p value is much more larger than those two values, so it would not make a difference to the test result.

<iframe src="assets/G11.html" width=800 height=600 frameBorder=0></iframe>

This histogram plot shows the distribution of difference in mean in permutation test.