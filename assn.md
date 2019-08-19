\#Question 1 : Case on Green Buildings

Do you agree with the conclusions of her on-staff stats guru? If so,
point to evidence supporting his case. If not, explain specifically
where and why the analysis goes wrong, and how it can be improved. (For
example, do you see the possibility of confounding variables for the
relationship between rent and green status?) Tell your story mainly in
pictures, with appropriate introductory and supporting text.

\#\#Approach

First, several set of parameters were looked into and their correlations
and their effect on the rent for the property was looked by different
graphs. First few graphs were plotted to see the overall distribution of
the data. Few additional columns were created to combine multiple
redundant columns and convert zeros and ones to categorical variables.

    ## -- Attaching packages ------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.2.0     v purrr   0.3.2
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   0.8.3     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts ---------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    ## Loading required package: lattice

    ## Loading required package: ggformula

    ## Loading required package: ggstance

    ## 
    ## Attaching package: 'ggstance'

    ## The following objects are masked from 'package:ggplot2':
    ## 
    ##     geom_errorbarh, GeomErrorbarh

    ## 
    ## New to ggformula?  Try the tutorials: 
    ##  learnr::run_tutorial("introduction", package = "ggformula")
    ##  learnr::run_tutorial("refining", package = "ggformula")

    ## Loading required package: mosaicData

    ## Loading required package: Matrix

    ## 
    ## Attaching package: 'Matrix'

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     expand

    ## Registered S3 method overwritten by 'mosaic':
    ##   method                           from   
    ##   fortify.SpatialPolygonsDataFrame ggplot2

    ## 
    ## The 'mosaic' package masks several functions from core packages in order to add 
    ## additional features.  The original behavior of these functions should not be affected by this.
    ## 
    ## Note: If you use the Matrix package, be sure to load it BEFORE loading mosaic.

    ## 
    ## Attaching package: 'mosaic'

    ## The following object is masked from 'package:Matrix':
    ## 
    ##     mean

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     count, do, tally

    ## The following object is masked from 'package:purrr':
    ## 
    ##     cross

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     stat

    ## The following objects are masked from 'package:stats':
    ## 
    ##     binom.test, cor, cor.test, cov, fivenum, IQR, median,
    ##     prop.test, quantile, sd, t.test, var

    ## The following objects are masked from 'package:base':
    ## 
    ##     max, mean, min, prod, range, sample, sum

![](assn_files/figure-markdown_github/setup-1.png)![](assn_files/figure-markdown_github/setup-2.png)![](assn_files/figure-markdown_github/setup-3.png)
\#\#categorization of continous vairables

Categorization of few continous variables like Age, electricity price
and the total days above/below outside temperature for us to use them
for visualization

