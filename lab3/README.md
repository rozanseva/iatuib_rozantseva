# lab3
Rozanseva Elizaveta

Лабораторная работа №3

## Цель

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  ОС Windows 11
2.  RStudio Desktop
3.  Интерпретатор языка R 4.2.2
4.  dplyr
5.  nycflights13

## План

1.  Установить пакет ‘dplyr’
2.  Установить пакет ‘nycflights13’
3.  Выполнить задания

## Шаги

### Установка dplyr

Пакет dplyr можно установить в RStudio Desktop с помощью команды
install.packages(“dplyr”). Далее подключим пакет к текущему проекте с
помощью library(dplyr)

``` r
library(dplyr)
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

### Установка nycflights13

Пакет dplyr можно установить в RStudio Desktop с помощью команды
install.packages(“nycflights13”). Далее подключим пакет к текущему
проекте с помощью library(nycflights13)

``` r
library(nycflights13)
```

### Выполнение заданий

#### 1. Сколько встроенных в пакет nycflights13 датафреймов?

В пакет nycflights13 встроено 5 датафреймов: airlines, airports,
flights, planes, weather

#### 2. Сколько строк в каждом датафрейме?

``` r
airlines %>% nrow()
```

    [1] 16

``` r
airports %>% nrow()
```

    [1] 1458

``` r
flights %>% nrow()
```

    [1] 336776

``` r
planes %>% nrow()
```

    [1] 3322

``` r
weather %>% nrow()
```

    [1] 26115

#### 3. Сколько столбцов в каждом датафрейме?

``` r
airlines %>% ncol()
```

    [1] 2

``` r
airports %>% ncol()
```

    [1] 8

``` r
flights %>% ncol()
```

    [1] 19

``` r
planes %>% ncol()
```

    [1] 9

``` r
weather %>% ncol()
```

    [1] 15

#### 4. Как посмотреть примерный вид датафрейма?

``` r
airports %>% glimpse()
```

    Rows: 1,458
    Columns: 8
    $ faa   <chr> "04G", "06A", "06C", "06N", "09J", "0A9", "0G6", "0G7", "0P2", "…
    $ name  <chr> "Lansdowne Airport", "Moton Field Municipal Airport", "Schaumbur…
    $ lat   <dbl> 41.13047, 32.46057, 41.98934, 41.43191, 31.07447, 36.37122, 41.4…
    $ lon   <dbl> -80.61958, -85.68003, -88.10124, -74.39156, -81.42778, -82.17342…
    $ alt   <dbl> 1044, 264, 801, 523, 11, 1593, 730, 492, 1000, 108, 409, 875, 10…
    $ tz    <dbl> -5, -6, -6, -5, -5, -5, -5, -5, -5, -8, -5, -6, -5, -5, -5, -5, …
    $ dst   <chr> "A", "A", "A", "A", "A", "A", "A", "A", "U", "A", "A", "U", "A",…
    $ tzone <chr> "America/New_York", "America/Chicago", "America/Chicago", "Ameri…

#### 5. Сколько компаний-перевозчиков(carrier) представлено в этих наборах данных?

``` r
unique(airlines) %>% nrow()
```

    [1] 16

#### 6. Сколько рейсов принял аэропорт John F Kennedy intl в мае?

``` r
flights %>% filter(dest == "JFK" & month == 5) %>% nrow()
```

    [1] 0

#### 7. Какой самый северный аэропорт?

``` r
airports %>% filter(lat == max(lat))
```

    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     <NA> 

#### 8. Какой аэропорт самый высокогорный(находится выше всех над уровнем моря)?

``` r
airports %>% filter(alt == max(alt))
```

    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

#### 9. Какие бортовые номера у самых старых самолетов?

``` r
planes %>% 
  arrange(year) %>% 
  head(10) %>%
  select(tailnum)
```

    # A tibble: 10 × 1
       tailnum
       <chr>  
     1 N381AA 
     2 N201AA 
     3 N567AA 
     4 N378AA 
     5 N575AA 
     6 N14629 
     7 N615AA 
     8 N425AA 
     9 N383AA 
    10 N364AA 

#### 10. Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy intl(в грудусах Цельсия)?

``` r
tempMean <- weather %>% 
  filter(origin == "JFK" & month == 9) %>%
  summarize(tempMean = mean(temp))
(tempMean - 32) * 5 / 9 
```

      tempMean
    1 19.38764

#### 11. Самолеты какой авиакомпании совершили больше всего вылетов в июне?

``` r
flights %>% 
  filter(month == 6) %>%
  group_by(carrier) %>%
  summarize(amount = n()) %>%
  arrange(desc(amount)) %>%
  head(1)
```

    # A tibble: 1 × 2
      carrier amount
      <chr>    <int>
    1 UA        4975

#### 12. Самолеты какой авиакомпании задерживались чаще других в 2013 году?

``` r
flights %>%
  filter(arr_delay > 0 & year == 2013) %>%
  group_by(carrier) %>%
  summarize(delays = n()) %>%
  arrange(desc(delays)) %>%
  head(1)
```

    # A tibble: 1 × 2
      carrier delays
      <chr>    <int>
    1 EV       24484

## Оценка результата

В результате лабораторной работы были выполнены задания с использованием
пакета dplyr на датафреймах из пакета nycflights13

## Вывод

Мы получили базовые навыки работы с пакетом dplyr для языка R с новыми
наборами данных
