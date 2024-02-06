---
title: "Midterm 1 W24"
author: "Ryan Chu"
date: "2024-02-06"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code must be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. 

Your code must knit in order to be considered. If you are stuck and cannot answer a question, then comment out your code and knit the document. You may use your notes, labs, and homework to help you complete this exam. Do not use any other resources- including AI assistance.  

Don't forget to answer any questions that are asked in the prompt!  

Be sure to push your completed midterm to your repository. This exam is worth 30 points.  

## Background
In the data folder, you will find data related to a study on wolf mortality collected by the National Park Service. You should start by reading the `README_NPSwolfdata.pdf` file. This will provide an abstract of the study and an explanation of variables.  

The data are from: Cassidy, Kira et al. (2022). Gray wolf packs and human-caused wolf mortality. [Dryad](https://doi.org/10.5061/dryad.mkkwh713f). 

## Load the libraries

```r
library("tidyverse")
library("janitor")
```

## Load the wolves data
In these data, the authors used `NULL` to represent missing values. I am correcting this for you below and using `janitor` to clean the column names.

```r
wolves <- read.csv("data/NPS_wolfmortalitydata.csv", na = c("NULL")) %>% clean_names()
```

## Questions
Problem 1. (1 point) Let's start with some data exploration. What are the variable (column) names?  

The variable names are park, biolyr, pack, packcode, packsize_aug, mort_yn, mort_all, mort_lead, mort_nonlead, reprody1, and persisty1


```r
colnames(wolves)
```

```
##  [1] "park"         "biolyr"       "pack"         "packcode"     "packsize_aug"
##  [6] "mort_yn"      "mort_all"     "mort_lead"    "mort_nonlead" "reprody1"    
## [11] "persisty1"
```

Problem 2. (1 point) Use the function of your choice to summarize the data and get an idea of its structure.  


```r
str(wolves)
```

```
## 'data.frame':	864 obs. of  11 variables:
##  $ park        : chr  "DENA" "DENA" "DENA" "DENA" ...
##  $ biolyr      : int  1996 1991 2017 1996 1992 1994 2007 2007 1995 2003 ...
##  $ pack        : chr  "McKinley River1" "Birch Creek N" "Eagle Gorge" "East Fork" ...
##  $ packcode    : int  89 58 71 72 74 77 101 108 109 53 ...
##  $ packsize_aug: num  12 5 8 13 7 6 10 NA 9 8 ...
##  $ mort_yn     : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ mort_all    : int  4 2 2 2 2 2 2 2 2 1 ...
##  $ mort_lead   : int  2 2 0 0 0 0 1 2 1 1 ...
##  $ mort_nonlead: int  2 0 2 2 2 2 1 0 1 0 ...
##  $ reprody1    : int  0 0 NA 1 NA 0 0 1 0 1 ...
##  $ persisty1   : int  0 0 1 1 1 1 0 1 0 1 ...
```

Problem 3. (3 points) Which parks/ reserves are represented in the data? Don't just use the abstract, pull this information from the data.

The parks represented in the data are DENA, GNTP, VNP, YNP, and YUCH


```r
wolves$park <- as.factor(wolves$park)
levels(wolves$park)
```

```
## [1] "DENA" "GNTP" "VNP"  "YNP"  "YUCH"
```

Problem 4. (4 points) Which park has the largest number of wolf packs?

DENA has the largest number of wolf packs


```r
wolves %>%
  count(park, sort=T)
```

```
##   park   n
## 1 DENA 340
## 2  YNP 248
## 3 YUCH 151
## 4 GNTP  77
## 5  VNP  48
```

Problem 5. (4 points) Which park has the highest total number of human-caused mortalities `mort_all`?

YUCH has the highest total number of human-caused mortalities. 


```r
dena_packs <- filter(wolves, park =="DENA")
gntp_packs <- filter(wolves, park =="GNTP")
vnp_packs <- filter(wolves, park =="VNP")
ynp_packs <- filter(wolves, park == "YNP")
yuch_packs <- filter(wolves, park =="YUCH")
```


```r
dena_mort <- sum(dena_packs$mort_all)
gntp_mort <- sum(gntp_packs$mort_all)
vnp_mort <- sum(vnp_packs$mort_all)
ynp_mort <- sum(ynp_packs$mort_all)
yuch_mort <- sum(yuch_packs$mort_all)
```


```r
dena_mort
```

```
## [1] 64
```

```r
gntp_mort
```

```
## [1] 38
```

```r
vnp_mort
```

```
## [1] 11
```

```r
ynp_mort
```

```
## [1] 72
```

```r
yuch_mort
```

```
## [1] 136
```

The wolves in [Yellowstone National Park](https://www.nps.gov/yell/learn/nature/wolf-restoration.htm) are an incredible conservation success story. Let's focus our attention on this park.  

Problem 6. (2 points) Create a new object "ynp" that only includes the data from Yellowstone National Park.  


```r
ynp <- filter(wolves, park =="YNP")
```

Problem 7. (3 points) Among the Yellowstone wolf packs, the [Druid Peak Pack](https://www.pbs.org/wnet/nature/in-the-valley-of-the-wolves-the-druid-wolf-pack-story/209/) is one of most famous. What was the average pack size of this pack for the years represented in the data?

The average pack size of the Druid Peak pack was 13.9333


```r
dpp_pack <- filter(ynp, pack =="druid")
mean(dpp_pack$packsize_aug)
```

```
## [1] 13.93333
```

Problem 8. (4 points) Pack dynamics can be hard to predict- even for strong packs like the Druid Peak pack. At which year did the Druid Peak pack have the largest pack size? What do you think happened in 2010?

The Druid Peak pack had the largest pack size in 2001 at 37 members. In 2010, the pack completely died out. 


```r
max(dpp_pack$packsize_aug)
```

```
## [1] 37
```

```r
filter(dpp_pack, packsize_aug =="37")
```

```
##   park biolyr  pack packcode packsize_aug mort_yn mort_all mort_lead
## 1  YNP   2001 druid       26           37       0        0         0
##   mort_nonlead reprody1 persisty1
## 1            0        1         1
```

Problem 9. (5 points) Among the YNP wolf packs, which one has had the highest overall persistence `persisty1` for the years represented in the data? Look this pack up online and tell me what is unique about its behavior- specifically, what prey animals does this pack specialize on?  

The wolf pack with the highest overall persistence was the mollies pack. This pack specialized on hunting bison. 


```r
ynp %>%
  filter(!is.na(persisty1)) %>%
  group_by(pack) %>%
  summarise(persistence = sum(persisty1)) %>%
  arrange(desc(persistence))
```

```
## # A tibble: 46 × 2
##    pack        persistence
##    <chr>             <int>
##  1 mollies              26
##  2 cougar               20
##  3 yelldelta            18
##  4 druid                13
##  5 leopold              12
##  6 agate                10
##  7 8mile                 9
##  8 canyon                9
##  9 gibbon/mary           9
## 10 nezperce              9
## # ℹ 36 more rows
```

Problem 10. (3 points) Perform one analysis or exploration of your choice on the `wolves` data. Your answer needs to include at least two lines of code and not be a summary function.  

What was the mean pack size of all packs in Voyageurs National Park, and was it higher or lower than that of Yellowstone National Park?

The mean pack size of all packs in Voyageurs National Park was 5, which was lower than Yellowstone's average pack size of 11.238


```r
vnp <- filter(wolves, park =="VNP")
vnp_mean <- mean(vnp$packsize_aug, na.rm=T)
vnp_mean
```

```
## [1] 5
```


```r
ynp_mean <- mean(ynp$packsize_aug, na.rm=T)
ynp_mean
```

```
## [1] 11.23868
```
