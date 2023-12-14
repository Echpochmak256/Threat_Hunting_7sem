# Lab4

# Исследование метаданных DNS трафика

## Цель работы

1.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## План

Вы исследуете подозрительную сетевую активность во внутренней сети
Доброй Организации. Вам в руки по- пали метаданные о DNS трафике в
исследуемой сети. Исследуйте файлы, восстановите данные, подготовьте их
к анализу и дайте обоснованные ответы на поставленные вопросы
исследования.

    library(dplyr)
    library(tidyverse)

## Ход работы

Для выполнения предложенного задания Вам необходимо последовательно
проделать следующие шаги:

## Подготовка данных

1.Подготовка к работе (Импортируем данные из файлов dns.log и
header.csv)

    > log <- read.csv("C:\\Users\\artem\\Downloads\\dns.log", sep = "\t", header = FALSE)
    > header <- read.csv("C:\\Users\\artem\\Downloads\\header.csv")

2.Добавьте пропущенные данные о структуре данных (назначении столбцов)

    > names(log) <- c("ts","uid","id.orig_h","id.orig_p","id.resp_h","id.resp_p","proto","trans_id","query","qclass","qclass_name","qtype","qtype_name","rcode","rcode_name","AA","TC", "RD","RA","Z","answers","TTLs","rejected")
    > log
               ts                uid                 id.orig_h id.orig_p       id.resp_h id.resp_p proto trans_id
    1  1331901006 CWGtK431H9XuaTN4fi           192.168.202.100     45658  192.168.27.203       137   udp    33008
    2  1331901015  C36a282Jljz7BsbGH            192.168.202.76       137 192.168.202.255       137   udp    57402
    3  1331901016  C36a282Jljz7BsbGH            192.168.202.76       137 192.168.202.255       137   udp    57402
    4  1331901017  C36a282Jljz7BsbGH            192.168.202.76       137 192.168.202.255       137   udp    57402
    5  1331901006  C36a282Jljz7BsbGH            192.168.202.76       137 192.168.202.255       137   udp    57398
    6  1331901007  C36a282Jljz7BsbGH            192.168.202.76       137 192.168.202.255       137   udp    57398
    7  1331901007  C36a282Jljz7BsbGH            192.168.202.76       137 192.168.202.255       137   udp    57398
    8  1331901006 ClEZCt3GLkJdtGGmAa            192.168.202.89       137 192.168.202.255       137   udp    62187
    9  1331901007 ClEZCt3GLkJdtGGmAa            192.168.202.89       137 192.168.202.255       137   udp    62187
    10 1331901008 ClEZCt3GLkJdtGGmAa            192.168.202.89       137 192.168.202.255       137   udp    62187
    11 1331901007 CpD4i41jyaYqmTBMH3            192.168.202.89       137    10.7.136.159       137   udp    62190
    12 1331901009 CpD4i41jyaYqmTBMH3            192.168.202.89       137    10.7.136.159       137   udp    62190
    13 1331901010 CpD4i41jyaYqmTBMH3            192.168.202.89       137    10.7.136.159       137   udp    62190
    14 1331901018 CNR6Mr4ep4UVh9tSxj           192.168.202.100     45658  192.168.27.103      5353   udp        0
    15 1331901018 Cexi8C2x1d9kI3a8Hd           192.168.202.100     45659  192.168.27.103      5353   udp        0
    16 1331901025  CKGSIMayT9QWEqORb           192.168.202.100     45658  192.168.27.202       137   udp    33008
    17 1331901026 CbOoya1FcP9upkiSN5            192.168.202.85       137 192.168.202.255       137   udp    34107
    18 1331901026 CiQXPA3Dtwoz9CV0ii           192.168.202.102       137 192.168.202.255       137   udp    32821
    19 1331901026 CiQXPA3Dtwoz9CV0ii           192.168.202.102       137 192.168.202.255       137   udp    32818
    20 1331901007 Cgrcup1c5uGRx428V7            192.168.202.93     60821    172.19.1.100        53   udp     3550
    21 1331901008 Cgrcup1c5uGRx428V7            192.168.202.93     60821    172.19.1.100        53   udp     3550
    22 1331901011 Cgrcup1c5uGRx428V7            192.168.202.93     60821    172.19.1.100        53   udp    35599
    23 1331901020 Cgrcup1c5uGRx428V7            192.168.202.93     60821    172.19.1.100        53   udp    35599
    24 1331901011 CN3iol3Ge5ULjbEFph            192.168.202.93     61184    172.19.1.100        53   udp    40931
    25 1331901020 CN3iol3Ge5ULjbEFph            192.168.202.93     61184    172.19.1.100        53   udp    40931
    26 1331901007 CN3iol3Ge5ULjbEFph            192.168.202.93     61184    172.19.1.100        53   udp    25983
    27 1331901008 CN3iol3Ge5ULjbEFph            192.168.202.93     61184    172.19.1.100        53   udp    25983
    28 1331901024 CDzHPo17B429xLtaVb            192.168.202.97     59011   156.154.70.22        53   udp    58389
    29 1331901026 CDzHPo17B429xLtaVb            192.168.202.97     59011   156.154.70.22        53   udp    58389
    30 1331901028 CDzHPo17B429xLtaVb            192.168.202.97     59011   156.154.70.22        53   udp    58389
    31 1331901032 CDzHPo17B429xLtaVb            192.168.202.97     59011   156.154.70.22        53   udp    58389
    32 1331901025 COWLuQ2XIWQK3t4VPj            192.168.202.97     59011      8.26.56.26        53   udp    58389
    33 1331901028 COWLuQ2XIWQK3t4VPj            192.168.202.97     59011      8.26.56.26        53   udp    58389
    34 1331901032 COWLuQ2XIWQK3t4VPj            192.168.202.97     59011      8.26.56.26        53   udp    58389
    35 1331901026   CXxpisATPtm5jKlP fe80::ba8d:12ff:fe53:a8d8      5353        ff02::fb      5353   udp        0
    36 1331901030   CXxpisATPtm5jKlP fe80::ba8d:12ff:fe53:a8d8      5353        ff02::fb      5353   udp        0
    37 1331901026 CTgyMB2GL0FCCKgv04            192.168.202.71       137 192.168.202.255       137   udp    19333
    38 1331901027 CTgyMB2GL0FCCKgv04            192.168.202.71       137 192.168.202.255       137   udp    19333
    39 1331901028 CTgyMB2GL0FCCKgv04            192.168.202.71       137 192.168.202.255       137   udp    19333
    40 1331901026 CTgyMB2GL0FCCKgv04            192.168.202.71       137 192.168.202.255       137   udp    19341
    41 1331901027 CTgyMB2GL0FCCKgv04            192.168.202.71       137 192.168.202.255       137   udp    19333
    42 1331901028 CTgyMB2GL0FCCKgv04            192.168.202.71       137 192.168.202.255       137   udp    19333
    43 1331901026 CTgyMB2GL0FCCKgv04            192.168.202.71       137 192.168.202.255       137   udp    19317
                                                                         query qclass qclass_name qtype qtype_name rcode rcode_name    AA    TC    RD
    1  *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00      1  C_INTERNET    33        SRV     0    NOERROR FALSE FALSE FALSE
    2                                                                 HPE8AA67      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    3                                                                 HPE8AA67      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    4                                                                 HPE8AA67      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    5                                                                     WPAD      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    6                                                                     WPAD      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    7                                                                     WPAD      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    8                                                                   EWREP1      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    9                                                                   EWREP1      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    10                                                                  EWREP1      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    11 *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00      1  C_INTERNET    33        SRV     -          - FALSE FALSE FALSE
    12 *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00      1  C_INTERNET    33        SRV     -          - FALSE FALSE FALSE
    13 *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00      1  C_INTERNET    33        SRV     -          - FALSE FALSE FALSE
    14                                            _services._dns-sd._udp.local      1  C_INTERNET    12        PTR     -          - FALSE FALSE FALSE
    15                                            _services._dns-sd._udp.local      1  C_INTERNET    12        PTR     -          - FALSE FALSE FALSE
    16 *\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00      1  C_INTERNET    33        SRV     0    NOERROR FALSE FALSE FALSE
    17                                                         SDS-MACBOOK-PRO      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    18                                                            C02GN35UDJWR      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    19                                                         SDS-MACBOOK-PRO      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    20                                                           www.apple.com      1  C_INTERNET    28       AAAA     -          - FALSE FALSE  TRUE
    21                                                           www.apple.com      1  C_INTERNET    28       AAAA     -          - FALSE FALSE  TRUE
    22                                                           www.apple.com      1  C_INTERNET    28       AAAA     -          - FALSE FALSE  TRUE
    23                                                           www.apple.com      1  C_INTERNET    28       AAAA     -          - FALSE FALSE  TRUE
    24                                                           www.apple.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    25                                                           www.apple.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    26                                                           www.apple.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    27                                                           www.apple.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    28                                                          www.comodo.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    29                                                          www.comodo.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    30                                                          www.comodo.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    31                                                          www.comodo.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    32                                                          www.comodo.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    33                                                          www.comodo.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    34                                                          www.comodo.com      1  C_INTERNET     1          A     -          - FALSE FALSE  TRUE
    35                                                                       -      -           -     -          -     -          - FALSE FALSE FALSE
    36                                                                       -      -           -     -          -     -          - FALSE FALSE FALSE
    37                                                                 (empty)      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    38                                                                 (empty)      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    39                                                                 (empty)      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    40                                                                  MSHOME      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    41                                                                 (empty)      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    42                                                                 (empty)      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
    43                                             \\x01\\x02__MSBROWSE__\\x02      1  C_INTERNET    32         NB     -          - FALSE FALSE  TRUE
          RA Z answers TTLs rejected
    1  FALSE 1       -    -    FALSE
    2  FALSE 1       -    -    FALSE
    3  FALSE 1       -    -    FALSE
    4  FALSE 1       -    -    FALSE
    5  FALSE 1       -    -    FALSE
    6  FALSE 1       -    -    FALSE
    7  FALSE 1       -    -    FALSE
    8  FALSE 1       -    -    FALSE
    9  FALSE 1       -    -    FALSE
    10 FALSE 1       -    -    FALSE
    11 FALSE 0       -    -    FALSE
    12 FALSE 0       -    -    FALSE
    13 FALSE 0       -    -    FALSE
    14 FALSE 0       -    -    FALSE
    15 FALSE 0       -    -    FALSE
    16 FALSE 1       -    -    FALSE
    17 FALSE 1       -    -    FALSE
    18 FALSE 1       -    -    FALSE
    19 FALSE 1       -    -    FALSE
    20 FALSE 0       -    -    FALSE
    21 FALSE 0       -    -    FALSE
    22 FALSE 0       -    -    FALSE
    23 FALSE 0       -    -    FALSE
    24 FALSE 0       -    -    FALSE
    25 FALSE 0       -    -    FALSE
    26 FALSE 0       -    -    FALSE
    27 FALSE 0       -    -    FALSE
    28 FALSE 0       -    -    FALSE
    29 FALSE 0       -    -    FALSE
    30 FALSE 0       -    -    FALSE
    31 FALSE 0       -    -    FALSE
    32 FALSE 0       -    -    FALSE
    33 FALSE 0       -    -    FALSE
    34 FALSE 0       -    -    FALSE
    35 FALSE 0       -    -    FALSE
    36 FALSE 0       -    -    FALSE
    37 FALSE 1       -    -    FALSE
    38 FALSE 1       -    -    FALSE
    39 FALSE 1       -    -    FALSE
    40 FALSE 1       -    -    FALSE
    41 FALSE 1       -    -    FALSE
    42 FALSE 1       -    -    FALSE
    43 FALSE 1       -    -    FALSE
     [ reached 'max' / getOption("max.print") -- omitted 427892 rows ]

