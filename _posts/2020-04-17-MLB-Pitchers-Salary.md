---
layout: post
title: Simple Linear Regression on MLB Pitchers' Salary
---

This is my second project with Metis Data Science bootcamp.
I did a simple linear regression analysis (with LASSO regularization) to examine the relationship of an MLB pitcher's salary vs the previous year's stat.
Here is an [insightful website on Linear Regression](https://www.analyticsvidhya.com/blog/2017/06/a-comprehensive-guide-for-linear-ridge-and-lasso-regression/).

## Contents

* Motivation
* Data
* Methodology
* Results
* Future Work


## Motivation

Which baseball stats influence the next year's salary for MLB pitchers the most?


## Data: From Web Scraping using BeautifulSoup

* Data: I used BeautifulSoup to capture 2018 Stats and 2019 salary data from the following websites:

[2018 Stats](https://www.foxsports.com/mlb/stats?season=2018&category=PITCHING&group=1&sort=2&time=0&pos=0&qual=1&sortOrder=0&splitType=0&page=1&statID=0)

[2019 Salary](https://www.usatoday.com/sports/mlb/salaries/)

![Image test]({{ site.url }}/images/AlanLeeShireGandalf.JPG)

Ater merging two dataset, I got 360 samples 

## Methodology

I used EDA and Linear Regressions.

### Summary Statistics

* Top Earners

Even a simple observation on top earners can give meaningful insights. For example, Max Scherzer earned the most among all MLB players including all pitchers with $42 MM compensation in 2019. He indeed has stellar stats: Among the top 10 earners, he pitched second most games (33), won the most (tied with another pitcher), has the most quality starts (throwing at least 6 innings with giving out not more than 3 earned runs), and also pitched the most inning (220). His ERA was also the second best, only 0.01 behind the best one among top 10 pitchers. In general, most of the top 10 pitchers had strong stats. Interestinly, all of them were starters.

![Summary Stat1]({{ site.url }}/images/AlanLeeShireGandalf.JPG)

* Distribution of Salary

The distribution of the salaries for the sample pitchers were highly skewed right. For example, while the mean was $4.4 MM, the median was only $1.4 MM. While someoneneed to earn $22 MM to be in top 10, or $11 MM to be in top 40, the bottom 25% (90) earned less than $600K. Max Scherzer earned the most ($42 MM), while the minimnum salary for 2019 was $555K.

* OLS results using Statemodels

I scaled the dependent variable Y to be lgoSalary = log_10(Salary/555000). Then I did a traditional OLS regression on all 360 samples with the following features:

G: Number of games played

W: Number of wins

L: Numher of losses

WPct: Percentage of wins

CG: Completed games

SHO: Shut out (no runs given)

QS: Quality start (giving out not more than 3 earned runs for at least 6 innings

SV: Saves

QSSV: QS * SV (interaction term)

IP: Innings pitched

BB: walks

ERA: Earned run average

STARTER: dummy variable for being a starter

Only G, QS and SV had statistically significant coefficients with R^2 0.33
They were all positively related with the logSalary.

Also, the error term looked random, meaning that the OLS fits well.

* LASSO regularization

I used LASSO regularization after standardizing the X's. The result was conssistent with the findings from OLS: G, QS, SV, IP and STARTER had positive coefficients while number of losses, walks and ERA had 0 coefficients with the optimal Lamdba. R^2 was 0.31.


## Results

I repeated the same analysis with subsets of data and found the following:

For starters, the Inning Pitched was the most statistically significant coefficient. This makes sense since baseball does not have time limit and every game needs at least 27 outs or more (in case the game is tied).

For relievers, the number of games pitched and the number of saves where the most statistically sighnificant coefficients. This also makes sense. For a reliever, he may throw very short innings at every game, but appearing to many games and saving many are more important than pure number of innings throwing. A good closer may throw maximum 1 inning (last inning) each time he appears but may throw less than 100 innnings per season (Typically, 100 wins would put the team to the top of its league).

Lastly, being a STARTER was very important for higher salary, especially for those whose salary was more than $600K, meaning that a junior (1-3rd year in the team) will not get paid much even if he is a starter, once someone is beyond that period, being a starter commands more money than being a reliever.


## Future Work

In fact, MLB player's salary is dictated by 'Major League Baseball Collective Bargaining Agreement' so it is not completly dependent on the players' performance in the previous year.
For the first 3 years in the league, the team determines how much they pay to the players, so that the salary is near the minimum $555K.
Then from year 4 to 6, both the player and the team can arbitrage the salary. It is only from year 7 that a player becomes a 'free agent' and can have a contract with any team who can pay the most. For more details, please refer this research paper titled [Determinants of Major League Baseball Player Salaries](https://surface.syr.edu/cgi/viewcontent.cgi?article=1098&context=honors_capstone)

Thank you for reading.





