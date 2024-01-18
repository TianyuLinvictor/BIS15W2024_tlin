---
title: "Lab 3 Homework"
author: "Tianyu Lin"
date: "2024-01-18"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library("tidyverse")
```

### Mammals Sleep  
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R. The name of the data is `msleep`.  

```r
msleep
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # ℹ 73 more rows
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

2. Store these data into a new data frame `sleep`.  

```r
sleep <- msleep
head(sleep)
```

```
## # A tibble: 6 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Cheetah Acin… carni Carn… lc                  12.1      NA        NA      11.9
## 2 Owl mo… Aotus omni  Prim… <NA>                17         1.8      NA       7  
## 3 Mounta… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
## 4 Greate… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
## 5 Cow     Bos   herbi Arti… domesticated         4         0.7       0.667  20  
## 6 Three-… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
dim(sleep)
```

```
## [1] 83 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
summary(is.na(sleep))
```

```
##     name           genus            vore           order        
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:83        FALSE:83        FALSE:76        FALSE:83       
##                                  TRUE :7                        
##  conservation    sleep_total     sleep_rem       sleep_cycle    
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:54        FALSE:83        FALSE:61        FALSE:32       
##  TRUE :29                        TRUE :22        TRUE :51       
##    awake          brainwt          bodywt       
##  Mode :logical   Mode :logical   Mode :logical  
##  FALSE:83        FALSE:56        FALSE:83       
##                  TRUE :27
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

```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```
There are 32.

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 19kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small <- filter(sleep,bodywt <= 19)
head(small)
```

```
## # A tibble: 6 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Owl mo… Aotus omni  Prim… <NA>                17         1.8      NA       7  
## 2 Mounta… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
## 3 Greate… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
## 4 Three-… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
## 5 Vesper… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
## 6 Dog     Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

```r
large <- filter(sleep,bodywt >= 200)
head(large)
```

```
## # A tibble: 6 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Cow     Bos   herbi Arti… domesticated         4         0.7       0.667  20  
## 2 Asian … Elep… herbi Prob… en                   3.9      NA        NA      20.1
## 3 Horse   Equus herbi Peri… domesticated         2.9       0.6       1      21.1
## 4 Giraffe Gira… herbi Arti… cd                   1.9       0.4      NA      22.1
## 5 Pilot … Glob… carni Ceta… cd                   2.7       0.1      NA      21.4
## 6 Africa… Loxo… herbi Prob… vu                   3.3      NA        NA      20.7
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

8. What is the mean weight for both the small and large mammals?

```r
small_wtmean <- mean(small$bodywt)
small_wtmean
```

```
## [1] 1.797847
```


```r
large_wtmean <- mean(large$bodywt)
large_wtmean
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  


```r
small_sleepmean <- mean(small$sleep_total)
small_sleepmean
```

```
## [1] 11.78644
```


```r
large_sleepmean <- mean(large$sleep_total)
large_sleepmean
```

```
## [1] 3.3
```

10. Which animal is the sleepiest among the entire dataframe?


```r
find_sleepiest <- table(sleep$sleep_total)
sleepiest <- filter(sleep,sleep_total==19.9)
sleepiest 
```

```
## # A tibble: 1 × 11
##   name    genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr>   <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Little… Myot… inse… Chir… <NA>                19.9         2         0.2   4.1
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```
It is Little brown bat.

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
