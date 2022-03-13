---
title: "Lab 12 Homework"
author: "Please Add Your Name Here"
date: "2022-03-12"
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
library(ggmap)
```




## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  
2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure. As part of this step, produce a summary that provides the range of latitude and longitude so you can build an appropriate bounding box.

```r
grizzly <- read_csv(here("lab12", "data", "bear-sightings.csv")) %>% clean_names()
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
grizzly
```

```
## # A tibble: 494 × 3
##    bear_id longitude latitude
##      <dbl>     <dbl>    <dbl>
##  1       7     -149.     62.7
##  2      57     -153.     58.4
##  3      69     -145.     62.4
##  4      75     -153.     59.9
##  5     104     -143.     61.1
##  6     108     -150.     62.9
##  7     115     -152.     68.0
##  8     116     -147.     62.6
##  9     125     -157.     60.2
## 10     135     -156.     58.9
## # … with 484 more rows
```

2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.

```r
latitude <- c(55.02, 70.37)
longitude <- c(-166.2, -131.3)
bobox <- make_bbox(longitude, latitude, f = 0.05)
```

3. Load a map from `stamen` in a terrain style projection and display the map.

```r
mapter <- get_map(bobox, maptype = "terrain", source = "stamen")
```

```
## Source : http://tile.stamen.com/terrain/5/1/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/10.png
```

```r
ggmap(mapter)
```

![](lab12_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.

Let's switch to the wolves data. Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  



```r
ggmap(mapter) + 
  geom_point(data = grizzly, aes(longitude, latitude, alpha=0.1))+
  labs(title = "Grizzly Sightings in Alaska",
       x="Longitude",
       y="Latitude")
```

![](lab12_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

5. Load the data and evaluate its structure.  

```r
wolves <- read_csv(here("lab12", "data", "wolves_data", "wolves_dataset.csv"))
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

```r
wolves
```

```
## # A tibble: 1,986 × 23
##    pop     year age.cat sex   color   lat  long habitat human pop.density
##    <chr>  <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
##  1 AK.PEN  2006 S       F     G      57.0 -158.    254.  10.4           8
##  2 AK.PEN  2006 S       M     G      57.0 -158.    254.  10.4           8
##  3 AK.PEN  2006 A       F     G      57.0 -158.    254.  10.4           8
##  4 AK.PEN  2006 S       M     B      57.0 -158.    254.  10.4           8
##  5 AK.PEN  2006 A       M     B      57.0 -158.    254.  10.4           8
##  6 AK.PEN  2006 A       M     G      57.0 -158.    254.  10.4           8
##  7 AK.PEN  2006 A       F     G      57.0 -158.    254.  10.4           8
##  8 AK.PEN  2006 P       M     G      57.0 -158.    254.  10.4           8
##  9 AK.PEN  2006 S       F     G      57.0 -158.    254.  10.4           8
## 10 AK.PEN  2006 P       M     G      57.0 -158.    254.  10.4           8
## # … with 1,976 more rows, and 13 more variables: pack.size <dbl>,
## #   standard.habitat <dbl>, standard.human <dbl>, standard.pop <dbl>,
## #   standard.packsize <dbl>, standard.latitude <dbl>, standard.longitude <dbl>,
## #   cav.binary <dbl>, cdv.binary <dbl>, cpv.binary <dbl>, chv.binary <dbl>,
## #   neo.binary <dbl>, toxo.binary <dbl>
```

6. How many distinct wolf populations are included in this study? Mae a new object that restricts the data to the wolf populations in the lower 48 US states.

```r
n_distinct(wolves$pop)
```

```
## [1] 17
```

```r
wolves %>%
  count(pop)
```

```
## # A tibble: 17 × 2
##    pop         n
##    <chr>   <int>
##  1 AK.PEN    100
##  2 BAN.JAS    96
##  3 BC        145
##  4 DENALI    154
##  5 ELLES      11
##  6 GTNP       60
##  7 INT.AK     35
##  8 MEXICAN   181
##  9 MI        102
## 10 MT        351
## 11 N.NWT      67
## 12 ONT        60
## 13 SE.AK      10
## 14 SNF        92
## 15 SS.NWT     34
## 16 YNP       383
## 17 YUCH      105
```

```r
wolves_48 <- wolves %>%
  filter(lat<=49.5 & !pop=="MEXICAN")
wolves_48
```

```
## # A tibble: 988 × 23
##    pop    year age.cat sex   color   lat  long habitat human pop.density
##    <chr> <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
##  1 GTNP   2012 P       M     G      43.8 -111.  10375. 3924.        34.0
##  2 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
##  3 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
##  4 GTNP   2012 P       M     B      43.8 -111.  10375. 3924.        34.0
##  5 GTNP   2013 A       F     G      43.8 -111.  10375. 3924.        34.0
##  6 GTNP   2013 A       M     G      43.8 -111.  10375. 3924.        34.0
##  7 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
##  8 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
##  9 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
## 10 GTNP   2013 P       F     G      43.8 -111.  10375. 3924.        34.0
## # … with 978 more rows, and 13 more variables: pack.size <dbl>,
## #   standard.habitat <dbl>, standard.human <dbl>, standard.pop <dbl>,
## #   standard.packsize <dbl>, standard.latitude <dbl>, standard.longitude <dbl>,
## #   cav.binary <dbl>, cdv.binary <dbl>, cpv.binary <dbl>, chv.binary <dbl>,
## #   neo.binary <dbl>, toxo.binary <dbl>
```
 


7. Use the range of the latitude and longitude to build an appropriate bounding box for your map.

```r
long <- c(-110.99, -86.82)
lat <- c(43.82, 47.75)
wolf_bbox <- make_bbox(long, lat, f=0.05)
```

8.  Load a map from `stamen` in a `terrain-lines` projection and display the map.

```r
wolf_48 <- get_map(wolf_bbox, maptype = "terrain-lines", source= "stamen")
```

```
## Source : http://tile.stamen.com/terrain/7/24/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/25/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/26/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/27/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/28/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/29/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/30/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/31/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/32/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/33/44.png
```

```
## Source : http://tile.stamen.com/terrain/7/24/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/25/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/26/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/27/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/28/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/29/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/30/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/31/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/32/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/33/45.png
```

```
## Source : http://tile.stamen.com/terrain/7/24/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/25/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/26/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/27/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/28/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/29/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/30/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/31/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/32/46.png
```

```
## Source : http://tile.stamen.com/terrain/7/33/46.png
```

```r
ggmap(wolf_48)
```

![](lab12_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


9. Build a final map that overlays the recorded observations of wolves in the lower 48 states.

```r
ggmap(wolf_48) +geom_point(data=wolves_48, aes(long,lat))
```

![](lab12_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

10. Use the map from #9 above, but add some aesthetics. Try to `fill` and `color` by population.



```r
library(paletteer)
```





```r
ggmap(wolf_48) +geom_point(data=wolves_48, aes(long,lat, color=pop)) +labs(title = "Wolf Pathogen Exposure Locations")  +theme_dark() +theme(plot.title=element_text(size=15))
```

![](lab12_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


```
## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
