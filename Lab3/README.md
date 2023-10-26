# Lab3

#Основы обработки данных с помощью R (часть 2)

## Цель работы

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных.
2.  Закрепить знания основных функций обработки данных экосистемы
    `tidyverse` языка R.
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Ход работы

    > install.packages("dplyr")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/artem/AppData/Local/R/win-library/4.3’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/dplyr_1.1.3.zip'
    Content type 'application/zip' length 1553386 bytes (1.5 MB)
    downloaded 1.5 MB

    пакет ‘dplyr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\artem\AppData\Local\Temp\Rtmp4iLVNc\downloaded_packages
    > library(dplyr)

    Присоединяю пакет: ‘dplyr’

    Следующие объекты скрыты от ‘package:stats’:

        filter, lag

    Следующие объекты скрыты от ‘package:base’:

        intersect, setdiff, setequal, union

    > install.packages("nycflights13")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/artem/AppData/Local/R/win-library/4.3’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.3/nycflights13_1.0.2.zip'
    Content type 'application/zip' length 4510601 bytes (4.3 MB)
    downloaded 4.3 MB

    пакет ‘nycflights13’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\artem\AppData\Local\Temp\Rtmp4iLVNc\downloaded_packages
    > library(nycflights13)

## Ответы на вопросы

1.Сколько встроенных в пакет nycflights13 датафреймов?

    > nycflights13::airlines
    # A tibble: 16 × 2
       carrier name                       
       <chr>   <chr>                      
     1 9E      Endeavor Air Inc.          
     2 AA      American Airlines Inc.     
     3 AS      Alaska Airlines Inc.       
     4 B6      JetBlue Airways            
     5 DL      Delta Air Lines Inc.       
     6 EV      ExpressJet Airlines Inc.   
     7 F9      Frontier Airlines Inc.     
     8 FL      AirTran Airways Corporation
     9 HA      Hawaiian Airlines Inc.     
    10 MQ      Envoy Air                  
    11 OO      SkyWest Airlines Inc.      
    12 UA      United Air Lines Inc.      
    13 US      US Airways Inc.            
    14 VX      Virgin America             
    15 WN      Southwest Airlines Co.     
    16 YV      Mesa Airlines Inc.         
    > nycflights13::airports
    # A tibble: 1,458 × 8
       faa   name                             lat    lon   alt    tz dst   tzone              
       <chr> <chr>                          <dbl>  <dbl> <dbl> <dbl> <chr> <chr>              
     1 04G   Lansdowne Airport               41.1  -80.6  1044    -5 A     America/New_York   
     2 06A   Moton Field Municipal Airport   32.5  -85.7   264    -6 A     America/Chicago    
     3 06C   Schaumburg Regional             42.0  -88.1   801    -6 A     America/Chicago    
     4 06N   Randall Airport                 41.4  -74.4   523    -5 A     America/New_York   
     5 09J   Jekyll Island Airport           31.1  -81.4    11    -5 A     America/New_York   
     6 0A9   Elizabethton Municipal Airport  36.4  -82.2  1593    -5 A     America/New_York   
     7 0G6   Williams County Airport         41.5  -84.5   730    -5 A     America/New_York   
     8 0G7   Finger Lakes Regional Airport   42.9  -76.8   492    -5 A     America/New_York   
     9 0P2   Shoestring Aviation Airfield    39.8  -76.6  1000    -5 U     America/New_York   
    10 0S9   Jefferson County Intl           48.1 -123.    108    -8 A     America/Los_Angeles
    # ℹ 1,448 more rows
    # ℹ Use `print(n = ...)` to see more rows
    > nycflights13::flights
    # A tibble: 336,776 × 19
        year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight tailnum origin dest  air_time distance  hour
       <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>     <dbl> <chr>    <int> <chr>   <chr>  <chr>    <dbl>    <dbl> <dbl>
     1  2013     1     1      517            515         2      830            819        11 UA        1545 N14228  EWR    IAH        227     1400     5
     2  2013     1     1      533            529         4      850            830        20 UA        1714 N24211  LGA    IAH        227     1416     5
     3  2013     1     1      542            540         2      923            850        33 AA        1141 N619AA  JFK    MIA        160     1089     5
     4  2013     1     1      544            545        -1     1004           1022       -18 B6         725 N804JB  JFK    BQN        183     1576     5
     5  2013     1     1      554            600        -6      812            837       -25 DL         461 N668DN  LGA    ATL        116      762     6
     6  2013     1     1      554            558        -4      740            728        12 UA        1696 N39463  EWR    ORD        150      719     5
     7  2013     1     1      555            600        -5      913            854        19 B6         507 N516JB  EWR    FLL        158     1065     6
     8  2013     1     1      557            600        -3      709            723       -14 EV        5708 N829AS  LGA    IAD         53      229     6
     9  2013     1     1      557            600        -3      838            846        -8 B6          79 N593JB  JFK    MCO        140      944     6
    10  2013     1     1      558            600        -2      753            745         8 AA         301 N3ALAA  LGA    ORD        138      733     6
    # ℹ 336,766 more rows
    # ℹ 2 more variables: minute <dbl>, time_hour <dttm>
    # ℹ Use `print(n = ...)` to see more rows
    > nycflights13::planes
    # A tibble: 3,322 × 9
       tailnum  year type                    manufacturer     model     engines seats speed engine   
       <chr>   <int> <chr>                   <chr>            <chr>       <int> <int> <int> <chr>    
     1 N10156   2004 Fixed wing multi engine EMBRAER          EMB-145XR       2    55    NA Turbo-fan
     2 N102UW   1998 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
     3 N103US   1999 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
     4 N104UW   1999 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
     5 N10575   2002 Fixed wing multi engine EMBRAER          EMB-145LR       2    55    NA Turbo-fan
     6 N105UW   1999 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
     7 N107US   1999 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
     8 N108UW   1999 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
     9 N109UW   1999 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
    10 N110UW   1999 Fixed wing multi engine AIRBUS INDUSTRIE A320-214        2   182    NA Turbo-fan
    # ℹ 3,312 more rows
    # ℹ Use `print(n = ...)` to see more rows
    > nycflights13::weather
    # A tibble: 26,115 × 15
       origin  year month   day  hour  temp  dewp humid wind_dir wind_speed wind_gust precip pressure visib time_hour          
       <chr>  <int> <int> <int> <int> <dbl> <dbl> <dbl>    <dbl>      <dbl>     <dbl>  <dbl>    <dbl> <dbl> <dttm>             
     1 EWR     2013     1     1     1  39.0  26.1  59.4      270      10.4         NA      0    1012     10 2013-01-01 01:00:00
     2 EWR     2013     1     1     2  39.0  27.0  61.6      250       8.06        NA      0    1012.    10 2013-01-01 02:00:00
     3 EWR     2013     1     1     3  39.0  28.0  64.4      240      11.5         NA      0    1012.    10 2013-01-01 03:00:00
     4 EWR     2013     1     1     4  39.9  28.0  62.2      250      12.7         NA      0    1012.    10 2013-01-01 04:00:00
     5 EWR     2013     1     1     5  39.0  28.0  64.4      260      12.7         NA      0    1012.    10 2013-01-01 05:00:00
     6 EWR     2013     1     1     6  37.9  28.0  67.2      240      11.5         NA      0    1012.    10 2013-01-01 06:00:00
     7 EWR     2013     1     1     7  39.0  28.0  64.4      240      15.0         NA      0    1012.    10 2013-01-01 07:00:00
     8 EWR     2013     1     1     8  39.9  28.0  62.2      250      10.4         NA      0    1012.    10 2013-01-01 08:00:00
     9 EWR     2013     1     1     9  39.9  28.0  62.2      260      15.0         NA      0    1013.    10 2013-01-01 09:00:00
    10 EWR     2013     1     1    10  41    28.0  59.6      260      13.8         NA      0    1012.    10 2013-01-01 10:00:00
    # ℹ 26,105 more rows
    # ℹ Use `print(n = ...)` to see more rows

