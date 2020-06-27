Data cleaning
================
Granát Marcell

``` r
library(readxl)
library(tidyverse)
```

    -- Attaching packages ----------------------------------------------------------------- tidyverse 1.3.0 --

    <U+221A> ggplot2 3.3.0     <U+221A> purrr   0.3.3
    <U+221A> tibble  3.0.1     <U+221A> dplyr   0.8.3
    <U+221A> tidyr   1.1.0     <U+221A> stringr 1.4.0
    <U+221A> readr   1.3.1     <U+221A> forcats 0.5.0

    -- Conflicts -------------------------------------------------------------------- tidyverse_conflicts() --
    x dplyr::filter() masks stats::filter()
    x dplyr::lag()    masks stats::lag()

``` r
survey <- read_excel("Egyetemistak elkepzelesei a jovo munkahelyerol-adat-2020-04-16.xlsx")
```

# Magyar

``` r
survey <- survey[-(duplicated(survey[2]) == T) * (is.na(survey[2]) == F)*seq(nrow(survey)),] %>% select_if(~sum(!is.na(.)) > 0) %>% 
  .[,] %>% select(-c(1,2))
```

``` r
df_names <- data.frame(new_names = str_c("v", seq_along(survey)), "original_names" = names(survey) %>% str_split("\\. ") %>% sapply("[",2))
names(survey) <- str_c("v", seq_along(survey))
```

``` r
survey <- survey %>% mutate_each(funs(as.factor)) %>% mutate(
  v59 = as.integer(v59),
  v60 = as.integer(v60),
  v3 = factor(v3, levels = c("legfeljebb általános iskola", "szakiskola vagy szakmunkásképző", "középiskola érettségivel", "főiskola", "egyetem"))
)
```

``` r
survey$v3
```

