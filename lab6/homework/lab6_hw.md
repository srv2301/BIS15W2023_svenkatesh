---
title: "Lab 6 Homework"
author: "Joel Ledford"
date: "2023-02-01"
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

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
names(fisheries)
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```

```r
dim(fisheries)
```

```
## [1] 17692    71
```

```r
anyNA(fisheries)
```

```
## [1] TRUE
```

```r
class(fisheries)
```

```
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```

```r
fisheries
```

```
## # A tibble: 17,692 × 71
##    Country Commo…¹ ISSCA…² ISSCA…³ ASFIS…⁴ ASFIS…⁵ FAO m…⁶ Measure `1950` `1951`
##    <chr>   <chr>     <dbl> <chr>   <chr>   <chr>     <dbl> <chr>   <chr>  <chr> 
##  1 Albania Angels…      38 Sharks… 10903X… Squati…      37 Quanti… <NA>   <NA>  
##  2 Albania Atlant…      36 Tunas,… 175010… Sarda …      37 Quanti… <NA>   <NA>  
##  3 Albania Barrac…      37 Miscel… 177100… Sphyra…      37 Quanti… <NA>   <NA>  
##  4 Albania Blue a…      45 Shrimp… 228020… Ariste…      37 Quanti… <NA>   <NA>  
##  5 Albania Blue w…      32 Cods, … 148040… Microm…      37 Quanti… <NA>   <NA>  
##  6 Albania Bluefi…      37 Miscel… 170202… Pomato…      37 Quanti… <NA>   <NA>  
##  7 Albania Bogue        33 Miscel… 170392… Boops …      37 Quanti… <NA>   <NA>  
##  8 Albania Caramo…      45 Shrimp… 228010… Penaeu…      37 Quanti… <NA>   <NA>  
##  9 Albania Catsha…      38 Sharks… 108010… Scylio…      37 Quanti… <NA>   <NA>  
## 10 Albania Common…      57 Squids… 321020… Sepia …      37 Quanti… <NA>   <NA>  
## # … with 17,682 more rows, 61 more variables: `1952` <chr>, `1953` <chr>,
## #   `1954` <chr>, `1955` <chr>, `1956` <chr>, `1957` <chr>, `1958` <chr>,
## #   `1959` <chr>, `1960` <chr>, `1961` <chr>, `1962` <chr>, `1963` <chr>,
## #   `1964` <chr>, `1965` <chr>, `1966` <chr>, `1967` <chr>, `1968` <chr>,
## #   `1969` <chr>, `1970` <chr>, `1971` <chr>, `1972` <chr>, `1973` <chr>,
## #   `1974` <chr>, `1975` <chr>, `1976` <chr>, `1977` <chr>, `1978` <chr>,
## #   `1979` <chr>, `1980` <chr>, `1981` <chr>, `1982` <chr>, `1983` <chr>, …
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- janitor::clean_names(fisheries)
```


```r
fisheries <- fisheries %>% 
  mutate_if(is.character, factor)
```


```r
view(fisheries)
```


We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
 mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
 mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
 mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
 mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
 mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```


```r
view(fisheries_tidy)
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
fisheries_tidy %>% 
  group_by(country) %>% 
  summarise(n_countries = n())
```

```
## # A tibble: 203 × 2
##    country             n_countries
##    <fct>                     <int>
##  1 Albania                     934
##  2 Algeria                    1561
##  3 American Samoa              556
##  4 Angola                     2119
##  5 Anguilla                    129
##  6 Antigua and Barbuda         356
##  7 Argentina                  3403
##  8 Aruba                       172
##  9 Australia                  8183
## 10 Bahamas                     423
## # … with 193 more rows
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_tidy <- fisheries_tidy %>% 
  select(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)
```


```r
fisheries_tidy
```

```
## # A tibble: 376,771 × 6
##    country isscaap_taxonomic_group asfis_species_name asfis_specie…¹  year catch
##    <fct>   <fct>                   <fct>              <fct>          <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1995    NA
##  2 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1996    53
##  3 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1997    20
##  4 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1998    31
##  5 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1999    30
##  6 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2000    30
##  7 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2001    16
##  8 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2002    79
##  9 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2003     1
## 10 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2004     4
## # … with 376,761 more rows, and abbreviated variable name ¹​asfis_species_number
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries_tidy %>% 
  group_by(asfis_species_number) %>% 
  summarise(n_fishes = n_distinct(asfis_species_number),
            n_total = n())
```

```
## # A tibble: 1,551 × 3
##    asfis_species_number n_fishes n_total
##    <fct>                   <int>   <int>
##  1 1020100101                  1     103
##  2 1020100201                  1       2
##  3 10201XXXXX                  1      23
##  4 1030300603                  1      10
##  5 10303XXXXX                  1      54
##  6 1050200201                  1      45
##  7 1050200301                  1       4
##  8 1050200502                  1      26
##  9 1060100301                  1     119
## 10 1060200501                  1      23
## # … with 1,541 more rows
```

