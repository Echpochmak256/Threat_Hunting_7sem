# Lab6

Лабораторная работа №6

## Цель Работы

1.  Закрепить навыки исследования данных журнала Windows Active
    Directory
2.  Изучить структуру журнала системы Windows Active Directory
3.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
4.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R

## Ход работы

### Установка пакетов

    library(dplyr)
    library(jsonlite)
    library(tidyr)
    library(xml2)
    library(rvest)

### Импорт и подготовка данных DNS

1.Импортируйте данные в R

    > url <- "https://storage.yandexcloud.net/iamcth-data/dataset.tar.gz"
    > download.file(url, destfile = tf <- tempfile(fileext = ".tar.gz"), mode = "wb")
    пробую URL 'https://storage.yandexcloud.net/iamcth-data/dataset.tar.gz'
    Content type 'application/gzip' length 12608123 bytes (12.0 MB)
    downloaded 12.0 MB

    > temp_dir <- tempdir()
    > untar(tf, exdir = temp_dir)
    > json_files <- list.files(temp_dir, pattern="\\.json$", full.names = TRUE, recursive = TRUE)
    > data <- stream_in(file(json_files))
    opening file input connection.
     Imported 101904 records. Simplifying...
    closing file input connection.

2.Привести датасеты в вид “аккуратных данных”, преобразовать типы
столбцов в соответствии с типом данных

    data <- data %>%
      mutate(`@timestamp` = as.POSIXct(`@timestamp`, format = "%Y-%m-%dT%H:%M:%OSZ", tz = "UTC")) %>%
      rename(timestamp = `@timestamp`, metadata = `@metadata`)

3.Просмотрите общую структуру данных с помощью функции glimpse()

    > data %>% glimpse
    Rows: 101,904
    Columns: 9
    $ timestamp <dttm> 2019-10-20 20:11:06, 2019-10-20 20:11:07, 2019-10-20 20:11:09, 2019-10-20 20:11:10, 2019-10-20 20:11:11, 2019-10-20 20:11:15, 2019-10-20 20:11:15, 2019-10-20 2…
    $ metadata  <df[,4]> <data.frame[60 x 4]>
    $ event     <df[,4]> <data.frame[60 x 4]>
    $ log       <df[,1]> <data.frame[60 x 1]>
    $ message   <chr> "A token right was adjusted.\n\nSubject:\n\tSecurity ID:\t\tS-1-5-18\n\tAccount Name:\t\tHR001$\n\tAccount Domain:\t\tshire\n\tLogon ID:\t\t0x3E7\n\nTarget A…
    $ winlog    <df[,16]> <data.frame[60 x 16]>
    $ ecs       <df[,1]> <data.frame[60 x 1]>
    $ host      <df[,1]> <data.frame[60 x 1]>
    $ agent     <df[,5]> <data.frame[60 x 5]>

### Анализ данных

