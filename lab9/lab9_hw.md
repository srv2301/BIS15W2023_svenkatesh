---
title: "Lab 9 Homework"
author: "Srinidhi Venkatesh"
date: "2023-02-13"
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
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- read_csv("data/ca_college_data.csv")
```

```
## Rows: 341 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.

```r
glimpse(colleges)
```

```
## Rows: 341
## Columns: 10
## $ INSTNM        <chr> "Grossmont College", "College of the Sequoias", "College…
## $ CITY          <chr> "El Cajon", "Visalia", "San Mateo", "Ventura", "Oxnard",…
## $ STABBR        <chr> "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "C…
## $ ZIP           <chr> "92020-1799", "93277-2214", "94402-3784", "93003-3872", …
## $ ADM_RATE      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ SAT_AVG       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ PCIP26        <dbl> 0.0016, 0.0066, 0.0038, 0.0035, 0.0085, 0.0151, 0.0000, …
## $ COSTT4_A      <dbl> 7956, 8109, 8278, 8407, 8516, 8577, 8580, 9181, 9281, 93…
## $ C150_4_POOLED <dbl> NA, NA, NA, NA, NA, NA, 0.2334, NA, NA, NA, NA, 0.1704, …
## $ PFTFTUG1_EF   <dbl> 0.3546, 0.5413, 0.3567, 0.3824, 0.2753, 0.4286, 0.2307, …
```


```r
colleges %>% 
  summarise_all(~(sum(is.na(.))))
```

```
## # A tibble: 1 × 10
##   INSTNM  CITY STABBR   ZIP ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_PO…¹ PFTFT…²
##    <int> <int>  <int> <int>    <int>   <int>  <int>    <int>       <int>   <int>
## 1      0     0      0     0      240     276     35      124         221      53
## # … with abbreviated variable names ¹​C150_4_POOLED, ²​PFTFTUG1_EF
```


```r
colleges
```

```
## # A tibble: 341 × 10
##    INSTNM      CITY  STABBR ZIP   ADM_R…¹ SAT_AVG PCIP26 COSTT…² C150_…³ PFTFT…⁴
##    <chr>       <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>   <dbl>
##  1 Grossmont … El C… CA     9202…      NA      NA 0.0016    7956  NA       0.355
##  2 College of… Visa… CA     9327…      NA      NA 0.0066    8109  NA       0.541
##  3 College of… San … CA     9440…      NA      NA 0.0038    8278  NA       0.357
##  4 Ventura Co… Vent… CA     9300…      NA      NA 0.0035    8407  NA       0.382
##  5 Oxnard Col… Oxna… CA     9303…      NA      NA 0.0085    8516  NA       0.275
##  6 Moorpark C… Moor… CA     9302…      NA      NA 0.0151    8577  NA       0.429
##  7 Skyline Co… San … CA     9406…      NA      NA 0         8580   0.233   0.231
##  8 Glendale C… Glen… CA     9120…      NA      NA 0.002     9181  NA       0.421
##  9 Citrus Col… Glen… CA     9174…      NA      NA 0.0021    9281  NA       0.440
## 10 Fresno Cit… Fres… CA     93741      NA      NA 0.0324    9370  NA       0.366
## # … with 331 more rows, and abbreviated variable names ¹​ADM_RATE, ²​COSTT4_A,
## #   ³​C150_4_POOLED, ⁴​PFTFTUG1_EF
```

2. Which cities in California have the highest number of colleges?

```r
colleges %>% 
  count(CITY, sort = T)
```

```
## # A tibble: 161 × 2
##    CITY              n
##    <chr>         <int>
##  1 Los Angeles      24
##  2 San Diego        18
##  3 San Francisco    15
##  4 Sacramento       10
##  5 Berkeley          9
##  6 Oakland           9
##  7 Claremont         7
##  8 Pasadena          6
##  9 Fresno            5
## 10 Irvine            5
## # … with 151 more rows
```

3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.

