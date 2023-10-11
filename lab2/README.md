Информационно-аналитические технологии поиска угроз информационной
безопасности
# lab2
Rozanseva Elizaveta

Лабораторная работа №2

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

## План

1.  Установить пакет ‘dplyr’
2.  Выполнить задания на наборе данных ‘starwars’

## Шаги

### Установка dplyr

``` r
library(dplyr)
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

### Выполнение заданий на наборе данных ‘starwars’

#### 1. Сколько строк в датафрейме?

``` r
starwars |> nrow()
```

    [1] 87

#### 2. Сколько столбцов в датафрейме?

``` r
starwars |> ncol()
```

    [1] 14

#### 3. Как посмотреть примерный вид датафрейма?

``` r
starwars |> glimpse()
```

    Rows: 87
    Columns: 14
    $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Or…
    $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 2…
    $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.…
    $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", N…
    $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "…
    $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue",…
    $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, …
    $ sex        <chr> "male", "none", "none", "male", "female", "male", "female",…
    $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "femini…
    $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "T…
    $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Huma…
    $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return…
    $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp…
    $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",…

#### 4. Сколько уникальных рас персонажей(species) представленно в данных?

``` r
unique(starwars %>% select(species))
```

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

#### 5. Найти самого высокого персонажа

``` r
starwars %>% slice(which.max(height))
```

    # A tibble: 1 × 14
      name      height  mass hair_color skin_color eye_color birth_year sex   gender
      <chr>      <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
    1 Yarael P…    264    NA none       white      yellow            NA male  mascu…
    # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>

#### 6. Найти всех персонажей ниже 170

``` r
filter(starwars, height < 170)
```

    # A tibble: 23 × 14
       name     height  mass hair_color skin_color eye_color birth_year sex   gender
       <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
     1 C-3PO       167    75 <NA>       gold       yellow           112 none  mascu…
     2 R2-D2        96    32 <NA>       white, bl… red               33 none  mascu…
     3 Leia Or…    150    49 brown      light      brown             19 fema… femin…
     4 Beru Wh…    165    75 brown      light      blue              47 fema… femin…
     5 R5-D4        97    32 <NA>       white, red red               NA none  mascu…
     6 Yoda         66    17 white      green      brown            896 male  mascu…
     7 Mon Mot…    150    NA auburn     fair       blue              48 fema… femin…
     8 Wicket …     88    20 brown      brown      brown              8 male  mascu…
     9 Nien Nu…    160    68 none       grey       black             NA male  mascu…
    10 Watto       137    NA black      blue, grey yellow            NA male  mascu…
    # ℹ 13 more rows
    # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>

#### 7. Подсчитать ИМТ для всех персонажей. ИМТ подсчитать по формуле I = m/h^2, где m - масса, а h - рост

``` r
starwars <- transform(starwars, imt = mass/height/height)

