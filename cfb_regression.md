College Football Regression
================
Blake Behrens
2026-03-08

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

Below is this model’s top 10 rankings, with the strength estimate
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

### Playoff Accuracy

    ## Basic Model:  
    ##  Accuracy: 0.6 
    ##  MSE: 206.7454 
    ##  R-Sq. : 0.2326878 
    ##  AIC: 1067.377 
    ##  BIC: 1691.447

Now that there is a baseline for postseason prediction, I will construct
some higher order models. I will incorporate various indicator variables
referencing states about the game. This includes site, AP rank, and
more.

## Model 2: Home Field Advantage

The first additional parameter I will investigate is adding an indicator
for home field versus neutral site games. This will use the same model
as before,

``` math
M_{ij} = H + S_i - S_j + \epsilon_{ij} 
```

but with the added $`H`$ term representing the effect of a game being
played on a non-neutral site.

    ##    Team Strength
    ## 1    IU 37.24405
    ## 2   OSU 32.36028
    ## 3   TTU 31.89195
    ## 4   ORE 29.99413
    ## 5    ND 29.27365
    ## 6  UTAH 25.53838
    ## 7   MIA 24.52197
    ## 8   UGA 22.44637
    ## 9   USC 21.31213
    ## 10  VAN 20.99708

As expected, since most games are not played on a neutral site, the
estimated team strengths did not change much, and the top 10 remained
the same. Hopefully, adding the term will help reduce our error in
margin prediction, as we can expect it won’t change the accuracy by
much.

### Playoff Accuracy

    ##            Basic_Model Home_Field_Model
    ## Accuracy     0.6000000        0.5333333
    ## MSE        206.7454302      209.3698378
    ## R-Squared    0.2326878        0.2187154
    ## AIC       1067.3772498    -2150.0565030
    ## BIC       1691.4466536    -1521.3643629

To my surprise, adding the term for a “home field advantage” reduces
model accuracy and increases the mean squared error. My assumption is
that since the model is trained on 99.1% of non-neutral games, the
estimate for the home field advantage parameter is biased. Therefore, it
does not perform well on post-season games where only 10.8% are played
at home. I will also note that of the 5 post season games played with a
home field advantage, this model correctly picked the winner 4 times,
with mean squared error of 94.5. In summary, a model like this would
perhaps perform better in NFL post-season prediction where only 1 game
(Super Bowl) can be played at a neutral site.

## Model 3: AP Ranks as factors

Now, I will investigate how AP rankings can assist the linear model. I
will no longer be including the home field parameter model. Further,
since AP ranks teams 1-25 and therefore most teams don’t have an
assigned rank, I will instead use indicators that say whether or not AP
ranked that team:

``` math
M_{ij} = S_i - S_j + [I(AP_i) *A_h] -[I(AP_j)*A_a] + \epsilon_{ij}
```

Where:

- $`I(\text{AP}_i)`$ and $`I(\text{AP}_j)`$ are indicator functions of
  whether team *i* and *j* are ranked, respectively

- $`A_h,A_a`$ are the effects on the margin of victory from a home or
  away team, respectively, being ranked

<!-- -->

    ##    Team Strength
    ## 1    IU 43.38999
    ## 2   OSU 39.63486
    ## 3   TTU 37.26240
    ## 4    ND 36.36653
    ## 5   ORE 34.45597
    ## 6   MIA 29.92305
    ## 7   ALA 27.37165
    ## 8  TA&M 27.19228
    ## 9  UTAH 26.93076
    ## 10  UGA 26.63548

Here we notice that the top 10 is different (excluding the top 3 teams,
Indiana, Ohio State, and Texas Tech). It appears AP rank had more impact
on the parameter estimation than including home field advantage did.

    ##            Basic_Model     AP_Model
    ## Accuracy     0.6000000 5.434783e-01
    ## MSE        206.7454302 2.677898e+02
    ## R-Squared    0.2326878 8.587117e-02
    ## AIC       1067.3772498 8.990160e+02
    ## BIC       1691.4466536 1.569314e+03
