College Football Regression
================
Blake Behrens
2026-03-02

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax
for authoring HTML, PDF, and MS Word documents. For more details on
using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that
includes both content as well as the output of any embedded R code
chunks within the document. You can embed an R code chunk like this:

``` r
summary(scores)
```

    ##      home               away               margin             week       
    ##  Length:887         Length:887         Min.   :-53.000   Min.   : 1.000  
    ##  Class :character   Class :character   1st Qu.: -6.000   1st Qu.: 3.000  
    ##  Mode  :character   Mode  :character   Median :  7.000   Median : 7.000  
    ##                                        Mean   :  9.098   Mean   : 7.153  
    ##                                        3rd Qu.: 24.000   3rd Qu.:11.000  
    ##                                        Max.   : 74.000   Max.   :15.000  
    ##                                                                          
    ##       site        FBS          home_conf          away_conf        
    ##  Min.   :1   Min.   :0.0000   Length:887         Length:887        
    ##  1st Qu.:1   1st Qu.:1.0000   Class :character   Class :character  
    ##  Median :1   Median :1.0000   Mode  :character   Mode  :character  
    ##  Mean   :1   Mean   :0.8478                                        
    ##  3rd Qu.:1   3rd Qu.:1.0000                                        
    ##  Max.   :1   Max.   :1.0000                                        
    ##                                                                    
    ##     home_AP         away_AP     
    ##  Min.   : 1.00   Min.   : 1.00  
    ##  1st Qu.: 7.00   1st Qu.: 7.50  
    ##  Median :14.00   Median :13.00  
    ##  Mean   :13.16   Mean   :13.33  
    ##  3rd Qu.:19.00   3rd Qu.:20.00  
    ##  Max.   :25.00   Max.   :25.00  
    ##  NA's   :705     NA's   :752

## Including Plots

You can also embed plots, for example:

![](cfb_regression_files/figure-gfm/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.
