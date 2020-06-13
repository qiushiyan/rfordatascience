
# Categorical data (facotr)


## Frequency and contingency table



###  `frq()` and `flat_table()`   


sjmisc

https://strengejacke.github.io/sjmisc/articles/exploringdatasets.html  


```r
library(sjmisc)
```


```r
data(efc)
(efc <- efc %>% as_tibble())
#> # A tibble: 908 x 26
#>   c12hour e15relat e16sex e17age e42dep c82cop1 c83cop2 c84cop3 c85cop4 c86cop5
#>     <dbl>    <dbl>  <dbl>  <dbl>  <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
#> 1      16        2      2     83      3       3       2       2       2       1
#> 2     148        2      2     88      3       3       3       3       3       4
#> 3      70        1      2     82      3       2       2       1       4       1
#> 4     168        1      2     67      4       4       1       3       1       1
#> 5     168        2      2     84      4       3       2       1       2       2
#> 6      16        2      2     85      4       2       2       3       3       3
#> # ... with 902 more rows, and 16 more variables: c87cop6 <dbl>, c88cop7 <dbl>,
#> #   c89cop8 <dbl>, c90cop9 <dbl>, c160age <dbl>, c161sex <dbl>, c172code <dbl>,
#> #   c175empl <dbl>, barthtot <dbl>, neg_c_7 <dbl>, pos_v_4 <dbl>, quol_5 <dbl>,
#> #   resttotn <dbl>, tot_sc_e <dbl>, n4pstu <dbl>, nur_pst <dbl>
```


```r
efc %>%
  frq(c161sex)
#> 
#> carer's gender (c161sex) <numeric>
#> # total N=908  valid N=901  mean=1.76  sd=0.43
#> 
#> Value |  Label |   N | Raw % | Valid % | Cum. %
#> -----------------------------------------------
#>     1 |   Male | 215 | 23.68 |   23.86 |  23.86
#>     2 | Female | 686 | 75.55 |   76.14 | 100.00
#>  <NA> |   <NA> |   7 |  0.77 |    <NA> |   <NA>
```

```r
efc %>% 
  group_by(e42dep) %>% 
  frq(c161sex)
#> 
#> carer's gender (c161sex) <numeric>
#> # grouped by: independent
#> # total N=66  valid N=66  mean=1.73  sd=0.45
#> 
#> Value |  Label |  N | Raw % | Valid % | Cum. %
#> ----------------------------------------------
#>     1 |   Male | 18 | 27.27 |   27.27 |  27.27
#>     2 | Female | 48 | 72.73 |   72.73 | 100.00
#>  <NA> |   <NA> |  0 |  0.00 |    <NA> |   <NA>
#> 
#> 
#> carer's gender (c161sex) <numeric>
#> # grouped by: slightly dependent
#> # total N=225  valid N=224  mean=1.76  sd=0.43
#> 
#> Value |  Label |   N | Raw % | Valid % | Cum. %
#> -----------------------------------------------
#>     1 |   Male |  54 | 24.00 |   24.11 |  24.11
#>     2 | Female | 170 | 75.56 |   75.89 | 100.00
#>  <NA> |   <NA> |   1 |  0.44 |    <NA> |   <NA>
#> 
#> 
#> carer's gender (c161sex) <numeric>
#> # grouped by: moderately dependent
#> # total N=306  valid N=306  mean=1.74  sd=0.44
#> 
#> Value |  Label |   N | Raw % | Valid % | Cum. %
#> -----------------------------------------------
#>     1 |   Male |  80 | 26.14 |   26.14 |  26.14
#>     2 | Female | 226 | 73.86 |   73.86 | 100.00
#>  <NA> |   <NA> |   0 |  0.00 |    <NA> |   <NA>
#> 
#> 
#> carer's gender (c161sex) <numeric>
#> # grouped by: severely dependent
#> # total N=304  valid N=304  mean=1.79  sd=0.41
#> 
#> Value |  Label |   N | Raw % | Valid % | Cum. %
#> -----------------------------------------------
#>     1 |   Male |  63 | 20.72 |   20.72 |  20.72
#>     2 | Female | 241 | 79.28 |   79.28 | 100.00
#>  <NA> |   <NA> |   0 |  0.00 |    <NA> |   <NA>
```




```r
flat_table(efc, e42dep, c161sex)
#>                      c161sex Male Female
#> e42dep                                  
#> independent                    18     48
#> slightly dependent             54    170
#> moderately dependent           80    226
#> severely dependent             63    241
```



