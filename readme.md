---
title: "Untitled"
output: 
  html_document: 
    keep_md: yes
---





```r
library(xml2)
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 2.2.1     ✔ purrr   0.2.5
## ✔ tibble  1.4.2     ✔ dplyr   0.7.5
## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
## ✔ readr   1.1.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(rvest)
```

```
## 
## Attaching package: 'rvest'
```

```
## The following object is masked from 'package:purrr':
## 
##     pluck
```

```
## The following object is masked from 'package:readr':
## 
##     guess_encoding
```

```r
webpage_url <- "https://gamewith.net/pokemon-swordshield/article/show/13923"


webpage <- xml2::read_html(webpage_url) %>%
  html_nodes("table") 
```


```r
tables_from_page <- webpage %>%
        map(html_table)#[[3]]
stats_table <- tables_from_page[3] 
stats_table <- stats_table[1] 
stats_table <- stats_table[[1]]
```


```r
stats_table %>%
        mutate(tough = H + D + SD,
               max_attack = pmax(A, SA),
               strongness = tough + max_attack,
               offense = max_attack + SPE,
               alt_total = offense + tough,
               strongness_rank = dense_rank(desc(strongness)),
               offense_rank = dense_rank(desc(offense)),
               alt_total_rank = dense_rank(desc(alt_total))) %>% 
        head()
```

```
##     Pokemon   H   A  D SA SD SPE Total tough max_attack strongness offense
## 1   Grookey  50  65 50 40 40  65   310   140         65        205     130
## 2  Thwackey  70  85 70 55 60  80   420   200         85        285     165
## 3 Rillaboom 100 125 90 60 70  85   530   260        125        385     210
## 4 Scorbunny  50  71 40 40 40  69   310   130         71        201     140
## 5    Raboot  65  86 60 55 60  94   420   185         86        271     180
## 6 Cinderace  80 116 75 65 75 119   530   230        116        346     235
##   alt_total strongness_rank offense_rank alt_total_rank
## 1       270             132           77            105
## 2       365              84           54             68
## 3       470              25           18              8
## 4       270             135           69            105
## 5       365              90           41             68
## 6       465              47            9              9
```