6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy %>% 
  filter(year == 2000) %>% 
  group_by(country) %>% 
  summarise(catch_total = sum(catch, na.rm = T)) %>% 
  arrange(-catch_total)
```

```
## # A tibble: 193 × 2
##    country                  catch_total
##    <fct>                          <dbl>
##  1 China                          25899
##  2 Russian Federation             12181
##  3 United States of America       11762
##  4 Japan                           8510
##  5 Indonesia                       8341
##  6 Peru                            7443
##  7 Chile                           6906
##  8 India                           6351
##  9 Thailand                        6243
## 10 Korea, Republic of              6124
## # … with 183 more rows
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy %>% 
  group_by(country) %>% 
  filter(between(year, 1990, 2000)) %>% 
  filter(asfis_species_name == "Sardina pilchardus") %>% 
  summarise(n_sardines = sum(catch, na.rm = T)) %>% 
  arrange(-n_sardines)
```

```
## # A tibble: 37 × 2
##    country               n_sardines
##    <fct>                      <dbl>
##  1 Morocco                     7470
##  2 Spain                       3507
##  3 Russian Federation          1639
##  4 Ukraine                     1030
##  5 France                       966
##  6 Portugal                     818
##  7 Greece                       528
##  8 Italy                        507
##  9 Serbia and Montenegro        478
## 10 Denmark                      477
## # … with 27 more rows
```

8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy %>% 
  filter(between(year, 2008, 2012)) %>% 
  filter(isscaap_taxonomic_group == "Squids, cuttlefishes, octopuses") %>% 
  group_by(country) %>% 
  summarise(n_cephalopods = sum(catch, na.rm = T)) %>% 
  head(n = 5) %>% 
  arrange(-n_cephalopods)
```

```
## # A tibble: 5 × 2
##   country        n_cephalopods
##   <fct>                  <dbl>
## 1 Argentina                814
## 2 Albania                  631
## 3 Angola                   223
## 4 Algeria                  162
## 5 American Samoa             1
```

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy %>% 
  group_by(asfis_species_name) %>% 
  filter(between(year, 2008, 2012)) %>% 
  filter(asfis_species_name!="Osteichthyes") %>% 
  summarise(n_catch = sum(catch, na.rm = T)) %>% 
  arrange(-n_catch)
```

```
## # A tibble: 1,471 × 2
##    asfis_species_name    n_catch
##    <fct>                   <dbl>
##  1 Theragra chalcogramma   41075
##  2 Engraulis ringens       35523
##  3 Katsuwonus pelamis      32153
##  4 Trichiurus lepturus     30400
##  5 Clupea harengus         28527
##  6 Thunnus albacares       20119
##  7 Scomber japonicus       14723
##  8 Gadus morhua            13253
##  9 Thunnus alalunga        12019
## 10 Natantia                11984
## # … with 1,461 more rows
```

10. Use the data to do at least one analysis of your choice.

```r
summary(fisheries_tidy)
```

```
##                      country                        isscaap_taxonomic_group
##  United States of America: 18080   Miscellaneous coastal fishes : 69821    
##  Spain                   : 17482   Tunas, bonitos, billfishes   : 61839    
##  Japan                   : 15429   Miscellaneous pelagic fishes : 38992    
##  Portugal                : 11570   Miscellaneous demersal fishes: 27360    
##  Korea, Republic of      : 10824   Sharks, rays, chimaeras      : 23210    
##  France                  : 10639   Cods, hakes, haddocks        : 21616    
##  (Other)                 :292747   (Other)                      :133933    
##           asfis_species_name    asfis_species_number      year     
##  Osteichthyes      : 16204   199XXXXXXX010: 14289    Min.   :1950  
##  Thunnus albacares :  6866   1750102610   :  6866    1st Qu.:1977  
##  Elasmobranchii    :  6405   199XXXXXXX054:  6405    Median :1991  
##  Katsuwonus pelamis:  5785   1750102501   :  5785    Mean   :1989  
##  Thunnus obesus    :  5341   1750102612   :  5341    3rd Qu.:2003  
##  Xiphias gladius   :  5143   1750400301   :  5143    Max.   :2012  
##  (Other)           :331027   (Other)      :332942                  
##      catch         
##  Min.   :    0.00  
##  1st Qu.:    2.00  
##  Median :    7.00  
##  Mean   :   39.36  
##  3rd Qu.:   32.00  
##  Max.   :77000.00  
##  NA's   :133583
```

```r
fisheries_tidy %>% 
  group_by(country) %>% 
  filter(year == "2004") %>% 
  summarise(n_catch = sum(catch, na.rm = T)) %>% 
  arrange(-n_catch)
```

```
## # A tibble: 193 × 2
##    country                  n_catch
##    <fct>                      <dbl>
##  1 China                      24420
##  2 United States of America   19343
##  3 Chile                      13571
##  4 Peru                        9983
##  5 Myanmar                     9640
##  6 Japan                       8212
##  7 Spain                       7896
##  8 Indonesia                   7522
##  9 Russian Federation          6225
## 10 Portugal                    6168
## # … with 183 more rows
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
