---
title: "Lab 6 Homework"
author: "Joel Ledford"
date: "2022-01-27"
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
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
dim(fisheries)
```

```
## [1] 17692    71
```

```r
summary(fisheries)
```

```
##    Country          Common name        ISSCAAP group#  ISSCAAP taxonomic group
##  Length:17692       Length:17692       Min.   :11.00   Length:17692           
##  Class :character   Class :character   1st Qu.:33.00   Class :character       
##  Mode  :character   Mode  :character   Median :36.00   Mode  :character       
##                                        Mean   :37.38                          
##                                        3rd Qu.:38.00                          
##                                        Max.   :77.00                          
##  ASFIS species#     ASFIS species name FAO major fishing area
##  Length:17692       Length:17692       Min.   :18.00         
##  Class :character   Class :character   1st Qu.:31.00         
##  Mode  :character   Mode  :character   Median :37.00         
##                                        Mean   :45.34         
##                                        3rd Qu.:57.00         
##                                        Max.   :88.00         
##    Measure              1950               1951               1952          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1953               1954               1955               1956          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1957               1958               1959               1960          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1961               1962               1963               1964          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1965               1966               1967               1968          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1969               1970               1971               1972          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1973               1974               1975               1976          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1977               1978               1979               1980          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1981               1982               1983               1984          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1985               1986               1987               1988          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1989               1990               1991               1992          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1993               1994               1995               1996          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1997               1998               1999               2000          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2001               2002               2003               2004          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2005               2006               2007               2008          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2009               2010               2011               2012          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
## 
```

```r
anyNA(fisheries)
```

```
## [1] TRUE
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- janitor::clean_names(fisheries)
```


```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
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

3. How many countries are represented in the data? Provide a count and list their names.

```r
levels(fisheries_tidy$country)
```

