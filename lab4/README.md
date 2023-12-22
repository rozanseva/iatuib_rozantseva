l# Лабораторная работа №4
Розанцева Елизавета Бисо-03-20

## Цель работы

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  ОС Windows 10
2.  Интерпретатор R v-4.3.1
3.  RStudio Desktop
4.  dplyr
5.  tidyverse

## Общая ситуация

Вы исследуете подозрительную сетевую активность во внутренней сети
Доброй Организации. Вам в руки попали метаданные о DNS трафике в
исследуемой сети. Исследуйте файлы, восстановите данные, подготовьте их
к анализу и дайте обоснованные ответы на поставленные вопросы
исследования

##Задания

### Импорт и подготовка данных DNS

#### 1. Импортируйте данные DNS

``` r
library(dplyr)
```

    Warning: пакет 'dplyr' был собран под R версии 4.3.2


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

``` r
DNS <- read.table("dns.log", header = FALSE, sep = "\t", quote = "", encoding="UTF-8")
```

#### 2. Добавьте пропущенные данные о структуре данных (назначении столбцов)

В документации zeek(https://docs.zeek.org/en/master/logs/dns.html) есть
формат header’a:

    Field ,Type 
    ts,time,Timestamp of the DNS request
    uid,string,Unique id of the connection 
    id.orig_h,addr,The source IP address of the host that initiated the DNS query. 
    id.orig_p,port,The source port of the host that initiated the DNS query.
    id.resp_h,addr,The destination IP address, typically the DNS server, that received the DNS query.
    id_resp_p,port,The destination port number, usually port 53 for DNS, on the server that received the DNS query.
    proto,enum,Protocol of DNS transaction – TCP or UDP 
    trans_id,count,16 bit identifier assigned by DNS client; responses match 
    query,string,Domain name subject of the query 
    qclass,count,Value specifying the query class 
    qclass_name,string,Descriptive name of the query class (e.g. C_INTERNET) 
    qtype,count,Value specifying the query type 
    qtype_name,string,"Name of the query type (e.g. A, AAAA, PTR) "
    rcode,count,Response code value in the DNS response 
    rcode_name,string,"Descriptive name of the response code (e.g. NOERROR, NXDOMAIN) "
    AA,bool,Authoritative Answer. T = server is authoritative for query 
    TC,bool,Truncation. T = message was truncated Recursion Desired. 
    RD,bool,T = request recursive lookup of query
    RA,bool,Recursion Available. T = server supports recursive queries 
    Z,count,"Reserved field, should be zero in all queries & responses "
    answers,vector,List of resource descriptions in answer to the query 
    TTLs,vector,Caching intervals of the answers 
    rejected,bool,Whether the DNS query was rejected by the server 

``` r
colnames(DNS) <- read.csv("header.csv", header = FALSE, skip = 1)$V1
```

#### 3. Просмотрите общую структуру данных с помощью функции glimpse()

``` r
DNS %>%
  glimpse()
```

    Rows: 427,935
    Columns: 23
    $ ts          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901006…
    $ uid         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz7Bs…
    $ id.orig_h   <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", "19…
    $ id.orig_p   <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 1…
    $ id.resp_h   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255", "1…
    $ id_resp_p   <int> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ proto       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "u…
    $ trans_id    <int> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187, 62…
    $ query       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…
    $ qclass      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    $ qclass_name <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C…
    $ qtype       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "32"…
    $ qtype_name  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB…
    $ rcode       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rcode_name  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-…
    $ AA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ TC          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ RD          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRU…
    $ RA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ Z           <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0…
    $ answers     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ TTLs        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rejected    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…

### Анализ данных

#### 4. Сколько участников информационного обмена в сети Доброй Организации?

``` r
hosts <- c(unique(DNS$id.orig_h), unique(DNS$id.resp_h))
hosts %>% unique() %>% length()
```

    [1] 1359

#### 5. Какое соотношение участников обмена внутри сети и участников обращений к внешним ресурсам?

``` r
ip_pattern_inside <- c("192.168.", "10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).")
ips_inside <- hosts[grep(paste(ip_pattern_inside, collapse = "|"), hosts)]
ips_inside_num <- sum(hosts %in% ips_inside)
ips_outside_num <- length(hosts) - ips_inside_num

ratio <- ips_inside_num / ips_outside_num
ratio
```

    [1] 16.24419

#### 6. Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность.

``` r
most_active <- DNS %>%
  group_by(id.orig_h) %>%
  summarise(request_num = n()) %>%
  arrange(desc(request_num)) %>%
  top_n(10, request_num)

most_active
```

    # A tibble: 10 × 2
       id.orig_h       request_num
       <chr>                 <int>
     1 10.10.117.210         75943
     2 192.168.202.93        26522
     3 192.168.202.103       18121
     4 192.168.202.76        16978
     5 192.168.202.97        16176
     6 192.168.202.141       14967
     7 10.10.117.209         14222
     8 192.168.202.110       13372
     9 192.168.203.63        12148
    10 192.168.202.106       10784

#### 7. Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений

``` r
most_requested <- DNS %>%
  group_by(query) %>%
  summarise(request_num = n()) %>%
  arrange(desc(request_num)) %>%
  top_n(10, request_num)

most_requested
```

    # A tibble: 10 × 2
       query                                                             request_num
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\…       10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

#### 8. Опеределите базовые статистические характеристики (функция summary()) интервала времени между последовательным обращениями к топ-10 доменам.

``` r
most_requested_filtered <- DNS %>% 
  filter(tolower(query) %in% most_requested$query) %>%
  arrange(ts)
intervals <- diff(most_requested_filtered$ts)

summary(intervals)
```

        Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
        0.00     0.00     0.10     1.07     0.54 49677.59 

#### 9. Часто вредоносное программное обеспечение использует DNS канал в качестве канала управления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

``` r
ip_domain_num <- DNS %>%
  group_by(ip = id.orig_h, query) %>%
  summarise(request_num = n()) %>%
  filter(request_num > 1)
```

    `summarise()` has grouped output by 'ip'. You can override using the `.groups`
    argument.

``` r
unique_ips_with_periodic_requests <- unique(ip_domain_num$ip)

unique_ips_with_periodic_requests %>% length()
```

    [1] 240

### Обогащение данных

#### 10. Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы, например https://v4.ifconfig.co/.

``` r
most_requested
```

    # A tibble: 10 × 2
       query                                                             request_num
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\…       10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

1.  teredo.ipv6.microsoft.com

-   IP: 20.112.250.133
-   Country: United States
-   Timezone: America/Chicago
-   Organization: Microsoft

1.  tools.google.com

-   IP: 173.194.222.100
-   Country: United States
-   Timezone: America/Chicago
-   Organization: Google

1.  www.apple.com

-   IP: 17.253.144.10
-   Country: United States
-   Timezone: America/Chicago
-   Organization: Apple-Engineering

1.  safebrowsing.clients.google.com

-   IP: 64.233.164.100
-   Country: United States
-   Timezone: America/Chicago
-   Organization: Google

1.  44.206.168.192.in-addr.arpa

-   IP: 44.206.168.192
-   Country: United States
-   City: Ashburn
-   Timezone: America/New_York
-   Organization: Amazon

## Оценка результата

В результате лабораторной работы были выполнены задания с использованием
пакета dplyr

## Вывод

В ходе лабораторной работы были импортированы, подготовлены,
проанализированы и обогащены данные трафика DNS.
