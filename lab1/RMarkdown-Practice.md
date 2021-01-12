---
title: "RMarkdown Practice"
output: 
   html_document:
      keep_md: yes
---



## R Markdown

# Hi 
## Hello
*Hi*

**Hello**

This is the RMarkdown Practice


[BIS15L Webpage](https://jmledford3115.github.io/datascibiol)

[Weidi Zhang](mailto: wdizhang@ucdavis.edu)


```r
4*12
```

```
## [1] 48
```

```r
(4*12)/2
```

```
## [1] 24
```

```r
x <- c(4, 6, 8, 5, 6, 7, 7, 7)
mean(x)
```

```
## [1] 6.25
```

```r
median(x)
```

```
## [1] 6.5
```

```r
#install.packages("tidyverse")
library("tidyverse")
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## √ ggplot2 3.3.3     √ purrr   0.3.4
## √ tibble  3.0.4     √ dplyr   1.0.2
## √ tidyr   1.1.2     √ stringr 1.4.0
## √ readr   1.4.0     √ forcats 0.5.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](RMarkdown-Practice_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

