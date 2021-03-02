---
title: "Lab 13 Homework"
author: "Weidi Zhang"
date: "2021-03-02"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc: no
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## √ ggplot2 3.3.3     √ purrr   0.3.4
## √ tibble  3.0.6     √ dplyr   1.0.4
## √ tidyr   1.1.2     √ stringr 1.4.0
## √ readr   1.4.0     √ forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

There are NAs in the data. Also, there are "unknown", and "All" in Ethnicity.

```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", ~
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, ~
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"~
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano~
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35~
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15~
```

```r
naniar::miss_var_summary(UC_admit)
```

```
## # A tibble: 6 x 3
##   variable        n_miss pct_miss
##   <chr>            <int>    <dbl>
## 1 Perc FR              1   0.0463
## 2 FilteredCountFR      1   0.0463
## 3 Campus               0   0     
## 4 Academic_Yr          0   0     
## 5 Category             0   0     
## 6 Ethnicity            0   0
```



**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
UC_admit <- UC_admit %>% 
  filter(Ethnicity!="Unknown", Ethnicity!="All")
```


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Admissions by Ethnicity"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "UC Campuses", width = 3,
  selectInput("x", "Select Campus", choices = c("Berkeley", "Davis", "Irvine", "Los_Angeles", "Merced", "Riverside", "Santa_Barbara","San_Diego", "Santa_Cruz"), 
              selected = "Berkeley"),
  selectInput("y", "Select Category", choices = c("Applicants", "Admits", "Enrollees"),
              selected = "Applicants"),
  selectInput("z", "Select Year", choices = c("2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019"),
              selected = "2010")
  ), # close the first box
  box(title = "UC Admissions by Ethnicity", width = 7,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  UC_admit %>% 
      filter(Campus == input$x, Category == input$y, Academic_Yr == input$z)%>%
  ggplot(aes(x = reorder(Ethnicity, FilteredCountFR), y = FilteredCountFR)) +
  geom_col(color="black", fill="steelblue", alpha=0.75)+
  theme_light(base_size = 18)+
  theme(axis.text.x = element_text(angle = 30, hjust = 1))+
  labs(x ="Ethnicity",
       y = "Number")
  })
  
  # stop the app when we close it
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}



**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Admissions by Year"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "UC Campuses", width = 3,
  selectInput("x", "Select Campus", choices = c("Berkeley", "Davis", "Irvine", "Los_Angeles", "Merced", "Riverside", "Santa_Barbara","San_Diego", "Santa_Cruz"), 
              selected = "Berkeley"),
  selectInput("y", "Select Category", choices = c("Applicants", "Admits", "Enrollees"),
              selected = "Applicants"),
  selectInput("z", "Select Ethnicity", choices = c("Asian", "African American", "American Indian", "Chicano/Latino", "White", "International", "Unknown"),
              selected = "Unknown")
  ), # close the first box
  box(title = "UC Admissions by Year", width = 7,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  UC_admit %>% 
      filter(Campus == input$x, Category == input$y, Ethnicity == input$z)%>%
  ggplot(aes(x = reorder(Academic_Yr, FilteredCountFR), y = FilteredCountFR)) +
  geom_col(color="black", fill="steelblue", alpha=0.75)+
  theme_light(base_size = 18)+
  theme(axis.text.x = element_text(angle = 30, hjust = 1))+
  labs(x ="Year",
       y = "Number")
  })
  
  # stop the app when we close it
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}




## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