``` r
d1 = data %>%
  mutate(agecat = cut(age, c(0,15,30,100)))%>%
  mutate(elecat = cut(Electricity_Costs, c(0,0.01,0.02,0.03,0.05,0.1)))%>%
  mutate(td = cut(total_dd_07, c(0,2000,4000,6000,8000)))
summary(data)
```

    ##  CS_PropertyID        cluster            size            empl_gr       
    ##  Min.   :      1   Min.   :   1.0   Min.   :   1624   Min.   :-24.950  
    ##  1st Qu.: 157452   1st Qu.: 272.0   1st Qu.:  50891   1st Qu.:  1.740  
    ##  Median : 313253   Median : 476.0   Median : 128838   Median :  1.970  
    ##  Mean   : 453003   Mean   : 588.6   Mean   : 234638   Mean   :  3.207  
    ##  3rd Qu.: 441188   3rd Qu.:1044.0   3rd Qu.: 294212   3rd Qu.:  2.380  
    ##  Max.   :6208103   Max.   :1230.0   Max.   :3781045   Max.   : 67.780  
    ##                                                       NA's   :74       
    ##       Rent         leasing_rate       stories            age        
    ##  Min.   :  2.98   Min.   :  0.00   Min.   :  1.00   Min.   :  0.00  
    ##  1st Qu.: 19.50   1st Qu.: 77.85   1st Qu.:  4.00   1st Qu.: 23.00  
    ##  Median : 25.16   Median : 89.53   Median : 10.00   Median : 34.00  
    ##  Mean   : 28.42   Mean   : 82.61   Mean   : 13.58   Mean   : 47.24  
    ##  3rd Qu.: 34.18   3rd Qu.: 96.44   3rd Qu.: 19.00   3rd Qu.: 79.00  
    ##  Max.   :250.00   Max.   :100.00   Max.   :110.00   Max.   :187.00  
    ##                                                                     
    ##    renovated         class_a          class_b            LEED         
    ##  Min.   :0.0000   Min.   :0.0000   Min.   :0.0000   Min.   :0.000000  
    ##  1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.000000  
    ##  Median :0.0000   Median :0.0000   Median :0.0000   Median :0.000000  
    ##  Mean   :0.3795   Mean   :0.3999   Mean   :0.4595   Mean   :0.006841  
    ##  3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:0.000000  
    ##  Max.   :1.0000   Max.   :1.0000   Max.   :1.0000   Max.   :1.000000  
    ##                                                                       
    ##    Energystar      green_rating           net              amenities     
    ##  Min.   :0.00000   Length:7894        Length:7894        Min.   :0.0000  
    ##  1st Qu.:0.00000   Class :character   Class :character   1st Qu.:0.0000  
    ##  Median :0.00000   Mode  :character   Mode  :character   Median :1.0000  
    ##  Mean   :0.08082                                         Mean   :0.5266  
    ##  3rd Qu.:0.00000                                         3rd Qu.:1.0000  
    ##  Max.   :1.00000                                         Max.   :1.0000  
    ##                                                                          
    ##   cd_total_07     hd_total07    total_dd_07   Precipitation  
    ##  Min.   :  39   Min.   :   0   Min.   :2103   Min.   :10.46  
    ##  1st Qu.: 684   1st Qu.:1419   1st Qu.:2869   1st Qu.:22.71  
    ##  Median : 966   Median :2739   Median :4979   Median :23.16  
    ##  Mean   :1229   Mean   :3432   Mean   :4661   Mean   :31.08  
    ##  3rd Qu.:1620   3rd Qu.:4796   3rd Qu.:6413   3rd Qu.:43.89  
    ##  Max.   :5240   Max.   :7200   Max.   :8244   Max.   :58.02  
    ##                                                              
    ##    Gas_Costs        Electricity_Costs  cluster_rent      class          
    ##  Min.   :0.009487   Min.   :0.01780   Min.   : 9.00   Length:7894       
    ##  1st Qu.:0.010296   1st Qu.:0.02330   1st Qu.:20.00   Class :character  
    ##  Median :0.010296   Median :0.03274   Median :25.14   Mode  :character  
    ##  Mean   :0.011336   Mean   :0.03096   Mean   :27.50                     
    ##  3rd Qu.:0.011816   3rd Qu.:0.03781   3rd Qu.:34.00                     
    ##  Max.   :0.028914   Max.   :0.06280   Max.   :71.44                     
    ##                                                                         
    ##   greentype        
    ##  Length:7894       
    ##  Class :character  
    ##  Mode  :character  
    ##                    
    ##                    
    ##                    
    ## 

``` r
d1 = na.omit(d1)
d1 = subset(d1, !is.na(age))
```

\#\#Data Analysis and Visualization

\#\#\#Distribution of Green Buidlings across different categories

![](assn_files/figure-markdown_github/pressure-1.png)![](assn_files/figure-markdown_github/pressure-2.png)![](assn_files/figure-markdown_github/pressure-3.png)![](assn_files/figure-markdown_github/pressure-4.png)

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.

