---
title: "Lab 11 Homework"
author: "Srinidhi Venkatesh"
date: "2023-02-20"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
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
#install.packages("gapminder")
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  

```r
glimpse(gapminder)
```

```
## Rows: 1,704
## Columns: 6
## $ country   <fct> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", …
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, 1997, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854, 40.8…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 14880372, 12…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.1134, …
```

```r
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```

```r
anyNA(gapminder)
```

```
## [1] FALSE
```

```r
gapminder
```

```
## # A tibble: 1,704 × 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # … with 1,694 more rows
```

**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**

```r
gapminder %>% 
  select(year, lifeExp) %>% 
  mutate(year=as_factor(year)) %>% 
  ggplot(aes(x=year, y=lifeExp, fill=year))+
  geom_col()+
  labs(title = "Change in Life Expectancy from 1952 to 2007",
       x= "Year",
       y= "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-4-1.png)<!-- -->
The global life expectancy has increased from 1952 to 2007.

**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**

```r
gapminder %>% 
  filter(year == 1952 | year == 2007) %>% 
  mutate(year=as_factor(year)) %>% 
  group_by(year) %>% 
  ggplot(aes(x=year, y=lifeExp, fill=year))+
  geom_boxplot()+
  labs(title = "Distribution of Life Expectancy for 1952 & 2007",
       x = "Year",
       y = "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**

```r
gapminder %>% 
  group_by(continent) %>% 
  summarise(mean_lifeExp = mean(lifeExp),
            max_lifeExp = max(lifeExp),
            min_lifeExp = min(lifeExp))
```

```
## # A tibble: 5 × 4
##   continent mean_lifeExp max_lifeExp min_lifeExp
##   <fct>            <dbl>       <dbl>       <dbl>
## 1 Africa            48.9        76.4        23.6
## 2 Americas          64.7        80.7        37.6
## 3 Asia              60.1        82.6        28.8
## 4 Europe            71.9        81.8        43.6
## 5 Oceania           74.3        81.2        69.1
```
**5. How has life expectancy changed between 1952-2007 for each continent?**

```r
gapminder %>% 
  group_by(continent) %>% 
  ggplot(aes(x=year, y=lifeExp, fill = continent))+
  geom_col(position = "dodge")+
  theme(axis.text.x = element_text(angle = 45, hjust = 1))+
  labs(title = "Change in Life Expectancy from 1952 to 2007 by continent",
       x = "Year",
       y = "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**

```r
gapminder %>% 
  select(lifeExp, gdpPercap) %>% 
  ggplot(aes(x=gdpPercap, y= lifeExp))+
  geom_point()+
  coord_flip()+
  labs(title = "Relationship between per capita GDP & Life Expectancy",
       x= "Life Expectancy",
       y = "GDP per capita")
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
**7. Which countries have had the largest population growth since 1952?**

```r
gapminder1 <- gapminder %>% 
  select(year, country, pop) %>% 
  filter(year==1952|year==2007) %>% 
  pivot_wider(names_from = year,
              values_from = pop) %>% 
  mutate(delta = `2007`-`1952`) %>% 
  arrange(-delta)
gapminder1
```

```
## # A tibble: 142 × 4
##    country          `1952`     `2007`     delta
##    <fct>             <int>      <int>     <int>
##  1 China         556263527 1318683096 762419569
##  2 India         372000000 1110396331 738396331
##  3 United States 157553000  301139947 143586947
##  4 Indonesia      82052000  223547000 141495000
##  5 Brazil         56602560  190010647 133408087
##  6 Pakistan       41346560  169270617 127924057
##  7 Bangladesh     46886859  150448339 103561480
##  8 Nigeria        33119096  135031164 101912068
##  9 Mexico         30144317  108700891  78556574
## 10 Philippines    22438691   91077287  68638596
## # … with 132 more rows
```

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
gapminder1 %>% 
  slice_head(n=5) %>% 
  ggplot(aes(x=reorder(country,delta), y=delta, fill=country))+
  geom_col()+
  coord_flip()+
  labs(title = "Population growth for top 5 countries since 1952",
       x = "Population Growth",
       y = "Countries")
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
**9. How does per-capita GDP growth compare between these same five countries?**

```r
gapminder2 <- gapminder %>% 
  select(year, country, gdpPercap) %>% 
  filter(year==1952|year==2007) %>% 
  pivot_wider(names_from = year,
              values_from = gdpPercap) %>% 
  mutate(delta = `2007`-`1952`) %>% 
  arrange(-delta)
```

```r
gapminder2 %>% 
  slice_head(n=5) %>% 
  ggplot(aes(x=reorder(country,delta), y=delta, fill=country))+
  geom_col()+
  coord_flip()+
  labs(title = "GDP per capita growth for top 5 countries since 1952",
       x = "GDP per capita growth",
       y = "Countries")
```

![](lab11_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

**10. Make one plot of your choice that uses faceting!**



## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
