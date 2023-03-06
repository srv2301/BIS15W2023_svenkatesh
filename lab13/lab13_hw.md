---
title: "Lab 13 Homework"
author: "Srinidhi Venkatesh"
date: "2023-03-05"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries  

```r
library(tidyverse)
library(janitor)
library(here)
library(devtools)
#install.packages("ggmap")
library(ggmap)
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  

2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure.  

```r
grizzly <- read_csv("data/bear-sightings.csv")
```

```
## Rows: 494 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (3): bear.id, longitude, latitude
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
summary(grizzly)
```

```
##     bear.id       longitude         latitude    
##  Min.   :   7   Min.   :-166.2   Min.   :55.02  
##  1st Qu.:2569   1st Qu.:-154.2   1st Qu.:58.13  
##  Median :4822   Median :-151.0   Median :60.97  
##  Mean   :4935   Mean   :-149.1   Mean   :61.41  
##  3rd Qu.:7387   3rd Qu.:-145.6   3rd Qu.:64.13  
##  Max.   :9996   Max.   :-131.3   Max.   :70.37
```
2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.  

```r
lat <- c(55.02, 70.37)
long <- c(-166.2, -131.3)
bbox <- make_bbox(long, lat, f=0.05)
```

3. Load a map from `stamen` in a terrain style projection and display the map.  

```r
map1 <- get_map(bbox, maptype = "terrain", source = "stamen")
```

```
## ℹ Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```

```r
ggmap(map1)
```

![](lab13_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.  

```r
ggmap(map1) + 
  geom_point(data = grizzly, aes(longitude, latitude)) +
  labs(x= "Longitude", y= "Latitude", title="Grizzly Bear Sightings")
```

![](lab13_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

Let's switch to the wolves data. Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

5. Load the data and evaluate its structure.  

```r
wolves <- read_csv("data/wolves_data/wolves_dataset.csv")
```

```
## Rows: 1986 Columns: 23
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): pop, age.cat, sex, color
## dbl (19): year, lat, long, habitat, human, pop.density, pack.size, standard....
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
6. How many distinct wolf populations are included in this study? Mae a new object that restricts the data to the wolf populations in the lower 48 US states.  

```r
wolves_lower_48 <- wolves %>% 
  filter(lat<=48)
```

7. Use the range of the latitude and longitude to build an appropriate bounding box for your map.  

```r
summary(wolves_lower_48)
```

```
##      pop                 year        age.cat              sex           
##  Length:1169        Min.   :1997   Length:1169        Length:1169       
##  Class :character   1st Qu.:2007   Class :character   Class :character  
##  Mode  :character   Median :2011   Mode  :character   Mode  :character  
##                     Mean   :2010                                        
##                     3rd Qu.:2014                                        
##                     Max.   :2019                                        
##                                                                         
##     color                lat             long            habitat     
##  Length:1169        Min.   :33.89   Min.   :-110.99   Min.   : 9511  
##  Class :character   1st Qu.:44.60   1st Qu.:-110.99   1st Qu.:11166  
##  Mode  :character   Median :44.60   Median :-110.55   Median :11211  
##                     Mean   :43.95   Mean   :-106.91   Mean   :12744  
##                     3rd Qu.:46.83   3rd Qu.:-109.17   3rd Qu.:11211  
##                     Max.   :47.75   Max.   : -86.82   Max.   :32018  
##                                                                      
##      human       pop.density      pack.size     standard.habitat   
##  Min.   :2788   Min.   : 3.99   Min.   :4.040   Min.   :-0.419600  
##  1st Qu.:3240   1st Qu.:11.63   1st Qu.:5.620   1st Qu.:-0.202400  
##  Median :3924   Median :23.03   Median :5.620   Median :-0.196500  
##  Mean   :3810   Mean   :19.33   Mean   :6.431   Mean   : 0.004642  
##  3rd Qu.:3973   3rd Qu.:28.93   3rd Qu.:8.250   3rd Qu.:-0.196500  
##  Max.   :6229   Max.   :33.96   Max.   :8.250   Max.   : 2.533100  
##                                                                    
##  standard.human    standard.pop     standard.packsize  standard.latitude
##  Min.   :0.3648   Min.   :-1.1081   Min.   :-1.47050   Min.   :-1.8059  
##  1st Qu.:0.5834   1st Qu.:-0.2976   1st Qu.:-0.54180   1st Qu.:-0.6369  
##  Median :0.9144   Median : 0.9119   Median :-0.54180   Median :-0.6369  
##  Mean   :0.8591   Mean   : 0.5197   Mean   :-0.06482   Mean   :-0.7071  
##  3rd Qu.:0.9383   3rd Qu.: 1.5378   3rd Qu.: 1.00410   3rd Qu.:-0.3926  
##  Max.   :2.0290   Max.   : 2.0715   Max.   : 1.00410   Max.   :-0.2927  
##                                                                         
##  standard.longitude   cav.binary       cdv.binary       cpv.binary    
##  Min.   :0.3069     Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:0.3069     1st Qu.:1.0000   1st Qu.:0.0000   1st Qu.:1.0000  
##  Median :0.3302     Median :1.0000   Median :0.0000   Median :1.0000  
##  Mean   :0.5207     Mean   :0.8342   Mean   :0.2526   Mean   :0.8764  
##  3rd Qu.:0.4022     3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :1.5716     Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##                     NA's   :222      NA's   :17       NA's   :4       
##    chv.binary       neo.binary      toxo.binary    
##  Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:1.0000   1st Qu.:0.0000   1st Qu.:0.0000  
##  Median :1.0000   Median :0.0000   Median :0.0000  
##  Mean   :0.7903   Mean   :0.3777   Mean   :0.4817  
##  3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##  NA's   :382      NA's   :388      NA's   :677
```


```r
lat <- c(33.89,47.75)
long <- c(-110.99, -86.82)
bbox <- make_bbox(long, lat, f=0.05)
```

8.  Load a map from `stamen` in a `terrain-lines` projection and display the map.  

```r
map2 <- get_map(bbox, maptype = "terrain-lines", source = "stamen")
```

```
## ℹ Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```


```r
ggmap(map2)
```

![](lab13_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

9. Build a final map that overlays the recorded observations of wolves in the lower 48 states.  

```r
ggmap(map2) + 
  geom_point(data = wolves_lower_48, aes(long, lat)) +
  labs(x= "Longitude", y= "Latitude", title="Wolves in the Lower 48")
```

![](lab13_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

10. Use the map from #9 above, but add some aesthetics. Try to `fill` and `color` by population.  

```r
ggmap(map2) + 
  geom_point(data = wolves_lower_48, aes(long, lat, fill=pop, color=pop), size=2.0) +
  labs(x= "Longitude", y= "Latitude", title="Wolves in the Lower 48")
```

![](lab13_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
