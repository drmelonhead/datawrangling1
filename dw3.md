Tidy Data
================

``` r
library(tidyverse)
```

    ## -- Attaching packages ------------------ tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.2
    ## v tidyr   1.1.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts --------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

## Read in some data

Read in the litters dataset.

``` r
pulse_df  =
  haven::read_sas("./data_import_examples/data_import_examples/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()
```

\#\#Wide format to long formant

``` r
pulse_tidy =
  pulse_df %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi") %>% 
  relocate(id, visit) %>% 
  mutate(vist=recode(visit, "bl"="00m"))

pulse_tidy
```

    ## # A tibble: 4,348 x 6
    ##       id visit   age sex     bdi vist 
    ##    <dbl> <chr> <dbl> <chr> <dbl> <chr>
    ##  1 10003 bl     48.0 male      7 00m  
    ##  2 10003 01m    48.0 male      1 01m  
    ##  3 10003 06m    48.0 male      2 06m  
    ##  4 10003 12m    48.0 male      0 12m  
    ##  5 10015 bl     72.5 male      6 00m  
    ##  6 10015 01m    72.5 male     NA 01m  
    ##  7 10015 06m    72.5 male     NA 06m  
    ##  8 10015 12m    72.5 male     NA 12m  
    ##  9 10022 bl     58.5 male     14 00m  
    ## 10 10022 01m    58.5 male      3 01m  
    ## # ... with 4,338 more rows