3.Преобразуйте данные в столбцах в нужный формат

4.Просмотрите общую структуру данных с помощью функции glimpse()

    > glimpse(log)
    Rows: 427,935
    Columns: 23
    $ ts          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901006, 1331901007, 1331901007, 1331901006, 1331901007, 1331901008, 1331901007, 1331901009, 1331901010, 13…
    $ uid         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz7BsbGH", "C36a282Jljz7BsbGH", "C36a282Jljz7BsbGH", "C36a282Jljz7BsbGH", "C36a282Jljz7BsbGH", "ClEZCt3GL…
    $ id.orig_h   <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", "192.168.202.76", "192.168.202.76", "192.168.202.76", "192.168.202.76", "192.168.202.89", "192.168.202.…
    $ id.orig_p   <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 45658, 45659, 45658, 137, 137, 137, 60821, 60821, 60821, 60821, 61184, 61184, 61184, 61184,…
    $ id.resp_h   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255", "192.168.202.255", "192.168.202.255", "192.168.202.255", "192.168.202.255", "192.168.202.255", "192.16…
    $ id.resp_p   <int> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 5353, 5353, 137, 137, 137, 137, 53, 53, 53, 53, 53, 53, 53, 53, 53, 53, 53, 53, 53, 53, 53, 5…
    $ proto       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp…
    $ trans_id    <int> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187, 62187, 62187, 62190, 62190, 62190, 0, 0, 33008, 34107, 32821, 32818, 3550, 3550, 35599, 35599, 40931, …
    $ query       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00", "HPE8AA67", "HPE8AA67", "HPE8AA67", "WPAD", "WPAD", "WPAD", "EWREP1", "EWREP1", "EW…
    $ qclass      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    $ qclass_name <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_I…
    $ qtype       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "32", "33", "33", "33", "12", "12", "33", "32", "32", "32", "28", "28", "28", "28", "1", "1", "1", "1", …
    $ qtype_name  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "SRV", "SRV", "SRV", "PTR", "PTR", "SRV", "NB", "NB", "NB", "AAAA", "AAAA", "AAAA", "AAAA", "A", …
    $ rcode       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rcode_name  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "…
    $ AA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
    $ TC          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
    $ RD          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, T…
    $ RA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
    $ Z           <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ answers     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ TTLs        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rejected    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…

