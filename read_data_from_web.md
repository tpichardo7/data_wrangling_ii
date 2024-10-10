Reading Data from the Web
================

Load necessary libraries.

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

## Extracting tables - National Survey on Drug Use and Health

``` r
url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"
  
drug_use_html = read_html(url)
```

Get the pieces I actually need.

``` r
marj_us_df = 
  drug_use_html |> 
  html_table() |> 
  first() |> 
  slice(-1)
```

## Extracting tables - NY Cost of Living

Read in cost of living data.

``` r
ny_cost_df = 
  read_html("https://www.bestplaces.net/cost_of_living/city/new_york/new_york") |> 
  html_table(header = TRUE) |> 
  first()
```
