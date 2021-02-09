---
title: "Midterm 1"
author: "Weidi Zhang"
date: "2021-02-09"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc: no
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  

_R is an open source program language, and we use RStudio as its GUI. The code we wrote in R can be submitted and shared on GitHub. RMarkdown allowed us easily annotate codes, show results of analyses, and display graphs, while keep the format clean. It can also be outputted as many types of file, like html, and pdf._    

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**

_Data frames, matrices and vectors are the three types of data structures that we have discussed. In BIS15L, we use data frames most frequently because they allowed us to manipulate and visualize complex data with lots of varieties. Data frames are kind of like the combination of vectors and matrices, and keep their functions at the same time._

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">
**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
#setwd("D:/GitHub Database/Course BIS15L/BIS15W2021_wzhang/midterm 1/Data")
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
str(elephants)
```

```
## spec_tbl_df [288 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Age   : num [1:288] 1.4 17.5 12.8 11.2 12.7 ...
##  $ Height: num [1:288] 120 227 235 210 220 ...
##  $ Sex   : chr [1:288] "M" "M" "M" "M" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Age = col_double(),
##   ..   Height = col_double(),
##   ..   Sex = col_character()
##   .. )
```

_If you use set_wd() in your code then it will only run on your computer. In order to get your code to run, I had to adjust the path._

</div>

**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```


```r
elephants <- janitor::clean_names(elephants)
head(elephants)
```

```
## # A tibble: 6 x 3
##     age height sex  
##   <dbl>  <dbl> <chr>
## 1   1.4    120 M    
## 2  17.5    227 M    
## 3  12.8    235 M    
## 4  11.2    210 M    
## 5  12.7    220 M    
## 6  12.7    189 M
```


```r
elephants$sex <- as_factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```

**5. (2 points) How many male and female elephants are represented in the data?**  

_There are 138 males, and 150 females._

```r
elephants%>%
  count(sex)
```

```
## # A tibble: 2 x 2
##   sex       n
## * <fct> <int>
## 1 M       138
## 2 F       150
```

**6. (2 points) What is the average age all elephants in the data?**  

_The average age of all elephants in this data is 10.97 years._

```r
elephants%>%
  select(age)%>%
  summarize(mean_age_all = mean(age))
```

```
## # A tibble: 1 x 1
##   mean_age_all
##          <dbl>
## 1         11.0
```

**7. (2 points) How does the average age and height of elephants compare by sex?**

_Female elephants have higher average age and height than male._

```r
elephants%>%
  group_by(sex)%>%
  summarize(mean_age = mean(age),
            mean_height = mean(height))
```

```
## # A tibble: 2 x 3
##   sex   mean_age mean_height
## * <fct>    <dbl>       <dbl>
## 1 M         8.95        185.
## 2 F        12.8         190.
```

**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

Male: 8 samples; mean=272.67, min=237.19, max=304.06

Female: 25 samples; mean=232.74, min=206.07, max=277.8

```r
elephants%>%
  filter(age > 25)%>%
  select(sex, height)%>%
  group_by(sex)%>%
  summarize(mean_height_25 = mean(height),
            min_height_25 = min(height),
            max_height_25 = max(height),
            total = n())
```

```
## # A tibble: 2 x 5
##   sex   mean_height_25 min_height_25 max_height_25 total
## * <fct>          <dbl>         <dbl>         <dbl> <int>
## 1 M               273.          237.          304.     8
## 2 F               233.          206.          278.    25
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**

```r
#setwd("D:/GitHub Database/Course BIS15L/BIS15W2021_wzhang/midterm 1/Data")
verberate <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

```r
str(verberate)
```

