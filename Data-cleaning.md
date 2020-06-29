Data cleaning
================
Granát Marcell

``` r
library(readxl)
library(tidyverse)
```

``` r
survey <- read_excel("Egyetemistak elkepzelesei a jovo munkahelyerol-adat-2020-04-16.xlsx")
```

# General cleaning

``` r
survey <-
  survey[-(duplicated(survey[2]) == T) * (is.na(survey[2]) == F) * seq(nrow(survey)), ] %>% # remove the answer from participants, who answered for 2nd time
  select_if(~ sum(!is.na(.)) > 0) %>% # remove empty columns
  .[, ] %>%
  select(-c(1, 2)) # remove user rank & e-mail ---> not relevant
```

``` r
df_names <- # contains the original Qs to the variables
  data.frame(new_names = str_c("v", seq_along(survey)), "original_names" = names(survey) %>% str_split("\\. ") %>% sapply("[", 2))
names(survey) <- str_c("v", seq_along(survey)) # add IDs to the colmumns

df_names$original_names <- as.vector(df_names$original_names) # originally made a factor ---> cant edit

# manage the few errors in the prev cleaning
df_names[45, 2] <- "Melyiket tartja fontosabbnak? (Hozzáadott érték vagy munkaidő kitöltése)"
df_names[46, 2] <- "Melyiket tartja fontosabbnak? (Multiple career path vagy Up or out)"
df_names[96, 2] <- "Mostanában milyen gyakran volt boldog?"
df_names[97, 2] <- "Mostanában milyen gyakran volt stresszes?"
df_names[98, 2] <- "Mostanában milyen gyakran érezte magát magányosnak?"
```

``` r
survey_W_outliers <- # before cleaning from outliers at v59 & v60
  survey %>%
  mutate_each(funs(as.factor)) %>%
  mutate(
    v59 = as.integer(survey$v59),
    v60 = as.integer(survey$v60),
    # factors which has to be sorted
    v3 = factor(v3, levels = c("legfeljebb általános iskola", "szakiskola vagy szakmunkásképző", "középiskola érettségivel", "főiskola", "egyetem")),
    v5 = factor(v5, levels = c("alapképzés", "mesterképzés", "osztatlan képzés", "PHD képzés", "egyéb")),
    v8 = factor(v8, levels = c("nincs", "alap", "közép", "felső")),
    v9 = factor(v9, levels = c("nincs", "alap", "közép", "felső")),
    v12 = factor(v12, levels = c("3 hónapnál rövidebb ideig", "3 hónap és fél év között", "fél évnél hosszabb ideig")),
    v52 = factor(v52, levels = c("még ritkábban", "havonta", "havonta kétszer", "hetente egyszer", "hetente többször", "napi")),
    v62 = factor(v62, levels = c("beosztott", "középvezető", "felsővezető", "független szakértő / saját cégen belüli munkavégzés")),
    v77 = factor(v77, levels = c("nem", "igen, heti pár órára", "igen, heti egy napra", "igen, hetente több napra")),
    v78 = factor(v78, levels = c("fix idejű", "bizonyos keretek között rugalmas", "teljesen rugalmas")),
    v80 = factor(v80, levels = c("egyáltalán nem", "a környező települések még szóba jöhetnek", "távolabbi települések is szóba jöhetnek, de csak Magyarországon", "külföldön is vállalnék munkát")),
    v96 = factor(v96, levels = c("soha", "ritkán", "időnként", "többnyire", "mindig")),
    v97 = factor(v96, levels = c("soha", "ritkán", "időnként", "többnyire", "mindig")),
    v98 = factor(v96, levels = c("soha", "ritkán", "időnként", "többnyire", "mindig"))
  )
```

# Remove outliers

``` r
outlier_remove <- function(v) {
  # replace outliers w NAs, detection eqvivalent w the method of a boxplot
  ifelse(
    v < quantile(v, probs = .25, na.rm = T) - 1.5 * IQR(v, na.rm = T) |
      v > quantile(v, probs = .75, na.rm = T) + 1.5 * IQR(v, na.rm = T),
    NA, v
  )
}
```

``` r
survey <- survey_W_outliers %>% mutate(
  v59 = ifelse(v59 < 10000, v59 * 1000, v59), # manage commit execution error by participant ---> probably thougth to '000 Ft or wrote '.' instead ','
  v60 = ifelse(v60 < 10000, v60 * 1000, v60), ##
  v59 = outlier_remove(v59),
  v60 = outlier_remove(v60)
)
```
