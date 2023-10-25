# Lab2

#Основы обработки данных с помощью R (часть 1)

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Ход работы

    > install.packages("dplyr")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/artem/AppData/Local/R/win-library/4.3’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимости ‘fansi’, ‘utf8’, ‘pkgconfig’, ‘generics’, ‘pillar’, ‘tibble’, ‘tidyselect’

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/fansi_1.0.5.zip'
    Content type 'application/zip' length 313996 bytes (306 KB)
    downloaded 306 KB

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/utf8_1.2.4.zip'
    Content type 'application/zip' length 149764 bytes (146 KB)
    downloaded 146 KB

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/pkgconfig_2.0.3.zip'
    Content type 'application/zip' length 22549 bytes (22 KB)
    downloaded 22 KB

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/generics_0.1.3.zip'
    Content type 'application/zip' length 80221 bytes (78 KB)
    downloaded 78 KB

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/pillar_1.9.0.zip'
    Content type 'application/zip' length 659259 bytes (643 KB)
    downloaded 643 KB

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/tibble_3.2.1.zip'
    Content type 'application/zip' length 690840 bytes (674 KB)
    downloaded 674 KB

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/tidyselect_1.2.0.zip'
    Content type 'application/zip' length 224242 bytes (218 KB)
    downloaded 218 KB

    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/dplyr_1.1.3.zip'
    Content type 'application/zip' length 1553386 bytes (1.5 MB)
    downloaded 1.5 MB

    пакет ‘fansi’ успешно распакован, MD5-суммы проверены
    пакет ‘utf8’ успешно распакован, MD5-суммы проверены
    пакет ‘pkgconfig’ успешно распакован, MD5-суммы проверены
    пакет ‘generics’ успешно распакован, MD5-суммы проверены
    пакет ‘pillar’ успешно распакован, MD5-суммы проверены
    пакет ‘tibble’ успешно распакован, MD5-суммы проверены
    пакет ‘tidyselect’ успешно распакован, MD5-суммы проверены
    пакет ‘dplyr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\artem\AppData\Local\Temp\Rtmps7bhsP\downloaded_packages
    > library(dplyr)

    Присоединяю пакет: ‘dplyr’

    Следующие объекты скрыты от ‘package:stats’:

        filter, lag

    Следующие объекты скрыты от ‘package:base’:

        intersect, setdiff, setequal, union

## Ответы на вопросы

1.Сколько строк в датафрейме?

    > starwars %>% nrow()
    [1] 87

2.Сколько столбцов в датафрейме?

    > starwars %>% ncol()
    [1] 14

3.Как посмотреть примерный вид датафрейма?

    > starwars %>% glimpse()
    Rows: 87
    Columns: 14
    $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Organa", "Owen Lars", "Beru Whitesun lars", "R5-D4", "Bigg…
    $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 228, 180, 173, 175, 170, 180, 66, 170, 183, 200, 190, 177…
    $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.0, 84.0, NA, 112.0, 80.0, 74.0, 1358.0, 77.0, 110.0, 17.…
    $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", NA, "black", "auburn, white", "blond", "auburn, grey", "b…
    $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "light", "white, red", "light", "fair", "fair", "fair", "…
    $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue", "red", "brown", "blue-gray", "blue", "blue", "blue", "b…
    $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, 41.9, 64.0, 200.0, 29.0, 44.0, 600.0, 21.0, NA, 896.0, 8…
    $ sex        <chr> "male", "none", "none", "male", "female", "male", "female", "none", "male", "male", "male", "male", "male", "male",…
    $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "feminine", "masculine", "feminine", "masculine", "masculine", …
    $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "Tatooine", "Tatooine", "Tatooine", "Tatooine", "Stewjon",…
    $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Human", "Droid", "Human", "Human", "Human", "Human", "Wookie…
    $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return of the Jedi", "A New Hope", "The Force Awakens">, <"The…
    $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imperial Speeder Bike", <>, <>, <>, <>, "Tribubble bongo", …
    $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1", <>, <>, <>, <>, "X-wing", <"Jedi starfighter", "Trade F…

4.Сколько уникальных рас персонажей (species) представлено в данных?

    > starwars %>% select(species) %>% unique()
    # A tibble: 38 × 1
       species       
       <chr>         
     1 Human         
     2 Droid         
     3 Wookiee       
     4 Rodian        
     5 Hutt          
     6 Yoda's species
     7 Trandoshan    
     8 Mon Calamari  
     9 Ewok          
    10 Sullustan     
    # ℹ 28 more rows
    # ℹ Use `print(n = ...)` to see more rows

5.Найти самого высокого персонажа.

    > starwars %>% filter(height == max(height, na.rm = TRUE))
    # A tibble: 1 × 14
      name        height  mass hair_color skin_color eye_color birth_year sex   gender    homeworld species  films     vehicles  starships
      <chr>        <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr>     <chr>     <chr>    <list>    <list>    <list>   
    1 Yarael Poof    264    NA none       white      yellow            NA male  masculine Quermia   Quermian <chr [1]> <chr [0]> <chr [0]>