1.Раскройте датафрейм избавившись от вложенных датафреймов. Для
обнаружения таких можно использовать функцию dplyr::glimpse() , а для
раскрытия вложенности – tidyr::unnest() . Обратите внимание, что при
раскрытии теряются внешние названия колонок – это можно предотвратить
если использовать параметр tidyr::unnest(…, names_sep = ).

    > data_unnested <- data %>%
    +     unnest(c(metadata, event, log, winlog, ecs, host, agent), names_sep = ".")
    > data_unnested %>% glimpse
    Rows: 101,904
    Columns: 34
    $ timestamp            <dttm> 2019-10-20 20:11:06, 2019-10-20 20:11:07, 2019-10-20 20:11:09, 2019-10-20 20:11:10, 2019-10-20 20:11:11, 2019-10-20 20:11:15, 2019-10-20 20:11:15, 2…
    $ metadata.beat        <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbe…
    $ metadata.type        <chr> "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc", "_doc…
    $ metadata.version     <chr> "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.…
    $ metadata.topic       <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbe…
    $ event.created        <chr> "2019-10-20T20:11:09.988Z", "2019-10-20T20:11:09.988Z", "2019-10-20T20:11:11.995Z", "2019-10-20T20:11:14.013Z", "2019-10-20T20:11:14.013Z", "2019-10-…
    $ event.kind           <chr> "event", "event", "event", "event", "event", "event", "event", "event", "event", "event", "event", "event", "event", "event", "event", "event", "even…
    $ event.code           <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7, 7, 7, 4689, 10, 5, 4703, 10, 10, 10, 10, 5158, 5156, 5156, 4672, 4624, 4627, 4634, 12, 12, 3, 3, 1…
    $ event.action         <chr> "Token Right Adjusted Events", "Sensitive Privilege Use", "Process accessed (rule: ProcessAccess)", "Process accessed (rule: ProcessAccess)", "Proces…
    $ log.level            <chr> "information", "information", "information", "information", "information", "information", "information", "information", "information", "information",…
    $ message              <chr> "A token right was adjusted.\n\nSubject:\n\tSecurity ID:\t\tS-1-5-18\n\tAccount Name:\t\tHR001$\n\tAccount Domain:\t\tshire\n\tLogon ID:\t\t0x3E7\n\n…
    $ winlog.event_data    <df[,234]> <data.frame[60 x 234]>
    $ winlog.event_id      <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7, 7, 7, 4689, 10, 5, 4703, 10, 10, 10, 10, 5158, 5156, 5156, 4672, 4624, 4627, 4634, 12, 12, 3,…
    $ winlog.provider_name <chr> "Microsoft-Windows-Security-Auditing", "Microsoft-Windows-Security-Auditing", "Microsoft-Windows-Sysmon", "Microsoft-Windows-Sysmon", "Microsoft-Wind…
    $ winlog.api           <chr> "wineventlog", "wineventlog", "wineventlog", "wineventlog", "wineventlog", "wineventlog", "wineventlog", "wineventlog", "wineventlog", "wineventlog",…
    $ winlog.record_id     <int> 50588, 104875, 226649, 153525, 163488, 153526, 134651, 226650, 226651, 226652, 226653, 162367, 162368, 162369, 26042, 226654, 226655, 22614, 134652, …
    $ winlog.computer_name <chr> "HR001.shire.com", "HFDC01.shire.com", "IT001.shire.com", "HR001.shire.com", "ACCT001.shire.com", "HR001.shire.com", "FILE001.shire.com", "IT001.shir…
    $ winlog.process       <df[,2]> <data.frame[60 x 2]>
    $ winlog.keywords      <list> "Audit Success", "Audit Failure", <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "Audit Success", <N…
    $ winlog.provider_guid <chr> "{54849625-5478-4994-a5ba-3e3b0328c30d}", "{54849625-5478-4994-a5ba-3e3b0328c30d}", "{5770385f-c22a-43e0-bf4c-06f5698ffbd9}", "{5770385f-c22a-43e0…
    $ winlog.channel       <chr> "security", "Security", "Microsoft-Windows-Sysmon/Operational", "Microsoft-Windows-Sysmon/Operational", "Microsoft-Windows-Sysmon/Operational", "Mic…
    $ winlog.task          <chr> "Token Right Adjusted Events", "Sensitive Privilege Use", "Process accessed (rule: ProcessAccess)", "Process accessed (rule: ProcessAccess)", "Proces…
    $ winlog.opcode        <chr> "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info…
    $ winlog.version       <int> NA, NA, 3, 3, 3, 3, 2, 3, 3, 3, 3, 3, 3, 3, NA, 3, 3, NA, 3, 3, 3, 3, NA, 1, 1, NA, 2, NA, NA, 2, 2, 5, 5, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, …
    $ winlog.user          <df[,4]> <data.frame[60 x 4]>
    $ winlog.activity_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ winlog.user_data     <df[,30]> <data.frame[60 x 30]>
    $ ecs.version          <chr> "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.…
    $ host.name            <chr> "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WE…
    $ agent.ephemeral_id   <chr> "b372be1f-ba0a-4d7e-b4df-79eac86e1fde", "b372be1f-ba0a-4d7e-b4df-79eac86e1fde", "b372be1f-ba0a-4d7e-b4df-79eac86e1fde", "b372be1f-ba0a-4d7e-b4df-79ea…
    $ agent.hostname       <chr> "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "W…
    $ agent.id             <chr> "d347d9a4-bff4-476c-b5a4-d51119f78250", "d347d9a4-bff4-476c-b5a4-d51119f78250", "d347d9a4-bff4-476c-b5a4-d51119f78250", "d347d9a4-bff4-476c-b5a4-d511…
    $ agent.version        <chr> "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.0", "7.4.…
    $ agent.type           <chr> "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbeat", "winlogbe…

