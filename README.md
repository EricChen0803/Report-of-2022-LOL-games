# Report-of-2022-LOL-games

Eric Chen, Ruijia Xiao

## Introduction

The dataset of the League of Legends 2022 Competitive Matches records all the matches took place in Season 2022 from Year 2022 to 2023. It records the statistics of competitions from 49 leagues, 595 teams. In our report, we study the relationship between team KDR, the ratio between team’s total kills and team’s total death, and playoffs. This is meaningful question to ask because playoffs are the most popular contests between the strongest teams within each leagues, and players are so eager to win in order to get to tickets to the World Championship. In that case, the performances of each team matter a lot. There are 149400 rows in the datasets, and we have selected out 5 columns “datacompleteness”, “teamname”, “playoffs”, “teamkills” , “teamdeaths”.

## Cleaning and EDA

Firstly, we clean the teamname column by setting the Nan value to 'MISSING'. Then, we find out that the playoffs column is not bool, so we convert it to boolean to make it reasonable. And our exploration question requires KDR of the teams in each matches, which are not concluded in the dataset, so we made a new column of KDR. In this process, the problem is that some teams have 0 death in one match, which can cause the original formula: teamkills/teamdeath have infinity. So we did some research and find out that when calculating KDR, people use 1 death to represent 0 death, so we defined a new function that convert 0 to 1 death when calculating KDR. At last, we grab the column we want from the dataset: 'datacompleteness','teamname','playoffs', 'teamkills','teamdeaths','KDR'.