## Анализ

1.Сколько участников информационного обмена в сети Доброй Организации?

    > unique_ips <- union(unique(log$id.orig_h), unique(log$id.resp_h))
    > unique_ips %>% length()
    [1] 1359

2.Какое соотношение участников обмена внутри сети и участников обращений
к внешним ресурсам?

    > internal_ip_pattern <- c("192.168.", "10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).")
    > internal_ips <- unique_ips[grep(paste(internal_ip_pattern, collapse = "|"), unique_ips)]
    > internal <- sum(unique_ips %in% internal_ips)
    > external <- length(unique_ips) - internal
    > internal_ip_pattern <- c("192.168.", "10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).")
    > internal_ips <- unique_ips[grep(paste(internal_ip_pattern, collapse = "|"), unique_ips)]
    > internal <- sum(unique_ips %in% internal_ips)
    > external <- length(unique_ips) - internal
    > 
    > ratio <- external / internal
    > ratio
    [1] 0.064213

3.Найдите топ-10 участников сети, проявляющих наибольшую сетевую
активность.

    > top_10_activity <- log%>%
         group_by(ip=id.orig_h)%>%
         summarise(activity = n())%>%
         arrange(desc(activity))%>%
         head(10)
    > ip <- select(top_10_activity, ip)
    > ip
    # A tibble: 10 × 1
       ip             
       <chr>          
     1 10.10.117.210  
     2 192.168.202.93 
     3 192.168.202.103
     4 192.168.202.76 
     5 192.168.202.97 
     6 192.168.202.141
     7 10.10.117.209  
     8 192.168.202.110
     9 192.168.203.63 
    10 192.168.202.106