```r
colleges %>% 
  count(CITY, sort = T) %>% 
  slice_max(n, n=10) %>% 
  ggplot(aes(x=reorder(CITY,n), y=n))+
  geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?

```r
colleges %>% 
  group_by(CITY) %>% 
  select(CITY, COSTT4_A, STABBR) %>% 
  summarise(average_COSTT4_A = mean(COSTT4_A, na.rm = T)) %>% 
  arrange(-average_COSTT4_A)
```

```
## # A tibble: 161 × 2
##    CITY                average_COSTT4_A
##    <chr>                          <dbl>
##  1 Claremont                      66498
##  2 Malibu                         66152
##  3 Valencia                       64686
##  4 Orange                         64501
##  5 Redlands                       61542
##  6 Moraga                         61095
##  7 Atherton                       56035
##  8 Thousand Oaks                  54373
##  9 Rancho Palos Verdes            50758
## 10 La Verne                       50603
## # … with 151 more rows
```

5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).

```r
colleges %>% 
  filter(CITY=="Claremont" | CITY == "Davis") %>% 
  group_by(COSTT4_A) %>% 
  select(COSTT4_A, CITY) %>% 
  ggplot(aes(x=COSTT4_A, y=CITY))+
  geom_jitter()
```

```
## Warning: Removed 2 rows containing missing values (`geom_point()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?

```r
colleges %>% 
  select(ADM_RATE, C150_4_POOLED) %>% 
  ggplot(aes(x=ADM_RATE, y=C150_4_POOLED))+
  geom_jitter()
```

```
## Warning: Removed 251 rows containing missing values (`geom_point()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?

```r
colleges %>% 
  select(COSTT4_A, ADM_RATE) %>% 
  ggplot(aes(x=COSTT4_A, y=ADM_RATE))+
  geom_jitter()
```

```
## Warning: Removed 249 rows containing missing values (`geom_point()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
There is no relationship which means that there is no correlation between the two variables.

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.

```r
colleges %>% 
  filter_all(any_vars(str_detect(.,pattern="University of California")))
```

```
## # A tibble: 10 × 10
##    INSTNM      CITY  STABBR ZIP   ADM_R…¹ SAT_AVG PCIP26 COSTT…² C150_…³ PFTFT…⁴
##    <chr>       <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>   <dbl>
##  1 University… La J… CA     92093   0.357    1324  0.216   31043   0.872   0.662
##  2 University… Irvi… CA     92697   0.406    1206  0.107   31198   0.876   0.725
##  3 University… Rive… CA     92521   0.663    1078  0.149   31494   0.73    0.811
##  4 University… Los … CA     9009…   0.180    1334  0.155   33078   0.911   0.661
##  5 University… Davis CA     9561…   0.423    1218  0.198   33904   0.850   0.605
##  6 University… Sant… CA     9506…   0.578    1201  0.193   34608   0.776   0.786
##  7 University… Berk… CA     94720   0.169    1422  0.105   34924   0.916   0.709
##  8 University… Sant… CA     93106   0.358    1281  0.108   34998   0.816   0.708
##  9 University… San … CA     9410…  NA          NA NA          NA  NA      NA    
## 10 University… San … CA     9414…  NA          NA NA          NA  NA      NA    
## # … with abbreviated variable names ¹​ADM_RATE, ²​COSTT4_A, ³​C150_4_POOLED,
## #   ⁴​PFTFTUG1_EF
```

Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_calif_final <- colleges %>% 
  filter_all(any_vars(str_detect(.,pattern="University of California"))) %>% 
  filter(INSTNM != "University of California-San Francisco") %>% 
  filter(INSTNM != "University of California-Hastings College of Law")