2.Минимизируйте количество колонок в датафрейме – уберите колоки с
единственным значением параметра.

    > data_clear <- data_unnested %>%
    +     select(-metadata.beat, -metadata.type, -metadata.version, -metadata.topic, 
    +            -event.kind, -winlog.api, -agent.ephemeral_id, -agent.hostname, 
    +            -agent.id, -agent.version, -agent.type)
    > data_clear %>% glimpse
    Rows: 101,904
    Columns: 23
    $ timestamp            <dttm> 2019-10-20 20:11:06, 2019-10-20 20:11:07, 2019-10-20 20:11:09, 2019-10-20 20:11:10, 2019-10-20 20:11:11, 2019-10-20 20:11:15, 2019-10-20 20:11:15, 2…
    $ event.created        <chr> "2019-10-20T20:11:09.988Z", "2019-10-20T20:11:09.988Z", "2019-10-20T20:11:11.995Z", "2019-10-20T20:11:14.013Z", "2019-10-20T20:11:14.013Z", "2019-10-…
    $ event.code           <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7, 7, 7, 4689, 10, 5, 4703, 10, 10, 10, 10, 5158, 5156, 5156, 4672, 4624, 4627, 4634, 12, 12, 3, 3, 1…
    $ event.action         <chr> "Token Right Adjusted Events", "Sensitive Privilege Use", "Process accessed (rule: ProcessAccess)", "Process accessed (rule: ProcessAccess)", "Proces…
    $ log.level            <chr> "information", "information", "information", "information", "information", "information", "information", "information", "information", "information",…
    $ message              <chr> "A token right was adjusted.\n\nSubject:\n\tSecurity ID:\t\tS-1-5-18\n\tAccount Name:\t\tHR001$\n\tAccount Domain:\t\tshire\n\tLogon ID:\t\t0x3E7\n\n…
    $ winlog.event_data    <df[,234]> <data.frame[60 x 234]>
    $ winlog.event_id      <int> 4703, 4673, 10, 10, 10, 10, 11, 10, 10, 10, 10, 7, 7, 7, 4689, 10, 5, 4703, 10, 10, 10, 10, 5158, 5156, 5156, 4672, 4624, 4627, 4634, 12, 12, 3,…
    $ winlog.provider_name <chr> "Microsoft-Windows-Security-Auditing", "Microsoft-Windows-Security-Auditing", "Microsoft-Windows-Sysmon", "Microsoft-Windows-Sysmon", "Microsoft-Wind…
    $ winlog.record_id     <int> 50588, 104875, 226649, 153525, 163488, 153526, 134651, 226650, 226651, 226652, 226653, 162367, 162368, 162369, 26042, 226654, 226655, 22614, 134652, …
    $ winlog.computer_name <chr> "HR001.shire.com", "HFDC01.shire.com", "IT001.shire.com", "HR001.shire.com", "ACCT001.shire.com", "HR001.shire.com", "FILE001.shire.com", "IT001.shir…
    $ winlog.process       <df[,2]> <data.frame[60 x 2]>
    $ winlog.keywords      <list> "Audit Success", "Audit Failure", <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, <NULL>, "Audit Success", <N…
    $ winlog.provider_guid <chr> "{54849625-5478-4994-a5ba-3e3b0328c30d}", "{54849625-5478-4994-a5ba-3e3b0328c30d}", "{5770385f-c22a-43e0-bf4c-06f5698ffbd9}", "{5770385f-c22a-43e0…
    $ winlog.channel       <chr> "security", "Security", "Microsoft-Windows-Sysmon/Operational", "Microsoft-Windows-Sysmon/Operational", "Microsoft-Windows-Sysmon/Operational", "Mic…
    $ winlog.task          <chr> "Token Right Adjusted Events", "Sensitive Privilege Use", "Process accessed (rule: ProcessAccess)", "Process accessed (rule: ProcessAccess)", "Proces…
    $ winlog.opcode        <chr> "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info", "Info…
    $ winlog.version       <int> NA, NA, 3, 3, 3, 3, 2, 3, 3, 3, 3, 3, 3, 3, NA, 3, 3, NA, 3, 3, 3, 3, NA, 1, 1, NA, 2, NA, NA, 2, 2, 5, 5, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, …
    $ winlog.user          <df[,4]> <data.frame[60 x 4]>
    $ winlog.activity_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ winlog.user_data     <df[,30]> <data.frame[60 x 30]>
    $ ecs.version          <chr> "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.0", "1.1.…
    $ host.name            <chr> "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WECServer", "WE…