```
## spec_tbl_df [24 × 26] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ TransectID             : num [1:24] 1 2 2 3 4 5 6 7 8 9 ...
##  $ Distance               : num [1:24] 7.14 17.31 18.32 20.85 15.95 ...
##  $ HuntCat                : chr [1:24] "Moderate" "None" "None" "None" ...
##  $ NumHouseholds          : num [1:24] 54 54 29 29 29 29 29 54 25 73 ...
##  $ LandUse                : chr [1:24] "Park" "Park" "Park" "Logging" ...
##  $ Veg_Rich               : num [1:24] 16.7 15.8 16.9 12.4 17.1 ...
##  $ Veg_Stems              : num [1:24] 31.2 37.4 32.3 29.4 36 ...
##  $ Veg_liana              : num [1:24] 5.78 13.25 4.75 9.78 13.25 ...
##  $ Veg_DBH                : num [1:24] 49.6 34.6 42.8 36.6 41.5 ...
##  $ Veg_Canopy             : num [1:24] 3.78 3.75 3.43 3.75 3.88 2.5 4 4 3 3.25 ...
##  $ Veg_Understory         : num [1:24] 2.89 3.88 3 2.75 3.25 3 2.38 2.71 3.25 3.13 ...
##  $ RA_Apes                : num [1:24] 1.87 0 4.49 12.93 0 ...
##  $ RA_Birds               : num [1:24] 52.7 52.2 37.4 59.3 52.6 ...
##  $ RA_Elephant            : num [1:24] 0 0.86 1.33 0.56 1 0 1.11 0.43 2.2 0 ...
##  $ RA_Monkeys             : num [1:24] 38.6 28.5 41.8 19.9 41.3 ...
##  $ RA_Rodent              : num [1:24] 4.22 6.04 1.06 3.66 2.52 1.83 3.1 1.26 4.37 6.31 ...
##  $ RA_Ungulate            : num [1:24] 2.66 12.41 13.86 3.71 2.53 ...
##  $ Rich_AllSpecies        : num [1:24] 22 20 22 19 20 22 23 19 19 19 ...
##  $ Evenness_AllSpecies    : num [1:24] 0.793 0.773 0.74 0.681 0.811 0.786 0.818 0.757 0.773 0.668 ...
##  $ Diversity_AllSpecies   : num [1:24] 2.45 2.31 2.29 2.01 2.43 ...
##  $ Rich_BirdSpecies       : num [1:24] 11 10 11 8 8 10 11 11 11 9 ...
##  $ Evenness_BirdSpecies   : num [1:24] 0.732 0.704 0.688 0.559 0.799 0.771 0.801 0.687 0.784 0.573 ...
##  $ Diversity_BirdSpecies  : num [1:24] 1.76 1.62 1.65 1.16 1.66 ...
##  $ Rich_MammalSpecies     : num [1:24] 11 10 11 11 12 12 12 8 8 10 ...
##  $ Evenness_MammalSpecies : num [1:24] 0.736 0.705 0.65 0.619 0.736 0.694 0.776 0.79 0.821 0.783 ...
##  $ Diversity_MammalSpecies: num [1:24] 1.76 1.62 1.56 1.48 1.83 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   TransectID = col_double(),
##   ..   Distance = col_double(),
##   ..   HuntCat = col_character(),
##   ..   NumHouseholds = col_double(),
##   ..   LandUse = col_character(),
##   ..   Veg_Rich = col_double(),
##   ..   Veg_Stems = col_double(),
##   ..   Veg_liana = col_double(),
##   ..   Veg_DBH = col_double(),
##   ..   Veg_Canopy = col_double(),
##   ..   Veg_Understory = col_double(),
##   ..   RA_Apes = col_double(),
##   ..   RA_Birds = col_double(),
##   ..   RA_Elephant = col_double(),
##   ..   RA_Monkeys = col_double(),
##   ..   RA_Rodent = col_double(),
##   ..   RA_Ungulate = col_double(),
##   ..   Rich_AllSpecies = col_double(),
##   ..   Evenness_AllSpecies = col_double(),
##   ..   Diversity_AllSpecies = col_double(),
##   ..   Rich_BirdSpecies = col_double(),
##   ..   Evenness_BirdSpecies = col_double(),
##   ..   Diversity_BirdSpecies = col_double(),
##   ..   Rich_MammalSpecies = col_double(),
##   ..   Evenness_MammalSpecies = col_double(),
##   ..   Diversity_MammalSpecies = col_double()
##   .. )
```

```r
verberate$HuntCat <- as_factor(verberate$HuntCat)
verberate$LandUse <- as_factor(verberate$LandUse)
class(verberate$HuntCat)
```

```
## [1] "factor"
```

```r
class(verberate$LandUse)
```

```
## [1] "factor"
```

**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

The average diversities are higher in high hunting intensity transects.

```r
verberate%>%
  select(HuntCat, Diversity_BirdSpecies, Diversity_MammalSpecies)%>%
  filter(HuntCat == "High" | HuntCat == "Moderate" )%>%
  group_by(HuntCat)%>%
  summarize(mean_birds = mean(Diversity_BirdSpecies),
            mean_mammals = mean(Diversity_MammalSpecies))
```

```
## # A tibble: 2 x 3
##   HuntCat  mean_birds mean_mammals
## * <fct>         <dbl>        <dbl>
## 1 Moderate       1.62         1.68
## 2 High           1.66         1.74
```

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">
**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

```r
verberate%>%
  filter(Distance < 5 | Distance > 20)%>%
  group_by(Distance)%>%
  summarize(across(contains("RA")))
```

```
## # A tibble: 6 x 8
##   Distance TransectID RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent
## *    <dbl>      <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>
## 1     2.7          15    0        85.0       0.290       9.09      3.74
## 2     2.92         27    0.24     68.2       0          25.6       4.05
## 3     3.83         17    0        57.8       0          37.8       3.19
## 4    20.8           3   12.9      59.3       0.56       19.8       3.66
## 5    24.1           6    3.78     42.7       1.11       46.2       3.1 
## 6    26.8          24    4.91     31.6       0          54.1       1.29
## # … with 1 more variable: RA_Ungulate <dbl>
```
_group_by() will not work here because the distances are unique._  

</div>

**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

Whether the numbers of households in different transects have great impact on diversity and richness of all species.  

The data shows that the numbers of households in different transects seems to have no great impact on diversity and richness of all species.  

```r
verberate%>%
  group_by(TransectID)%>%
  summarize(NumHouseholds, Diversity_AllSpecies, Rich_AllSpecies)%>%
  arrange(NumHouseholds)
```

```
## `summarise()` has grouped output by 'TransectID'. You can override using the `.groups` argument.
```

```
## # A tibble: 24 x 4
## # Groups:   TransectID [23]
##    TransectID NumHouseholds Diversity_AllSpecies Rich_AllSpecies
##         <dbl>         <dbl>                <dbl>           <dbl>
##  1         22            13                 2.37              22
##  2         27            13                 2.38              19
##  3         18            17                 2.36              20
##  4         17            19                 2.45              20
##  5         20            24                 2.27              16
##  6         21            24                 2.45              19
##  7          8            25                 2.28              19
##  8          2            29                 2.29              22
##  9          3            29                 2.01              19
## 10          4            29                 2.43              20
## # … with 14 more rows
```

_Nice job Weidi! There are only a few problems, mostly related to the path issue I point out. Also, be sure to review the group_by() function_