univ_calif_final
```

```
## # A tibble: 8 × 10
##   INSTNM       CITY  STABBR ZIP   ADM_R…¹ SAT_AVG PCIP26 COSTT…² C150_…³ PFTFT…⁴
##   <chr>        <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>   <dbl>
## 1 University … La J… CA     92093   0.357    1324  0.216   31043   0.872   0.662
## 2 University … Irvi… CA     92697   0.406    1206  0.107   31198   0.876   0.725
## 3 University … Rive… CA     92521   0.663    1078  0.149   31494   0.73    0.811
## 4 University … Los … CA     9009…   0.180    1334  0.155   33078   0.911   0.661
## 5 University … Davis CA     9561…   0.423    1218  0.198   33904   0.850   0.605
## 6 University … Sant… CA     9506…   0.578    1201  0.193   34608   0.776   0.786
## 7 University … Berk… CA     94720   0.169    1422  0.105   34924   0.916   0.709
## 8 University … Sant… CA     93106   0.358    1281  0.108   34998   0.816   0.708
## # … with abbreviated variable names ¹​ADM_RATE, ²​COSTT4_A, ³​C150_4_POOLED,
## #   ⁴​PFTFTUG1_EF
```

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_calif_final <- univ_calif_final %>% 
  separate(INSTNM, into = c("UNIV", "CAMPUS"), sep ="-")
univ_calif_final
```

```
## # A tibble: 8 × 11
##   UNIV  CAMPUS CITY  STABBR ZIP   ADM_R…¹ SAT_AVG PCIP26 COSTT…² C150_…³ PFTFT…⁴
##   <chr> <chr>  <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>   <dbl>
## 1 Univ… San D… La J… CA     92093   0.357    1324  0.216   31043   0.872   0.662
## 2 Univ… Irvine Irvi… CA     92697   0.406    1206  0.107   31198   0.876   0.725
## 3 Univ… River… Rive… CA     92521   0.663    1078  0.149   31494   0.73    0.811
## 4 Univ… Los A… Los … CA     9009…   0.180    1334  0.155   33078   0.911   0.661
## 5 Univ… Davis  Davis CA     9561…   0.423    1218  0.198   33904   0.850   0.605
## 6 Univ… Santa… Sant… CA     9506…   0.578    1201  0.193   34608   0.776   0.786
## 7 Univ… Berke… Berk… CA     94720   0.169    1422  0.105   34924   0.916   0.709
## 8 Univ… Santa… Sant… CA     93106   0.358    1281  0.108   34998   0.816   0.708
## # … with abbreviated variable names ¹​ADM_RATE, ²​COSTT4_A, ³​C150_4_POOLED,
## #   ⁴​PFTFTUG1_EF
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>% 
  group_by(ADM_RATE) %>% 
  select(ADM_RATE, CAMPUS) %>% 
  arrange(-ADM_RATE)
```

```
## # A tibble: 8 × 2
## # Groups:   ADM_RATE [8]
##   ADM_RATE CAMPUS       
##      <dbl> <chr>        
## 1    0.663 Riverside    
## 2    0.578 Santa Cruz   
## 3    0.423 Davis        
## 4    0.406 Irvine       
## 5    0.358 Santa Barbara
## 6    0.357 San Diego    
## 7    0.180 Los Angeles  
## 8    0.169 Berkeley
```

UC Riverside has the highest admission rate and UC Berkeley has the lowest admission rate.


```r
univ_calif_final %>% 
  group_by(ADM_RATE) %>% 
  select(ADM_RATE, CAMPUS) %>% 
  arrange(-ADM_RATE) %>% 
  ggplot(aes(x=CAMPUS, y=ADM_RATE))+
  geom_col()
```

![](lab9_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>% 
  select(PCIP26, CAMPUS) %>% 
  arrange(-PCIP26)
```

```
## # A tibble: 8 × 2
##   PCIP26 CAMPUS       
##    <dbl> <chr>        
## 1  0.216 San Diego    
## 2  0.198 Davis        
## 3  0.193 Santa Cruz   
## 4  0.155 Los Angeles  
## 5  0.149 Riverside    
## 6  0.108 Santa Barbara
## 7  0.107 Irvine       
## 8  0.105 Berkeley
```


```r
univ_calif_final %>% 
  select(PCIP26, CAMPUS) %>% 
  arrange(-PCIP26) %>% 
  ggplot(aes(x=CAMPUS, y=PCIP26))+
  geom_col()
```

![](lab9_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
