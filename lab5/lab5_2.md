---
title: "dplyr Superhero"
date: "2021-01-21"
output:
  html_document:
    keep_md : yes
    theme: spacelab
    toc: yes
    toc_float: yes
---

## Breakout Rooms  
Please take 5-8 minutes to check over your answers to the HW in your group. If you are stuck, please remember that you can check the key in [Joel's repository](https://github.com/jmledford3115/BIS15LW2021_jledford).  

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab 5 today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. We will spend most of the time in our breakout rooms. Your lab 5 homework will be to knit and push this file to your repository.  

## Load the tidyverse

```r
library("tidyverse")
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## √ ggplot2 3.3.3     √ purrr   0.3.4
## √ tibble  3.0.4     √ dplyr   1.0.2
## √ tidyr   1.1.2     √ stringr 1.4.0
## √ readr   1.4.0     √ forcats 0.5.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   Gender = col_character(),
##   `Eye color` = col_character(),
##   Race = col_character(),
##   `Hair color` = col_character(),
##   Height = col_double(),
##   Publisher = col_character(),
##   `Skin color` = col_character(),
##   Alignment = col_character(),
##   Weight = col_double()
## )
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## i Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
head(superhero_info)
```

```
## # A tibble: 6 x 10
##   name  Gender `Eye color` Race  `Hair color` Height Publisher `Skin color`
##   <chr> <chr>  <chr>       <chr> <chr>         <dbl> <chr>     <chr>       
## 1 A-Bo~ Male   yellow      Human No Hair         203 Marvel C~ <NA>        
## 2 Abe ~ Male   blue        Icth~ No Hair         191 Dark Hor~ blue        
## 3 Abin~ Male   blue        Unga~ No Hair         185 DC Comics red         
## 4 Abom~ Male   green       Huma~ No Hair         203 Marvel C~ <NA>        
## 5 Abra~ Male   blue        Cosm~ Black            NA Marvel C~ <NA>        
## 6 Abso~ Male   blue        Human No Hair         193 Marvel C~ <NA>        
## # ... with 2 more variables: Alignment <chr>, Weight <dbl>
```

```r
superhero_info <- janitor::clean_names(superhero_info)
```

```
## Warning in FUN(X[[i]], ...): strings not representable in native encoding will
## be translated to UTF-8
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00C4>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00D6>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00E4>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00F6>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00DF>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00C6>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00E6>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00D8>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00F8>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00C5>' to native encoding
```

```
## Warning in FUN(X[[i]], ...): unable to translate '<U+00E5>' to native encoding
```


Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names Agility `Accelerated He~ `Lantern Power ~ `Dimensional Aw~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati~ FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # ... with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>, `Danger
## #   Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons
## #   Master` <lgl>, `Power Augmentation` <lgl>, `Animal Attributes` <lgl>,
## #   Longevity <lgl>, Intelligence <lgl>, `Super Strength` <lgl>,
## #   Cryokinesis <lgl>, Telepathy <lgl>, `Energy Armor` <lgl>, `Energy
## #   Blasts` <lgl>, Duplication <lgl>, `Size Changing` <lgl>, `Density
## #   Control` <lgl>, Stamina <lgl>, `Astral Travel` <lgl>, `Audio
## #   Control` <lgl>, Dexterity <lgl>, Omnitrix <lgl>, `Super Speed` <lgl>,
## #   Possession <lgl>, `Animal Oriented Powers` <lgl>, `Weapon-based
## #   Powers` <lgl>, Electrokinesis <lgl>, `Darkforce Manipulation` <lgl>, `Death
## #   Touch` <lgl>, Teleportation <lgl>, `Enhanced Senses` <lgl>,
## #   Telekinesis <lgl>, `Energy Beams` <lgl>, Magic <lgl>, Hyperkinesis <lgl>,
## #   Jump <lgl>, Clairvoyance <lgl>, `Dimensional Travel` <lgl>, `Power
## #   Sense` <lgl>, Shapeshifting <lgl>, `Peak Human Condition` <lgl>,
## #   Immortality <lgl>, Camouflage <lgl>, `Element Control` <lgl>,
## #   Phasing <lgl>, `Astral Projection` <lgl>, `Electrical Transport` <lgl>,
## #   `Fire Control` <lgl>, Projection <lgl>, Summoning <lgl>, `Enhanced
## #   Memory` <lgl>, Reflexes <lgl>, Invulnerability <lgl>, `Energy
## #   Constructs` <lgl>, `Force Fields` <lgl>, `Self-Sustenance` <lgl>,
## #   `Anti-Gravity` <lgl>, Empathy <lgl>, `Power Nullifier` <lgl>, `Radiation
## #   Control` <lgl>, `Psionic Powers` <lgl>, Elasticity <lgl>, `Substance
## #   Secretion` <lgl>, `Elemental Transmogrification` <lgl>,
## #   `Technopath/Cyberpath` <lgl>, `Photographic Reflexes` <lgl>, `Seismic
## #   Power` <lgl>, Animation <lgl>, Precognition <lgl>, `Mind Control` <lgl>,
## #   `Fire Resistance` <lgl>, `Power Absorption` <lgl>, `Enhanced
## #   Hearing` <lgl>, `Nova Force` <lgl>, Insanity <lgl>, Hypnokinesis <lgl>,
## #   `Animal Control` <lgl>, `Natural Armor` <lgl>, Intangibility <lgl>,
## #   `Enhanced Sight` <lgl>, `Molecular Manipulation` <lgl>, `Heat
## #   Generation` <lgl>, Adaptation <lgl>, Gliding <lgl>, `Power Suit` <lgl>,
## #   `Mind Blast` <lgl>, `Probability Manipulation` <lgl>, `Gravity
## #   Control` <lgl>, Regeneration <lgl>, `Light Control` <lgl>,
## #   Echolocation <lgl>, Levitation <lgl>, `Toxin and Disease Control` <lgl>,
## #   Banish <lgl>, `Energy Manipulation` <lgl>, `Heat Resistance` <lgl>, ...
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
superhero_info %>%
  select(name, alignment) %>%
  filter(alignment == "neutral")
```

```
## # A tibble: 24 x 2
##    name         alignment
##    <chr>        <chr>    
##  1 Bizarro      neutral  
##  2 Black Flash  neutral  
##  3 Captain Cold neutral  
##  4 Copycat      neutral  
##  5 Deadpool     neutral  
##  6 Deathstroke  neutral  
##  7 Etrigan      neutral  
##  8 Galactus     neutral  
##  9 Gladiator    neutral  
## 10 Indigo       neutral  
## # ... with 14 more rows
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info%>%
  select(name, alignment, race)
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # ... with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info%>%
  filter(race != "Human")
```

```
## # A tibble: 222 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  2 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  3 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  4 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  5 Ajax  Male   brown     Cybo~ Black         193 Marvel C~ <NA>       bad      
##  6 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  7 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  8 Angel Male   <NA>      Vamp~ <NA>           NA Dark Hor~ <NA>       good     
##  9 Ange~ Female yellow    Muta~ Black         165 Marvel C~ <NA>       good     
## 10 Anti~ Male   yellow    God ~ No Hair        61 DC Comics <NA>       bad      
## # ... with 212 more rows, and 1 more variable: weight <dbl>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guy <- filter(superhero_info, alignment == "good")
good_guy
```

```
## # A tibble: 496 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  3 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  4 Adam~ Male   blue      <NA>  Blond          NA NBC - He~ <NA>       good     
##  5 Adam~ Male   blue      Human Blond         185 DC Comics <NA>       good     
##  6 Agen~ Female blue      <NA>  Blond         173 Marvel C~ <NA>       good     
##  7 Agen~ Male   brown     Human Brown         178 Marvel C~ <NA>       good     
##  8 Agen~ Male   <NA>      <NA>  <NA>          191 Marvel C~ <NA>       good     
##  9 Alan~ Male   blue      <NA>  Blond         180 DC Comics <NA>       good     
## 10 Alex~ Male   <NA>      <NA>  <NA>           NA NBC - He~ <NA>       good     
## # ... with 486 more rows, and 1 more variable: weight <dbl>
```

```r
bad_guy <- filter(superhero_info, alignment == "bad")
bad_guy
```

```
## # A tibble: 207 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  2 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  3 Abso~ Male   blue      Human No Hair       193 Marvel C~ <NA>       bad      
##  4 Air-~ Male   blue      <NA>  White         188 Marvel C~ <NA>       bad      
##  5 Ajax  Male   brown     Cybo~ Black         193 Marvel C~ <NA>       bad      
##  6 Alex~ Male   <NA>      Human <NA>           NA Wildstorm <NA>       bad      
##  7 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  8 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  9 Ammo  Male   brown     Human Black         188 Marvel C~ <NA>       bad      
## 10 Ange~ Female <NA>      <NA>  <NA>           NA Image Co~ <NA>       bad      
## # ... with 197 more rows, and 1 more variable: weight <dbl>
```


6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guy,race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
good_guy%>%
  filter(race == "Asgardian")
```

```
## # A tibble: 3 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Sif   Female blue      Asga~ Black         188 Marvel C~ <NA>       good     
## 2 Thor  Male   blue      Asga~ Blond         198 Marvel C~ <NA>       good     
## 3 Thor~ Female blue      Asga~ Blond         175 Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
bad_guy%>%
  filter(gender == "Male" & height > 200)
```

```
## # A tibble: 22 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  2 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  3 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  4 Apoc~ Male   red       Muta~ Black         213 Marvel C~ grey       bad      
##  5 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
##  6 Dark~ Male   red       New ~ No Hair       267 DC Comics grey       bad      
##  7 Doct~ Male   brown     Human Brown         201 Marvel C~ <NA>       bad      
##  8 Doct~ Male   brown     <NA>  Brown         201 Marvel C~ <NA>       bad      
##  9 Doom~ Male   red       Alien White         244 DC Comics <NA>       bad      
## 10 Kill~ Male   red       Meta~ No Hair       244 DC Comics green      bad      
## # ... with 12 more rows, and 1 more variable: weight <dbl>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
good_guy%>%
  filter(hair_color == "No Hair")
```

```
## # A tibble: 37 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  3 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  4 Beta~ Male   <NA>      <NA>  No Hair       201 Marvel C~ <NA>       good     
##  5 Bish~ Male   brown     Muta~ No Hair       198 Marvel C~ <NA>       good     
##  6 Blac~ Male   brown     <NA>  No Hair       185 DC Comics <NA>       good     
##  7 Blaq~ <NA>   black     <NA>  No Hair        NA Marvel C~ <NA>       good     
##  8 Bloo~ Male   black     Muta~ No Hair        NA Marvel C~ <NA>       good     
##  9 Crim~ Male   brown     <NA>  No Hair       180 Marvel C~ <NA>       good     
## 10 Dona~ Male   green     Muta~ No Hair        NA IDW Publ~ green      good     
## # ... with 27 more rows, and 1 more variable: weight <dbl>
```

```r
bad_guy%>%
  filter(hair_color == "No Hair")
```

```
## # A tibble: 35 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom~ Male   green     Huma~ No Hair     203   Marvel C~ <NA>       bad      
##  2 Abso~ Male   blue      Human No Hair     193   Marvel C~ <NA>       bad      
##  3 Alien Male   <NA>      Xeno~ No Hair     244   Dark Hor~ black      bad      
##  4 Anni~ Male   green     <NA>  No Hair     180   Marvel C~ <NA>       bad      
##  5 Anti~ Male   yellow    God ~ No Hair      61   DC Comics <NA>       bad      
##  6 Blac~ Male   black     Human No Hair     188   DC Comics <NA>       bad      
##  7 Bloo~ Male   white     <NA>  No Hair      30.5 Marvel C~ <NA>       bad      
##  8 Brai~ Male   green     Andr~ No Hair     198   DC Comics green      bad      
##  9 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
## 10 Dart~ Male   yellow    Cybo~ No Hair     198   George L~ <NA>       bad      
## # ... with 25 more rows, and 1 more variable: weight <dbl>
```

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight over 300?

```r
superhero_info%>%
  filter(height > 200 | weight >300)
```

```
## # A tibble: 65 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  3 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  4 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  5 Ant-~ Male   blue      Human Blond         211 Marvel C~ <NA>       good     
##  6 Anti~ Male   blue      Symb~ Blond         229 Marvel C~ <NA>       <NA>     
##  7 Apoc~ Male   red       Muta~ Black         213 Marvel C~ grey       bad      
##  8 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
##  9 Beta~ Male   <NA>      <NA>  No Hair       201 Marvel C~ <NA>       good     
## 10 Bloo~ Female blue      Human Brown         218 Marvel C~ <NA>       bad      
## # ... with 55 more rows, and 1 more variable: weight <dbl>
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info%>%
  filter(height > 300)
```

```
## # A tibble: 8 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Fin ~ Male   red       Kaka~ No Hair      975  Marvel C~ green      good     
## 2 Gala~ Male   black     Cosm~ Black        876  Marvel C~ <NA>       neutral  
## 3 Groot Male   yellow    Flor~ <NA>         701  Marvel C~ <NA>       good     
## 4 MODOK Male   white     Cybo~ Brownn       366  Marvel C~ <NA>       bad      
## 5 Onsl~ Male   red       Muta~ No Hair      305  Marvel C~ <NA>       bad      
## 6 Sasq~ Male   red       <NA>  Orange       305  Marvel C~ <NA>       good     
## 7 Wolf~ Female green     <NA>  Auburn       366  Marvel C~ <NA>       good     
## 8 Ymir  Male   white     Fros~ No Hair      305. Marvel C~ white      good     
## # ... with 1 more variable: weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info%>%
  filter(weight > 450)
```

```
## # A tibble: 8 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bloo~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
## 2 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
## 3 Giga~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
## 4 Hulk  Male   green     Huma~ Green       244   Marvel C~ green      good     
## 5 Jugg~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
## 6 Red ~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
## 7 Sasq~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
## 8 Wolf~ Female green     <NA>  Auburn      366   Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
superhero_info%>%
  mutate(HtW_Ration = weight/height)%>%
  arrange(desc(HtW_Ration))
```

```
## # A tibble: 734 x 11
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Giga~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
##  2 Utga~ Male   blue      Fros~ White        15.2 Marvel C~ <NA>       bad      
##  3 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
##  4 Jugg~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
##  5 Red ~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
##  6 Sasq~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
##  7 Hulk  Male   green     Huma~ Green       244   Marvel C~ green      good     
##  8 Bloo~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
##  9 Than~ Male   red       Eter~ No Hair     201   Marvel C~ purple     bad      
## 10 A-Bo~ Male   yellow    Human No Hair     203   Marvel C~ <NA>       good     
## # ... with 724 more rows, and 2 more variables: weight <dbl>, HtW_Ration <dbl>
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
str(superhero_powers)
```

```
## tibble [667 x 168] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ hero_names                  : chr [1:667] "3-D Man" "A-Bomb" "Abe Sapien" "Abin Sur" ...
##  $ agility                     : logi [1:667] TRUE FALSE TRUE FALSE FALSE FALSE ...
##  $ accelerated_healing         : logi [1:667] FALSE TRUE TRUE FALSE TRUE FALSE ...
##  $ lantern_power_ring          : logi [1:667] FALSE FALSE FALSE TRUE FALSE FALSE ...
##  $ dimensional_awareness       : logi [1:667] FALSE FALSE FALSE FALSE FALSE TRUE ...
##  $ cold_resistance             : logi [1:667] FALSE FALSE TRUE FALSE FALSE FALSE ...
##  $ durability                  : logi [1:667] FALSE TRUE TRUE FALSE FALSE FALSE ...
##  $ stealth                     : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ energy_absorption           : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ flight                      : logi [1:667] FALSE FALSE FALSE FALSE FALSE TRUE ...
##  $ danger_sense                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ underwater_breathing        : logi [1:667] FALSE FALSE TRUE FALSE FALSE FALSE ...
##  $ marksmanship                : logi [1:667] FALSE FALSE TRUE FALSE FALSE FALSE ...
##  $ weapons_master              : logi [1:667] FALSE FALSE TRUE FALSE FALSE FALSE ...
##  $ power_augmentation          : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ animal_attributes           : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ longevity                   : logi [1:667] FALSE TRUE TRUE FALSE FALSE FALSE ...
##  $ intelligence                : logi [1:667] FALSE FALSE TRUE FALSE TRUE TRUE ...
##  $ super_strength              : logi [1:667] TRUE TRUE TRUE FALSE TRUE TRUE ...
##  $ cryokinesis                 : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ telepathy                   : logi [1:667] FALSE FALSE TRUE FALSE FALSE FALSE ...
##  $ energy_armor                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ energy_blasts               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ duplication                 : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ size_changing               : logi [1:667] FALSE FALSE FALSE FALSE FALSE TRUE ...
##  $ density_control             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ stamina                     : logi [1:667] TRUE TRUE TRUE FALSE TRUE FALSE ...
##  $ astral_travel               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ audio_control               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ dexterity                   : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ omnitrix                    : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ super_speed                 : logi [1:667] TRUE FALSE FALSE FALSE TRUE TRUE ...
##  $ possession                  : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ animal_oriented_powers      : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ weapon_based_powers         : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ electrokinesis              : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ darkforce_manipulation      : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ death_touch                 : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ teleportation               : logi [1:667] FALSE FALSE FALSE FALSE FALSE TRUE ...
##  $ enhanced_senses             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ telekinesis                 : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ energy_beams                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ magic                       : logi [1:667] FALSE FALSE FALSE FALSE FALSE TRUE ...
##  $ hyperkinesis                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ jump                        : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ clairvoyance                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ dimensional_travel          : logi [1:667] FALSE FALSE FALSE FALSE FALSE TRUE ...
##  $ power_sense                 : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ shapeshifting               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ peak_human_condition        : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ immortality                 : logi [1:667] FALSE FALSE TRUE FALSE FALSE TRUE ...
##  $ camouflage                  : logi [1:667] FALSE TRUE FALSE FALSE FALSE FALSE ...
##  $ element_control             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ phasing                     : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ astral_projection           : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ electrical_transport        : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ fire_control                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ projection                  : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ summoning                   : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ enhanced_memory             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ reflexes                    : logi [1:667] FALSE FALSE TRUE FALSE FALSE FALSE ...
##  $ invulnerability             : logi [1:667] FALSE FALSE FALSE FALSE TRUE TRUE ...
##  $ energy_constructs           : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ force_fields                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ self_sustenance             : logi [1:667] FALSE TRUE FALSE FALSE FALSE FALSE ...
##  $ anti_gravity                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ empathy                     : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ power_nullifier             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ radiation_control           : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ psionic_powers              : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ elasticity                  : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ substance_secretion         : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ elemental_transmogrification: logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ technopath_cyberpath        : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ photographic_reflexes       : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ seismic_power               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ animation                   : logi [1:667] FALSE FALSE FALSE FALSE TRUE FALSE ...
##  $ precognition                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ mind_control                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ fire_resistance             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ power_absorption            : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ enhanced_hearing            : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ nova_force                  : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ insanity                    : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ hypnokinesis                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ animal_control              : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ natural_armor               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ intangibility               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ enhanced_sight              : logi [1:667] FALSE FALSE TRUE FALSE FALSE FALSE ...
##  $ molecular_manipulation      : logi [1:667] FALSE FALSE FALSE FALSE FALSE TRUE ...
##  $ heat_generation             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ adaptation                  : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ gliding                     : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ power_suit                  : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ mind_blast                  : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ probability_manipulation    : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ gravity_control             : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ regeneration                : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ light_control               : logi [1:667] FALSE FALSE FALSE FALSE FALSE FALSE ...
##   [list output truncated]
##  - attr(*, "spec")=
##   .. cols(
##   ..   hero_names = col_character(),
##   ..   Agility = col_logical(),
##   ..   `Accelerated Healing` = col_logical(),
##   ..   `Lantern Power Ring` = col_logical(),
##   ..   `Dimensional Awareness` = col_logical(),
##   ..   `Cold Resistance` = col_logical(),
##   ..   Durability = col_logical(),
##   ..   Stealth = col_logical(),
##   ..   `Energy Absorption` = col_logical(),
##   ..   Flight = col_logical(),
##   ..   `Danger Sense` = col_logical(),
##   ..   `Underwater breathing` = col_logical(),
##   ..   Marksmanship = col_logical(),
##   ..   `Weapons Master` = col_logical(),
##   ..   `Power Augmentation` = col_logical(),
##   ..   `Animal Attributes` = col_logical(),
##   ..   Longevity = col_logical(),
##   ..   Intelligence = col_logical(),
##   ..   `Super Strength` = col_logical(),
##   ..   Cryokinesis = col_logical(),
##   ..   Telepathy = col_logical(),
##   ..   `Energy Armor` = col_logical(),
##   ..   `Energy Blasts` = col_logical(),
##   ..   Duplication = col_logical(),
##   ..   `Size Changing` = col_logical(),
##   ..   `Density Control` = col_logical(),
##   ..   Stamina = col_logical(),
##   ..   `Astral Travel` = col_logical(),
##   ..   `Audio Control` = col_logical(),
##   ..   Dexterity = col_logical(),
##   ..   Omnitrix = col_logical(),
##   ..   `Super Speed` = col_logical(),
##   ..   Possession = col_logical(),
##   ..   `Animal Oriented Powers` = col_logical(),
##   ..   `Weapon-based Powers` = col_logical(),
##   ..   Electrokinesis = col_logical(),
##   ..   `Darkforce Manipulation` = col_logical(),
##   ..   `Death Touch` = col_logical(),
##   ..   Teleportation = col_logical(),
##   ..   `Enhanced Senses` = col_logical(),
##   ..   Telekinesis = col_logical(),
##   ..   `Energy Beams` = col_logical(),
##   ..   Magic = col_logical(),
##   ..   Hyperkinesis = col_logical(),
##   ..   Jump = col_logical(),
##   ..   Clairvoyance = col_logical(),
##   ..   `Dimensional Travel` = col_logical(),
##   ..   `Power Sense` = col_logical(),
##   ..   Shapeshifting = col_logical(),
##   ..   `Peak Human Condition` = col_logical(),
##   ..   Immortality = col_logical(),
##   ..   Camouflage = col_logical(),
##   ..   `Element Control` = col_logical(),
##   ..   Phasing = col_logical(),
##   ..   `Astral Projection` = col_logical(),
##   ..   `Electrical Transport` = col_logical(),
##   ..   `Fire Control` = col_logical(),
##   ..   Projection = col_logical(),
##   ..   Summoning = col_logical(),
##   ..   `Enhanced Memory` = col_logical(),
##   ..   Reflexes = col_logical(),
##   ..   Invulnerability = col_logical(),
##   ..   `Energy Constructs` = col_logical(),
##   ..   `Force Fields` = col_logical(),
##   ..   `Self-Sustenance` = col_logical(),
##   ..   `Anti-Gravity` = col_logical(),
##   ..   Empathy = col_logical(),
##   ..   `Power Nullifier` = col_logical(),
##   ..   `Radiation Control` = col_logical(),
##   ..   `Psionic Powers` = col_logical(),
##   ..   Elasticity = col_logical(),
##   ..   `Substance Secretion` = col_logical(),
##   ..   `Elemental Transmogrification` = col_logical(),
##   ..   `Technopath/Cyberpath` = col_logical(),
##   ..   `Photographic Reflexes` = col_logical(),
##   ..   `Seismic Power` = col_logical(),
##   ..   Animation = col_logical(),
##   ..   Precognition = col_logical(),
##   ..   `Mind Control` = col_logical(),
##   ..   `Fire Resistance` = col_logical(),
##   ..   `Power Absorption` = col_logical(),
##   ..   `Enhanced Hearing` = col_logical(),
##   ..   `Nova Force` = col_logical(),
##   ..   Insanity = col_logical(),
##   ..   Hypnokinesis = col_logical(),
##   ..   `Animal Control` = col_logical(),
##   ..   `Natural Armor` = col_logical(),
##   ..   Intangibility = col_logical(),
##   ..   `Enhanced Sight` = col_logical(),
##   ..   `Molecular Manipulation` = col_logical(),
##   ..   `Heat Generation` = col_logical(),
##   ..   Adaptation = col_logical(),
##   ..   Gliding = col_logical(),
##   ..   `Power Suit` = col_logical(),
##   ..   `Mind Blast` = col_logical(),
##   ..   `Probability Manipulation` = col_logical(),
##   ..   `Gravity Control` = col_logical(),
##   ..   Regeneration = col_logical(),
##   ..   `Light Control` = col_logical(),
##   ..   Echolocation = col_logical(),
##   ..   Levitation = col_logical(),
##   ..   `Toxin and Disease Control` = col_logical(),
##   ..   Banish = col_logical(),
##   ..   `Energy Manipulation` = col_logical(),
##   ..   `Heat Resistance` = col_logical(),
##   ..   `Natural Weapons` = col_logical(),
##   ..   `Time Travel` = col_logical(),
##   ..   `Enhanced Smell` = col_logical(),
##   ..   Illusions = col_logical(),
##   ..   Thirstokinesis = col_logical(),
##   ..   `Hair Manipulation` = col_logical(),
##   ..   Illumination = col_logical(),
##   ..   Omnipotent = col_logical(),
##   ..   Cloaking = col_logical(),
##   ..   `Changing Armor` = col_logical(),
##   ..   `Power Cosmic` = col_logical(),
##   ..   Biokinesis = col_logical(),
##   ..   `Water Control` = col_logical(),
##   ..   `Radiation Immunity` = col_logical(),
##   ..   `Vision - Telescopic` = col_logical(),
##   ..   `Toxin and Disease Resistance` = col_logical(),
##   ..   `Spatial Awareness` = col_logical(),
##   ..   `Energy Resistance` = col_logical(),
##   ..   `Telepathy Resistance` = col_logical(),
##   ..   `Molecular Combustion` = col_logical(),
##   ..   Omnilingualism = col_logical(),
##   ..   `Portal Creation` = col_logical(),
##   ..   Magnetism = col_logical(),
##   ..   `Mind Control Resistance` = col_logical(),
##   ..   `Plant Control` = col_logical(),
##   ..   Sonar = col_logical(),
##   ..   `Sonic Scream` = col_logical(),
##   ..   `Time Manipulation` = col_logical(),
##   ..   `Enhanced Touch` = col_logical(),
##   ..   `Magic Resistance` = col_logical(),
##   ..   Invisibility = col_logical(),
##   ..   `Sub-Mariner` = col_logical(),
##   ..   `Radiation Absorption` = col_logical(),
##   ..   `Intuitive aptitude` = col_logical(),
##   ..   `Vision - Microscopic` = col_logical(),
##   ..   Melting = col_logical(),
##   ..   `Wind Control` = col_logical(),
##   ..   `Super Breath` = col_logical(),
##   ..   Wallcrawling = col_logical(),
##   ..   `Vision - Night` = col_logical(),
##   ..   `Vision - Infrared` = col_logical(),
##   ..   `Grim Reaping` = col_logical(),
##   ..   `Matter Absorption` = col_logical(),
##   ..   `The Force` = col_logical(),
##   ..   Resurrection = col_logical(),
##   ..   Terrakinesis = col_logical(),
##   ..   `Vision - Heat` = col_logical(),
##   ..   Vitakinesis = col_logical(),
##   ..   `Radar Sense` = col_logical(),
##   ..   `Qwardian Power Ring` = col_logical(),
##   ..   `Weather Control` = col_logical(),
##   ..   `Vision - X-Ray` = col_logical(),
##   ..   `Vision - Thermal` = col_logical(),
##   ..   `Web Creation` = col_logical(),
##   ..   `Reality Warping` = col_logical(),
##   ..   `Odin Force` = col_logical(),
##   ..   `Symbiote Costume` = col_logical(),
##   ..   `Speed Force` = col_logical(),
##   ..   `Phoenix Force` = col_logical(),
##   ..   `Molecular Dissipation` = col_logical(),
##   ..   `Vision - Cryo` = col_logical(),
##   ..   Omnipresent = col_logical(),
##   ..   Omniscient = col_logical()
##   .. )
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers%>%
  filter(accelerated_healing == "TRUE" & durability == "TRUE" & super_strength == "TRUE")
```

```
## # A tibble: 97 x 168
##    hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##    <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
##  1 A-Bomb     FALSE   TRUE             FALSE            FALSE           
##  2 Abe Sapien TRUE    TRUE             FALSE            FALSE           
##  3 Angel      TRUE    TRUE             FALSE            FALSE           
##  4 Anti-Moni~ FALSE   TRUE             FALSE            TRUE            
##  5 Anti-Venom FALSE   TRUE             FALSE            FALSE           
##  6 Aquaman    TRUE    TRUE             FALSE            FALSE           
##  7 Arachne    TRUE    TRUE             FALSE            FALSE           
##  8 Archangel  TRUE    TRUE             FALSE            FALSE           
##  9 Ardina     TRUE    TRUE             FALSE            FALSE           
## 10 Ares       TRUE    TRUE             FALSE            FALSE           
## # ... with 87 more rows, and 163 more variables: cold_resistance <lgl>,
## #   durability <lgl>, stealth <lgl>, energy_absorption <lgl>, flight <lgl>,
## #   danger_sense <lgl>, underwater_breathing <lgl>, marksmanship <lgl>,
## #   weapons_master <lgl>, power_augmentation <lgl>, animal_attributes <lgl>,
## #   longevity <lgl>, intelligence <lgl>, super_strength <lgl>,
## #   cryokinesis <lgl>, telepathy <lgl>, energy_armor <lgl>,
## #   energy_blasts <lgl>, duplication <lgl>, size_changing <lgl>,
## #   density_control <lgl>, stamina <lgl>, astral_travel <lgl>,
## #   audio_control <lgl>, dexterity <lgl>, omnitrix <lgl>, super_speed <lgl>,
## #   possession <lgl>, animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, ...
```

## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
superhero_powers%>%
  select(hero_names, contains("kinesis"))%>%
  filter_all(any_vars(. == "TRUE"))
```

```
## # A tibble: 112 x 10
##    hero_names cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <chr>      <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 Alan Scott FALSE       FALSE          FALSE       FALSE        TRUE        
##  2 Amazo      TRUE        FALSE          FALSE       FALSE        FALSE       
##  3 Apocalypse FALSE       FALSE          TRUE        FALSE        FALSE       
##  4 Aqualad    TRUE        FALSE          FALSE       FALSE        FALSE       
##  5 Beyonder   FALSE       FALSE          TRUE        FALSE        FALSE       
##  6 Bizarro    TRUE        FALSE          FALSE       FALSE        TRUE        
##  7 Black Abb~ FALSE       FALSE          TRUE        FALSE        FALSE       
##  8 Black Adam FALSE       FALSE          TRUE        FALSE        FALSE       
##  9 Black Lig~ FALSE       TRUE           FALSE       FALSE        FALSE       
## 10 Black Mam~ FALSE       FALSE          FALSE       FALSE        TRUE        
## # ... with 102 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```

16. Pick your favorite superhero and let's see their powers!

```r
superhero_powers%>%
  filter(hero_names == "Bizarro")
```

```
## # A tibble: 1 x 168
##   hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 Bizarro    TRUE    TRUE             FALSE            FALSE           
## # ... with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>,
## #   omnitrix <lgl>, super_speed <lgl>, possession <lgl>,
## #   animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, ...
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
