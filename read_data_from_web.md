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
