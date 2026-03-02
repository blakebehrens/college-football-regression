College Football Regression
================
Blake Behrens
2026-03-02

# Linear Regression in College Football Prediction

The use of linear regression models in sports prediction is well
documented and proved as a consistent and overall reliable model. The
goal of this project is to analyze which predictors do best in
forecasting the margin of scores between home team and away team. For
this research, the regular season data will be built to train the model
which will then be evaluated on playoff and bowl games (2025 season
only).

## Basic Model

The first, most basic model to be fit is a multiple linear regression
where observed score margin is explained by a difference in 2 team
strengths.

``` math
M_{ij} = S_i - S_j + \epsilon_{ij} 
```
Where:

- $`M_{ij}`$ is the observed scoring margin for team *i* playing at home
  against team *j*

- $`S_i,S_j`$ are strengths of team *i* and *j* respectively

- $`\epsilon_{ij}`$ is the unobserved, mean-zero random error for the
  game between *i* and *j*

### Rankings:

Below is this model’s top 25 rankings, with the strength estimate
representing the expected margin of victory against a true average team.

    ##    Team Strength
    ## 1    IU 37.92457
    ## 2   OSU 33.71724
    ## 3   TTU 32.69825
    ## 4   ORE 31.29676
    ## 5    ND 30.57864
    ## 6   MIA 26.28002
    ## 7  UTAH 25.73853
    ## 8   UGA 23.52089
    ## 9   USC 22.55699
    ## 10  VAN 22.17273
    ## 11  ALA 22.01848
    ## 12 IOWA 22.01228
    ## 13 TA&M 21.55047
    ## 14 MISS 20.93584
    ## 15   OU 20.42450
    ## 16  BYU 18.83701
    ## 17 WASH 18.82568
    ## 18  PSU 17.88027
    ## 19 MICH 17.79704
    ## 20  TEX 16.21278
    ## 21 ARIZ 15.54874
    ## 22  MIZ 15.42863
    ## 23  USF 14.79978
    ## 24  ILL 13.96123
    ## 25 TENN 13.92771

### Playoff Accuracy

    ## Basic Model:  
    ##  Accuracy: 0.6 
    ##  MSE: 206.7454 
    ##  R-Sq. : 0.2326878

Now that we have a baseline for postseason prediction
