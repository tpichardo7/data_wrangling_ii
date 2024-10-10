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

``` r
url = "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"
  
drug_use_html = read_html(url)
```

Get the pieces I actually need.

``` r
mari_us_df = 
  drug_use_html |> 
  html_table() |> 
  first()
```