4.Найдите топ-10 доменов, к которым обращаются пользователи сети и
соответственное количество обращений.

    > top_10_domain <- log %>%
         group_by(domain = tolower(query))%>%
         summarise(request = n())%>%
         arrange(desc(request))%>%
         head(10)
    > domains <- select(top_10_domain, domain)
    > domains
    # A tibble: 10 × 1
       domain                                                                   
       <chr>                                                                    
     1 "teredo.ipv6.microsoft.com"                                              
     2 "tools.google.com"                                                       
     3 "www.apple.com"                                                          
     4 "time.apple.com"                                                         
     5 "safebrowsing.clients.google.com"                                        
     6 "wpad"                                                                   
     7 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00"
     8 "isatap"                                                                 
     9 "44.206.168.192.in-addr.arpa"                                            
    10 "hpe8aa67" 

5.Опеределите базовые статистические характеристики (функция summary())
интервала времени между последовательным обращениями к топ-10 доменам.

    > summary(log$top_10_domains)
    Length  Class   Mode 
         0   NULL   NULL

    > top_domains_filter <- log %>% 
         filter(tolower(query) %in% top_10_domain$domain) %>%
         arrange(ts)
    > time <- diff(top_domains_filter$ts)
    > summary(time)
        Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
        0.00     0.00     0.13     0.85     0.54 49677.59

6.Часто вредоносное программное обеспечение использует DNS канал в
качестве канала управления, периодически отправляя запросы на
подконтрольный злоумышленникам DNS сервер. По периодическим запросам на
один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP
адреса в исследуемом датасете?

    > ip_domains <- log %>%
         group_by(ip = tolower(id.orig_h), domain = tolower(query)) %>%
         summarise(request = n(), .groups = 'drop') %>%
         filter(request > 1)
    > unique_ips_periodic_requests <- unique(ip_domains$ip)
    > num_unique_ips <- length(unique_ips_periodic_requests)
    > head(unique_ips_periodic_requests)
    [1] "10.10.10.10"     "10.10.117.209"   "10.10.117.210"   "128.244.37.196"  "169.254.109.123" "169.254.228.26" 

## Вывод

Были изучены возможности библиотек tidyverse, readr.
