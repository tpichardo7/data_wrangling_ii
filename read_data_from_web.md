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

## Extracting Tables

### National Survey on Drug Use and Health

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

### NY Cost of Living

Read in cost of living data.

``` r
ny_cost_df = 
  read_html("https://www.bestplaces.net/cost_of_living/city/new_york/new_york") |> 
  html_table(header = TRUE) |> 
  first()
```

## CSS Selectors

### Star Wars Movies

``` r
swm_url = "https://www.imdb.com/list/ls070150896/"

swm_html = read_html(swm_url)
```

``` r
swm_title_vec = 
  swm_html |> 
  html_elements(".ipc-title-link-wrapper .ipc-title__text") |> 
  html_text()

swm_runtime_vec = 
  swm_html |> 
  html_elements(".dli-title-metadata-item:nth-child(2)") |> 
  html_text()

swm_score_vec = 
  swm_html |> 
  html_elements(".metacritic-score-box") |> 
  html_text()

swm_df = 
  tibble(
    title = swm_title_vec, 
    runtime = swm_runtime_vec, 
    score = swm_score_vec
  )
```

### Books

Let’s import some books.

``` r
books_html = read_html("https://books.toscrape.com/")

books_html |> 
  html_elements(".product_pod a") |> 
  html_text()
```

    ##  [1] ""                                     
    ##  [2] "A Light in the ..."                   
    ##  [3] ""                                     
    ##  [4] "Tipping the Velvet"                   
    ##  [5] ""                                     
    ##  [6] "Soumission"                           
    ##  [7] ""                                     
    ##  [8] "Sharp Objects"                        
    ##  [9] ""                                     
    ## [10] "Sapiens: A Brief History ..."         
    ## [11] ""                                     
    ## [12] "The Requiem Red"                      
    ## [13] ""                                     
    ## [14] "The Dirty Little Secrets ..."         
    ## [15] ""                                     
    ## [16] "The Coming Woman: A ..."              
    ## [17] ""                                     
    ## [18] "The Boys in the ..."                  
    ## [19] ""                                     
    ## [20] "The Black Maria"                      
    ## [21] ""                                     
    ## [22] "Starving Hearts (Triangular Trade ..."
    ## [23] ""                                     
    ## [24] "Shakespeare's Sonnets"                
    ## [25] ""                                     
    ## [26] "Set Me Free"                          
    ## [27] ""                                     
    ## [28] "Scott Pilgrim's Precious Little ..."  
    ## [29] ""                                     
    ## [30] "Rip it Up and ..."                    
    ## [31] ""                                     
    ## [32] "Our Band Could Be ..."                
    ## [33] ""                                     
    ## [34] "Olio"                                 
    ## [35] ""                                     
    ## [36] "Mesaerion: The Best Science ..."      
    ## [37] ""                                     
    ## [38] "Libertarianism for Beginners"         
    ## [39] ""                                     
    ## [40] "It's Only the Himalayas"

``` r
books_html |> 
  html_elements(".price_color") |> 
  html_text()
```

    ##  [1] "£51.77" "£53.74" "£50.10" "£47.82" "£54.23" "£22.65" "£33.34" "£17.93"
    ##  [9] "£22.60" "£52.15" "£13.99" "£20.66" "£17.46" "£52.29" "£35.02" "£57.25"
    ## [17] "£23.88" "£37.59" "£51.33" "£45.17"

## Using an API

Get water data.

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") |> 
  content()
```

    ## Rows: 45 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Get BRFSS data.

``` r
brfss_df = 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv",
      query = list("$limit" = 5000)) |> 
  content()
```

    ## Rows: 5000 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (16): locationabbr, locationdesc, class, topic, question, response, data...
    ## dbl  (6): year, sample_size, data_value, confidence_limit_low, confidence_li...
    ## lgl  (1): locationid
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Pokemon API

``` r
pokemon = 
  GET("https://pokeapi.co/api/v2/pokemon/ditto") |> 
  content()
```

``` r
pokemon$height
```

    ## [1] 3

``` r
pokemon$abilities
```

    ## [[1]]
    ## [[1]]$ability
    ## [[1]]$ability$name
    ## [1] "limber"
    ## 
    ## [[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/7/"
    ## 
    ## 
    ## [[1]]$is_hidden
    ## [1] FALSE
    ## 
    ## [[1]]$slot
    ## [1] 1
    ## 
    ## 
    ## [[2]]
    ## [[2]]$ability
    ## [[2]]$ability$name
    ## [1] "imposter"
    ## 
    ## [[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/150/"
    ## 
    ## 
    ## [[2]]$is_hidden
    ## [1] TRUE
    ## 
    ## [[2]]$slot
    ## [1] 3