```r
flat_table(efc, e42dep, c161sex, margin = "col")
#>                      c161sex  Male Female
#> e42dep                                   
#> independent                   8.37   7.01
#> slightly dependent           25.12  24.82
#> moderately dependent         37.21  32.99
#> severely dependent           29.30  35.18
```



```r
library(janitor)
efc %>% tabyl(e42dep, c161sex, show_na = FALSE)
#>  e42dep  1   2
#>       1 18  48
#>       2 54 170
#>       3 80 226
#>       4 63 241
```

## Coding


###  `rec()`

sjmisc

https://strengejacke.github.io/sjmisc/articles/recodingvariables.html


```r
efc$burden <- rec(
  efc$neg_c_7,
  rec = c("min:9=1 [low]; 10:12=2 [moderate]; 13:max=3 [high]; else=NA"),
  var.label = "Subjective burden",
  as.num = FALSE # we want a factor
)
# print frequencies
frq(efc$burden)
#> 
#> Subjective burden (x) <categorical>
#> # total N=908  valid N=892  mean=2.03  sd=0.81
#> 
#> Value |    Label |   N | Raw % | Valid % | Cum. %
#> -------------------------------------------------
#>     1 |      low | 280 | 30.84 |   31.39 |  31.39
#>     2 | moderate | 301 | 33.15 |   33.74 |  65.13
#>     3 |     high | 311 | 34.25 |   34.87 | 100.00
#>  <NA> |     <NA> |  16 |  1.76 |    <NA> |   <NA>
```

## Cutting


### `chop()` 

santoku

https://hughjonesd.github.io/santoku/tutorials/00-visualintroduction.html


```r
# devtools::install_github("hughjonesd/santoku")
# library(tidyverse) (load tidyverse before santoku to avoid conflicts)
library(santoku)
```

`cut()` in base R  


```r
x <- rnorm(100)
cut(x, 5) %>% table()  # 5 equal intervals
#> .
#>  (-2.88,-1.62] (-1.62,-0.368] (-0.368,0.884]   (0.884,2.14]     (2.14,3.4] 
#>              5             25             52             17              1
cut(x, -3:3) %>% table()
#> .
#> (-3,-2] (-2,-1]  (-1,0]   (0,1]   (1,2]   (2,3] 
#>       3      15      27      38      16       0
```

`ntile()` in dplyr:  


```r
ntile(x, 5) %>% table()
#> .
#>  1  2  3  4  5 
#> 20 20 20 20 20
```




`chop()`


```r
chopped <- chop(x, breaks = -5:5)

chopped %>% table()
#> .
#> [-3, -2) [-2, -1)  [-1, 0)   [0, 1)   [1, 2)   [3, 4) 
#>        3       15       27       38       16        1

# chop() returns a factor
tibble(x, chopped)
#> # A tibble: 100 x 2
#>         x chopped 
#>     <dbl> <fct>   
#> 1 -0.422  [-1, 0) 
#> 2  0.0569 [0, 1)  
#> 3  0.711  [0, 1)  
#> 4 -1.59   [-2, -1)
#> 5  0.597  [0, 1)  
#> 6  1.22   [1, 2)  
#> # ... with 94 more rows
```

If data is beyond the limits of `breaks`, they will be extended automatically, unless `extend = FALSE`, and values beyond the bounds will be converted to `NA`:  


```r
chopped <- chop(x, breaks = -1:1, extend = FALSE)
tibble(x, chopped)
#> # A tibble: 100 x 2
#>         x chopped
#>     <dbl> <fct>  
#> 1 -0.422  [-1, 0)
#> 2  0.0569 [0, 1] 
#> 3  0.711  [0, 1] 
#> 4 -1.59   <NA>   
#> 5  0.597  [0, 1] 
#> 6  1.22   <NA>   
#> # ... with 94 more rows
```

To chop a single number into a separate category, put the number twice in `breaks`:


```r
x_zeros <- x 
x_zeros[1:5] <- 0

chopped <- chop(x_zeros, c(-1, 0, 0, 1))
tibble(x, chopped)
#> # A tibble: 100 x 2
#>         x chopped  
#>     <dbl> <fct>    
#> 1 -0.422  {0}      
#> 2  0.0569 {0}      
#> 3  0.711  {0}      
#> 4 -1.59   {0}      
#> 5  0.597  {0}      
#> 6  1.22   (1, 3.39]
#> # ... with 94 more rows
```

To quickly produce a table of chopped data, use `tab()`:  


```r
tab(x, breaks = -3:3)
#> x
#>  [-3, -2)  [-2, -1)   [-1, 0)    [0, 1)    [1, 2) (3, 3.39] 
#>         3        15        27        38        16         1
```



