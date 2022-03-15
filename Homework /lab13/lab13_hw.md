---
title: "Lab 13 Homework"
author: "Abdelwahab Abdusamad"
date: "2022-03-04"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
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
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.8
## ✓ tidyr   1.2.0     ✓ stringr 1.4.0
## ✓ readr   2.1.2     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
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
## Rows: 2160 Columns: 6
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Campus, Category, Ethnicity, Perc FR
## dbl (2): Academic_Yr, FilteredCountFR
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  


```r
names(UC_admit)
```

```
## [1] "Campus"          "Academic_Yr"     "Category"        "Ethnicity"      
## [5] "Perc FR"         "FilteredCountFR"
```



```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", …
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, …
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"…
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano…
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35…
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15…
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "University Of California Ethinic Distribution"),
  dashboardSidebar(disable = T),
  dashboardBody(
    fluidRow(
      box(title = "Plot Choices", width = 3,
       radioButtons("x", "Choose Year", choices = c("2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019"), 
              selected = "2010"),
  selectInput("y", "Choose Campus", choices = c("Davis", "Irvine", "Berkeley", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"),
              selected = "Davis"),
  selectInput("z", "Choose Admit Category", choices = c("Applicants", "Admits", "Enrollees"),
              selected = "Admits")
  ), 
  box(title= "University of California Admissions (Distribution by Ethnicity)", width = 9,
  plotOutput("plot", width = "800px", height = "800px")
  )
  )
  )
)
server <- function(input, output, session) { 
  output$plot <- renderPlot({
    UC_admit %>% 
      filter(Academic_Yr==input$x & Campus==input$y & Category==input$z) %>% 
  ggplot(aes(x=reorder(Ethnicity, FilteredCountFR), y=FilteredCountFR))+
  geom_col(fill="blue", position = "dodge", alpha=1, size=6)+
  theme_linedraw(base_size = 20)+ 
      labs(x=NULL, y="Total Individuals", fill="Fill Variable")+
  theme(axis.text.x = element_text(angle = 65, hjust = .7))
  })
  session$onSessionEnded(stopApp)
  }
shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "University Of California Ethinic Distribution"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Cboices", width = 3,
       selectInput("x", "Choose Campus", choices = c("Davis", "Irvine", "Berkeley", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"),
              selected = "Davis"),
  selectInput("z", "Choose Admit Category", choices = c("Applicants", "Admits", "Enrollees"),
              selected = "Applicants"),
  radioButtons("y", "Select Ethnicity", choices = c("International", "Unknown", "Asian", "American Indian", "Chicano/Latino", "White", "African American"),
              selected = "International")
  ), 
  box(title= "University of California Admissions (Distribution by Ethnicity)", width = 7,
  plotOutput("plot", width = "800px", height = "800px")
  )
  )
  )
)
server <- function(input, output, session) { 
  output$plot <- renderPlot({
    UC_admit %>% 
      filter(Campus==input$x & Ethnicity==input$y & Category==input$z) %>% 
  ggplot(aes(x=Academic_Yr, y=FilteredCountFR))+
  geom_col(fill="red", position = "dodge", alpha=1, size=4)+
  theme_linedraw(base_size = 18)+ 
      labs(x="Year", y="Enrollment", fill="Fill Variable")+
  theme(axis.text.x = element_text(angle = 65, hjust = .7))
  })
  session$onSessionEnded(stopApp)
  }
shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
