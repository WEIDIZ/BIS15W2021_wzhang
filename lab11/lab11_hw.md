---
title: "Lab 11 Homework"
author: "Weidi Zhang"
date: "2021-02-17"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc: no
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  

It seems that there is no NA in the data.

```r
gapminder <- clean_names(gapminder)
names(gapminder)
```

```
## [1] "country"    "continent"  "year"       "life_exp"   "pop"       
## [6] "gdp_percap"
```


```r
glimpse(gapminder)
```

```
## Rows: 1,704
## Columns: 6
## $ country    <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, Afgh...
## $ continent  <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, As...
## $ year       <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, 19...
## $ life_exp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854, ...
## $ pop        <int> 8425333, 9240934, 10267083, 11537966, 13079460, 14880372...
## $ gdp_percap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.11...
```

```r
naniar::miss_var_summary(gapminder)
```

```
## # A tibble: 6 x 3
##   variable   n_miss pct_miss
##   <chr>       <int>    <dbl>
## 1 country         0        0
## 2 continent       0        0
## 3 year            0        0
## 4 life_exp        0        0
## 5 pop             0        0
## 6 gdp_percap      0        0
```



**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**


```r
gapminder$year <- as.factor(gapminder$year)
```


```r
gapminder %>%
  group_by(year)%>%
  summarize(mean_exp_year = mean(life_exp))%>%
  ggplot(aes(x = year, y = mean_exp_year, color = year))+
  geom_line(aes(group = 1))+
  geom_point(shape = 1)+
  labs(title = "Global Expectancy",
       x = "Year",
       y = "Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**

```r
gapminder %>%
  group_by(year)%>%
  filter(year == "1952" | year == "2007")%>%
  ggplot(aes(x = year, y = life_exp, fill = year))+
  geom_boxplot()+
  labs(title = "Global Expectancy",
       x = "Year",
       y = "Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**

```r
gapminder %>%
  group_by(continent)%>%
  ggplot(aes(x = continent, y = life_exp, color = continent))+
  geom_boxplot()+
  labs(title = "Global Expectancy",
       x = "Continent",
       y = "Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

**5. How has life expectancy changed between 1952-2007 for each continent?**

They all raised in some degree.

```r
gapminder %>%
  group_by(year, continent)%>%
  summarize(mean_exp_continent = mean(life_exp))%>%
  ggplot(aes(x = year, y = mean_exp_continent,group = continent, color = continent))+
  geom_line()+
  geom_point(shape = 1)+
  labs(title = "Global Expectancy",
       x = "Continent",
       y = "Expectancy")
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**

Seems like you can live longer if you have more money.

```r
gapminder%>%
  ggplot(aes(x = log10(gdp_percap), y = life_exp))+
  geom_jitter()+
  geom_smooth(method = lm, se = F)+
  labs(title = " Relationship between per capita GDP and Life Expectancy",
       x = "GDP",
       y = "Expectancy")
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

**7. Which countries have had the largest population growth since 1952?**

```r
gapminder%>%
  group_by(country)%>%
  filter(year==1952|year==2007)%>%
  mutate(pop_growth=pop-lag(pop,default = pop[1]))%>%
  arrange(desc(pop_growth))%>%
  select(country, pop_growth)
```

```
## # A tibble: 284 x 2
## # Groups:   country [142]
##    country       pop_growth
##    <fct>              <int>
##  1 China          762419569
##  2 India          738396331
##  3 United States  143586947
##  4 Indonesia      141495000
##  5 Brazil         133408087
##  6 Pakistan       127924057
##  7 Bangladesh     103561480
##  8 Nigeria        101912068
##  9 Mexico          78556574
## 10 Philippines     68638596
## # ... with 274 more rows
```

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
gapminder%>%
  group_by(country)%>%
  filter(year==1952|year==2007)%>%
  mutate(pop_growth = pop-lag(pop,default = pop[1]))%>%
  filter(year == 2007)%>%
  summarise(country, pop_growth)%>%
  top_n(5, pop_growth)%>%
  ggplot(aes(x = country, y = pop_growth, fill = country))+
  geom_col()+
  labs(title = "Top 5 countries of Population Growth",
       x = "country",
       y = "Population Growth")
```

![](lab11_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

**9. How does per-capita GDP growth compare between these same five countries?**

```r
gapminder%>%
  group_by(country)%>%
  filter(year==1952|year==2007)%>%
  mutate(gdp_growth = gdp_percap-lag(gdp_percap,default = 0))%>%
  filter(country == "Brazil" | country == "China" | country == "India" | country == "Indonesia" | country == "United States")%>%
  ggplot(aes(x = country, y = gdp_growth, fill = country))+
  geom_col()+
  labs(title = "GDP Growth of the Top 5 Population Growth Counties",
       x = "country",
       y = "GDP Growth")
```

![](lab11_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

**10. Make one plot of your choice that uses faceting!**

```r
p2 <- gapminder%>%
  ggplot(aes(x = gdp_percap))+
  geom_density()
```

```r
p2+facet_wrap(~continent)
```

![](lab11_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