starwars
```

                        name height   mass    hair_color          skin_color
    1         Luke Skywalker    172   77.0         blond                fair
    2                  C-3PO    167   75.0          <NA>                gold
    3                  R2-D2     96   32.0          <NA>         white, blue
    4            Darth Vader    202  136.0          none               white
    5            Leia Organa    150   49.0         brown               light
    6              Owen Lars    178  120.0   brown, grey               light
    7     Beru Whitesun lars    165   75.0         brown               light
    8                  R5-D4     97   32.0          <NA>          white, red
    9      Biggs Darklighter    183   84.0         black               light
    10        Obi-Wan Kenobi    182   77.0 auburn, white                fair
    11      Anakin Skywalker    188   84.0         blond                fair
    12        Wilhuff Tarkin    180     NA  auburn, grey                fair
    13             Chewbacca    228  112.0         brown             unknown
    14              Han Solo    180   80.0         brown                fair
    15                Greedo    173   74.0          <NA>               green
    16 Jabba Desilijic Tiure    175 1358.0          <NA>    green-tan, brown
    17        Wedge Antilles    170   77.0         brown                fair
    18      Jek Tono Porkins    180  110.0         brown                fair
    19                  Yoda     66   17.0         white               green
    20             Palpatine    170   75.0          grey                pale
    21             Boba Fett    183   78.2         black                fair
    22                 IG-88    200  140.0          none               metal
    23                 Bossk    190  113.0          none               green
    24      Lando Calrissian    177   79.0         black                dark
    25                 Lobot    175   79.0          none               light
    26                Ackbar    180   83.0          none        brown mottle
    27            Mon Mothma    150     NA        auburn                fair
    28          Arvel Crynyd     NA     NA         brown                fair
    29 Wicket Systri Warrick     88   20.0         brown               brown
    30             Nien Nunb    160   68.0          none                grey
    31          Qui-Gon Jinn    193   89.0         brown                fair
    32           Nute Gunray    191   90.0          none       mottled green
    33         Finis Valorum    170     NA         blond                fair
    34         Jar Jar Binks    196   66.0          none              orange
    35          Roos Tarpals    224   82.0          none                grey
    36            Rugor Nass    206     NA          none               green
    37              Ric Olié    183     NA         brown                fair
    38                 Watto    137     NA         black          blue, grey
    39               Sebulba    112   40.0          none           grey, red
    40         Quarsh Panaka    183     NA         black                dark
    41        Shmi Skywalker    163     NA         black                fair
    42            Darth Maul    175   80.0          none                 red
    43           Bib Fortuna    180     NA          none                pale
    44           Ayla Secura    178   55.0          none                blue
    45              Dud Bolt     94   45.0          none          blue, grey
    46               Gasgano    122     NA          none         white, blue
    47        Ben Quadinaros    163   65.0          none grey, green, yellow
    48            Mace Windu    188   84.0          none                dark
    49          Ki-Adi-Mundi    198   82.0         white                pale
    50             Kit Fisto    196   87.0          none               green
    51             Eeth Koth    171     NA         black               brown
    52            Adi Gallia    184   50.0          none                dark
    53           Saesee Tiin    188     NA          none                pale
    54           Yarael Poof    264     NA          none               white
    55              Plo Koon    188   80.0          none              orange
    56            Mas Amedda    196     NA          none                blue
    57          Gregar Typho    185   85.0         black                dark
    58                 Cordé    157     NA         brown               light
    59           Cliegg Lars    183     NA         brown                fair
    60     Poggle the Lesser    183   80.0          none               green
    61       Luminara Unduli    170   56.2         black              yellow
    62         Barriss Offee    166   50.0         black              yellow
    63                 Dormé    165     NA         brown               light
    64                 Dooku    193   80.0         white                fair
    65   Bail Prestor Organa    191     NA         black                 tan
    66            Jango Fett    183   79.0         black                 tan
    67            Zam Wesell    168   55.0        blonde fair, green, yellow
    68       Dexter Jettster    198  102.0          none               brown
    69               Lama Su    229   88.0          none                grey
    70               Taun We    213     NA          none                grey
    71            Jocasta Nu    167     NA         white                fair
    72         Ratts Tyerell     79   15.0          none          grey, blue
    73                R4-P17     96     NA          none         silver, red
    74            Wat Tambor    193   48.0          none         green, grey
    75              San Hill    191     NA          none                grey
    76              Shaak Ti    178   57.0          none    red, blue, white
    77              Grievous    216  159.0          none        brown, white
    78               Tarfful    234  136.0         brown               brown
    79       Raymus Antilles    188   79.0         brown               light
    80             Sly Moore    178   48.0          none                pale
    81            Tion Medon    206   80.0          none                grey
    82                  Finn     NA     NA         black                dark
    83                   Rey     NA     NA         brown               light
    84           Poe Dameron     NA     NA         brown               light
    85                   BB8     NA     NA          none                none
    86        Captain Phasma     NA     NA       unknown             unknown
    87         Padmé Amidala    165   45.0         brown               light
           eye_color birth_year            sex    gender      homeworld
    1           blue       19.0           male masculine       Tatooine
    2         yellow      112.0           none masculine       Tatooine
    3            red       33.0           none masculine          Naboo
    4         yellow       41.9           male masculine       Tatooine
    5          brown       19.0         female  feminine       Alderaan
    6           blue       52.0           male masculine       Tatooine
    7           blue       47.0         female  feminine       Tatooine
    8            red         NA           none masculine       Tatooine
    9          brown       24.0           male masculine       Tatooine
    10     blue-gray       57.0           male masculine        Stewjon
    11          blue       41.9           male masculine       Tatooine
    12          blue       64.0           male masculine         Eriadu
    13          blue      200.0           male masculine       Kashyyyk
    14         brown       29.0           male masculine       Corellia
    15         black       44.0           male masculine          Rodia
    16        orange      600.0 hermaphroditic masculine      Nal Hutta
    17         hazel       21.0           male masculine       Corellia
    18          blue         NA           male masculine     Bestine IV
    19         brown      896.0           male masculine           <NA>
    20        yellow       82.0           male masculine          Naboo
    21         brown       31.5           male masculine         Kamino
    22           red       15.0           none masculine           <NA>
    23           red       53.0           male masculine      Trandosha
    24         brown       31.0           male masculine        Socorro
    25          blue       37.0           male masculine         Bespin
    26        orange       41.0           male masculine       Mon Cala
    27          blue       48.0         female  feminine      Chandrila
    28         brown         NA           male masculine           <NA>
    29         brown        8.0           male masculine          Endor
    30         black         NA           male masculine        Sullust
    31          blue       92.0           male masculine           <NA>
    32           red         NA           male masculine Cato Neimoidia
    33          blue       91.0           male masculine      Coruscant
    34        orange       52.0           male masculine          Naboo
    35        orange         NA           male masculine          Naboo
    36        orange         NA           male masculine          Naboo
    37          blue         NA           <NA>      <NA>          Naboo
    38        yellow         NA           male masculine       Toydaria
    39        orange         NA           male masculine      Malastare
    40         brown       62.0           <NA>      <NA>          Naboo
    41         brown       72.0         female  feminine       Tatooine
    42        yellow       54.0           male masculine       Dathomir
    43          pink         NA           male masculine         Ryloth
    44         hazel       48.0         female  feminine         Ryloth
    45        yellow         NA           male masculine        Vulpter
    46         black         NA           male masculine        Troiken
    47        orange         NA           male masculine           Tund
    48         brown       72.0           male masculine     Haruun Kal
    49        yellow       92.0           male masculine          Cerea
    50         black         NA           male masculine    Glee Anselm
    51         brown         NA           male masculine       Iridonia
    52          blue         NA         female  feminine      Coruscant
    53        orange         NA           male masculine        Iktotch
    54        yellow         NA           male masculine        Quermia
    55         black       22.0           male masculine          Dorin
    56          blue         NA           male masculine       Champala
    57         brown         NA           male masculine          Naboo
    58         brown         NA         female  feminine          Naboo
    59          blue       82.0           male masculine       Tatooine
    60        yellow         NA           male masculine       Geonosis
    61          blue       58.0         female  feminine         Mirial
    62          blue       40.0         female  feminine         Mirial
    63         brown         NA         female  feminine          Naboo
    64         brown      102.0           male masculine        Serenno
    65         brown       67.0           male masculine       Alderaan
    66         brown       66.0           male masculine   Concord Dawn
    67        yellow         NA         female  feminine          Zolan
    68        yellow         NA           male masculine           Ojom
    69         black         NA           male masculine         Kamino
    70         black         NA         female  feminine         Kamino
    71          blue         NA         female  feminine      Coruscant
    72       unknown         NA           male masculine    Aleen Minor
    73     red, blue         NA           none  feminine           <NA>
    74       unknown         NA           male masculine          Skako
    75          gold         NA           male masculine     Muunilinst
    76         black         NA         female  feminine          Shili
    77 green, yellow         NA           male masculine          Kalee
    78          blue         NA           male masculine       Kashyyyk
    79         brown         NA           male masculine       Alderaan
    80         white         NA           <NA>      <NA>         Umbara
    81         black         NA           male masculine         Utapau
    82          dark         NA           male masculine           <NA>
    83         hazel         NA         female  feminine           <NA>
    84         brown         NA           male masculine           <NA>
    85         black         NA           none masculine           <NA>
    86       unknown         NA           <NA>      <NA>           <NA>
    87         brown       46.0         female  feminine          Naboo
              species
    1           Human
    2           Droid
    3           Droid
    4           Human
    5           Human
    6           Human
    7           Human
    8           Droid
    9           Human
    10          Human
    11          Human
    12          Human
    13        Wookiee
    14          Human
    15         Rodian
    16           Hutt
    17          Human
    18          Human
    19 Yoda's species
    20          Human
    21          Human
    22          Droid
    23     Trandoshan
    24          Human
    25          Human
    26   Mon Calamari
    27          Human
    28          Human
    29           Ewok
    30      Sullustan
    31          Human
    32      Neimodian
    33          Human
    34         Gungan
    35         Gungan
    36         Gungan
    37           <NA>
    38      Toydarian
    39            Dug
    40           <NA>
    41          Human
    42         Zabrak
    43        Twi'lek
    44        Twi'lek
    45     Vulptereen
    46          Xexto
    47          Toong
    48          Human
    49         Cerean
    50       Nautolan
    51         Zabrak
    52     Tholothian
    53       Iktotchi
    54       Quermian
    55        Kel Dor
    56       Chagrian
    57          Human
    58          Human
    59          Human
    60      Geonosian
    61       Mirialan
    62       Mirialan
    63          Human
    64          Human
    65          Human
    66          Human
    67       Clawdite
    68       Besalisk
    69       Kaminoan
    70       Kaminoan
    71          Human
    72         Aleena
    73          Droid
    74        Skakoan
    75           Muun
    76        Togruta
    77        Kaleesh
    78        Wookiee
    79          Human
    80           <NA>
    81         Pau'an
    82          Human
    83          Human
    84          Human
    85          Droid
    86           <NA>
    87          Human
                                                                                                                                           films
    1                                            The Empire Strikes Back, Revenge of the Sith, Return of the Jedi, A New Hope, The Force Awakens
    2                     The Empire Strikes Back, Attack of the Clones, The Phantom Menace, Revenge of the Sith, Return of the Jedi, A New Hope
    3  The Empire Strikes Back, Attack of the Clones, The Phantom Menace, Revenge of the Sith, Return of the Jedi, A New Hope, The Force Awakens
    4                                                               The Empire Strikes Back, Revenge of the Sith, Return of the Jedi, A New Hope
    5                                            The Empire Strikes Back, Revenge of the Sith, Return of the Jedi, A New Hope, The Force Awakens
    6                                                                                      Attack of the Clones, Revenge of the Sith, A New Hope
    7                                                                                      Attack of the Clones, Revenge of the Sith, A New Hope
    8                                                                                                                                 A New Hope
    9                                                                                                                                 A New Hope
    10                    The Empire Strikes Back, Attack of the Clones, The Phantom Menace, Revenge of the Sith, Return of the Jedi, A New Hope
    11                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
    12                                                                                                           Revenge of the Sith, A New Hope
    13                                           The Empire Strikes Back, Revenge of the Sith, Return of the Jedi, A New Hope, The Force Awakens
    14                                                                The Empire Strikes Back, Return of the Jedi, A New Hope, The Force Awakens
    15                                                                                                                                A New Hope
    16                                                                                        The Phantom Menace, Return of the Jedi, A New Hope
    17                                                                                   The Empire Strikes Back, Return of the Jedi, A New Hope
    18                                                                                                                                A New Hope
    19                                The Empire Strikes Back, Attack of the Clones, The Phantom Menace, Revenge of the Sith, Return of the Jedi
    20                                The Empire Strikes Back, Attack of the Clones, The Phantom Menace, Revenge of the Sith, Return of the Jedi
    21                                                                         The Empire Strikes Back, Attack of the Clones, Return of the Jedi
    22                                                                                                                   The Empire Strikes Back
    23                                                                                                                   The Empire Strikes Back
    24                                                                                               The Empire Strikes Back, Return of the Jedi
    25                                                                                                                   The Empire Strikes Back
    26                                                                                                     Return of the Jedi, The Force Awakens
    27                                                                                                                        Return of the Jedi
    28                                                                                                                        Return of the Jedi
    29                                                                                                                        Return of the Jedi
    30                                                                                                                        Return of the Jedi
    31                                                                                                                        The Phantom Menace
    32                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
    33                                                                                                                        The Phantom Menace
    34                                                                                                  Attack of the Clones, The Phantom Menace
    35                                                                                                                        The Phantom Menace
    36                                                                                                                        The Phantom Menace
    37                                                                                                                        The Phantom Menace
    38                                                                                                  Attack of the Clones, The Phantom Menace
    39                                                                                                                        The Phantom Menace
    40                                                                                                                        The Phantom Menace
    41                                                                                                  Attack of the Clones, The Phantom Menace
    42                                                                                                                        The Phantom Menace
    43                                                                                                                        Return of the Jedi
    44                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
    45                                                                                                                        The Phantom Menace
    46                                                                                                                        The Phantom Menace
    47                                                                                                                        The Phantom Menace
    48                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
    49                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
    50                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
    51                                                                                                   The Phantom Menace, Revenge of the Sith
    52                                                                                                   The Phantom Menace, Revenge of the Sith
    53                                                                                                   The Phantom Menace, Revenge of the Sith
    54                                                                                                                        The Phantom Menace
    55                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
    56                                                                                                  Attack of the Clones, The Phantom Menace
    57                                                                                                                      Attack of the Clones
    58                                                                                                                      Attack of the Clones
    59                                                                                                                      Attack of the Clones
    60                                                                                                 Attack of the Clones, Revenge of the Sith
    61                                                                                                 Attack of the Clones, Revenge of the Sith
    62                                                                                                                      Attack of the Clones
    63                                                                                                                      Attack of the Clones
    64                                                                                                 Attack of the Clones, Revenge of the Sith
    65                                                                                                 Attack of the Clones, Revenge of the Sith
    66                                                                                                                      Attack of the Clones
    67                                                                                                                      Attack of the Clones
    68                                                                                                                      Attack of the Clones
    69                                                                                                                      Attack of the Clones
    70                                                                                                                      Attack of the Clones
    71                                                                                                                      Attack of the Clones
    72                                                                                                                        The Phantom Menace
    73                                                                                                 Attack of the Clones, Revenge of the Sith
    74                                                                                                                      Attack of the Clones
    75                                                                                                                      Attack of the Clones
    76                                                                                                 Attack of the Clones, Revenge of the Sith
    77                                                                                                                       Revenge of the Sith
    78                                                                                                                       Revenge of the Sith
    79                                                                                                           Revenge of the Sith, A New Hope
    80                                                                                                 Attack of the Clones, Revenge of the Sith
    81                                                                                                                       Revenge of the Sith
    82                                                                                                                         The Force Awakens
    83                                                                                                                         The Force Awakens
    84                                                                                                                         The Force Awakens
    85                                                                                                                         The Force Awakens
    86                                                                                                                         The Force Awakens
    87                                                                             Attack of the Clones, The Phantom Menace, Revenge of the Sith
                                   vehicles
    1    Snowspeeder, Imperial Speeder Bike
    2                                      
    3                                      
    4                                      
    5                 Imperial Speeder Bike
    6                                      
    7                                      
    8                                      
    9                                      
    10                      Tribubble bongo
    11 Zephyr-G swoop bike, XJ-6 airspeeder
    12                                     
    13                                AT-ST
    14                                     
    15                                     
    16                                     
    17                          Snowspeeder
    18                                     
    19                                     
    20                                     
    21                                     
    22                                     
    23                                     
    24                                     
    25                                     
    26                                     
    27                                     
    28                                     
    29                                     
    30                                     
    31                      Tribubble bongo
    32                                     
    33                                     
    34                                     
    35                                     
    36                                     
    37                                     
    38                                     
    39                                     
    40                                     
    41                                     
    42                         Sith speeder
    43                                     
    44                                     
    45                                     
    46                                     
    47                                     
    48                                     
    49                                     
    50                                     
    51                                     
    52                                     
    53                                     
    54                                     
    55                                     
    56                                     
    57                                     
    58                                     
    59                                     
    60                                     
    61                                     
    62                                     
    63                                     
    64                     Flitknot speeder
    65                                     
    66                                     
    67           Koro-2 Exodrive airspeeder
    68                                     
    69                                     
    70                                     
    71                                     
    72                                     
    73                                     
    74                                     
    75                                     
    76                                     
    77          Tsmeu-6 personal wheel bike
    78                                     
    79                                     
    80                                     
    81                                     
    82                                     
    83                                     
    84                                     
    85                                     
    86                                     
    87                                     
                                                                                                      starships
    1                                                                                  X-wing, Imperial shuttle
    2                                                                                                          
    3                                                                                                          
    4                                                                                           TIE Advanced x1
    5                                                                                                          
    6                                                                                                          
    7                                                                                                          
    8                                                                                                          
    9                                                                                                    X-wing
    10 Jedi starfighter, Trade Federation cruiser, Naboo star skiff, Jedi Interceptor, Belbullab-22 starfighter
    11                                                Trade Federation cruiser, Jedi Interceptor, Naboo fighter
    12                                                                                                         
    13                                                                      Millennium Falcon, Imperial shuttle
    14                                                                      Millennium Falcon, Imperial shuttle
    15                                                                                                         
    16                                                                                                         
    17                                                                                                   X-wing
    18                                                                                                   X-wing
    19                                                                                                         
    20                                                                                                         
    21                                                                                                  Slave 1
    22                                                                                                         
    23                                                                                                         
    24                                                                                        Millennium Falcon
    25                                                                                                         
    26                                                                                                         
    27                                                                                                         
    28                                                                                                   A-wing
    29                                                                                                         
    30                                                                                        Millennium Falcon
    31                                                                                                         
    32                                                                                                         
    33                                                                                                         
    34                                                                                                         
    35                                                                                                         
    36                                                                                                         
    37                                                                                     Naboo Royal Starship
    38                                                                                                         
    39                                                                                                         
    40                                                                                                         
    41                                                                                                         
    42                                                                                                 Scimitar
    43                                                                                                         
    44                                                                                                         
    45                                                                                                         
    46                                                                                                         
    47                                                                                                         
    48                                                                                                         
    49                                                                                                         
    50                                                                                                         
    51                                                                                                         
    52                                                                                                         
    53                                                                                                         
    54                                                                                                         
    55                                                                                         Jedi starfighter
    56                                                                                                         
    57                                                                                            Naboo fighter
    58                                                                                                         
    59                                                                                                         
    60                                                                                                         
    61                                                                                                         
    62                                                                                                         
    63                                                                                                         
    64                                                                                                         
    65                                                                                                         
    66                                                                                                         
    67                                                                                                         
    68                                                                                                         
    69                                                                                                         
    70                                                                                                         
    71                                                                                                         
    72                                                                                                         
    73                                                                                                         
    74                                                                                                         
    75                                                                                                         
    76                                                                                                         
    77                                                                                 Belbullab-22 starfighter
    78                                                                                                         
    79                                                                                                         
    80                                                                                                         
    81                                                                                                         
    82                                                                                                         
    83                                                                                                         
    84                                                                                      T-70 X-wing fighter
    85                                                                                                         
    86                                                                                                         
    87                                                     H-type Nubian yacht, Naboo star skiff, Naboo fighter
               imt
    1  0.002602758
    2  0.002689232
    3  0.003472222
    4  0.003333007
    5  0.002177778
    6  0.003787401
    7  0.002754821
    8  0.003400999
    9  0.002508286
    10 0.002324598
    11 0.002376641
    12          NA
    13 0.002154509
    14 0.002469136
    15 0.002472518
    16 0.044342857
    17 0.002664360
    18 0.003395062
    19 0.003902663
    20 0.002595156
    21 0.002335095
    22 0.003500000
    23 0.003130194
    24 0.002521625
    25 0.002579592
    26 0.002561728
    27          NA
    28          NA
    29 0.002582645
    30 0.002656250
    31 0.002389326
    32 0.002467038
    33          NA
    34 0.001718034
    35 0.001634247
    36          NA
    37          NA
    38          NA
    39 0.003188776
    40          NA
    41          NA
    42 0.002612245
    43          NA
    44 0.001735892
    45 0.005092802
    46          NA
    47 0.002446460
    48 0.002376641
    49 0.002091623
    50 0.002264681
    51          NA
    52 0.001476843
    53          NA
    54          NA
    55 0.002263468
    56          NA
    57 0.002483565
    58          NA
    59          NA
    60 0.002388844
    61 0.001944637
    62 0.001814487
    63          NA
    64 0.002147709
    65          NA
    66 0.002358984
    67 0.001948696
    68 0.002601775
    69 0.001678076
    70          NA
    71          NA
    72 0.002403461
    73          NA
    74 0.001288625
    75          NA
    76 0.001799015
    77 0.003407922
    78 0.002483746
    79 0.002235174
    80 0.001514960
    81 0.001885192
    82          NA
    83          NA
    84          NA
    85          NA
    86          NA
    87 0.001652893

#### 8. Найти 10 самых вытянутых персонажей. Вытянутость оценить по отношению массы к росту персонажей

``` r
starwars <- transform(starwars, vit = mass / height)
starwars[ order(-starwars$vit), ] %>% top_n(10)
```

    Selecting by vit

                        name height mass  hair_color       skin_color     eye_color
    1  Jabba Desilijic Tiure    175 1358        <NA> green-tan, brown        orange
    2               Grievous    216  159        none     brown, white green, yellow
    3                  IG-88    200  140        none            metal           red
    4              Owen Lars    178  120 brown, grey            light          blue
    5            Darth Vader    202  136        none            white        yellow
    6       Jek Tono Porkins    180  110       brown             fair          blue
    7                  Bossk    190  113        none            green           red
    8                Tarfful    234  136       brown            brown          blue
    9        Dexter Jettster    198  102        none            brown        yellow
    10             Chewbacca    228  112       brown          unknown          blue
       birth_year            sex    gender  homeworld    species
    1       600.0 hermaphroditic masculine  Nal Hutta       Hutt
    2          NA           male masculine      Kalee    Kaleesh
    3        15.0           none masculine       <NA>      Droid
    4        52.0           male masculine   Tatooine      Human
    5        41.9           male masculine   Tatooine      Human
    6          NA           male masculine Bestine IV      Human
    7        53.0           male masculine  Trandosha Trandoshan
    8          NA           male masculine   Kashyyyk    Wookiee
    9          NA           male masculine       Ojom   Besalisk
    10      200.0           male masculine   Kashyyyk    Wookiee
                                                                                                 films
    1                                               The Phantom Menace, Return of the Jedi, A New Hope
    2                                                                              Revenge of the Sith
    3                                                                          The Empire Strikes Back
    4                                            Attack of the Clones, Revenge of the Sith, A New Hope
    5                     The Empire Strikes Back, Revenge of the Sith, Return of the Jedi, A New Hope
    6                                                                                       A New Hope
    7                                                                          The Empire Strikes Back
    8                                                                              Revenge of the Sith
    9                                                                             Attack of the Clones
    10 The Empire Strikes Back, Revenge of the Sith, Return of the Jedi, A New Hope, The Force Awakens
                          vehicles                           starships         imt
    1                                                                  0.044342857
    2  Tsmeu-6 personal wheel bike            Belbullab-22 starfighter 0.003407922
    3                                                                  0.003500000
    4                                                                  0.003787401
    5                                                  TIE Advanced x1 0.003333007
    6                                                           X-wing 0.003395062
    7                                                                  0.003130194
    8                                                                  0.002483746
    9                                                                  0.002601775
    10                       AT-ST Millennium Falcon, Imperial shuttle 0.002154509
             vit
    1  7.7600000
    2  0.7361111
    3  0.7000000
    4  0.6741573
    5  0.6732673
    6  0.6111111
    7  0.5947368
    8  0.5811966
    9  0.5151515
    10 0.4912281

#### 9. Найти средний возраст персонажей каждой рассы вселенной звездных воин

``` r
starwars %>% 
  group_by(species) %>% 
  summarize(meanAge = mean(birth_year, na.rm=TRUE))
