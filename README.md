# ML_bootcamp_17_1
## This is my solution for machine learning competition run by Mail.ru in Q1 2017.

More info (in Russian) can be found here:
http://mlbootcamp.ru/sandbox/10/

In this competition I predicted will player drop the online game or not based on his game stats.
All stats are calculated during last two weeks.

There are 12 features in this dataset:
- maxPlayerLevel - max level that player won
- numberOfAttemptedLevels - number of levels that player tried
- attemptsOnTheHighestLevel - number of attempts to win highest level
- totalNumOfAttempts - overall number of attempts
- averageNumOfTurnsPerCompletedLevel - mean number of turns on completed levels
- doReturnOnLowerLevels - does player retried levels that were already complete?
- numberOfBoostersUsed - total number of used boosters
- fractionOfUsefullBoosters - fraction of busters which led to level completion
- totalScore - total score during all gaming period
- totalBonusScore - total bonus score during all gaming period
- totalStarsCount - total number of stars during all gaming period
- numberOfDaysActuallyPlayed - number of days when player played the game

We have two datasets with 25289 players each, one for training and one for validation.
Test dataset is split in two parts: 40% is used for public leaderboard and 60% is used for final results calculation.

Metric was logloss.


### First impression
From the beginning this competition seemed a bit boring for me: many features are correlating with each other. It's really hard to do feature engineering here, because I don't see any difference between "score", “bonus score" and “stars”, features don't make much sense for me. No categorical features at all, nowhere to find additional data. 
During solving process I realized that this is amazing opportunity to sharpen my skills in many fields: 
- working with only "numbers" which don't mean much for me
- fine tune algorithms
- get stable solution without overfitting based not only on test dataset results, but also on my own common sense
- stacking/boosting/bagging algorithms


### During the contest I found out:
That linear algorithms performed pretty well on cross validation but poor on leaderboard. Tree ensemble algorithms were more stable in this case (I think because boosting is reducing variance and much more robust to overfitting).

There is always a way to generate new features which will improve score:
- cross dividing features improved both tree and linear algorithm’s score
- cross subtraction features improved tree based algorithm’s performance
- clustering values with non linear clusters size improved linear algorithm’s performance

Dropping weak features was very important to speed up learning process and also improved tree algorithm’s score.

Cross validation with two folds made scoring very close to leaderboard. It’s still a mystery for me, the only guess I have is that in this case train/test size ratio is very close to train/leaderboard dataset sizes.

During visual data analysis I found out that there are a lot of strange users, who is on level 0 but have bunch of score and stars. First I thought that this score was given by default during registration and tried to subtract it from scores, to understand how much score players gain and lose during two weeks of gaming. I suggested that it should improve linear algorithm’s performance, bit it didn’t. After competition end I found out reason for this.

First time I spend a lot of time tuning XGB parameters to get better result. This made a huge difference and I moved from 80 place to 38.

Before competition end my position was bottom of top 10% (52 out of 614).


### After competition end:
My submission dropped to 81-th place and now is only in top 15%.

I found out why there were level 0 players with a lot of scores. In competition chat organizers mentioned that this players did not entered game during two weeks period and their maximum level could be above (and is) 0.

Top of the leader board (top 20 solutions) changed dramatically and person who was on first place dropped below top 30%.

During reading winners’ blogs I found out that all winners used models ensembles and stacking heavily. Which I didn’t because when I tried, leaderboard score dropped. Trust your common sense, not only public leaderboard score!

This is a great experience for me. I totally got out of my comfort zone with this “only numbers” dataset and learned a lot about tuning model parameters, feature engineering without feature understanding and also about model stacking and voting methods.