6.Найти всех персонажей ниже 170.

    > starwars %>% filter(height < 170)
    # A tibble: 23 × 14
       name                  height  mass hair_color skin_color  eye_color birth_year sex    gender    homeworld species        films     vehicles  starships
       <chr>                  <int> <dbl> <chr>      <chr>       <chr>          <dbl> <chr>  <chr>     <chr>     <chr>          <list>    <list>    <list>   
     1 C-3PO                    167    75 NA         gold        yellow           112 none   masculine Tatooine  Droid          <chr [6]> <chr [0]> <chr [0]>
     2 R2-D2                     96    32 NA         white, blue red               33 none   masculine Naboo     Droid          <chr [7]> <chr [0]> <chr [0]>
     3 Leia Organa              150    49 brown      light       brown             19 female feminine  Alderaan  Human          <chr [5]> <chr [1]> <chr [0]>
     4 Beru Whitesun lars       165    75 brown      light       blue              47 female feminine  Tatooine  Human          <chr [3]> <chr [0]> <chr [0]>
     5 R5-D4                     97    32 NA         white, red  red               NA none   masculine Tatooine  Droid          <chr [1]> <chr [0]> <chr [0]>
     6 Yoda                      66    17 white      green       brown            896 male   masculine NA        Yoda's species <chr [5]> <chr [0]> <chr [0]>
     7 Mon Mothma               150    NA auburn     fair        blue              48 female feminine  Chandrila Human          <chr [1]> <chr [0]> <chr [0]>
     8 Wicket Systri Warrick     88    20 brown      brown       brown              8 male   masculine Endor     Ewok           <chr [1]> <chr [0]> <chr [0]>
     9 Nien Nunb                160    68 none       grey        black             NA male   masculine Sullust   Sullustan      <chr [1]> <chr [0]> <chr [1]>
    10 Watto                    137    NA black      blue, grey  yellow            NA male   masculine Toydaria  Toydarian      <chr [2]> <chr [0]> <chr [0]>
    # ℹ 13 more rows
    # ℹ Use `print(n = ...)` to see more rows

7.Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать
по формуле I = m/h2, где m – масса (weight), а h – рост (height).

    > starwars %>% mutate(BMI = mass / ((height / 100) ^ 2)) %>% select(name, BMI)
    # A tibble: 87 × 2
       name                 BMI
       <chr>              <dbl>
     1 Luke Skywalker      26.0
     2 C-3PO               26.9
     3 R2-D2               34.7
     4 Darth Vader         33.3
     5 Leia Organa         21.8
     6 Owen Lars           37.9
     7 Beru Whitesun lars  27.5
     8 R5-D4               34.0
     9 Biggs Darklighter   25.1
    10 Obi-Wan Kenobi      23.2
    # ℹ 77 more rows
    # ℹ Use `print(n = ...)` to see more rows

8.Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по
отношению массы (mass) к росту (height) персонажей.

    > starwars %>% mutate(long_per = mass / height) %>% arrange(desc(long_per)) %>% head(10) %>% select(name, long_per)
    # A tibble: 10 × 2
       name                  long_per
       <chr>                    <dbl>
     1 Jabba Desilijic Tiure    7.76 
     2 Grievous                 0.736
     3 IG-88                    0.7  
     4 Owen Lars                0.674
     5 Darth Vader              0.673
     6 Jek Tono Porkins         0.611
     7 Bossk                    0.595
     8 Tarfful                  0.581
     9 Dexter Jettster          0.515
    10 Chewbacca                0.491

9.Найти средний возраст персонажей каждой расы вселенной Звездных войн.

    > starwars %>% group_by(species) %>% summarise(mean(birth_year, na.rm = TRUE))
    # A tibble: 38 × 2
       species   `mean(birth_year, na.rm = TRUE)`
       <chr>                                <dbl>
     1 Aleena                               NaN  
     2 Besalisk                             NaN  
     3 Cerean                                92  
     4 Chagrian                             NaN  
     5 Clawdite                             NaN  
     6 Droid                                 53.3
     7 Dug                                  NaN  
     8 Ewok                                   8  
     9 Geonosian                            NaN  
    10 Gungan                                52  
    # ℹ 28 more rows
    # ℹ Use `print(n = ...)` to see more rows

10.Найти самый распространенный цвет глаз персонажей вселенной Звездных
войн.

    > starwars %>% count(eye_color) %>% filter(n == max(n))
    # A tibble: 1 × 2
      eye_color     n
      <chr>     <int>
    1 brown        21

11.Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн.

    > starwars %>% group_by(species) %>% summarise(avg_name_len = mean(nchar(name)))
    # A tibble: 38 × 2
       species   avg_name_len
       <chr>            <dbl>
     1 Aleena           13   
     2 Besalisk         15   
     3 Cerean           12   
     4 Chagrian         10   
     5 Clawdite         10   
     6 Droid             4.83
     7 Dug               7   
     8 Ewok             21   
     9 Geonosian        17   
    10 Gungan           11.7 
    # ℹ 28 more rows
    # ℹ Use `print(n = ...)` to see more rows

##Вывод

Были получены базовые навыки обработки данных с помощью языка R и
встроенного пакета `dplyr`.