```
##   [1] "Albania"                   "Algeria"                  
##   [3] "American Samoa"            "Angola"                   
##   [5] "Anguilla"                  "Antigua and Barbuda"      
##   [7] "Argentina"                 "Aruba"                    
##   [9] "Australia"                 "Bahamas"                  
##  [11] "Bahrain"                   "Bangladesh"               
##  [13] "Barbados"                  "Belgium"                  
##  [15] "Belize"                    "Benin"                    
##  [17] "Bermuda"                   "Bonaire/S.Eustatius/Saba" 
##  [19] "Bosnia and Herzegovina"    "Brazil"                   
##  [21] "British Indian Ocean Ter"  "British Virgin Islands"   
##  [23] "Brunei Darussalam"         "Bulgaria"                 
##  [25] "C<f4>te d'Ivoire"          "Cabo Verde"               
##  [27] "Cambodia"                  "Cameroon"                 
##  [29] "Canada"                    "Cayman Islands"           
##  [31] "Channel Islands"           "Chile"                    
##  [33] "China"                     "China, Hong Kong SAR"     
##  [35] "China, Macao SAR"          "Colombia"                 
##  [37] "Comoros"                   "Congo, Dem. Rep. of the"  
##  [39] "Congo, Republic of"        "Cook Islands"             
##  [41] "Costa Rica"                "Croatia"                  
##  [43] "Cuba"                      "Cura<e7>ao"               
##  [45] "Cyprus"                    "Denmark"                  
##  [47] "Djibouti"                  "Dominica"                 
##  [49] "Dominican Republic"        "Ecuador"                  
##  [51] "Egypt"                     "El Salvador"              
##  [53] "Equatorial Guinea"         "Eritrea"                  
##  [55] "Estonia"                   "Ethiopia"                 
##  [57] "Falkland Is.(Malvinas)"    "Faroe Islands"            
##  [59] "Fiji, Republic of"         "Finland"                  
##  [61] "France"                    "French Guiana"            
##  [63] "French Polynesia"          "French Southern Terr"     
##  [65] "Gabon"                     "Gambia"                   
##  [67] "Georgia"                   "Germany"                  
##  [69] "Ghana"                     "Gibraltar"                
##  [71] "Greece"                    "Greenland"                
##  [73] "Grenada"                   "Guadeloupe"               
##  [75] "Guam"                      "Guatemala"                
##  [77] "Guinea"                    "GuineaBissau"             
##  [79] "Guyana"                    "Haiti"                    
##  [81] "Honduras"                  "Iceland"                  
##  [83] "India"                     "Indonesia"                
##  [85] "Iran (Islamic Rep. of)"    "Iraq"                     
##  [87] "Ireland"                   "Isle of Man"              
##  [89] "Israel"                    "Italy"                    
##  [91] "Jamaica"                   "Japan"                    
##  [93] "Jordan"                    "Kenya"                    
##  [95] "Kiribati"                  "Korea, Dem. People's Rep" 
##  [97] "Korea, Republic of"        "Kuwait"                   
##  [99] "Latvia"                    "Lebanon"                  
## [101] "Liberia"                   "Libya"                    
## [103] "Lithuania"                 "Madagascar"               
## [105] "Malaysia"                  "Maldives"                 
## [107] "Malta"                     "Marshall Islands"         
## [109] "Martinique"                "Mauritania"               
## [111] "Mauritius"                 "Mayotte"                  
## [113] "Mexico"                    "Micronesia, Fed.States of"
## [115] "Monaco"                    "Montenegro"               
## [117] "Montserrat"                "Morocco"                  
## [119] "Mozambique"                "Myanmar"                  
## [121] "Namibia"                   "Nauru"                    
## [123] "Netherlands"               "Netherlands Antilles"     
## [125] "New Caledonia"             "New Zealand"              
## [127] "Nicaragua"                 "Nigeria"                  
## [129] "Niue"                      "Norfolk Island"           
## [131] "Northern Mariana Is."      "Norway"                   
## [133] "Oman"                      "Other nei"                
## [135] "Pakistan"                  "Palau"                    
## [137] "Palestine, Occupied Tr."   "Panama"                   
## [139] "Papua New Guinea"          "Peru"                     
## [141] "Philippines"               "Pitcairn Islands"         
## [143] "Poland"                    "Portugal"                 
## [145] "Puerto Rico"               "Qatar"                    
## [147] "R<e9>union"                "Romania"                  
## [149] "Russian Federation"        "Saint Barth<e9>lemy"      
## [151] "Saint Helena"              "Saint Kitts and Nevis"    
## [153] "Saint Lucia"               "Saint Vincent/Grenadines" 
## [155] "SaintMartin"               "Samoa"                    
## [157] "Sao Tome and Principe"     "Saudi Arabia"             
## [159] "Senegal"                   "Serbia and Montenegro"    
## [161] "Seychelles"                "Sierra Leone"             
## [163] "Singapore"                 "Sint Maarten"             
## [165] "Slovenia"                  "Solomon Islands"          
## [167] "Somalia"                   "South Africa"             
## [169] "Spain"                     "Sri Lanka"                
## [171] "St. Pierre and Miquelon"   "Sudan"                    
## [173] "Sudan (former)"            "Suriname"                 
## [175] "Svalbard and Jan Mayen"    "Sweden"                   
## [177] "Syrian Arab Republic"      "Taiwan Province of China" 
## [179] "Tanzania, United Rep. of"  "Thailand"                 
## [181] "TimorLeste"                "Togo"                     
## [183] "Tokelau"                   "Tonga"                    
## [185] "Trinidad and Tobago"       "Tunisia"                  
## [187] "Turkey"                    "Turks and Caicos Is."     
## [189] "Tuvalu"                    "Ukraine"                  
## [191] "Un. Sov. Soc. Rep."        "United Arab Emirates"     
## [193] "United Kingdom"            "United States of America" 
## [195] "Uruguay"                   "US Virgin Islands"        
## [197] "Vanuatu"                   "Venezuela, Boliv Rep of"  
## [199] "Viet Nam"                  "Wallis and Futuna Is."    
## [201] "Western Sahara"            "Yemen"                    
## [203] "Yugoslavia SFR"            "Zanzibar"
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_tidy_focused <- select(fisheries_tidy, country, isscaap_group_number, asfis_species_name, asfis_species_number, year, catch)
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries_tidy_focused%>%
  summarise(sum_distinct = n_distinct(asfis_species_number))
```

