College Football Regression
================
Blake Behrens
2026-03-03

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

``` r
hfa.df = scores[scores$FBS == 1,c("home","away","margin","neutral")]
hfa.df$homegame = 1 - hfa.df$neutral
hfa.df$neutral = NULL
N = nrow(hfa.df)
teams = unique(hfa.df$home)
nteams = length(teams)
home = hfa.df$home
away = hfa.df$away

X = matrix(0, nrow = N, ncol = nteams+1) # +1 for adding homegame parameter
i = 2
for(team in teams){
X[,i] = (home == team) -
  (away == team)
i = i + 1
}
X[,1] = hfa.df$homegame

y = hfa.df$margin
hfa.model = lm(y ~ X + 0)
b = coef(hfa.model)[-1]
b = b - mean(b)
H = coef(hfa.model)[1]
hfa.ranks = data.frame("Team" = teams, "Strength" = b, row.names = 1:nteams)
head(arrange(hfa.ranks,-Strength),10)
```

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

``` r
hfa.evaluation = postszn %>% 
  mutate(margin = h.score - a.score) %>% 
  left_join(hfa.ranks, by = join_by(home == Team)) %>% 
  dplyr::select(home,away,margin,neutral,homestrength = Strength) %>% 
  left_join(hfa.ranks, by = join_by(away == Team)) %>% 
  dplyr::select(home,away,margin,neutral,homestrength, awaystrength = Strength) %>% 
  mutate(homegame = 1- neutral,
         pred = homestrength - awaystrength + (H * homegame)) %>% 
  filter(!is.na(homestrength))

accuracy.h = mean(sign(hfa.evaluation$margin) == sign(hfa.evaluation$pred))
MSE.h = mean((hfa.evaluation$margin - hfa.evaluation$pred)^2)
r2.h = cor(hfa.evaluation$margin, hfa.evaluation$pred)^2
n.h = nrow(hfa.df)
p.h = length(hfa.model$coefficients)
rss.h = sum(hfa.model$residuals)
AIC.h <- n.h * log(rss.h / n.h) + 2 * p.h
BIC.h <- n.h * log(rss.h / n.h) + p.h * log(n.h)
data.frame("Basic_Model" = c(accuracy,MSE,r2,AIC,BIC),
           "Home_Field_Model" = c(accuracy.h,MSE.h,r2.h,AIC.h,BIC.h))
```

    ##    Basic_Model Home_Field_Model
    ## 1    0.6000000        0.5333333
    ## 2  206.7454302      209.3698378
    ## 3    0.2326878        0.2187154
    ## 4 1067.3772498    -2150.0565030
    ## 5 1691.4466536    -1521.3643629

To my surprise, adding the term for a “home field advantage” reduces
model accuracy and increases the mean squared error. My assumption is
that since the model is trained on 99.1% of non-neutral games, the
estimate for the home field advantage parameter is biased and does not
perform well on postseason games where only 10.8% of these are played at
home. I will note that of the 5 post season games played with a home
field advantage, this model correctly picked the winner 4 times, with
mean squared error of 94.5. In summary, a model like this would perhaps
perform better in NFL post-season prediction where only 1 game (Super
Bowl) can be played at a neutral site.
