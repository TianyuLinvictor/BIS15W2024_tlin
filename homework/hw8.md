---
title: "Homework 8"
author: "Tianyu Lin"
date: "2024-02-13"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches<- read_csv("data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
names(sydneybeaches)
```

```
## [1] "BeachId"                 "Region"                 
## [3] "Council"                 "Site"                   
## [5] "Longitude"               "Latitude"               
## [7] "Date"                    "Enterococci (cfu/100ml)"
```


```r
dim(sydneybeaches)
```

```
## [1] 3690    8
```


```r
glimpse(sydneybeaches)
```

```
## Rows: 3,690
## Columns: 8
## $ BeachId                   <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, …
## $ Region                    <chr> "Sydney City Ocean Beaches", "Sydney City Oc…
## $ Council                   <chr> "Randwick Council", "Randwick Council", "Ran…
## $ Site                      <chr> "Clovelly Beach", "Clovelly Beach", "Clovell…
## $ Longitude                 <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.…
## $ Latitude                  <dbl> -33.91449, -33.91449, -33.91449, -33.91449, …
## $ Date                      <chr> "02/01/2013", "06/01/2013", "12/01/2013", "1…
## $ `Enterococci (cfu/100ml)` <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8…
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at /Users/victoria/Desktop/BIS15W2024_tlin
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("homework", "data", "sydneybeaches.csv")) %>% clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

For the definitions of the tidyverse, this data is tidy.Because it meets the three requirements of tidy. (1) each variable has its own column, (2) each observation has its own row, (3) each value has its own cell. They are in long format.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long<-sydneybeaches %>% 
    select(site,date,enterococci_cfu_100ml)
```

4. Pivot the data such that the dates are column names and each beach only appears once (wide format). Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide<-sydneybeaches_long %>% 
  pivot_wider(names_from = "date",
              values_from = "enterococci_cfu_100ml")
```

5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide %>% 
  pivot_longer(-site,
               names_to = "date",
               values_to = "enterococci_cfu_100ml")
```

```
## # A tibble: 3,784 × 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ℹ 3,774 more rows
```

6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_long %>% 
  separate(date, into= c("day", "month","year"), sep = "/")
```

```
## # A tibble: 3,690 × 5
##    site           day   month year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ℹ 3,680 more rows
```

7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
avg_enterococci_long <- sydneybeaches_long %>% 
  separate(date, into= c("day", "month","year"), sep = "/")%>%
  group_by(year)%>%
  summarize(mean_enterococci=mean(enterococci_cfu_100ml,na.rm = T))
avg_enterococci_long
```

```
## # A tibble: 6 × 2
##   year  mean_enterococci
##   <chr>            <dbl>
## 1 2013              50.6
## 2 2014              26.3
## 3 2015              31.2
## 4 2016              42.2
## 5 2017              20.7
## 6 2018              33.1
```

8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
avg_enterococci_wide<-avg_enterococci_long%>% 
  pivot_wider(names_from = "year",
              values_from = "mean_enterococci")
avg_enterococci_wide
```

```
## # A tibble: 1 × 6
##   `2013` `2014` `2015` `2016` `2017` `2018`
##    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1   50.6   26.3   31.2   42.2   20.7   33.1
```

9. What was the most polluted beach in 2013?

```r
avg_enterococci_long%>%
  filter(year=="2013")%>%
  arrange(desc(mean_enterococci))
```

```
## # A tibble: 1 × 2
##   year  mean_enterococci
##   <chr>            <dbl>
## 1 2013              50.6
```
It is the Little Bay Beach.

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
