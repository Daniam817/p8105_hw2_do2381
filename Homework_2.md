Homework 2
================
Daniel Ojerant

Read data

``` r
trashwheel.df=
  read_xlsx("/Users/danie/Documents/Columbia Semester 1 Files/Data Science  R Code/Data Wrangling/p8105_hw2_do2381/Trash-Wheel-Collection-Totals-8-6-19.xlsx", range = cell_cols("A:N")) %>% janitor::clean_names() %>% 
  drop_na(dumpster) %>% 
mutate(
  sports_balls = round(sports_balls),
  sports_balls = as.integer(sports_balls)
)
```

Read precipitation

``` r
 precip.2018=
  read_xlsx("/Users/danie/Documents/Columbia Semester 1 Files/Data Science  R Code/Data Wrangling/p8105_hw2_do2381/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
  sheet = "2018 Precipitation",
  skip = 1
  ) %>% 
  janitor::clean_names() %>% 
  drop_na(month) %>% 
  mutate(year = 2018) %>% 
  relocate(year)
  
  precip.2017 =
  read_xlsx("/Users/danie/Documents/Columbia Semester 1 Files/Data Science  R Code/Data Wrangling/p8105_hw2_do2381/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
  sheet = "2017 Precipitation",
  skip = 1
  ) %>% 
  janitor::clean_names() %>% 
  drop_na(month) %>% 
  mutate(year = 2017) %>% 
  relocate(year)
```

Now combine annual precipitation

``` r
month.df =
  tibble(
    month = 1:12,
    month_name = month.name
  )
precip.df =
  bind_rows(precip.2018,precip.2017)

left_join(precip.df, month.df, by = "month")
```

    ## # A tibble: 24 x 4
    ##     year month total month_name
    ##    <dbl> <dbl> <dbl> <chr>     
    ##  1  2018     1  0.94 January   
    ##  2  2018     2  4.8  February  
    ##  3  2018     3  2.69 March     
    ##  4  2018     4  4.69 April     
    ##  5  2018     5  9.27 May       
    ##  6  2018     6  4.77 June      
    ##  7  2018     7 10.2  July      
    ##  8  2018     8  6.45 August    
    ##  9  2018     9 10.5  September 
    ## 10  2018    10  2.12 October   
    ## # ... with 14 more rows

This dataset contains information from Mr.Trashwheel trash collector in
Baltimore, Maryland. As trash enters the inner harbor, the trashwheel
collects the trash, and stores it in a dumpster. The dataset contains
information on year, month, and trash collected, include some specific
kinds of trash. There are a total of 344 rows in our final dataset.
Additional data sheets include month precipitation date

## Problem 2

``` r
NYC.df = 
  read_csv("/Users/danie/Documents/Columbia Semester 1 Files/Data Science  R Code/Data Wrangling/p8105_hw2_do2381/NYC_Transit_Subway_Entrance_And_Exit_Data.csv") %>% 
  janitor::clean_names() %>% 
  select(line:entrance_type, entry, vending, ada) %>% 
  mutate(entry = recode(entry, "YES" = 1, "NO" = 0))
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `Station Latitude` = col_double(),
    ##   `Station Longitude` = col_double(),
    ##   Route8 = col_double(),
    ##   Route9 = col_double(),
    ##   Route10 = col_double(),
    ##   Route11 = col_double(),
    ##   ADA = col_logical(),
    ##   `Free Crossover` = col_logical(),
    ##   `Entrance Latitude` = col_double(),
    ##   `Entrance Longitude` = col_double()
    ## )

    ## See spec(...) for full column specifications.