2.Сколько строк в каждом датафрейме?

    > airlines %>% nrow()
    [1] 16
    > airports %>% nrow()
    [1] 1458
    > flights %>% nrow()
    [1] 336776
    > planes %>% nrow()
    [1] 3322
    > weather %>% nrow()
    [1] 26115

3.Сколько столбцов в каждом датафрейме?

    > airlines %>% ncol()
    [1] 2
    > airports %>% ncol()
    [1] 8
    > flights %>% ncol()
    [1] 19
    > planes %>% ncol()
    [1] 9
    > weather %>% ncol()
    [1] 15

4.Как просмотреть примерный вид датафрейма?

    > nycflights13::airlines %>% glimpse()
    Rows: 16
    Columns: 2
    $ carrier <chr> "9E", "AA", "AS", "B6", "DL", "EV", "F9", "FL", "HA", "MQ", "OO", "UA", "US", "VX", "WN", "YV"
    $ name    <chr> "Endeavor Air Inc.", "American Airlines Inc.", "Alaska Airlines Inc.", "JetBlue Airways", "Delta Air Lines Inc.", "ExpressJet Airlines …
    > nycflights13::airports %>% glimpse()
    Rows: 1,458
    Columns: 8
    $ faa   <chr> "04G", "06A", "06C", "06N", "09J", "0A9", "0G6", "0G7", "0P2", "0S9", "0W3", "10C", "17G", "19A", "1A3", "1B9", "1C9", "1CS", "1G3", "1G4…
    $ name  <chr> "Lansdowne Airport", "Moton Field Municipal Airport", "Schaumburg Regional", "Randall Airport", "Jekyll Island Airport", "Elizabethton Mu…
    $ lat   <dbl> 41.13047, 32.46057, 41.98934, 41.43191, 31.07447, 36.37122, 41.46731, 42.88356, 39.79482, 48.05381, 39.56684, 42.40289, 40.78156, 34.1758…
    $ lon   <dbl> -80.61958, -85.68003, -88.10124, -74.39156, -81.42778, -82.17342, -84.50678, -76.78123, -76.64719, -122.81064, -76.20240, -88.37511, -82.…
    $ alt   <dbl> 1044, 264, 801, 523, 11, 1593, 730, 492, 1000, 108, 409, 875, 1003, 951, 1789, 122, 152, 670, 1134, 4813, 585, 885, 10, 320, 681, 104, 92…
    $ tz    <dbl> -5, -6, -6, -5, -5, -5, -5, -5, -5, -8, -5, -6, -5, -5, -5, -5, -8, -6, -5, -7, -6, -5, -8, -6, -5, -5, -6, -5, -5, -5, -5, -5, -6, -5, -…
    $ dst   <chr> "A", "A", "A", "A", "A", "A", "A", "A", "U", "A", "A", "U", "A", "U", "A", "A", "A", "U", "A", "A", "A", "U", "A", "A", "A", "A", "A", "A…
    $ tzone <chr> "America/New_York", "America/Chicago", "America/Chicago", "America/New_York", "America/New_York", "America/New_York", "America/New_York",…
    > nycflights13::flights %>% glimpse()
    Rows: 336,776
    Columns: 19
    $ year           <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 20…
    $ month          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ day            <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ dep_time       <int> 517, 533, 542, 544, 554, 554, 555, 557, 557, 558, 558, 558, 558, 558, 559, 559, 559, 600, 600, 601, 602, 602, 606, 606, 607, 608…
    $ sched_dep_time <int> 515, 529, 540, 545, 600, 558, 600, 600, 600, 600, 600, 600, 600, 600, 600, 559, 600, 600, 600, 600, 610, 605, 610, 610, 607, 600…
    $ dep_delay      <dbl> 2, 4, 2, -1, -6, -4, -5, -3, -3, -2, -2, -2, -2, -2, -1, 0, -1, 0, 0, 1, -8, -3, -4, -4, 0, 8, 11, 3, 0, 0, -8, 13, -4, -6, -6, …
    $ arr_time       <int> 830, 850, 923, 1004, 812, 740, 913, 709, 838, 753, 849, 853, 924, 923, 941, 702, 854, 851, 837, 844, 812, 821, 858, 837, 858, 80…
    $ sched_arr_time <int> 819, 830, 850, 1022, 837, 728, 854, 723, 846, 745, 851, 856, 917, 937, 910, 706, 902, 858, 825, 850, 820, 805, 910, 845, 915, 73…
    $ arr_delay      <dbl> 11, 20, 33, -18, -25, 12, 19, -14, -8, 8, -2, -3, 7, -14, 31, -4, -8, -7, 12, -6, -8, 16, -12, -8, -17, 32, 14, 4, -21, -9, 3, 5…
    $ carrier        <chr> "UA", "UA", "AA", "B6", "DL", "UA", "B6", "EV", "B6", "AA", "B6", "B6", "UA", "UA", "AA", "B6", "UA", "B6", "MQ", "B6", "DL", "M…
    $ flight         <int> 1545, 1714, 1141, 725, 461, 1696, 507, 5708, 79, 301, 49, 71, 194, 1124, 707, 1806, 1187, 371, 4650, 343, 1919, 4401, 1895, 1743…
    $ tailnum        <chr> "N14228", "N24211", "N619AA", "N804JB", "N668DN", "N39463", "N516JB", "N829AS", "N593JB", "N3ALAA", "N793JB", "N657JB", "N29129"…
    $ origin         <chr> "EWR", "LGA", "JFK", "JFK", "LGA", "EWR", "EWR", "LGA", "JFK", "LGA", "JFK", "JFK", "JFK", "EWR", "LGA", "JFK", "EWR", "LGA", "L…
    $ dest           <chr> "IAH", "IAH", "MIA", "BQN", "ATL", "ORD", "FLL", "IAD", "MCO", "ORD", "PBI", "TPA", "LAX", "SFO", "DFW", "BOS", "LAS", "FLL", "A…
    $ air_time       <dbl> 227, 227, 160, 183, 116, 150, 158, 53, 140, 138, 149, 158, 345, 361, 257, 44, 337, 152, 134, 147, 170, 105, 152, 128, 157, 139, …
    $ distance       <dbl> 1400, 1416, 1089, 1576, 762, 719, 1065, 229, 944, 733, 1028, 1005, 2475, 2565, 1389, 187, 2227, 1076, 762, 1023, 1020, 502, 1085…
    $ hour           <dbl> 5, 5, 5, 5, 6, 5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6,…
    $ minute         <dbl> 15, 29, 40, 45, 0, 58, 0, 0, 0, 0, 0, 0, 0, 0, 0, 59, 0, 0, 0, 0, 10, 5, 10, 10, 7, 0, 0, 10, 15, 15, 30, 10, 27, 30, 30, 30, 30…
    $ time_hour      <dttm> 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013-01-01 06:00:00, 2013-01-01 05:00:00, 2…
    > nycflights13::planes %>% glimpse()
    Rows: 3,322
    Columns: 9
    $ tailnum      <chr> "N10156", "N102UW", "N103US", "N104UW", "N10575", "N105UW", "N107US", "N108UW", "N109UW", "N110UW", "N11106", "N11107", "N11109", …
    $ year         <int> 2004, 1998, 1999, 1999, 2002, 1999, 1999, 1999, 1999, 1999, 2002, 2002, 2002, 2002, 2002, 2003, 2003, 2003, 2003, 2003, 2004, 2004…
    $ type         <chr> "Fixed wing multi engine", "Fixed wing multi engine", "Fixed wing multi engine", "Fixed wing multi engine", "Fixed wing multi engi…
    $ manufacturer <chr> "EMBRAER", "AIRBUS INDUSTRIE", "AIRBUS INDUSTRIE", "AIRBUS INDUSTRIE", "EMBRAER", "AIRBUS INDUSTRIE", "AIRBUS INDUSTRIE", "AIRBUS …
    $ model        <chr> "EMB-145XR", "A320-214", "A320-214", "A320-214", "EMB-145LR", "A320-214", "A320-214", "A320-214", "A320-214", "A320-214", "EMB-145…
    $ engines      <int> 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2…
    $ seats        <int> 55, 182, 182, 182, 55, 182, 182, 182, 182, 182, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55, 55…
    $ speed        <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ engine       <chr> "Turbo-fan", "Turbo-fan", "Turbo-fan", "Turbo-fan", "Turbo-fan", "Turbo-fan", "Turbo-fan", "Turbo-fan", "Turbo-fan", "Turbo-fan", …
    > nycflights13::weather %>% glimpse()
    Rows: 26,115
    Columns: 15
    $ origin     <chr> "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR", "EWR",…
    $ year       <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, …
    $ month      <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    $ day        <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, …
    $ hour       <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15,…
    $ temp       <dbl> 39.02, 39.02, 39.02, 39.92, 39.02, 37.94, 39.02, 39.92, 39.92, 41.00, 41.00, 39.20, 39.02, 37.94, 37.04, 35.96, 33.98, 33.08, 32.00,…
    $ dewp       <dbl> 26.06, 26.96, 28.04, 28.04, 28.04, 28.04, 28.04, 28.04, 28.04, 28.04, 26.96, 28.40, 24.08, 24.08, 19.94, 19.04, 15.08, 12.92, 15.08,…
    $ humid      <dbl> 59.37, 61.63, 64.43, 62.21, 64.43, 67.21, 64.43, 62.21, 62.21, 59.65, 57.06, 69.67, 54.68, 57.04, 49.62, 49.83, 45.43, 42.84, 49.19,…
    $ wind_dir   <dbl> 270, 250, 240, 250, 260, 240, 240, 250, 260, 260, 260, 330, 280, 290, 300, 330, 310, 320, 310, 320, 320, 310, 310, 330, 330, 320, 33…
    $ wind_speed <dbl> 10.35702, 8.05546, 11.50780, 12.65858, 12.65858, 11.50780, 14.96014, 10.35702, 14.96014, 13.80936, 14.96014, 16.11092, 13.80936, 9.2…
    $ wind_gust  <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, 20.71404, NA, 25.31716, NA, NA, 26.46794, 25.31716, NA, 25.31716, 24.16638, …
    $ precip     <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    $ pressure   <dbl> 1012.0, 1012.3, 1012.5, 1012.2, 1011.9, 1012.4, 1012.2, 1012.2, 1012.7, 1012.4, 1011.4, NA, 1010.8, 1011.9, 1012.1, 1013.2, 1014.1, …
    $ visib      <dbl> 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, …
    $ time_hour  <dttm> 2013-01-01 01:00:00, 2013-01-01 02:00:00, 2013-01-01 03:00:00, 2013-01-01 04:00:00, 2013-01-01 05:00:00, 2013-01-01 06:00:00, 2013-…

