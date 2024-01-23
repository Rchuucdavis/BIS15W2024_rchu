---
title: "Lab 3 Homework"
author: "Ryan Chu"
date: "2024-01-22"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

### Mammals Sleep  
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R. The name of the data is `msleep`.  

```r
help(msleep)
```

```
## starting httpd help server ... done
```

2. Store these data into a new data frame `sleep`.  

```r
sleep <- (msleep)
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.
 The dimensions of this data frame are 83 observations by 11 variables. Using str(sleep) shows 11 different variables and 62 observed animals. 

```r
str(sleep)
```

```
## tibble [83 × 11] (S3: tbl_df/tbl/data.frame)
##  $ name        : chr [1:83] "Cheetah" "Owl monkey" "Mountain beaver" "Greater short-tailed shrew" ...
##  $ genus       : chr [1:83] "Acinonyx" "Aotus" "Aplodontia" "Blarina" ...
##  $ vore        : chr [1:83] "carni" "omni" "herbi" "omni" ...
##  $ order       : chr [1:83] "Carnivora" "Primates" "Rodentia" "Soricomorpha" ...
##  $ conservation: chr [1:83] "lc" NA "nt" "lc" ...
##  $ sleep_total : num [1:83] 12.1 17 14.4 14.9 4 14.4 8.7 7 10.1 3 ...
##  $ sleep_rem   : num [1:83] NA 1.8 2.4 2.3 0.7 2.2 1.4 NA 2.9 NA ...
##  $ sleep_cycle : num [1:83] NA NA NA 0.133 0.667 ...
##  $ awake       : num [1:83] 11.9 7 9.6 9.1 20 9.6 15.3 17 13.9 21 ...
##  $ brainwt     : num [1:83] NA 0.0155 NA 0.00029 0.423 NA NA NA 0.07 0.0982 ...
##  $ bodywt      : num [1:83] 50 0.48 1.35 0.019 600 ...
```

4. Are there any NAs in the data? How did you determine this? Please show your code. 
 Using str(sleep) shows that there are NAs in the data. 

```r
str(sleep)
```

```
## tibble [83 × 11] (S3: tbl_df/tbl/data.frame)
##  $ name        : chr [1:83] "Cheetah" "Owl monkey" "Mountain beaver" "Greater short-tailed shrew" ...
##  $ genus       : chr [1:83] "Acinonyx" "Aotus" "Aplodontia" "Blarina" ...
##  $ vore        : chr [1:83] "carni" "omni" "herbi" "omni" ...
##  $ order       : chr [1:83] "Carnivora" "Primates" "Rodentia" "Soricomorpha" ...
##  $ conservation: chr [1:83] "lc" NA "nt" "lc" ...
##  $ sleep_total : num [1:83] 12.1 17 14.4 14.9 4 14.4 8.7 7 10.1 3 ...
##  $ sleep_rem   : num [1:83] NA 1.8 2.4 2.3 0.7 2.2 1.4 NA 2.9 NA ...
##  $ sleep_cycle : num [1:83] NA NA NA 0.133 0.667 ...
##  $ awake       : num [1:83] 11.9 7 9.6 9.1 20 9.6 15.3 17 13.9 21 ...
##  $ brainwt     : num [1:83] NA 0.0155 NA 0.00029 0.423 NA NA NA 0.07 0.0982 ...
##  $ bodywt      : num [1:83] 50 0.48 1.35 0.019 600 ...
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data? 
 32 herbivores are represented in the data. 

```r
herbivores <- filter(sleep, vore=="herbi")
herbivores
```

```
## # A tibble: 32 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  2 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  3 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  4 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
##  5 Goat   Capri herbi Arti… lc                   5.3       0.6      NA      18.7
##  6 Guine… Cavis herbi Rode… domesticated         9.4       0.8       0.217  14.6
##  7 Chinc… Chin… herbi Rode… domesticated        12.5       1.5       0.117  11.5
##  8 Tree … Dend… herbi Hyra… lc                   5.3       0.5      NA      18.7
##  9 Asian… Elep… herbi Prob… en                   3.9      NA        NA      20.1
## 10 Horse  Equus herbi Peri… domesticated         2.9       0.6       1      21.1
## # ℹ 22 more rows
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 19kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
mammal_small <- filter(sleep, bodywt <= 19)
mammal_large <- filter(sleep, bodywt >= 200)
```

8. What is the mean weight for both the small and large mammals?

```r
mammal_weight_small <- mammal_small$bodywt
mean_weight_small <- mean(mammal_weight_small)
mean_weight_small
```

```
## [1] 1.797847
```


```r
mammal_weight_large <- mammal_large$bodywt
mean_weight_large <- mean(mammal_weight_large)
mean_weight_large
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  
 Based on the data, on average, smaller animals sleep longer than larger animals. 


```r
mammal_sleeptime_small <- mammal_small$sleep_total
mammal_sleeptime_large <- mammal_large$sleep_total
```

## Small Mammal Avg Sleep Time

```r
mean_sleeptime_small <- mean(mammal_sleeptime_small)
mean_sleeptime_small
```

```
## [1] 11.78644
```

## Large Mammal Avg Sleep Time

```r
mean_sleeptime_large <- mean(mammal_sleeptime_large)
mean_sleeptime_large
```

```
## [1] 3.3
```

10. Which animal is the sleepiest among the entire dataframe?
 Little brown bat is the sleepiest animal in the data frame. 

```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```

```r
filter(sleep, sleep_total == 19.9)
```

```
## # A tibble: 1 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Little… Myot… inse… Chir… <NA>                19.9         2         0.2   4.1
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