3.Какое количество хостов представлено в данном датасете?

    > data_clear %>%
    +     select(host.name) %>%
    +     unique
    # A tibble: 1 × 1
      host.name
      <chr>    
    1 WECServer

4.Подготовьте датафрейм с расшифровкой Windows Event_ID, приведите типы
данных к типу их значений.

    > webpage_url <- "https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor"
    > webpage <- xml2::read_html(webpage_url)
    > event_df <- rvest::html_table(webpage)[[1]]
    > event_df %>% glimpse
    Rows: 381
    Columns: 4
    $ `Current Windows Event ID` <chr> "4618", "4649", "4719", "4765", "4766", "4794", "4897", "4964", "5124", "N/A", "1102", "4621", "4675", "4692", "4693", "4706", "4713", "4714", …
    $ `Legacy Windows Event ID`  <chr> "N/A", "N/A", "612", "N/A", "N/A", "N/A", "801", "N/A", "N/A", "550", "517", "N/A", "N/A", "N/A", "N/A", "610", "617", "618", "N/A", "620", "62…
    $ `Potential Criticality`    <chr> "High", "High", "High", "High", "High", "High", "High", "High", "High", "Medium to High", "Medium to High", "Medium", "Medium", "Medium", "Medi…
    $ `Event Summary`            <chr> "A monitored security event pattern has occurred.", "A replay attack was detected. May be a harmless false positive due to misconfiguration err…

Подготовим данные:

    event_df <- event_df %>%
      mutate_at(vars(`Current Windows Event ID`, `Legacy Windows Event ID`), as.integer) %>%
      rename(c(Current_Windows_Event_ID = `Current Windows Event ID`, 
               Legacy_Windows_Event_ID = `Legacy Windows Event ID`, 
               Potential_Criticality = `Potential Criticality`, 
               Event_Summary = `Event Summary`))

    event_df %>% glimpse

5.Есть ли в логе события с высоким и средним уровнем значимости? Сколько
их?

    > event_df %>% 
    +     group_by(Potential_Criticality) %>%
    +     summarize(count = n()) %>%
    +     arrange(desc(count))
    # A tibble: 4 × 2
      Potential_Criticality count
      <chr>                 <int>
    1 Low                     291
    2 Medium                   79
    3 High                      9
    4 Medium to High            2

Количество событий со средним уровнем значимости: 79 Количество событий
с высоким уровнем значимости: 9

### Оценка результата

В результате лабораторной работы были выполнены задания по анализу
данных трафика Wi-Fi сетей

### Вывод

В ходе лабораторной работы были импортированы, подготовлены,
проанализированы данные трафика Wi-Fi сетей