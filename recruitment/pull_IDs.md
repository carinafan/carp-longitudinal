CARP longitudinal - pull subject IDs for recruitment
================
Last updated: June 22, 2021

-   [Session info](#session-info)

This script pulls the original IDs and emails from the original study.

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(readxl)
library(writexl)
library(magrittr)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.4     ✓ dplyr   1.0.2
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.0

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x lubridate::as.difftime() masks base::as.difftime()
    ## x lubridate::date()        masks base::date()
    ## x tidyr::extract()         masks magrittr::extract()
    ## x dplyr::filter()          masks stats::filter()
    ## x lubridate::intersect()   masks base::intersect()
    ## x dplyr::lag()             masks stats::lag()
    ## x purrr::set_names()       masks magrittr::set_names()
    ## x lubridate::setdiff()     masks base::setdiff()
    ## x lubridate::union()       masks base::union()

``` r
df.id = read_excel("../../online/carp/data/old/2019-01-25-cogniciti-forManuscript.xlsx",
                   sheet = "qualtrics") %>% 
  select(subjectID)

df.email = read_excel("../../online/carp/data/old/2019-01-25-cogniciti-subjectID.xlsx") %>% 
  select(subjectID, email) %>% 
  filter(subjectID %in% df.id$subjectID)

df.date = read.csv("../../online/carp/data/old/2019-01-25-cogniciti-clean.csv") %>% 
  select(subjectID, recorded_date) %>% 
  filter(subjectID %in% df.id$subjectID)
```

``` r
df = df.id %>% 
  full_join(df.email) %>% 
  full_join(df.date)
```

    ## Joining, by = "subjectID"
    ## Joining, by = "subjectID"

``` r
df$recorded_date %<>% 
  as_date() %>% 
  year()
```

``` r
df %>% 
  write_xlsx("IDs.xlsx")
```

<!-- ======================================================================= -->

# Session info

``` r
sessionInfo()
```

    ## R version 4.0.5 (2021-03-31)
    ## Platform: x86_64-apple-darwin17.0 (64-bit)
    ## Running under: macOS Catalina 10.15.7
    ## 
    ## Matrix products: default
    ## BLAS:   /Library/Frameworks/R.framework/Versions/4.0/Resources/lib/libRblas.dylib
    ## LAPACK: /Library/Frameworks/R.framework/Versions/4.0/Resources/lib/libRlapack.dylib
    ## 
    ## locale:
    ## [1] en_CA.UTF-8/en_CA.UTF-8/en_CA.UTF-8/C/en_CA.UTF-8/en_CA.UTF-8
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ##  [1] forcats_0.5.0   stringr_1.4.0   dplyr_1.0.2     purrr_0.3.4    
    ##  [5] readr_1.4.0     tidyr_1.1.2     tibble_3.0.4    ggplot2_3.3.2  
    ##  [9] tidyverse_1.3.0 magrittr_1.5    writexl_1.3.1   readxl_1.3.1   
    ## [13] lubridate_1.7.9
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_1.0.5       cellranger_1.1.0 pillar_1.4.6     compiler_4.0.5  
    ##  [5] dbplyr_2.0.0     tools_4.0.5      digest_0.6.27    jsonlite_1.7.1  
    ##  [9] evaluate_0.14    lifecycle_0.2.0  gtable_0.3.0     pkgconfig_2.0.3 
    ## [13] rlang_0.4.8      reprex_0.3.0     cli_2.1.0        rstudioapi_0.11 
    ## [17] DBI_1.1.0        yaml_2.2.1       haven_2.3.1      xfun_0.19       
    ## [21] withr_2.3.0      xml2_1.3.2       httr_1.4.2       knitr_1.30      
    ## [25] fs_1.5.0         hms_0.5.3        generics_0.1.0   vctrs_0.3.4     
    ## [29] grid_4.0.5       tidyselect_1.1.0 glue_1.4.2       R6_2.5.0        
    ## [33] fansi_0.4.1      rmarkdown_2.5    modelr_0.1.8     backports_1.2.0 
    ## [37] scales_1.1.1     ellipsis_0.3.1   htmltools_0.5.0  rvest_0.3.6     
    ## [41] assertthat_0.2.1 colorspace_1.4-1 stringi_1.5.3    munsell_0.5.0   
    ## [45] broom_0.7.2      crayon_1.3.4
