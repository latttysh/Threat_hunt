## Основы обработки данных с помощью R

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функция обработки данных
    пакета dplyre - функций select(), filter(), mutate(), arrange(),
    group\_by()

## Задание

Проанализировать встроенный пакет dplyr набор данных starwars с помощью
языа R и ответить на вопросы:

1.  Сколько строк в датафрейме?

<!-- -->

    install.packages("tidyverse")

    library(dplyr)

    starwars <- starwars
    starwars %>% nrow()

    ## [1] 87

Ответ: 87 rows

1.  Cколько столбцов в датафрейме?

<!-- -->

    starwars %>% ncol()

    ## [1] 14

Ответ: 14

1.  Как просмотреть примерный вид датафрейма?

<!-- -->

    starwars %>% glimpse()

    ## Rows: 87
    ## Columns: 14
    ## $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Or…
    ## $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 2…
    ## $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.…
    ## $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", N…
    ## $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "…
    ## $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue",…
    ## $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, …
    ## $ sex        <chr> "male", "none", "none", "male", "female", "male", "female",…
    ## $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "femini…
    ## $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "T…
    ## $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Huma…
    ## $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return…
    ## $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp…
    ## $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",…

1.  Сколько уникальных рас персонажей(species) представлено в данных?

<!-- -->

    unique(starwars['species'], incomparables = FALSE)

    ## # A tibble: 38 × 1
    ##    species       
    ##    <chr>         
    ##  1 Human         
    ##  2 Droid         
    ##  3 Wookiee       
    ##  4 Rodian        
    ##  5 Hutt          
    ##  6 Yoda's species
    ##  7 Trandoshan    
    ##  8 Mon Calamari  
    ##  9 Ewok          
    ## 10 Sullustan     
    ## # … with 28 more rows

Ответ: 38, включая NA.

5)Найти самого высокого персонажа

    starwars['height'][is.na(starwars['height'])] <- 0
    starwarsDF <- as.data.frame(dplyr::starwars) %>% dplyr::filter(height == max(starwars['height']))
    starwarsDF$name

    ## [1] "Yarael Poof"

Ответ: Yarael Poof

1.  Найти всех персонажей ниже 170 Ответ:

<!-- -->

    starwars_lo <- as.data.frame(dplyr::starwars) %>% dplyr::filter(height < 170)
    starwars_lo$name

    ##  [1] "C-3PO"                 "R2-D2"                 "Leia Organa"          
    ##  [4] "Beru Whitesun lars"    "R5-D4"                 "Yoda"                 
    ##  [7] "Mon Mothma"            "Wicket Systri Warrick" "Nien Nunb"            
    ## [10] "Watto"                 "Sebulba"               "Shmi Skywalker"       
    ## [13] "Dud Bolt"              "Gasgano"               "Ben Quadinaros"       
    ## [16] "Cordé"                 "Barriss Offee"         "Dormé"                
    ## [19] "Zam Wesell"            "Jocasta Nu"            "Ratts Tyerell"        
    ## [22] "R4-P17"                "Padmé Amidala"

1.  Подсчитать ИМТ(индекс массы тела) для всех персонажей. Формула: i =
    масса / рост^2 Ответ:

<!-- -->

    starwars %>%
      mutate(name, imt = mass/((height/100)^2))%>%
      select(name, imt)

    ## # A tibble: 87 × 2
    ##    name                 imt
    ##    <chr>              <dbl>
    ##  1 Luke Skywalker      26.0
    ##  2 C-3PO               26.9
    ##  3 R2-D2               34.7
    ##  4 Darth Vader         33.3
    ##  5 Leia Organa         21.8
    ##  6 Owen Lars           37.9
    ##  7 Beru Whitesun lars  27.5
    ##  8 R5-D4               34.0
    ##  9 Biggs Darklighter   25.1
    ## 10 Obi-Wan Kenobi      23.2
    ## # … with 77 more rows

1.  Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по
    отношению массы (mass) к росту (height) персонажей Ответ:

<!-- -->

    elongated <- starwars %>% group_by(name)  %>% summarise(elong=mass/height)
    head(arrange(elongated, desc(elong)), 10)

    ## # A tibble: 10 × 2
    ##    name                  elong
    ##    <chr>                 <dbl>
    ##  1 Jabba Desilijic Tiure 7.76 
    ##  2 Grievous              0.736
    ##  3 IG-88                 0.7  
    ##  4 Owen Lars             0.674
    ##  5 Darth Vader           0.673
    ##  6 Jek Tono Porkins      0.611
    ##  7 Bossk                 0.595
    ##  8 Tarfful               0.581
    ##  9 Dexter Jettster       0.515
    ## 10 Chewbacca             0.491

1.  Найти средний возраст персонажей каждой расы вселенной Звездных войн
    Ответ:

<!-- -->

    starwars %>% 
      filter(!is.na(birth_year)) %>% filter(!is.na(species)) %>%
      group_by(species)  %>% summarise(mean_age= mean(birth_year))

    ## # A tibble: 15 × 2
    ##    species        mean_age
    ##    <chr>             <dbl>
    ##  1 Cerean             92  
    ##  2 Droid              53.3
    ##  3 Ewok                8  
    ##  4 Gungan             52  
    ##  5 Human              53.4
    ##  6 Hutt              600  
    ##  7 Kel Dor            22  
    ##  8 Mirialan           49  
    ##  9 Mon Calamari       41  
    ## 10 Rodian             44  
    ## 11 Trandoshan         53  
    ## 12 Twi'lek            48  
    ## 13 Wookiee           200  
    ## 14 Yoda's species    896  
    ## 15 Zabrak             54

1.  Найти самый распространенный цвет глаз персонажей вселенной Звездных
    войн

<!-- -->

    newDF <- starwars %>% group_by(eye_color) %>% summarise(count=n())
    head(arrange(newDF, desc(count)), 1)

    ## # A tibble: 1 × 2
    ##   eye_color count
    ##   <chr>     <int>
    ## 1 brown        21

Ответ: brown

1.  Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн

Ответ:

    mean_name_DF <- starwars %>% filter(!is.na(species)) %>% group_by(species)  %>% summarise(mean_lenght=mean(nchar(name)))
    mean_name_DF

    ## # A tibble: 37 × 2
    ##    species   mean_lenght
    ##    <chr>           <dbl>
    ##  1 Aleena          13   
    ##  2 Besalisk        15   
    ##  3 Cerean          12   
    ##  4 Chagrian        10   
    ##  5 Clawdite        10   
    ##  6 Droid            4.83
    ##  7 Dug              7   
    ##  8 Ewok            21   
    ##  9 Geonosian       17   
    ## 10 Gungan          11.7 
    ## # … with 27 more rows