5.Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных
(представлено в наборах данных)?

    > length(airlines$carrier)
    [1] 16

6.Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

    > flights %>% filter(origin=='JFK', month==5) %>% nrow()
    [1] 9397

7.Какой самый северный аэропорт?

    > airports %>% filter(lat == max(lat))
    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     NA  

8.Какой аэропорт самый высокогорный (находится выше всех над уровнем
моря)?

    > airports %>% filter(alt == max(alt))
    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

9.Какие бортовые номера у самых старых самолетов?

    > head(arrange(planes, year),n=10)
    # A tibble: 10 × 9
       tailnum  year type                     manufacturer model       engines seats speed engine       
       <chr>   <int> <chr>                    <chr>        <chr>         <int> <int> <int> <chr>        
     1 N381AA   1956 Fixed wing multi engine  DOUGLAS      DC-7BF            4   102   232 Reciprocating
     2 N201AA   1959 Fixed wing single engine CESSNA       150               1     2    90 Reciprocating
     3 N567AA   1959 Fixed wing single engine DEHAVILLAND  OTTER DHC-3       1    16    95 Reciprocating
     4 N378AA   1963 Fixed wing single engine CESSNA       172E              1     4   105 Reciprocating
     5 N575AA   1963 Fixed wing single engine CESSNA       210-5(205)        1     6    NA Reciprocating
     6 N14629   1965 Fixed wing multi engine  BOEING       737-524           2   149    NA Turbo-fan    
     7 N615AA   1967 Fixed wing multi engine  BEECH        65-A90            2     9   202 Turbo-prop   
     8 N425AA   1968 Fixed wing single engine PIPER        PA-28-180         1     4   107 Reciprocating
     9 N383AA   1972 Fixed wing multi engine  BEECH        E-90              2    10    NA Turbo-prop   
    10 N364AA   1973 Fixed wing multi engine  CESSNA       310Q              2     6   167 Reciprocating

10.Какая средняя температура воздуха была в сентябре в аэропорту John F
Kennedy Intl (в градусах Цельсия).

    > nycflights13::weather %>%filter(month == 9,origin == "JFK") %>%summarise("temp" = ((temp = mean(temp,0))-32)*5/9)
    # A tibble: 1 × 1
       temp
      <dbl>
    1  19.4

11.Самолеты какой авиакомпании совершили больше всего вылетов в июне?

    > flights %>% filter(month == 6) %>% group_by(carrier) %>% summarize(flights_count = n()) %>% arrange(desc(flights_count)) %>% head(1)
    # A tibble: 1 × 2
      carrier flights_count
      <chr>           <int>
    1 UA               4975

12.Самолеты какой авиакомпании задерживались чаще других в 2013 году?

    > flights %>% filter(year == 2013 & dep_delay > 0) %>% group_by(carrier) %>% summarise(delays_count = n()) %>% arrange(desc(delays_count)) %>% head(1)
    # A tibble: 1 × 2
      carrier delays_count
      <chr>          <int>
    1 UA             27261

##Вывод

Были получены базовые навыки обработки данных с помощью языка R и
встроенного пакета `dplyr`, а также получен навык работы с датафреймом
из пакета `nycflights13`.