```
## # A tibble: 1 x 1
##   sum_distinct
##          <int>
## 1         1551
```

6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy_focused %>%
  select(year,catch,country) %>%
  filter(year==2000) %>%
  group_by(country) %>%
  summarise(year2000_Overall_Catch = sum(catch, na.rm=TRUE)) %>%
  slice_max(year2000_Overall_Catch, n=10)
```

```
## # A tibble: 10 x 2
##    country                  year2000_Overall_Catch
##    <fct>                                     <dbl>
##  1 China                                     25899
##  2 Russian Federation                        12181
##  3 United States of America                  11762
##  4 Japan                                      8510
##  5 Indonesia                                  8341
##  6 Peru                                       7443
##  7 Chile                                      6906
##  8 India                                      6351
##  9 Thailand                                   6243
## 10 Korea, Republic of                         6124
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy_focused %>%
  filter(asfis_species_name == "Sardina pilchardus") %>%
  filter(between(year, 1990, 2000)) %>%
  group_by(country) %>%
  summarise(total_sardines = sum(catch, na.rm=T)) %>%
  arrange(desc(total_sardines))
```

```
## # A tibble: 37 x 2
##    country               total_sardines
##    <fct>                          <dbl>
##  1 Morocco                         7470
##  2 Spain                           3507
##  3 Russian Federation              1639
##  4 Ukraine                         1030
##  5 France                           966
##  6 Portugal                         818
##  7 Greece                           528
##  8 Italy                            507
##  9 Serbia and Montenegro            478
## 10 Denmark                          477
## # ... with 27 more rows
```

8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy%>%
  filter(isscaap_taxonomic_group == "Squids, cuttlefishes, octopuses")%>%
  filter(between(year, 2008, 2012))%>%
  group_by(country)%>%
  summarise(total_catch = sum(catch, na.rm = T))%>%
  arrange(desc(total_catch))
```

```
## # A tibble: 122 x 2
##    country                  total_catch
##    <fct>                          <dbl>
##  1 China                           8349
##  2 Korea, Republic of              3480
##  3 Peru                            3422
##  4 Japan                           3248
##  5 Chile                           2775
##  6 United States of America        2417
##  7 Indonesia                       1622
##  8 Taiwan Province of China        1394
##  9 Spain                           1147
## 10 France                          1138
## # ... with 112 more rows
```



9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy_focused%>%
  filter(year>=2008 & year<=2012)%>%
  group_by(asfis_species_name)%>%
  summarise(catch_total=sum(catch,na.rm=T))%>%
  arrange(desc(catch_total))
```

```
## # A tibble: 1,472 x 2
##    asfis_species_name    catch_total
##    <chr>                       <dbl>
##  1 Osteichthyes               107808
##  2 Theragra chalcogramma       41075
##  3 Engraulis ringens           35523
##  4 Katsuwonus pelamis          32153
##  5 Trichiurus lepturus         30400
##  6 Clupea harengus             28527
##  7 Thunnus albacares           20119
##  8 Scomber japonicus           14723
##  9 Gadus morhua                13253
## 10 Thunnus alalunga            12019
## # ... with 1,462 more rows
```

10. Use the data to do at least one analysis of your choice.

```r
fisheries_tidy_focused%>%
  filter(year<2006 & year<=2012)%>%
  group_by(asfis_species_name)%>%
  summarise(catch_total=sum(catch,na.rm=T))%>%
  arrange(desc(catch_total))
```

```
## # A tibble: 1,406 x 2
##    asfis_species_name      catch_total
##    <chr>                         <dbl>
##  1 Osteichthyes                 704562
##  2 Engraulis ringens            486101
##  3 Theragra chalcogramma        418534
##  4 Clupea harengus              228261
##  5 Gadus morhua                 200633
##  6 Sardinops sagax              175669
##  7 Scomber japonicus            154212
##  8 Katsuwonus pelamis           147301
##  9 Sardinops melanostictus      141299
## 10 Trachurus murphyi            129335
## # ... with 1,396 more rows
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