``` 
  [1] egyetem                         egyetem                        
  [3] főiskola                        <NA>                           
  [5] főiskola                        főiskola                       
  [7] középiskola érettségivel        középiskola érettségivel       
  [9] szakiskola vagy szakmunkásképző főiskola                       
 [11] egyetem                         szakiskola vagy szakmunkásképző
 [13] főiskola                        egyetem                        
 [15] főiskola                        főiskola                       
 [17] egyetem                         középiskola érettségivel       
 [19] középiskola érettségivel        egyetem                        
 [21] középiskola érettségivel        főiskola                       
 [23] egyetem                         egyetem                        
 [25] egyetem                         egyetem                        
 [27] főiskola                        egyetem                        
 [29] egyetem                         középiskola érettségivel       
 [31] középiskola érettségivel        egyetem                        
 [33] főiskola                        egyetem                        
 [35] középiskola érettségivel        főiskola                       
 [37] egyetem                         főiskola                       
 [39] egyetem                         középiskola érettségivel       
 [41] középiskola érettségivel        középiskola érettségivel       
 [43] főiskola                        főiskola                       
 [45] egyetem                         egyetem                        
 [47] főiskola                        középiskola érettségivel       
 [49] középiskola érettségivel        egyetem                        
 [51] középiskola érettségivel        főiskola                       
 [53] középiskola érettségivel        egyetem                        
 [55] szakiskola vagy szakmunkásképző egyetem                        
 [57] főiskola                        főiskola                       
 [59] egyetem                         egyetem                        
 [61] főiskola                        egyetem                        
 [63] egyetem                         középiskola érettségivel       
 [65] középiskola érettségivel        főiskola                       
 [67] egyetem                         főiskola                       
 [69] főiskola                        főiskola                       
 [71] egyetem                         szakiskola vagy szakmunkásképző
 [73] egyetem                         főiskola                       
 [75] egyetem                         szakiskola vagy szakmunkásképző
 [77] szakiskola vagy szakmunkásképző egyetem                        
 [79] főiskola                        főiskola                       
 [81] <NA>                            egyetem                        
 [83] egyetem                         egyetem                        
 [85] középiskola érettségivel        egyetem                        
 [87] középiskola érettségivel        egyetem                        
 [89] középiskola érettségivel        középiskola érettségivel       
 [91] egyetem                         középiskola érettségivel       
 [93] főiskola                        egyetem                        
 [95] egyetem                         főiskola                       
 [97] főiskola                        középiskola érettségivel       
 [99] egyetem                         középiskola érettségivel       
[101] szakiskola vagy szakmunkásképző középiskola érettségivel       
[103] középiskola érettségivel        középiskola érettségivel       
[105] egyetem                         egyetem                        
[107] szakiskola vagy szakmunkásképző középiskola érettségivel       
[109] középiskola érettségivel        középiskola érettségivel       
[111] középiskola érettségivel        szakiskola vagy szakmunkásképző
[113] főiskola                        középiskola érettségivel       
[115] szakiskola vagy szakmunkásképző középiskola érettségivel       
[117] egyetem                         egyetem                        
[119] főiskola                        középiskola érettségivel       
[121] főiskola                        egyetem                        
[123] egyetem                         főiskola                       
[125] főiskola                        egyetem                        
[127] középiskola érettségivel        egyetem                        
[129] főiskola                        egyetem                        
[131] egyetem                         egyetem                        
[133] egyetem                         egyetem                        
[135] középiskola érettségivel        főiskola                       
[137] szakiskola vagy szakmunkásképző főiskola                       
[139] egyetem                         egyetem                        
[141] középiskola érettségivel        főiskola                       
[143] főiskola                        középiskola érettségivel       
[145] egyetem                         egyetem                        
[147] főiskola                        egyetem                        
[149] főiskola                        egyetem                        
[151] főiskola                        egyetem                        
[153] főiskola                        egyetem                        
[155] középiskola érettségivel        főiskola                       
[157] egyetem                         középiskola érettségivel       
[159] egyetem                         szakiskola vagy szakmunkásképző
[161] egyetem                         egyetem                        
[163] egyetem                         főiskola                       
[165] szakiskola vagy szakmunkásképző főiskola                       
[167] egyetem                         főiskola                       
[169] főiskola                        egyetem                        
[171] egyetem                         főiskola                       
[173] középiskola érettségivel        középiskola érettségivel       
[175] egyetem                         középiskola érettségivel       
[177] főiskola                        főiskola                       
[179] egyetem                         egyetem                        
[181] középiskola érettségivel        középiskola érettségivel       
[183] főiskola                        főiskola                       
[185] egyetem                         főiskola                       
[187] egyetem                         egyetem                        
[189] egyetem                         egyetem                        
[191] egyetem                         egyetem                        
[193] egyetem                         egyetem                        
[195] szakiskola vagy szakmunkásképző középiskola érettségivel       
[197] egyetem                         főiskola                       
[199] egyetem                         szakiskola vagy szakmunkásképző
[201] főiskola                        középiskola érettségivel       
[203] egyetem                         középiskola érettségivel       
[205] középiskola érettségivel        egyetem                        
[207] főiskola                        egyetem                        
[209] főiskola                        egyetem                        
[211] egyetem                         egyetem                        
[213] egyetem                         egyetem                        
[215] főiskola                        egyetem                        
[217] egyetem                         főiskola                       
[219] szakiskola vagy szakmunkásképző szakiskola vagy szakmunkásképző
[221] középiskola érettségivel        középiskola érettségivel       
[223] középiskola érettségivel        egyetem                        
[225] egyetem                         egyetem                        
[227] középiskola érettségivel        egyetem                        
[229] egyetem                         főiskola                       
[231] szakiskola vagy szakmunkásképző főiskola                       
[233] szakiskola vagy szakmunkásképző egyetem                        
[235] egyetem                         egyetem                        
[237] középiskola érettségivel        egyetem                        
[239] egyetem                         egyetem                        
[241] főiskola                        szakiskola vagy szakmunkásképző
[243] egyetem                         főiskola                       
[245] egyetem                         szakiskola vagy szakmunkásképző
[247] egyetem                         főiskola                       
[249] középiskola érettségivel        egyetem                        
[251] középiskola érettségivel        középiskola érettségivel       
[253] egyetem                         egyetem                        
[255] egyetem                         szakiskola vagy szakmunkásképző
[257] főiskola                        egyetem                        
[259] középiskola érettségivel        középiskola érettségivel       
[261] szakiskola vagy szakmunkásképző középiskola érettségivel       
[263] egyetem                         egyetem                        
[265] középiskola érettségivel        középiskola érettségivel       
[267] egyetem                         egyetem                        
[269] középiskola érettségivel        egyetem                        
[271] főiskola                        egyetem                        
[273] egyetem                         egyetem                        
[275] egyetem                         főiskola                       
[277] főiskola                        középiskola érettségivel       
[279] egyetem                         egyetem                        
[281] egyetem                         egyetem                        
[283] egyetem                         középiskola érettségivel       
[285] szakiskola vagy szakmunkásképző egyetem                        
[287] egyetem                         főiskola                       
[289] főiskola                        egyetem                        
[291] középiskola érettségivel        egyetem                        
[293] középiskola érettségivel        főiskola                       
[295] középiskola érettségivel        egyetem                        
[297] főiskola                        egyetem                        
[299] középiskola érettségivel        középiskola érettségivel       
[301] középiskola érettségivel        szakiskola vagy szakmunkásképző
[303] főiskola                        főiskola                       
[305] egyetem                         főiskola                       
[307] egyetem                         egyetem                        
[309] egyetem                         egyetem                        
[311] egyetem                         egyetem                        
[313] egyetem                         egyetem                        
[315] egyetem                         középiskola érettségivel       
[317] egyetem                         egyetem                        
[319] középiskola érettségivel        középiskola érettségivel       
[321] szakiskola vagy szakmunkásképző egyetem                        
[323] egyetem                         középiskola érettségivel       
[325] egyetem                         főiskola                       
[327] egyetem                         főiskola                       
[329] középiskola érettségivel        szakiskola vagy szakmunkásképző
[331] szakiskola vagy szakmunkásképző főiskola                       
[333] középiskola érettségivel        szakiskola vagy szakmunkásképző
[335] főiskola                        egyetem                        
[337] egyetem                         egyetem                        
[339] szakiskola vagy szakmunkásképző középiskola érettségivel       
[341] főiskola                        egyetem                        
[343] egyetem                         szakiskola vagy szakmunkásképző
[345] főiskola                        egyetem                        
[347] legfeljebb általános iskola     szakiskola vagy szakmunkásképző
[349] egyetem                         egyetem                        
[351] egyetem                         egyetem                        
[353] főiskola                        középiskola érettségivel       
[355] középiskola érettségivel        főiskola                       
[357] szakiskola vagy szakmunkásképző főiskola                       
[359] szakiskola vagy szakmunkásképző főiskola                       
[361] egyetem                         egyetem                        
[363] egyetem                         középiskola érettségivel       
[365] középiskola érettségivel        középiskola érettségivel       
[367] középiskola érettségivel        egyetem                        
[369] középiskola érettségivel        középiskola érettségivel       
[371] középiskola érettségivel        szakiskola vagy szakmunkásképző
[373] főiskola                        egyetem                        
[375] szakiskola vagy szakmunkásképző egyetem                        
[377] egyetem                         szakiskola vagy szakmunkásképző
[379] főiskola                        egyetem                        
[381] egyetem                         egyetem                        
[383] egyetem                         szakiskola vagy szakmunkásképző
[385] középiskola érettségivel        egyetem                        
[387] egyetem                         főiskola                       
[389] egyetem                         középiskola érettségivel       
[391] középiskola érettségivel        főiskola                       
[393] egyetem                         egyetem                        
[395] középiskola érettségivel        egyetem                        
[397] középiskola érettségivel        főiskola                       
[399] középiskola érettségivel        egyetem                        
[401] egyetem                         középiskola érettségivel       
[403] középiskola érettségivel        szakiskola vagy szakmunkásképző
[405] főiskola                        szakiskola vagy szakmunkásképző
[407] középiskola érettségivel        egyetem                        
[409] egyetem                         egyetem                        
[411] főiskola                        középiskola érettségivel       
[413] középiskola érettségivel        egyetem                        
[415] szakiskola vagy szakmunkásképző főiskola                       
[417] középiskola érettségivel       
5 Levels: legfeljebb általános iskola ... egyetem
```