``` r
d4 = d1 %>%
  group_by(green_rating, class, agecat) %>%
  summarize(medianrent = median(Rent),countof =n())

ggplot(data = d4) + 
  geom_bar(mapping = aes(x=green_rating, y=countof, fill=agecat),
           stat='identity', position ='dodge') + 
  facet_wrap(~class) + 
  labs(title="Count of units", 
       y="Count of Units",
       x = "Green Rating",
       fill="Age")
```

![](assn_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
ggplot(data = d4) + 
  geom_bar(mapping = aes(x=green_rating, y=medianrent, fill=agecat),
           stat='identity', position ='dodge') + 
  facet_wrap(~class) + 
  labs(title="Distribution of Medianrent per Sqft", 
       y="Medianrent",
       x = "Green Ratings",
       fill="Age")
```

![](assn_files/figure-markdown_github/unnamed-chunk-1-2.png) From the
graphs it is evident that there is no considerable increase in
Medianrent for the non-green building to green building for any of the
classes and age groups. So this negates the conclusion of the Stats
person at the company, Though his observation was right, it was due to
higher share of Class A buildings and recent buildings in green rated
buildings than merely because of it being a green building. So for the
stats guru to improve his suggestion, he should sub-divide the analysis
across the class of building and age group for him to improve his
suggestion

\#\#\#Net Contract Level Analysis

``` r
d3net = d1 %>%
  group_by(green_rating, net) %>%
  summarize(medianrent = median(Rent),countof =n())

ggplot(data = d3net) + 
  geom_bar(mapping = aes(x=green_rating, y=medianrent, fill=net),
           stat='identity', position ='dodge') + 
  
  labs(title="MedianRent vs greenrating at contract level", 
       y="MedianRent",
       x = "Green_Rating",
       fill="Net Contract")
```

![](assn_files/figure-markdown_github/unnamed-chunk-2-1.png) Analysis
was done to figure out if there is any relation between the contract
type charges and the green buildings, the underlying phenomenean being
that green building reduce the consumption power, due to their design,
so should ideally have lesser difference between Net contract level and
Non net Contract. However this was not observed.

\#\#\#Net Contract Level Analysis with relation to Electricity Prices

``` r
d3netele = d1 %>%
  group_by(green_rating, net,elecat) %>%
  summarize(meanrent = median(Rent),countof =n())
ggplot(data = d3netele) + 
  geom_bar(mapping = aes(x=elecat, y=meanrent, fill=net),
           stat='identity', position ='dodge') + 
  facet_wrap(~green_rating) + 
  labs(title="Medianrent at Electricity price category level", 
       y="Medianrent",
       x = "Electricity price per Unit",
       fill="Net Contract")
```

![](assn_files/figure-markdown_github/unnamed-chunk-3-1.png) The
underlying assumption was to compare Net Contract to Non Net contract
and understand if there is any correlation between the electricity
charges with the difference between the contract and non contract
type.This is done to look into option of non net contract offering which
can help over the years in competitive pricing as the electricity and
gas prices hike and green building usually consume lesser electricity
and gas. As it can be observed from graphs above that the difference of
rent between the net and non net is higher for Non\_green buildings, in
higher electricity rate bracket of (.03 to .05).

\#\#\#Degree Days with Contract rent with Medianrent Comparison

``` r
d3netele = d1 %>%
  group_by(green_rating, net,td) %>%
  summarize(meanrent = median(Rent),countof =n())

ggplot(data = d3netele) + 
  geom_bar(mapping = aes(x=td, y=meanrent, fill=net),
           stat='identity', position ='dodge') + 
  facet_wrap(~green_rating) + 
  labs(title="Degreedays category wise Median rent across Contract type", 
       y="Medianrent",
       x = "DegreeDays Category",
       fill="Contract type")
```

![](assn_files/figure-markdown_github/unnamed-chunk-4-1.png) From the
graphs it is evident that the usual difference across contracts is
higher for Non green buildings, which might emphasize that the green
rated building are usually less maintanence and the construction company
is passing the benefit to the tenants by charging them only a minimal
amount.