```

    # A tibble: 38 × 2
       species   meanAge
       <chr>       <dbl>
     1 Aleena      NaN  
     2 Besalisk    NaN  
     3 Cerean       92  
     4 Chagrian    NaN  
     5 Clawdite    NaN  
     6 Droid        53.3
     7 Dug         NaN  
     8 Ewok          8  
     9 Geonosian   NaN  
    10 Gungan       52  
    # ℹ 28 more rows

#### 10.Найти самый распространенный цвет глаз персонажей вселенной звездный воин

``` r
starwars %>% 
  group_by(eye_color) %>%
  summarize(amount = n()) %>%
  arrange(desc(amount)) %>%
  head(1)
```

    # A tibble: 1 × 2
      eye_color amount
      <chr>      <int>
    1 brown         21

#### 11. Посчитать среднюю длину имени в каждой рассе вселенной Звездных воин

``` r
library(stringr)
starwars %>%
  group_by(species) %>%
  summarize(nameLength = mean(str_length(name)))
```

    # A tibble: 38 × 2
       species   nameLength
       <chr>          <dbl>
     1 Aleena         13   
     2 Besalisk       15   
     3 Cerean         12   
     4 Chagrian       10   
     5 Clawdite       10   
     6 Droid           4.83
     7 Dug             7   
     8 Ewok           21   
     9 Geonosian      17   
    10 Gungan         11.7 
    # ℹ 28 more rows

## Оценка результата

В результате лабораторной работы были выполнены задания с использованием
пакета dplyr

## Вывод

Мы получили базовые навыки работы с пакетом dplyr для языка R
