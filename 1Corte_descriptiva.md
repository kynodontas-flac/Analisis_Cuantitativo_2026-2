Estadística descriptiva
================
Juan Camilo Ortiz-González

Este es un [R Markdown](http://rmarkdown.rstudio.com) Notebook, un
cuaderno al estilo de Jupyter Notebooks en Python. En R, lo que
escribimos como *script* (código), se ejecuta secuencialmente (línea por
línea) y los resultados son arrojados en la consola. No obstante, el
Notebook es una gran herramienta de aprendizaje, pues registra las
salidas de cada “chunk”, la celda de código que se ejecuta para el
resultado que tiene debajo. Esto también tiene la ventaja de ser
reproducible en GitHub a través de declarar “output: github_document” en
el encabezado.

Bienvenidos a R

``` r
# Instalamos los paquetes que necesitaremos

install.packages(c("tidyverse", "lubridate", "ggplot", "tibble", "tidyr", "ggplot2", "dplyr", "stringr"), repos = "https://cloud.r-project.org")
```

``` r
# Este paso no es necesario, pero sobre todo cuando trabajamos en computadores de mucho uso, que tienen versiones de paquetes de R con conflictos es útil

install.packages("devtools", repos = "https://cloud.r-project.org")
devtools::install_github("r-lib/conflicted")
```

``` r
library(devtools)
library(conflicted)
```

``` r
# Resolvemos los conflictos. En este caso 1 paquete tenían el mismo comando

conflicts_prefer(dplyr::filter)
```

    ## [conflicted] Will prefer dplyr::filter over any other package.

``` r
# Revisamos el directorio de trabajo o "working directory" para saber dónde estaremos trabajando
getwd()
```

    ## [1] "C:/Users/juanc/Documentos/Rosario/Cuanti"

``` r
# Instalamos readr para cargar nuestro dataset
install.packages("readr", repos = "https://cloud.r-project.org")
```

``` r
# Cargamos nuestros datos (preferiblemente en .csv)

library(readr)

df <- read_csv("fifa_world_cup_2026_player_performance.csv")
```

    ## Rows: 54600 Columns: 75
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (13): player_id, player_name, nationality, team, position, preferred_fo...
    ## dbl  (61): age, jersey_number, height_cm, weight_kg, market_value_eur, goals...
    ## date  (1): match_date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
# Vemos parcialmente los datos para cerciorarnos de que cargaron bien

head(df)
```

    ## # A tibble: 6 × 75
    ##   player_id player_name   age nationality team  jersey_number position height_cm
    ##   <chr>     <chr>       <dbl> <chr>       <chr>         <dbl> <chr>        <dbl>
    ## 1 P00055    Rodri Fati     26 Spanish     Spain             3 Goalkee…       195
    ## 2 P00070    Ansu Le No…    19 Spanish     Spain            18 Midfiel…       178
    ## 3 P00066    Gavi Ramos     18 Spanish     Spain            14 Midfiel…       177
    ## 4 P00073    Pedro Cuba…    20 Spanish     Spain            21 Forward        182
    ## 5 P00059    Alvaro Oya…    23 Spanish     Spain             7 Defender       191
    ## 6 P00072    Dani Gavi      21 Spanish     Spain            20 Midfiel…       181
    ## # ℹ 67 more variables: weight_kg <dbl>, preferred_foot <chr>, club_name <chr>,
    ## #   market_value_eur <dbl>, match_id <chr>, match_date <date>, stadium <chr>,
    ## #   city <chr>, opponent_team <chr>, tournament_stage <chr>,
    ## #   match_result <chr>, goals_team <dbl>, goals_opponent <dbl>,
    ## #   minutes_played <dbl>, goals <dbl>, assists <dbl>, shots <dbl>,
    ## #   shots_on_target <dbl>, expected_goals_xg <dbl>, expected_assists_xa <dbl>,
    ## #   key_passes <dbl>, successful_passes <dbl>, total_passes <dbl>, …

``` r
# Hay varias formas para ver distintas cualidades de los datos
# Primera forma:

summary(df)
```

    ##      player_id        player_name         age          nationality   
    ##  Length   :54600   Length   :54600   Min.   :17.0   Length   :54600  
    ##  N.unique : 1248   N.unique : 1245   1st Qu.:23.0   N.unique :   48  
    ##  N.blank  :    0   N.blank  :    0   Median :26.0   N.blank  :    0  
    ##  Min.nchar:    6   Min.nchar:    7   Mean   :26.3   Min.nchar:    5  
    ##  Max.nchar:    6   Max.nchar:   25   3rd Qu.:29.0   Max.nchar:   13  
    ##                                      Max.   :39.0                    
    ##         team       jersey_number       position       height_cm    
    ##  Length   :54600   Min.   : 1.0   Length   :54600   Min.   :163.0  
    ##  N.unique :   48   1st Qu.: 7.0   N.unique :    4   1st Qu.:177.0  
    ##  N.blank  :    0   Median :13.5   N.blank  :    0   Median :182.0  
    ##  Min.nchar:    4   Mean   :13.5   Min.nchar:    7   Mean   :181.7  
    ##  Max.nchar:   13   3rd Qu.:20.0   Max.nchar:   10   3rd Qu.:186.0  
    ##                    Max.   :26.0                     Max.   :200.0  
    ##    weight_kg       preferred_foot      club_name     market_value_eur   
    ##  Min.   :65.00   Length   :54600   Length   :54600   Min.   :   528822  
    ##  1st Qu.:73.00   N.unique :    2   N.unique :  122   1st Qu.:  4444778  
    ##  Median :76.00   N.blank  :    0   N.blank  :    0   Median : 10271107  
    ##  Mean   :75.75   Min.nchar:    4   Min.nchar:    3   Mean   : 20084452  
    ##  3rd Qu.:78.00   Max.nchar:    5   Max.nchar:   24   3rd Qu.: 23420128  
    ##  Max.   :87.00                                       Max.   :200000000  
    ##       match_id       match_date              stadium             city      
    ##  Length   :54600   Min.   :2026-06-11   Length   :54600   Length   :54600  
    ##  N.unique : 1050   1st Qu.:2026-06-24   N.unique :   16   N.unique :   16  
    ##  N.blank  :    0   Median :2026-07-07   N.blank  :    0   N.blank  :    0  
    ##  Min.nchar:    6   Mean   :2026-07-06   Min.nchar:    8   Min.nchar:    5  
    ##  Max.nchar:    6   3rd Qu.:2026-07-20   Max.nchar:   23   Max.nchar:   15  
    ##                    Max.   :2026-07-31                                      
    ##    opponent_team    tournament_stage    match_result     goals_team  
    ##  Length   :54600   Length   :54600   Length   :54600   Min.   :0.00  
    ##  N.unique :   48   N.unique :    7   N.unique :    3   1st Qu.:0.00  
    ##  N.blank  :    0   N.blank  :    0   N.blank  :    0   Median :1.00  
    ##  Min.nchar:    4   Min.nchar:    5   Min.nchar:    1   Mean   :1.33  
    ##  Max.nchar:   13   Max.nchar:   17   Max.nchar:    1   3rd Qu.:2.00  
    ##                                                        Max.   :7.00  
    ##  goals_opponent minutes_played     goals            assists       
    ##  Min.   :0.00   Min.   : 0.0   Min.   :0.00000   Min.   :0.00000  
    ##  1st Qu.:0.00   1st Qu.: 0.0   1st Qu.:0.00000   1st Qu.:0.00000  
    ##  Median :1.00   Median :24.0   Median :0.00000   Median :0.00000  
    ##  Mean   :1.33   Mean   :36.2   Mean   :0.05538   Mean   :0.05236  
    ##  3rd Qu.:2.00   3rd Qu.:75.0   3rd Qu.:0.00000   3rd Qu.:0.00000  
    ##  Max.   :7.00   Max.   :90.0   Max.   :4.00000   Max.   :3.00000  
    ##      shots         shots_on_target   expected_goals_xg expected_assists_xa
    ##  Min.   : 0.0000   Min.   :0.00000   Min.   :0.00000   Min.   :0.00000    
    ##  1st Qu.: 0.0000   1st Qu.:0.00000   1st Qu.:0.00000   1st Qu.:0.00000    
    ##  Median : 0.0000   Median :0.00000   Median :0.00000   Median :0.00000    
    ##  Mean   : 0.4471   Mean   :0.04842   Mean   :0.01611   Mean   :0.01774    
    ##  3rd Qu.: 1.0000   3rd Qu.:0.00000   3rd Qu.:0.00000   3rd Qu.:0.00000    
    ##  Max.   :11.0000   Max.   :5.00000   Max.   :2.31000   Max.   :2.20000    
    ##    key_passes     successful_passes  total_passes    pass_accuracy   
    ##  Min.   :0.0000   Min.   : 0.00     Min.   :  0.00   Min.   :0.4200  
    ##  1st Qu.:0.0000   1st Qu.: 0.00     1st Qu.:  0.00   1st Qu.:0.7600  
    ##  Median :0.0000   Median : 9.00     Median : 11.00   Median :0.8200  
    ##  Mean   :0.4718   Mean   :15.47     Mean   : 19.18   Mean   :0.8084  
    ##  3rd Qu.:1.0000   3rd Qu.:27.00     3rd Qu.: 33.00   3rd Qu.:0.8600  
    ##  Max.   :8.0000   Max.   :97.00     Max.   :100.00   Max.   :0.9700  
    ##  dribbles_attempted successful_dribbles    crosses       successful_crosses
    ##  Min.   : 0.0000    Min.   :0.0000      Min.   :0.0000   Min.   :0.00000   
    ##  1st Qu.: 0.0000    1st Qu.:0.0000      1st Qu.:0.0000   1st Qu.:0.00000   
    ##  Median : 0.0000    Median :0.0000      Median :0.0000   Median :0.00000   
    ##  Mean   : 0.6003    Mean   :0.1629      Mean   :0.4478   Mean   :0.01853   
    ##  3rd Qu.: 1.0000    3rd Qu.:0.0000      3rd Qu.:1.0000   3rd Qu.:0.00000   
    ##  Max.   :10.0000    Max.   :6.0000      Max.   :8.0000   Max.   :2.00000   
    ##     tackles       interceptions      clearances          blocks      
    ##  Min.   :0.0000   Min.   :0.0000   Min.   : 0.0000   Min.   :0.0000  
    ##  1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.: 0.0000   1st Qu.:0.0000  
    ##  Median :0.0000   Median :0.0000   Median : 0.0000   Median :0.0000  
    ##  Mean   :0.8027   Mean   :0.6273   Mean   : 0.8087   Mean   :0.2266  
    ##  3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.: 1.0000   3rd Qu.:0.0000  
    ##  Max.   :8.0000   Max.   :7.0000   Max.   :12.0000   Max.   :4.0000  
    ##  aerial_duels_won aerial_duels_lost   recoveries     defensive_actions
    ##  Min.   :0.0000   Min.   :0.000     Min.   : 0.000   Min.   : 0.000   
    ##  1st Qu.:0.0000   1st Qu.:0.000     1st Qu.: 0.000   1st Qu.: 0.000   
    ##  Median :0.0000   Median :0.000     Median : 0.000   Median : 0.000   
    ##  Mean   :0.7453   Mean   :0.363     Mean   : 1.389   Mean   : 2.465   
    ##  3rd Qu.:1.0000   3rd Qu.:1.000     3rd Qu.: 2.000   3rd Qu.: 4.000   
    ##  Max.   :7.0000   Max.   :5.000     Max.   :12.000   Max.   :23.000   
    ##  fouls_committed  fouls_suffered    yellow_cards       red_cards       
    ##  Min.   :0.0000   Min.   :0.0000   Min.   :0.00000   Min.   :0.000000  
    ##  1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.00000   1st Qu.:0.000000  
    ##  Median :0.0000   Median :0.0000   Median :0.00000   Median :0.000000  
    ##  Mean   :0.4169   Mean   :0.3232   Mean   :0.09791   Mean   :0.005604  
    ##  3rd Qu.:1.0000   3rd Qu.:0.0000   3rd Qu.:0.00000   3rd Qu.:0.000000  
    ##  Max.   :5.0000   Max.   :4.0000   Max.   :1.00000   Max.   :1.000000  
    ##     offsides          saves         save_percentage      punches       
    ##  Min.   :0.0000   Min.   : 0.0000   Min.   :0.00000   Min.   :0.00000  
    ##  1st Qu.:0.0000   1st Qu.: 0.0000   1st Qu.:0.00000   1st Qu.:0.00000  
    ##  Median :0.0000   Median : 0.0000   Median :0.00000   Median :0.00000  
    ##  Mean   :0.0869   Mean   : 0.1213   Mean   :0.02627   Mean   :0.02625  
    ##  3rd Qu.:0.0000   3rd Qu.: 0.0000   3rd Qu.:0.00000   3rd Qu.:0.00000  
    ##  Max.   :4.0000   Max.   :11.0000   Max.   :1.00000   Max.   :3.00000  
    ##   clean_sheet       goals_conceded    penalty_saves      distance_covered_km
    ##  Min.   :0.000000   Min.   :0.00000   Min.   :0.000000   Min.   : 0.000     
    ##  1st Qu.:0.000000   1st Qu.:0.00000   1st Qu.:0.000000   1st Qu.: 0.000     
    ##  Median :0.000000   Median :0.00000   Median :0.000000   Median : 3.000     
    ##  Mean   :0.009176   Mean   :0.04864   Mean   :0.003333   Mean   : 3.997     
    ##  3rd Qu.:0.000000   3rd Qu.:0.00000   3rd Qu.:0.000000   3rd Qu.: 7.900     
    ##  Max.   :1.000000   Max.   :7.00000   Max.   :1.000000   Max.   :14.000     
    ##  sprint_distance_km top_speed_kmh   accelerations   decelerations   
    ##  Min.   :0.0000     Min.   : 0.00   Min.   : 0.00   Min.   : 0.000  
    ##  1st Qu.:0.0000     1st Qu.: 0.00   1st Qu.: 0.00   1st Qu.: 0.000  
    ##  Median :0.3000     Median :29.90   Median : 6.00   Median : 6.000  
    ##  Mean   :0.4624     Mean   :18.65   Mean   :10.04   Mean   : 8.849  
    ##  3rd Qu.:0.9000     3rd Qu.:32.70   3rd Qu.:20.00   3rd Qu.:17.000  
    ##  Max.   :2.0000     Max.   :37.00   Max.   :44.00   Max.   :40.000  
    ##  stamina_score   player_rating   performance_score offensive_contribution
    ##  Min.   :50.00   Min.   :0.000   Min.   : 0.00     Min.   : 1.00         
    ##  1st Qu.:74.60   1st Qu.:0.000   1st Qu.: 0.70     1st Qu.:25.68         
    ##  Median :82.60   Median :5.500   Median :54.10     Median :41.60         
    ##  Mean   :81.89   Mean   :3.635   Mean   :36.83     Mean   :44.72         
    ##  3rd Qu.:90.00   3rd Qu.:6.400   3rd Qu.:64.10     3rd Qu.:60.80         
    ##  Max.   :99.00   Max.   :9.400   Max.   :97.70     Max.   :99.00         
    ##  defensive_contribution possession_impact pressure_resistance creativity_score
    ##  Min.   : 5.00          Min.   : 0.00     Min.   :15.00       Min.   : 5.00   
    ##  1st Qu.:33.10          1st Qu.: 0.00     1st Qu.:43.70       1st Qu.:29.80   
    ##  Median :49.90          Median : 1.10     Median :57.00       Median :40.50   
    ##  Mean   :52.73          Mean   : 2.85     Mean   :60.32       Mean   :46.14   
    ##  3rd Qu.:71.30          3rd Qu.: 4.00     3rd Qu.:75.10       3rd Qu.:59.00   
    ##  Max.   :99.00          Max.   :37.00     Max.   :99.00       Max.   :99.00   
    ##  consistency_score clutch_performance_score total_goals_tournament
    ##  Min.   :25.00     Min.   :15.00            Min.   : 0.000        
    ##  1st Qu.:46.50     1st Qu.:46.40            1st Qu.: 0.000        
    ##  Median :61.10     Median :55.00            Median : 0.000        
    ##  Mean   :63.75     Mean   :55.57            Mean   : 0.644        
    ##  3rd Qu.:78.90     3rd Qu.:63.90            3rd Qu.: 1.000        
    ##  Max.   :99.00     Max.   :99.00            Max.   :10.000        
    ##  total_assists_tournament total_minutes_tournament player_of_match_awards
    ##  Min.   :0.0000           Min.   :  0.0            Min.   :0.00000       
    ##  1st Qu.:0.0000           1st Qu.:184.0            1st Qu.:0.00000       
    ##  Median :0.0000           Median :268.0            Median :0.00000       
    ##  Mean   :0.6076           Mean   :272.3            Mean   :0.03066       
    ##  3rd Qu.:1.0000           3rd Qu.:359.0            3rd Qu.:0.00000       
    ##  Max.   :8.0000           Max.   :615.0            Max.   :4.00000       
    ##  tournament_rating
    ##  Min.   :0.000    
    ##  1st Qu.:0.000    
    ##  Median :5.400    
    ##  Mean   :3.634    
    ##  3rd Qu.:6.400    
    ##  Max.   :9.500

``` r
# Segunda forma:

str(df)
```

    ## spc_tbl_ [54,600 × 75] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ player_id               : chr [1:54600] "P00055" "P00070" "P00066" "P00073" ...
    ##  $ player_name             : chr [1:54600] "Rodri Fati" "Ansu Le Normand" "Gavi Ramos" "Pedro Cubarsi" ...
    ##  $ age                     : num [1:54600] 26 19 18 20 23 21 27 27 24 25 ...
    ##  $ nationality             : chr [1:54600] "Spanish" "Spanish" "Spanish" "Spanish" ...
    ##  $ team                    : chr [1:54600] "Spain" "Spain" "Spain" "Spain" ...
    ##  $ jersey_number           : num [1:54600] 3 18 14 21 7 20 6 26 4 22 ...
    ##  $ position                : chr [1:54600] "Goalkeeper" "Midfielder" "Midfielder" "Forward" ...
    ##  $ height_cm               : num [1:54600] 195 178 177 182 191 181 188 182 176 172 ...
    ##  $ weight_kg               : num [1:54600] 75 75 72 74 81 78 80 72 78 70 ...
    ##  $ preferred_foot          : chr [1:54600] "Left" "Right" "Left" "Right" ...
    ##  $ club_name               : chr [1:54600] "RB Salzburg" "Chelsea" "AIK" "PSV Eindhoven" ...
    ##  $ market_value_eur        : num [1:54600] 4.38e+06 4.92e+06 1.25e+08 1.18e+07 1.33e+07 ...
    ##  $ match_id                : chr [1:54600] "M00001" "M00001" "M00001" "M00001" ...
    ##  $ match_date              : Date[1:54600], format: "2026-07-10" "2026-07-10" ...
    ##  $ stadium                 : chr [1:54600] "Hard Rock Stadium" "Hard Rock Stadium" "Hard Rock Stadium" "Hard Rock Stadium" ...
    ##  $ city                    : chr [1:54600] "Miami" "Miami" "Miami" "Miami" ...
    ##  $ opponent_team           : chr [1:54600] "South Africa" "South Africa" "South Africa" "South Africa" ...
    ##  $ tournament_stage        : chr [1:54600] "Group Stage" "Group Stage" "Group Stage" "Group Stage" ...
    ##  $ match_result            : chr [1:54600] "W" "W" "W" "W" ...
    ##  $ goals_team              : num [1:54600] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ goals_opponent          : num [1:54600] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ minutes_played          : num [1:54600] 72 90 73 80 79 90 79 77 71 82 ...
    ##  $ goals                   : num [1:54600] 0 0 1 1 0 0 0 1 0 0 ...
    ##  $ assists                 : num [1:54600] 0 0 0 1 0 0 0 0 0 0 ...
    ##  $ shots                   : num [1:54600] 0 0 2 5 1 1 0 1 0 3 ...
    ##  $ shots_on_target         : num [1:54600] 0 0 0 2 0 0 0 0 0 1 ...
    ##  $ expected_goals_xg       : num [1:54600] 0 0.01 0.08 0 0 0.01 0 0 0 1.19 ...
    ##  $ expected_assists_xa     : num [1:54600] 0 0 0.07 0.21 0 0.46 0.01 0 0 0.11 ...
    ##  $ key_passes              : num [1:54600] 0 1 2 0 1 1 0 1 1 2 ...
    ##  $ successful_passes       : num [1:54600] 15 35 72 12 33 94 44 17 36 39 ...
    ##  $ total_passes            : num [1:54600] 26 40 85 19 44 100 59 20 47 51 ...
    ##  $ pass_accuracy           : num [1:54600] 0.59 0.89 0.85 0.67 0.76 0.94 0.75 0.86 0.77 0.77 ...
    ##  $ dribbles_attempted      : num [1:54600] 0 1 0 1 2 4 1 2 0 4 ...
    ##  $ successful_dribbles     : num [1:54600] 0 0 0 0 1 1 0 0 0 2 ...
    ##  $ crosses                 : num [1:54600] 0 1 3 0 0 6 1 0 1 1 ...
    ##  $ successful_crosses      : num [1:54600] 0 0 0 0 0 1 0 0 0 0 ...
    ##  $ tackles                 : num [1:54600] 0 0 1 1 1 1 3 0 3 0 ...
    ##  $ interceptions           : num [1:54600] 0 1 1 1 3 2 3 0 3 0 ...
    ##  $ clearances              : num [1:54600] 0 3 0 1 0 1 1 0 4 0 ...
    ##  $ blocks                  : num [1:54600] 0 1 0 0 2 0 2 0 0 0 ...
    ##  $ aerial_duels_won        : num [1:54600] 0 2 1 3 1 2 1 0 2 2 ...
    ##  $ aerial_duels_lost       : num [1:54600] 0 0 2 0 2 1 0 1 0 2 ...
    ##  $ recoveries              : num [1:54600] 0 2 4 1 4 8 4 1 3 4 ...
    ##  $ defensive_actions       : num [1:54600] 0 5 2 3 6 4 9 0 10 0 ...
    ##  $ fouls_committed         : num [1:54600] 3 0 0 0 0 2 1 0 0 0 ...
    ##  $ fouls_suffered          : num [1:54600] 0 1 0 1 0 3 1 0 1 0 ...
    ##  $ yellow_cards            : num [1:54600] 0 0 0 0 0 0 0 0 0 1 ...
    ##  $ red_cards               : num [1:54600] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ offsides                : num [1:54600] 0 0 0 0 0 0 0 0 0 1 ...
    ##  $ saves                   : num [1:54600] 4 0 0 0 0 0 0 0 0 0 ...
    ##  $ save_percentage         : num [1:54600] 0.83 0 0 0 0 0 0 0 0 0 ...
    ##  $ punches                 : num [1:54600] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ clean_sheet             : num [1:54600] 1 0 0 0 0 0 0 0 0 0 ...
    ##  $ goals_conceded          : num [1:54600] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ penalty_saves           : num [1:54600] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ distance_covered_km     : num [1:54600] 7.8 10.4 8.8 9.6 7.5 9.7 7.9 8.2 8.6 6.9 ...
    ##  $ sprint_distance_km      : num [1:54600] 0.7 1.1 1.3 1 0.7 1.1 0.7 0.9 1.2 1 ...
    ##  $ top_speed_kmh           : num [1:54600] 26.5 29 33.7 32.1 30.5 28.5 32.4 30.6 32.8 29.5 ...
    ##  $ accelerations           : num [1:54600] 13 19 30 26 23 25 28 17 18 31 ...
    ##  $ decelerations           : num [1:54600] 23 17 19 19 18 27 17 28 10 24 ...
    ##  $ stamina_score           : num [1:54600] 81.9 85.5 88.8 89.2 73.6 96.7 83.7 94.2 96.4 86.7 ...
    ##  $ player_rating           : num [1:54600] 5.6 5.7 8.3 6.9 5.7 6.6 6.7 6.8 6.4 6.3 ...
    ##  $ performance_score       : num [1:54600] 50.9 55.9 82.9 67.5 55.4 72.1 65.3 70.9 63.2 61.3 ...
    ##  $ offensive_contribution  : num [1:54600] 3.3 37.9 79.8 47.3 33 99 43.2 50.4 29.3 79.9 ...
    ##  $ defensive_contribution  : num [1:54600] 48.2 29.4 78.6 6.9 75.6 85 85.6 24.2 70.5 12.8 ...
    ##  $ possession_impact       : num [1:54600] 1.1 3.5 15.3 1.2 6.2 17 6.4 2.4 6.4 11.1 ...
    ##  $ pressure_resistance     : num [1:54600] 44.2 38.2 99 19.8 44.1 65.4 48.7 48.2 52.7 99 ...
    ##  $ creativity_score        : num [1:54600] 55.9 43.7 99 42.3 33.5 85.8 15.3 35.6 31.3 68.8 ...
    ##  $ consistency_score       : num [1:54600] 42 31.1 83.4 40.9 60 91.1 61.8 60.9 68.3 92.1 ...
    ##  $ clutch_performance_score: num [1:54600] 51.8 52.7 54.8 78.5 56.6 48.2 55.1 60.4 59.2 47.6 ...
    ##  $ total_goals_tournament  : num [1:54600] 0 0 1 5 0 1 0 2 0 1 ...
    ##  $ total_assists_tournament: num [1:54600] 0 3 1 3 0 2 1 1 0 3 ...
    ##  $ total_minutes_tournament: num [1:54600] 242 342 245 422 440 332 264 191 324 547 ...
    ##  $ player_of_match_awards  : num [1:54600] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ tournament_rating       : num [1:54600] 5.8 5.5 8.4 6.7 5.7 6.7 7 6.8 6.7 5.7 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   player_id = col_character(),
    ##   ..   player_name = col_character(),
    ##   ..   age = col_double(),
    ##   ..   nationality = col_character(),
    ##   ..   team = col_character(),
    ##   ..   jersey_number = col_double(),
    ##   ..   position = col_character(),
    ##   ..   height_cm = col_double(),
    ##   ..   weight_kg = col_double(),
    ##   ..   preferred_foot = col_character(),
    ##   ..   club_name = col_character(),
    ##   ..   market_value_eur = col_double(),
    ##   ..   match_id = col_character(),
    ##   ..   match_date = col_date(format = ""),
    ##   ..   stadium = col_character(),
    ##   ..   city = col_character(),
    ##   ..   opponent_team = col_character(),
    ##   ..   tournament_stage = col_character(),
    ##   ..   match_result = col_character(),
    ##   ..   goals_team = col_double(),
    ##   ..   goals_opponent = col_double(),
    ##   ..   minutes_played = col_double(),
    ##   ..   goals = col_double(),
    ##   ..   assists = col_double(),
    ##   ..   shots = col_double(),
    ##   ..   shots_on_target = col_double(),
    ##   ..   expected_goals_xg = col_double(),
    ##   ..   expected_assists_xa = col_double(),
    ##   ..   key_passes = col_double(),
    ##   ..   successful_passes = col_double(),
    ##   ..   total_passes = col_double(),
    ##   ..   pass_accuracy = col_double(),
    ##   ..   dribbles_attempted = col_double(),
    ##   ..   successful_dribbles = col_double(),
    ##   ..   crosses = col_double(),
    ##   ..   successful_crosses = col_double(),
    ##   ..   tackles = col_double(),
    ##   ..   interceptions = col_double(),
    ##   ..   clearances = col_double(),
    ##   ..   blocks = col_double(),
    ##   ..   aerial_duels_won = col_double(),
    ##   ..   aerial_duels_lost = col_double(),
    ##   ..   recoveries = col_double(),
    ##   ..   defensive_actions = col_double(),
    ##   ..   fouls_committed = col_double(),
    ##   ..   fouls_suffered = col_double(),
    ##   ..   yellow_cards = col_double(),
    ##   ..   red_cards = col_double(),
    ##   ..   offsides = col_double(),
    ##   ..   saves = col_double(),
    ##   ..   save_percentage = col_double(),
    ##   ..   punches = col_double(),
    ##   ..   clean_sheet = col_double(),
    ##   ..   goals_conceded = col_double(),
    ##   ..   penalty_saves = col_double(),
    ##   ..   distance_covered_km = col_double(),
    ##   ..   sprint_distance_km = col_double(),
    ##   ..   top_speed_kmh = col_double(),
    ##   ..   accelerations = col_double(),
    ##   ..   decelerations = col_double(),
    ##   ..   stamina_score = col_double(),
    ##   ..   player_rating = col_double(),
    ##   ..   performance_score = col_double(),
    ##   ..   offensive_contribution = col_double(),
    ##   ..   defensive_contribution = col_double(),
    ##   ..   possession_impact = col_double(),
    ##   ..   pressure_resistance = col_double(),
    ##   ..   creativity_score = col_double(),
    ##   ..   consistency_score = col_double(),
    ##   ..   clutch_performance_score = col_double(),
    ##   ..   total_goals_tournament = col_double(),
    ##   ..   total_assists_tournament = col_double(),
    ##   ..   total_minutes_tournament = col_double(),
    ##   ..   player_of_match_awards = col_double(),
    ##   ..   tournament_rating = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<pointer: 0x0000024c084e5ed0>

``` r
# Tercera forma:

library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ purrr     1.2.2
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2

``` r
glimpse(df)
```

    ## Rows: 54,600
    ## Columns: 75
    ## $ player_id                <chr> "P00055", "P00070", "P00066", "P00073", "P000…
    ## $ player_name              <chr> "Rodri Fati", "Ansu Le Normand", "Gavi Ramos"…
    ## $ age                      <dbl> 26, 19, 18, 20, 23, 21, 27, 27, 24, 25, 24, 1…
    ## $ nationality              <chr> "Spanish", "Spanish", "Spanish", "Spanish", "…
    ## $ team                     <chr> "Spain", "Spain", "Spain", "Spain", "Spain", …
    ## $ jersey_number            <dbl> 3, 18, 14, 21, 7, 20, 6, 26, 4, 22, 17, 19, 2…
    ## $ position                 <chr> "Goalkeeper", "Midfielder", "Midfielder", "Fo…
    ## $ height_cm                <dbl> 195, 178, 177, 182, 191, 181, 188, 182, 176, …
    ## $ weight_kg                <dbl> 75, 75, 72, 74, 81, 78, 80, 72, 78, 70, 81, 7…
    ## $ preferred_foot           <chr> "Left", "Right", "Left", "Right", "Left", "Ri…
    ## $ club_name                <chr> "RB Salzburg", "Chelsea", "AIK", "PSV Eindhov…
    ## $ market_value_eur         <dbl> 4384884, 4918927, 125015698, 11805512, 133251…
    ## $ match_id                 <chr> "M00001", "M00001", "M00001", "M00001", "M000…
    ## $ match_date               <date> 2026-07-10, 2026-07-10, 2026-07-10, 2026-07-…
    ## $ stadium                  <chr> "Hard Rock Stadium", "Hard Rock Stadium", "Ha…
    ## $ city                     <chr> "Miami", "Miami", "Miami", "Miami", "Miami", …
    ## $ opponent_team            <chr> "South Africa", "South Africa", "South Africa…
    ## $ tournament_stage         <chr> "Group Stage", "Group Stage", "Group Stage", …
    ## $ match_result             <chr> "W", "W", "W", "W", "W", "W", "W", "W", "W", …
    ## $ goals_team               <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    ## $ goals_opponent           <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ minutes_played           <dbl> 72, 90, 73, 80, 79, 90, 79, 77, 71, 82, 85, 3…
    ## $ goals                    <dbl> 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, …
    ## $ assists                  <dbl> 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ shots                    <dbl> 0, 0, 2, 5, 1, 1, 0, 1, 0, 3, 1, 1, 1, 0, 0, …
    ## $ shots_on_target          <dbl> 0, 0, 0, 2, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, …
    ## $ expected_goals_xg        <dbl> 0.00, 0.01, 0.08, 0.00, 0.00, 0.01, 0.00, 0.0…
    ## $ expected_assists_xa      <dbl> 0.00, 0.00, 0.07, 0.21, 0.00, 0.46, 0.01, 0.0…
    ## $ key_passes               <dbl> 0, 1, 2, 0, 1, 1, 0, 1, 1, 2, 1, 1, 1, 2, 0, …
    ## $ successful_passes        <dbl> 15, 35, 72, 12, 33, 94, 44, 17, 36, 39, 46, 1…
    ## $ total_passes             <dbl> 26, 40, 85, 19, 44, 100, 59, 20, 47, 51, 59, …
    ## $ pass_accuracy            <dbl> 0.59, 0.89, 0.85, 0.67, 0.76, 0.94, 0.75, 0.8…
    ## $ dribbles_attempted       <dbl> 0, 1, 0, 1, 2, 4, 1, 2, 0, 4, 3, 0, 1, 1, 0, …
    ## $ successful_dribbles      <dbl> 0, 0, 0, 0, 1, 1, 0, 0, 0, 2, 1, 0, 0, 0, 0, …
    ## $ crosses                  <dbl> 0, 1, 3, 0, 0, 6, 1, 0, 1, 1, 1, 0, 0, 2, 0, …
    ## $ successful_crosses       <dbl> 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ tackles                  <dbl> 0, 0, 1, 1, 1, 1, 3, 0, 3, 0, 1, 1, 0, 1, 0, …
    ## $ interceptions            <dbl> 0, 1, 1, 1, 3, 2, 3, 0, 3, 0, 2, 0, 0, 1, 0, …
    ## $ clearances               <dbl> 0, 3, 0, 1, 0, 1, 1, 0, 4, 0, 1, 0, 0, 0, 0, …
    ## $ blocks                   <dbl> 0, 1, 0, 0, 2, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ aerial_duels_won         <dbl> 0, 2, 1, 3, 1, 2, 1, 0, 2, 2, 1, 1, 1, 2, 0, …
    ## $ aerial_duels_lost        <dbl> 0, 0, 2, 0, 2, 1, 0, 1, 0, 2, 0, 0, 0, 0, 0, …
    ## $ recoveries               <dbl> 0, 2, 4, 1, 4, 8, 4, 1, 3, 4, 1, 1, 0, 2, 0, …
    ## $ defensive_actions        <dbl> 0, 5, 2, 3, 6, 4, 9, 0, 10, 0, 4, 1, 0, 2, 0,…
    ## $ fouls_committed          <dbl> 3, 0, 0, 0, 0, 2, 1, 0, 0, 0, 0, 0, 0, 1, 0, …
    ## $ fouls_suffered           <dbl> 0, 1, 0, 1, 0, 3, 1, 0, 1, 0, 1, 0, 0, 0, 0, …
    ## $ yellow_cards             <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, …
    ## $ red_cards                <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ offsides                 <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, …
    ## $ saves                    <dbl> 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ save_percentage          <dbl> 0.83, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.0…
    ## $ punches                  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ clean_sheet              <dbl> 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ goals_conceded           <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ penalty_saves            <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ distance_covered_km      <dbl> 7.8, 10.4, 8.8, 9.6, 7.5, 9.7, 7.9, 8.2, 8.6,…
    ## $ sprint_distance_km       <dbl> 0.7, 1.1, 1.3, 1.0, 0.7, 1.1, 0.7, 0.9, 1.2, …
    ## $ top_speed_kmh            <dbl> 26.5, 29.0, 33.7, 32.1, 30.5, 28.5, 32.4, 30.…
    ## $ accelerations            <dbl> 13, 19, 30, 26, 23, 25, 28, 17, 18, 31, 15, 5…
    ## $ decelerations            <dbl> 23, 17, 19, 19, 18, 27, 17, 28, 10, 24, 15, 7…
    ## $ stamina_score            <dbl> 81.9, 85.5, 88.8, 89.2, 73.6, 96.7, 83.7, 94.…
    ## $ player_rating            <dbl> 5.6, 5.7, 8.3, 6.9, 5.7, 6.6, 6.7, 6.8, 6.4, …
    ## $ performance_score        <dbl> 50.9, 55.9, 82.9, 67.5, 55.4, 72.1, 65.3, 70.…
    ## $ offensive_contribution   <dbl> 3.3, 37.9, 79.8, 47.3, 33.0, 99.0, 43.2, 50.4…
    ## $ defensive_contribution   <dbl> 48.2, 29.4, 78.6, 6.9, 75.6, 85.0, 85.6, 24.2…
    ## $ possession_impact        <dbl> 1.1, 3.5, 15.3, 1.2, 6.2, 17.0, 6.4, 2.4, 6.4…
    ## $ pressure_resistance      <dbl> 44.2, 38.2, 99.0, 19.8, 44.1, 65.4, 48.7, 48.…
    ## $ creativity_score         <dbl> 55.9, 43.7, 99.0, 42.3, 33.5, 85.8, 15.3, 35.…
    ## $ consistency_score        <dbl> 42.0, 31.1, 83.4, 40.9, 60.0, 91.1, 61.8, 60.…
    ## $ clutch_performance_score <dbl> 51.8, 52.7, 54.8, 78.5, 56.6, 48.2, 55.1, 60.…
    ## $ total_goals_tournament   <dbl> 0, 0, 1, 5, 0, 1, 0, 2, 0, 1, 1, 0, 5, 3, 0, …
    ## $ total_assists_tournament <dbl> 0, 3, 1, 3, 0, 2, 1, 1, 0, 3, 2, 2, 0, 1, 0, …
    ## $ total_minutes_tournament <dbl> 242, 342, 245, 422, 440, 332, 264, 191, 324, …
    ## $ player_of_match_awards   <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ tournament_rating        <dbl> 5.8, 5.5, 8.4, 6.7, 5.7, 6.7, 7.0, 6.8, 6.7, …

``` r
# Esta es otro paquete muy útil:

install.packages("psych", repos = "https://cloud.r-project.org")
```

``` r
library(psych)

describe(df)
```

    ## Warning in FUN(newX[, i], ...): ningún argumento finito para min; retornando
    ## Inf

    ## Warning in FUN(newX[, i], ...): ningun argumento finito para max; retornando
    ## -Inf

    ##                          vars     n        mean          sd      median
    ## player_id                   1 54600      629.80      358.52      637.50
    ## player_name                 2 54600      615.16      361.47      613.00
    ## age                         3 54600       26.30        4.07       26.00
    ## nationality                 4 54600       24.55       13.65       25.00
    ## team                        5 54600       24.49       13.63       25.00
    ## jersey_number               6 54600       13.50        7.50       13.50
    ## position                    7 54600        2.38        1.24        2.00
    ## height_cm                   8 54600      181.65        6.28      182.00
    ## weight_kg                   9 54600       75.75        3.95       76.00
    ## preferred_foot             10 54600        1.74        0.44        2.00
    ## club_name                  11 54600       62.65       35.93       63.00
    ## market_value_eur           12 54600 20084451.76 27188660.40 10271107.00
    ## match_id                   13 54600      525.50      303.11      525.50
    ## match_date                 14 54600         NaN          NA          NA
    ## stadium                    15 54600        8.45        4.64        9.00
    ## city                       16 54600        8.63        4.59        9.00
    ## opponent_team              17 54600       24.49       13.63       25.00
    ## tournament_stage           18 54600        3.07        1.45        2.00
    ## match_result               19 54600        2.11        0.79        2.00
    ## goals_team                 20 54600        1.33        1.15        1.00
    ## goals_opponent             21 54600        1.33        1.15        1.00
    ## minutes_played             22 54600       36.20       36.42       24.00
    ## goals                      23 54600        0.06        0.25        0.00
    ## assists                    24 54600        0.05        0.24        0.00
    ## shots                      25 54600        0.45        0.95        0.00
    ## shots_on_target            26 54600        0.05        0.24        0.00
    ## expected_goals_xg          27 54600        0.02        0.07        0.00
    ## expected_assists_xa        28 54600        0.02        0.07        0.00
    ## key_passes                 29 54600        0.47        0.94        0.00
    ## successful_passes          30 54600       15.47       18.75        9.00
    ## total_passes               31 54600       19.18       22.62       11.00
    ## pass_accuracy              32 54600        0.81        0.07        0.82
    ## dribbles_attempted         33 54600        0.60        1.15        0.00
    ## successful_dribbles        34 54600        0.16        0.50        0.00
    ## crosses                    35 54600        0.45        0.86        0.00
    ## successful_crosses         36 54600        0.02        0.14        0.00
    ## tackles                    37 54600        0.80        1.34        0.00
    ## interceptions              38 54600        0.63        1.13        0.00
    ## clearances                 39 54600        0.81        1.53        0.00
    ## blocks                     40 54600        0.23        0.56        0.00
    ## aerial_duels_won           41 54600        0.75        1.17        0.00
    ## aerial_duels_lost          42 54600        0.36        0.71        0.00
    ## recoveries                 43 54600        1.39        2.02        0.00
    ## defensive_actions          44 54600        2.47        3.64        0.00
    ## fouls_committed            45 54600        0.42        0.77        0.00
    ## fouls_suffered             46 54600        0.32        0.65        0.00
    ## yellow_cards               47 54600        0.10        0.30        0.00
    ## red_cards                  48 54600        0.01        0.07        0.00
    ## offsides                   49 54600        0.09        0.32        0.00
    ## saves                      50 54600        0.12        0.70        0.00
    ## save_percentage            51 54600        0.03        0.14        0.00
    ## punches                    52 54600        0.03        0.21        0.00
    ## clean_sheet                53 54600        0.01        0.10        0.00
    ## goals_conceded             54 54600        0.05        0.33        0.00
    ## penalty_saves              55 54600        0.00        0.06        0.00
    ## distance_covered_km        56 54600        4.00        4.02        3.00
    ## sprint_distance_km         57 54600        0.46        0.48        0.30
    ## top_speed_kmh              58 54600       18.65       16.02       29.90
    ## accelerations              59 54600       10.04       10.58        6.00
    ## decelerations              60 54600        8.85        9.39        6.00
    ## stamina_score              61 54600       81.89       10.77       82.60
    ## player_rating              62 54600        3.63        3.16        5.50
    ## performance_score          63 54600       36.83       31.07       54.10
    ## offensive_contribution     64 54600       44.72       24.87       41.60
    ## defensive_contribution     65 54600       52.73       23.90       49.90
    ## possession_impact          66 54600        2.85        4.23        1.10
    ## pressure_resistance        67 54600       60.32       20.23       57.00
    ## creativity_score           68 54600       46.14       22.42       40.50
    ## consistency_score          69 54600       63.75       19.86       61.10
    ## clutch_performance_score   70 54600       55.57       13.66       55.00
    ## total_goals_tournament     71 54600        0.64        1.09        0.00
    ## total_assists_tournament   72 54600        0.61        0.93        0.00
    ## total_minutes_tournament   73 54600      272.30      116.81      268.00
    ## player_of_match_awards     74 54600        0.03        0.21        0.00
    ## tournament_rating          75 54600        3.63        3.16        5.40
    ##                              trimmed         mad       min       max
    ## player_id                     630.61      463.31      1.00 1.248e+03
    ## player_name                   614.20      465.54      1.00 1.245e+03
    ## age                            26.27        4.45     17.00 3.900e+01
    ## nationality                    24.52       16.31      1.00 4.800e+01
    ## team                           24.46       17.79      1.00 4.800e+01
    ## jersey_number                  13.50        9.64      1.00 2.600e+01
    ## position                        2.36        1.48      1.00 4.000e+00
    ## height_cm                     181.79        5.93    163.00 2.000e+02
    ## weight_kg                      75.79        4.45     65.00 8.700e+01
    ## preferred_foot                  1.81        0.00      1.00 2.000e+00
    ## club_name                      62.80       45.96      1.00 1.220e+02
    ## market_value_eur         13929561.22 10229505.60 528822.00 2.000e+08
    ## match_id                      525.50      389.18      1.00 1.050e+03
    ## match_date                       NaN          NA       Inf      -Inf
    ## stadium                         8.44        5.93      1.00 1.600e+01
    ## city                            8.66        5.93      1.00 1.600e+01
    ## opponent_team                  24.46       17.79      1.00 4.800e+01
    ## tournament_stage                2.89        0.00      1.00 7.000e+00
    ## match_result                    2.13        1.48      1.00 3.000e+00
    ## goals_team                      1.21        1.48      0.00 7.000e+00
    ## goals_opponent                  1.21        1.48      0.00 7.000e+00
    ## minutes_played                 34.09       35.58      0.00 9.000e+01
    ## goals                           0.00        0.00      0.00 4.000e+00
    ## assists                         0.00        0.00      0.00 3.000e+00
    ## shots                           0.21        0.00      0.00 1.100e+01
    ## shots_on_target                 0.00        0.00      0.00 5.000e+00
    ## expected_goals_xg               0.00        0.00      0.00 2.310e+00
    ## expected_assists_xa             0.00        0.00      0.00 2.200e+00
    ## key_passes                      0.24        0.00      0.00 8.000e+00
    ## successful_passes              12.29       13.34      0.00 9.700e+01
    ## total_passes                   15.50       16.31      0.00 1.000e+02
    ## pass_accuracy                   0.81        0.07      0.42 9.700e-01
    ## dribbles_attempted              0.32        0.00      0.00 1.000e+01
    ## successful_dribbles             0.03        0.00      0.00 6.000e+00
    ## crosses                         0.24        0.00      0.00 8.000e+00
    ## successful_crosses              0.00        0.00      0.00 2.000e+00
    ## tackles                         0.50        0.00      0.00 8.000e+00
    ## interceptions                   0.37        0.00      0.00 7.000e+00
    ## clearances                      0.43        0.00      0.00 1.200e+01
    ## blocks                          0.09        0.00      0.00 4.000e+00
    ## aerial_duels_won                0.49        0.00      0.00 7.000e+00
    ## aerial_duels_lost               0.19        0.00      0.00 5.000e+00
    ## recoveries                      0.98        0.00      0.00 1.200e+01
    ## defensive_actions               1.69        0.00      0.00 2.300e+01
    ## fouls_committed                 0.23        0.00      0.00 5.000e+00
    ## fouls_suffered                  0.17        0.00      0.00 4.000e+00
    ## yellow_cards                    0.00        0.00      0.00 1.000e+00
    ## red_cards                       0.00        0.00      0.00 1.000e+00
    ## offsides                        0.00        0.00      0.00 4.000e+00
    ## saves                           0.00        0.00      0.00 1.100e+01
    ## save_percentage                 0.00        0.00      0.00 1.000e+00
    ## punches                         0.00        0.00      0.00 3.000e+00
    ## clean_sheet                     0.00        0.00      0.00 1.000e+00
    ## goals_conceded                  0.00        0.00      0.00 7.000e+00
    ## penalty_saves                   0.00        0.00      0.00 1.000e+00
    ## distance_covered_km             3.67        4.45      0.00 1.400e+01
    ## sprint_distance_km              0.41        0.44      0.00 2.000e+00
    ## top_speed_kmh                  18.89        6.97      0.00 3.700e+01
    ## accelerations                   8.98        8.90      0.00 4.400e+01
    ## decelerations                   7.88        8.90      0.00 4.000e+01
    ## stamina_score                  82.35       11.27     50.00 9.900e+01
    ## player_rating                   3.62        2.37      0.00 9.400e+00
    ## performance_score              36.68       26.09      0.00 9.770e+01
    ## offensive_contribution         43.21       25.65      1.00 9.900e+01
    ## defensive_contribution         51.73       27.43      5.00 9.900e+01
    ## possession_impact               1.94        1.63      0.00 3.700e+01
    ## pressure_resistance            59.19       22.24     15.00 9.900e+01
    ## creativity_score               43.97       19.27      5.00 9.900e+01
    ## consistency_score              62.81       23.28     25.00 9.900e+01
    ## clutch_performance_score       55.16       13.05     15.00 9.900e+01
    ## total_goals_tournament          0.40        0.00      0.00 1.000e+01
    ## total_assists_tournament        0.42        0.00      0.00 8.000e+00
    ## total_minutes_tournament      270.93      128.99      0.00 6.150e+02
    ## player_of_match_awards          0.00        0.00      0.00 4.000e+00
    ## tournament_rating               3.61        2.67      0.00 9.500e+00
    ##                                 range  skew kurtosis        se
    ## player_id                     1247.00 -0.03    -1.22      1.53
    ## player_name                   1244.00  0.02    -1.21      1.55
    ## age                             22.00  0.10    -0.37      0.02
    ## nationality                     47.00  0.00    -1.17      0.06
    ## team                            47.00  0.00    -1.16      0.06
    ## jersey_number                   25.00  0.00    -1.20      0.03
    ## position                         3.00  0.20    -1.58      0.01
    ## height_cm                       37.00 -0.18    -0.19      0.03
    ## weight_kg                       22.00 -0.09    -0.28      0.02
    ## preferred_foot                   1.00 -1.12    -0.74      0.00
    ## club_name                      121.00 -0.02    -1.21      0.15
    ## market_value_eur         199471178.00  2.98    11.09 116356.72
    ## match_id                      1049.00  0.00    -1.20      1.30
    ## match_date                       -Inf    NA       NA        NA
    ## stadium                         15.00  0.00    -1.23      0.02
    ## city                            15.00 -0.04    -1.19      0.02
    ## opponent_team                   47.00  0.00    -1.16      0.06
    ## tournament_stage                 6.00  0.88    -0.46      0.01
    ## match_result                     2.00 -0.19    -1.37      0.00
    ## goals_team                       7.00  0.93     1.04      0.00
    ## goals_opponent                   7.00  0.93     1.04      0.00
    ## minutes_played                  90.00  0.26    -1.69      0.16
    ## goals                            4.00  5.06    29.95      0.00
    ## assists                          3.00  4.90    26.59      0.00
    ## shots                           11.00  2.89    10.79      0.00
    ## shots_on_target                  5.00  5.91    42.92      0.00
    ## expected_goals_xg                2.31 10.12   153.11      0.00
    ## expected_assists_xa              2.20  9.20   130.77      0.00
    ## key_passes                       8.00  2.58     8.14      0.00
    ## successful_passes               97.00  1.20     0.83      0.08
    ## total_passes                   100.00  1.10     0.51      0.10
    ## pass_accuracy                    0.55 -0.55     0.42      0.00
    ## dribbles_attempted              10.00  2.52     7.57      0.00
    ## successful_dribbles              6.00  3.83    18.26      0.00
    ## crosses                          8.00  2.32     6.06      0.00
    ## successful_crosses               2.00  7.54    58.44      0.00
    ## tackles                          8.00  1.95     3.78      0.01
    ## interceptions                    7.00  2.15     4.82      0.00
    ## clearances                      12.00  2.39     6.14      0.01
    ## blocks                           4.00  2.87     9.29      0.00
    ## aerial_duels_won                 7.00  1.78     3.12      0.01
    ## aerial_duels_lost                5.00  2.24     5.39      0.00
    ## recoveries                      12.00  1.72     2.94      0.01
    ## defensive_actions               23.00  1.69     2.38      0.02
    ## fouls_committed                  5.00  2.09     4.55      0.00
    ## fouls_suffered                   4.00  2.28     5.58      0.00
    ## yellow_cards                     1.00  2.71     5.32      0.00
    ## red_cards                        1.00 13.24   173.43      0.00
    ## offsides                         4.00  4.10    19.57      0.00
    ## saves                           11.00  6.71    49.71      0.00
    ## save_percentage                  1.00  5.08    24.37      0.00
    ## punches                          3.00  9.40   100.24      0.00
    ## clean_sheet                      1.00 10.29   103.99      0.00
    ## goals_conceded                   7.00  8.53    86.54      0.00
    ## penalty_saves                    1.00 17.23   294.99      0.00
    ## distance_covered_km             14.00  0.36    -1.43      0.02
    ## sprint_distance_km               2.00  0.52    -1.04      0.00
    ## top_speed_kmh                   37.00 -0.28    -1.88      0.07
    ## accelerations                   44.00  0.49    -1.22      0.05
    ## decelerations                   40.00  0.52    -1.16      0.04
    ## stamina_score                   49.00 -0.37    -0.36      0.05
    ## player_rating                    9.40 -0.22    -1.83      0.01
    ## performance_score               97.70 -0.21    -1.80      0.13
    ## offensive_contribution          98.00  0.46    -0.54      0.11
    ## defensive_contribution          94.00  0.31    -0.87      0.10
    ## possession_impact               37.00  2.25     6.30      0.02
    ## pressure_resistance             84.00  0.40    -0.86      0.09
    ## creativity_score                94.00  0.78    -0.16      0.10
    ## consistency_score               74.00  0.34    -1.05      0.08
    ## clutch_performance_score        84.00  0.36     0.51      0.06
    ## total_goals_tournament          10.00  2.24     6.17      0.00
    ## total_assists_tournament         8.00  1.80     3.66      0.00
    ## total_minutes_tournament       615.00  0.10    -0.68      0.50
    ## player_of_match_awards           4.00  8.18    81.82      0.00
    ## tournament_rating                9.50 -0.21    -1.81      0.01

``` r
# Otro paquete más. Podemos comparar cuál les parece más útil en este caso:

install.packages("modelsummary", repos = "https://cloud.r-project.org")
```

``` r
library(modelsummary)

datasummary_skim(df)
```

    ## Warning: These variables were omitted because they include more than 50 levels:
    ## player_id, player_name, club_name, match_id.

|  | Unique | Missing Pct. | Mean | SD | Min | Median | Max | Histogram |
|----|----|----|----|----|----|----|----|----|
| age | 23 | 0 | 26.3 | 4.1 | 17.0 | 26.0 | 3.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_51_id0i2r063gcbqbkvh1t1nw.png"
height="16" /> |
| jersey_number | 26 | 0 | 13.5 | 7.5 | 1.0 | 13.5 | 2.60e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_48_idev9wjf893mjjm75escrk.png"
height="16" /> |
| height_cm | 37 | 0 | 181.7 | 6.3 | 163.0 | 182.0 | 2.00e+02 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_59_idlt9g3np5freo5naqwo0e.png"
height="16" /> |
| weight_kg | 22 | 0 | 75.8 | 4.0 | 65.0 | 76.0 | 8.70e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_53_idk5gc6bpujfleer6uo57z.png"
height="16" /> |
| market_value_eur | 1246 | 0 | 20084451.8 | 27188660.4 | 528822.0 | 10271107.0 | 2.00e+08 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_61_id74ncvj8xofb8v5q9at3z.png"
height="16" /> |
| goals_team | 8 | 0 | 1.3 | 1.1 | 0.0 | 1.0 | 7.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_49_idty71hov305yozrom0an2.png"
height="16" /> |
| goals_opponent | 8 | 0 | 1.3 | 1.1 | 0.0 | 1.0 | 7.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_01_id8s429oc87jg50gzsz74d.png"
height="16" /> |
| minutes_played | 87 | 0 | 36.2 | 36.4 | 0.0 | 24.0 | 9.00e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_02_idmns1chx4m98v8x9ue9c5.png"
height="16" /> |
| goals | 5 | 0 | 0.1 | 0.3 | 0.0 | 0.0 | 4.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_03_id0wkzmnqse62yp1sn4quy.png"
height="16" /> |
| assists | 4 | 0 | 0.1 | 0.2 | 0.0 | 0.0 | 3.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_04_id7s5w2rl5gk6wd2qvgjg2.png"
height="16" /> |
| shots | 12 | 0 | 0.4 | 0.9 | 0.0 | 0.0 | 1.10e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_05_idn7b14aq63ix8qcsawd2i.png"
height="16" /> |
| shots_on_target | 6 | 0 | 0.0 | 0.2 | 0.0 | 0.0 | 5.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_06_idnosydng5kbqolz8it8sd.png"
height="16" /> |
| expected_goals_xg | 133 | 0 | 0.0 | 0.1 | 0.0 | 0.0 | 2.30e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_07_idihjq61u96egxd05mn2qn.png"
height="16" /> |
| expected_assists_xa | 130 | 0 | 0.0 | 0.1 | 0.0 | 0.0 | 2.20e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_08_idbsp0y9q6w6pbekmlo1ii.png"
height="16" /> |
| key_passes | 9 | 0 | 0.5 | 0.9 | 0.0 | 0.0 | 8.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_09_ide2il6rriytykrw56nks1.png"
height="16" /> |
| successful_passes | 97 | 0 | 15.5 | 18.7 | 0.0 | 9.0 | 9.70e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_10_id481zf0xe8q29xvhg0e3f.png"
height="16" /> |
| total_passes | 101 | 0 | 19.2 | 22.6 | 0.0 | 11.0 | 1.00e+02 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_11_id10vryr92elzjc3sek09d.png"
height="16" /> |
| pass_accuracy | 53 | 0 | 0.8 | 0.1 | 0.4 | 0.8 | 1.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_47_id90vc26333io0m410ceko.png"
height="16" /> |
| dribbles_attempted | 11 | 0 | 0.6 | 1.2 | 0.0 | 0.0 | 1.00e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_12_id1atetn8vqob98rqge2xd.png"
height="16" /> |
| successful_dribbles | 7 | 0 | 0.2 | 0.5 | 0.0 | 0.0 | 6.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_13_id9codz99cnaslwmyokjgc.png"
height="16" /> |
| crosses | 9 | 0 | 0.4 | 0.9 | 0.0 | 0.0 | 8.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_14_idigeb0mvv2k7r0xfv0x8a.png"
height="16" /> |
| successful_crosses | 3 | 0 | 0.0 | 0.1 | 0.0 | 0.0 | 2.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_15_idrl7fof6m3uw22b4g3blj.png"
height="16" /> |
| tackles | 9 | 0 | 0.8 | 1.3 | 0.0 | 0.0 | 8.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_16_idsqhj0u7sjbunvv2rko8o.png"
height="16" /> |
| interceptions | 8 | 0 | 0.6 | 1.1 | 0.0 | 0.0 | 7.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_17_idynmaoo521vp6cmuzcivl.png"
height="16" /> |
| clearances | 13 | 0 | 0.8 | 1.5 | 0.0 | 0.0 | 1.20e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_18_id247cq0770xbndbwy0h34.png"
height="16" /> |
| blocks | 5 | 0 | 0.2 | 0.6 | 0.0 | 0.0 | 4.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_19_idtjp0eqjidwu4eiqf7wvc.png"
height="16" /> |
| aerial_duels_won | 8 | 0 | 0.7 | 1.2 | 0.0 | 0.0 | 7.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_20_id7wcshy5ip8gr1eklmtrs.png"
height="16" /> |
| aerial_duels_lost | 6 | 0 | 0.4 | 0.7 | 0.0 | 0.0 | 5.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_21_idf2ygbdiwytxd26dxfazf.png"
height="16" /> |
| recoveries | 13 | 0 | 1.4 | 2.0 | 0.0 | 0.0 | 1.20e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_22_idjx1p7dd4kv51eq26c5pk.png"
height="16" /> |
| defensive_actions | 24 | 0 | 2.5 | 3.6 | 0.0 | 0.0 | 2.30e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_23_idu67y3w0r58cvlub1mknh.png"
height="16" /> |
| fouls_committed | 6 | 0 | 0.4 | 0.8 | 0.0 | 0.0 | 5.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_24_idawo1lx52oqs1d91zch8n.png"
height="16" /> |
| fouls_suffered | 5 | 0 | 0.3 | 0.7 | 0.0 | 0.0 | 4.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_25_idrqthv95i4l81omdk797h.png"
height="16" /> |
| yellow_cards | 2 | 0 | 0.1 | 0.3 | 0.0 | 0.0 | 1.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_26_idjsqo84uxdpg210rsgac9.png"
height="16" /> |
| red_cards | 2 | 0 | 0.0 | 0.1 | 0.0 | 0.0 | 1.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_27_id5cu9k7ruszwostcr12ko.png"
height="16" /> |
| offsides | 5 | 0 | 0.1 | 0.3 | 0.0 | 0.0 | 4.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_28_id9hw3wr5am0i5bnwyxcvm.png"
height="16" /> |
| saves | 11 | 0 | 0.1 | 0.7 | 0.0 | 0.0 | 1.10e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_29_idfi4jqdp934y1us1su23c.png"
height="16" /> |
| save_percentage | 61 | 0 | 0.0 | 0.1 | 0.0 | 0.0 | 1.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_30_id30sep7s5vj28rr75fa4i.png"
height="16" /> |
| punches | 4 | 0 | 0.0 | 0.2 | 0.0 | 0.0 | 3.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_31_id2nlz9gyuznwp0fox3ia4.png"
height="16" /> |
| clean_sheet | 2 | 0 | 0.0 | 0.1 | 0.0 | 0.0 | 1.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_32_idrfyct2ymdcfy5yirv2ny.png"
height="16" /> |
| goals_conceded | 8 | 0 | 0.0 | 0.3 | 0.0 | 0.0 | 7.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_33_idrwhvqiscezm79608mxe9.png"
height="16" /> |
| penalty_saves | 2 | 0 | 0.0 | 0.1 | 0.0 | 0.0 | 1.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_34_ido60rbsln2y6l2wbok0xn.png"
height="16" /> |
| distance_covered_km | 122 | 0 | 4.0 | 4.0 | 0.0 | 3.0 | 1.40e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_35_id1q3ax30hd9usnlho5vsz.png"
height="16" /> |
| sprint_distance_km | 19 | 0 | 0.5 | 0.5 | 0.0 | 0.3 | 2.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_36_idum1z8zh336c4m8vi1760.png"
height="16" /> |
| top_speed_kmh | 112 | 0 | 18.6 | 16.0 | 0.0 | 29.9 | 3.70e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_37_idwrj2etfp23iwr6hj2jg5.png"
height="16" /> |
| accelerations | 45 | 0 | 10.0 | 10.6 | 0.0 | 6.0 | 4.40e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_38_idg7ycnrl1s84jrk8fm4qb.png"
height="16" /> |
| decelerations | 41 | 0 | 8.8 | 9.4 | 0.0 | 6.0 | 4.00e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_39_id26a8uhvna9ppd8ysmsya.png"
height="16" /> |
| stamina_score | 491 | 0 | 81.9 | 10.8 | 50.0 | 82.6 | 9.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_54_idyu0eicbouglydml6d1pg.png"
height="16" /> |
| player_rating | 58 | 0 | 3.6 | 3.2 | 0.0 | 5.5 | 9.40e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_40_idiz5q0zowdu206o93begi.png"
height="16" /> |
| performance_score | 639 | 0 | 36.8 | 31.1 | 0.0 | 54.1 | 9.77e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_41_idesusnsnvc90sf48libu5.png"
height="16" /> |
| offensive_contribution | 981 | 0 | 44.7 | 24.9 | 1.0 | 41.6 | 9.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_50_idxf18mgn4wft7774f8i18.png"
height="16" /> |
| defensive_contribution | 941 | 0 | 52.7 | 23.9 | 5.0 | 49.9 | 9.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_55_idpzu68x9fzvbab4elfxjl.png"
height="16" /> |
| possession_impact | 308 | 0 | 2.9 | 4.2 | 0.0 | 1.1 | 3.70e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_42_id3hprf2aos1mwvscgy90t.png"
height="16" /> |
| pressure_resistance | 815 | 0 | 60.3 | 20.2 | 15.0 | 57.0 | 9.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_56_id94lahgd6dej3e1j6wsag.png"
height="16" /> |
| creativity_score | 941 | 0 | 46.1 | 22.4 | 5.0 | 40.5 | 9.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_52_idqsv03jh9phntea2rzur6.png"
height="16" /> |
| consistency_score | 738 | 0 | 63.8 | 19.9 | 25.0 | 61.1 | 9.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_57_idrpxafg289fkrqp90tub7.png"
height="16" /> |
| clutch_performance_score | 829 | 0 | 55.6 | 13.7 | 15.0 | 55.0 | 9.90e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_58_idja85xg4u5fdzam5trig2.png"
height="16" /> |
| total_goals_tournament | 11 | 0 | 0.6 | 1.1 | 0.0 | 0.0 | 1.00e+01 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_43_id6fxi7marr04xyzzh9swg.png"
height="16" /> |
| total_assists_tournament | 9 | 0 | 0.6 | 0.9 | 0.0 | 0.0 | 8.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_44_idv4g7gvle43ryei184vqh.png"
height="16" /> |
| total_minutes_tournament | 586 | 0 | 272.3 | 116.8 | 0.0 | 268.0 | 6.15e+02 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_60_idjseasc7c56qfpism08vp.png"
height="16" /> |
| player_of_match_awards | 5 | 0 | 0.0 | 0.2 | 0.0 | 0.0 | 4.00e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_45_idmlmc632687yl6ln9dq0s.png"
height="16" /> |
| tournament_rating | 63 | 0 | 3.6 | 3.2 | 0.0 | 5.4 | 9.50e+00 | <img
src="C:\Users\juanc\Documentos\Rosario\Cuanti\tinytable_assets\tinytable_46_idjmkdid11ar1rf32mumj3.png"
height="16" /> |
|  |  | N | % |  |  |  |  |  |
| nationality | Algerian | 1066 | 2.0 |  |  |  |  |  |
|  | American | 988 | 1.8 |  |  |  |  |  |
|  | Argentine | 884 | 1.6 |  |  |  |  |  |
|  | Australian | 1222 | 2.2 |  |  |  |  |  |
|  | Austrian | 858 | 1.6 |  |  |  |  |  |
|  | Belgian | 1326 | 2.4 |  |  |  |  |  |
|  | Brazilian | 1170 | 2.1 |  |  |  |  |  |
|  | Cameroonian | 1144 | 2.1 |  |  |  |  |  |
|  | Canadian | 1248 | 2.3 |  |  |  |  |  |
|  | Chilean | 962 | 1.8 |  |  |  |  |  |
|  | Colombian | 1196 | 2.2 |  |  |  |  |  |
|  | Costa Rican | 1196 | 2.2 |  |  |  |  |  |
|  | Croatian | 1274 | 2.3 |  |  |  |  |  |
|  | Danish | 1118 | 2.0 |  |  |  |  |  |
|  | Dutch | 1404 | 2.6 |  |  |  |  |  |
|  | Ecuadorian | 1222 | 2.2 |  |  |  |  |  |
|  | Egyptian | 1170 | 2.1 |  |  |  |  |  |
|  | English | 1144 | 2.1 |  |  |  |  |  |
|  | French | 884 | 1.6 |  |  |  |  |  |
|  | German | 884 | 1.6 |  |  |  |  |  |
|  | Ghanaian | 1170 | 2.1 |  |  |  |  |  |
|  | Iranian | 988 | 1.8 |  |  |  |  |  |
|  | Iraqi | 1066 | 2.0 |  |  |  |  |  |
|  | Italian | 1378 | 2.5 |  |  |  |  |  |
|  | Jamaican | 1534 | 2.8 |  |  |  |  |  |
|  | Japanese | 936 | 1.7 |  |  |  |  |  |
|  | Mexican | 1040 | 1.9 |  |  |  |  |  |
|  | Moroccan | 1430 | 2.6 |  |  |  |  |  |
|  | Nigerian | 1040 | 1.9 |  |  |  |  |  |
|  | Panamanian | 1222 | 2.2 |  |  |  |  |  |
|  | Peruvian | 1248 | 2.3 |  |  |  |  |  |
|  | Polish | 1014 | 1.9 |  |  |  |  |  |
|  | Portuguese | 1118 | 2.0 |  |  |  |  |  |
|  | Qatari | 1716 | 3.1 |  |  |  |  |  |
|  | Saudi | 1352 | 2.5 |  |  |  |  |  |
|  | Scottish | 1274 | 2.3 |  |  |  |  |  |
|  | Senegalese | 988 | 1.8 |  |  |  |  |  |
|  | Serbian | 1092 | 2.0 |  |  |  |  |  |
|  | South African | 910 | 1.7 |  |  |  |  |  |
|  | South Korean | 884 | 1.6 |  |  |  |  |  |
|  | Spanish | 1170 | 2.1 |  |  |  |  |  |
|  | Swedish | 936 | 1.7 |  |  |  |  |  |
|  | Swiss | 910 | 1.7 |  |  |  |  |  |
|  | Tunisian | 1222 | 2.2 |  |  |  |  |  |
|  | Turkish | 1300 | 2.4 |  |  |  |  |  |
|  | Ukrainian | 988 | 1.8 |  |  |  |  |  |
|  | Uruguayan | 1170 | 2.1 |  |  |  |  |  |
|  | Uzbek | 1144 | 2.1 |  |  |  |  |  |
| team | Algeria | 1066 | 2.0 |  |  |  |  |  |
|  | Argentina | 884 | 1.6 |  |  |  |  |  |
|  | Australia | 1222 | 2.2 |  |  |  |  |  |
|  | Austria | 858 | 1.6 |  |  |  |  |  |
|  | Belgium | 1326 | 2.4 |  |  |  |  |  |
|  | Brazil | 1170 | 2.1 |  |  |  |  |  |
|  | Cameroon | 1144 | 2.1 |  |  |  |  |  |
|  | Canada | 1248 | 2.3 |  |  |  |  |  |
|  | Chile | 962 | 1.8 |  |  |  |  |  |
|  | Colombia | 1196 | 2.2 |  |  |  |  |  |
|  | Costa Rica | 1196 | 2.2 |  |  |  |  |  |
|  | Croatia | 1274 | 2.3 |  |  |  |  |  |
|  | Denmark | 1118 | 2.0 |  |  |  |  |  |
|  | Ecuador | 1222 | 2.2 |  |  |  |  |  |
|  | Egypt | 1170 | 2.1 |  |  |  |  |  |
|  | England | 1144 | 2.1 |  |  |  |  |  |
|  | France | 884 | 1.6 |  |  |  |  |  |
|  | Germany | 884 | 1.6 |  |  |  |  |  |
|  | Ghana | 1170 | 2.1 |  |  |  |  |  |
|  | Iran | 988 | 1.8 |  |  |  |  |  |
|  | Iraq | 1066 | 2.0 |  |  |  |  |  |
|  | Italy | 1378 | 2.5 |  |  |  |  |  |
|  | Jamaica | 1534 | 2.8 |  |  |  |  |  |
|  | Japan | 936 | 1.7 |  |  |  |  |  |
|  | Mexico | 1040 | 1.9 |  |  |  |  |  |
|  | Morocco | 1430 | 2.6 |  |  |  |  |  |
|  | Netherlands | 1404 | 2.6 |  |  |  |  |  |
|  | Nigeria | 1040 | 1.9 |  |  |  |  |  |
|  | Panama | 1222 | 2.2 |  |  |  |  |  |
|  | Peru | 1248 | 2.3 |  |  |  |  |  |
|  | Poland | 1014 | 1.9 |  |  |  |  |  |
|  | Portugal | 1118 | 2.0 |  |  |  |  |  |
|  | Qatar | 1716 | 3.1 |  |  |  |  |  |
|  | Saudi Arabia | 1352 | 2.5 |  |  |  |  |  |
|  | Scotland | 1274 | 2.3 |  |  |  |  |  |
|  | Senegal | 988 | 1.8 |  |  |  |  |  |
|  | Serbia | 1092 | 2.0 |  |  |  |  |  |
|  | South Africa | 910 | 1.7 |  |  |  |  |  |
|  | South Korea | 884 | 1.6 |  |  |  |  |  |
|  | Spain | 1170 | 2.1 |  |  |  |  |  |
|  | Sweden | 936 | 1.7 |  |  |  |  |  |
|  | Switzerland | 910 | 1.7 |  |  |  |  |  |
|  | Tunisia | 1222 | 2.2 |  |  |  |  |  |
|  | Turkey | 1300 | 2.4 |  |  |  |  |  |
|  | Ukraine | 988 | 1.8 |  |  |  |  |  |
|  | United States | 988 | 1.8 |  |  |  |  |  |
|  | Uruguay | 1170 | 2.1 |  |  |  |  |  |
|  | Uzbekistan | 1144 | 2.1 |  |  |  |  |  |
| position | Defender | 18900 | 34.6 |  |  |  |  |  |
|  | Forward | 12600 | 23.1 |  |  |  |  |  |
|  | Goalkeeper | 6300 | 11.5 |  |  |  |  |  |
|  | Midfielder | 16800 | 30.8 |  |  |  |  |  |
| preferred_foot | Left | 13944 | 25.5 |  |  |  |  |  |
|  | Right | 40656 | 74.5 |  |  |  |  |  |
| stadium | Arrowhead Stadium | 3588 | 6.6 |  |  |  |  |  |
|  | AT&T Stadium | 3484 | 6.4 |  |  |  |  |  |
|  | BC Place | 3744 | 6.9 |  |  |  |  |  |
|  | BMO Field | 3172 | 5.8 |  |  |  |  |  |
|  | Estadio Akron | 3796 | 7.0 |  |  |  |  |  |
|  | Estadio Azteca | 2756 | 5.0 |  |  |  |  |  |
|  | Estadio BBVA | 4004 | 7.3 |  |  |  |  |  |
|  | Gillette Stadium | 2600 | 4.8 |  |  |  |  |  |
|  | Hard Rock Stadium | 3120 | 5.7 |  |  |  |  |  |
|  | Levi’s Stadium | 3432 | 6.3 |  |  |  |  |  |
|  | Lincoln Financial Field | 4264 | 7.8 |  |  |  |  |  |
|  | Lumen Field | 3172 | 5.8 |  |  |  |  |  |
|  | Mercedes-Benz Stadium | 3692 | 6.8 |  |  |  |  |  |
|  | MetLife Stadium | 2808 | 5.1 |  |  |  |  |  |
|  | NRG Stadium | 3536 | 6.5 |  |  |  |  |  |
|  | SoFi Stadium | 3432 | 6.3 |  |  |  |  |  |
| city | Atlanta | 3692 | 6.8 |  |  |  |  |  |
|  | Boston | 2600 | 4.8 |  |  |  |  |  |
|  | Dallas | 3484 | 6.4 |  |  |  |  |  |
|  | East Rutherford | 2808 | 5.1 |  |  |  |  |  |
|  | Guadalajara | 3796 | 7.0 |  |  |  |  |  |
|  | Houston | 3536 | 6.5 |  |  |  |  |  |
|  | Kansas City | 3588 | 6.6 |  |  |  |  |  |
|  | Los Angeles | 3432 | 6.3 |  |  |  |  |  |
|  | Mexico City | 2756 | 5.0 |  |  |  |  |  |
|  | Miami | 3120 | 5.7 |  |  |  |  |  |
|  | Monterrey | 4004 | 7.3 |  |  |  |  |  |
|  | Philadelphia | 4264 | 7.8 |  |  |  |  |  |
|  | San Francisco | 3432 | 6.3 |  |  |  |  |  |
|  | Seattle | 3172 | 5.8 |  |  |  |  |  |
|  | Toronto | 3172 | 5.8 |  |  |  |  |  |
|  | Vancouver | 3744 | 6.9 |  |  |  |  |  |
| opponent_team | Algeria | 1066 | 2.0 |  |  |  |  |  |
|  | Argentina | 884 | 1.6 |  |  |  |  |  |
|  | Australia | 1222 | 2.2 |  |  |  |  |  |
|  | Austria | 858 | 1.6 |  |  |  |  |  |
|  | Belgium | 1326 | 2.4 |  |  |  |  |  |
|  | Brazil | 1170 | 2.1 |  |  |  |  |  |
|  | Cameroon | 1144 | 2.1 |  |  |  |  |  |
|  | Canada | 1248 | 2.3 |  |  |  |  |  |
|  | Chile | 962 | 1.8 |  |  |  |  |  |
|  | Colombia | 1196 | 2.2 |  |  |  |  |  |
|  | Costa Rica | 1196 | 2.2 |  |  |  |  |  |
|  | Croatia | 1274 | 2.3 |  |  |  |  |  |
|  | Denmark | 1118 | 2.0 |  |  |  |  |  |
|  | Ecuador | 1222 | 2.2 |  |  |  |  |  |
|  | Egypt | 1170 | 2.1 |  |  |  |  |  |
|  | England | 1144 | 2.1 |  |  |  |  |  |
|  | France | 884 | 1.6 |  |  |  |  |  |
|  | Germany | 884 | 1.6 |  |  |  |  |  |
|  | Ghana | 1170 | 2.1 |  |  |  |  |  |
|  | Iran | 988 | 1.8 |  |  |  |  |  |
|  | Iraq | 1066 | 2.0 |  |  |  |  |  |
|  | Italy | 1378 | 2.5 |  |  |  |  |  |
|  | Jamaica | 1534 | 2.8 |  |  |  |  |  |
|  | Japan | 936 | 1.7 |  |  |  |  |  |
|  | Mexico | 1040 | 1.9 |  |  |  |  |  |
|  | Morocco | 1430 | 2.6 |  |  |  |  |  |
|  | Netherlands | 1404 | 2.6 |  |  |  |  |  |
|  | Nigeria | 1040 | 1.9 |  |  |  |  |  |
|  | Panama | 1222 | 2.2 |  |  |  |  |  |
|  | Peru | 1248 | 2.3 |  |  |  |  |  |
|  | Poland | 1014 | 1.9 |  |  |  |  |  |
|  | Portugal | 1118 | 2.0 |  |  |  |  |  |
|  | Qatar | 1716 | 3.1 |  |  |  |  |  |
|  | Saudi Arabia | 1352 | 2.5 |  |  |  |  |  |
|  | Scotland | 1274 | 2.3 |  |  |  |  |  |
|  | Senegal | 988 | 1.8 |  |  |  |  |  |
|  | Serbia | 1092 | 2.0 |  |  |  |  |  |
|  | South Africa | 910 | 1.7 |  |  |  |  |  |
|  | South Korea | 884 | 1.6 |  |  |  |  |  |
|  | Spain | 1170 | 2.1 |  |  |  |  |  |
|  | Sweden | 936 | 1.7 |  |  |  |  |  |
|  | Switzerland | 910 | 1.7 |  |  |  |  |  |
|  | Tunisia | 1222 | 2.2 |  |  |  |  |  |
|  | Turkey | 1300 | 2.4 |  |  |  |  |  |
|  | Ukraine | 988 | 1.8 |  |  |  |  |  |
|  | United States | 988 | 1.8 |  |  |  |  |  |
|  | Uruguay | 1170 | 2.1 |  |  |  |  |  |
|  | Uzbekistan | 1144 | 2.1 |  |  |  |  |  |
| tournament_stage | Final | 1092 | 2.0 |  |  |  |  |  |
|  | Group Stage | 30056 | 55.0 |  |  |  |  |  |
|  | Quarter Finals | 4368 | 8.0 |  |  |  |  |  |
|  | Round of 16 | 6552 | 12.0 |  |  |  |  |  |
|  | Round of 32 | 9256 | 17.0 |  |  |  |  |  |
|  | Semi Finals | 2184 | 4.0 |  |  |  |  |  |
|  | Third Place Match | 1092 | 2.0 |  |  |  |  |  |
| match_result | D | 14352 | 26.3 |  |  |  |  |  |
|  | L | 20124 | 36.9 |  |  |  |  |  |
|  | W | 20124 | 36.9 |  |  |  |  |  |

``` r
datasummary_skim(df, type = "categorical")
```

    ## Warning: These variables were omitted because they include more than 50 levels:
    ## player_id, player_name, club_name, match_id.

|                  |                         | N     | %    |
|------------------|-------------------------|-------|------|
| nationality      | Algerian                | 1066  | 2.0  |
|                  | American                | 988   | 1.8  |
|                  | Argentine               | 884   | 1.6  |
|                  | Australian              | 1222  | 2.2  |
|                  | Austrian                | 858   | 1.6  |
|                  | Belgian                 | 1326  | 2.4  |
|                  | Brazilian               | 1170  | 2.1  |
|                  | Cameroonian             | 1144  | 2.1  |
|                  | Canadian                | 1248  | 2.3  |
|                  | Chilean                 | 962   | 1.8  |
|                  | Colombian               | 1196  | 2.2  |
|                  | Costa Rican             | 1196  | 2.2  |
|                  | Croatian                | 1274  | 2.3  |
|                  | Danish                  | 1118  | 2.0  |
|                  | Dutch                   | 1404  | 2.6  |
|                  | Ecuadorian              | 1222  | 2.2  |
|                  | Egyptian                | 1170  | 2.1  |
|                  | English                 | 1144  | 2.1  |
|                  | French                  | 884   | 1.6  |
|                  | German                  | 884   | 1.6  |
|                  | Ghanaian                | 1170  | 2.1  |
|                  | Iranian                 | 988   | 1.8  |
|                  | Iraqi                   | 1066  | 2.0  |
|                  | Italian                 | 1378  | 2.5  |
|                  | Jamaican                | 1534  | 2.8  |
|                  | Japanese                | 936   | 1.7  |
|                  | Mexican                 | 1040  | 1.9  |
|                  | Moroccan                | 1430  | 2.6  |
|                  | Nigerian                | 1040  | 1.9  |
|                  | Panamanian              | 1222  | 2.2  |
|                  | Peruvian                | 1248  | 2.3  |
|                  | Polish                  | 1014  | 1.9  |
|                  | Portuguese              | 1118  | 2.0  |
|                  | Qatari                  | 1716  | 3.1  |
|                  | Saudi                   | 1352  | 2.5  |
|                  | Scottish                | 1274  | 2.3  |
|                  | Senegalese              | 988   | 1.8  |
|                  | Serbian                 | 1092  | 2.0  |
|                  | South African           | 910   | 1.7  |
|                  | South Korean            | 884   | 1.6  |
|                  | Spanish                 | 1170  | 2.1  |
|                  | Swedish                 | 936   | 1.7  |
|                  | Swiss                   | 910   | 1.7  |
|                  | Tunisian                | 1222  | 2.2  |
|                  | Turkish                 | 1300  | 2.4  |
|                  | Ukrainian               | 988   | 1.8  |
|                  | Uruguayan               | 1170  | 2.1  |
|                  | Uzbek                   | 1144  | 2.1  |
| team             | Algeria                 | 1066  | 2.0  |
|                  | Argentina               | 884   | 1.6  |
|                  | Australia               | 1222  | 2.2  |
|                  | Austria                 | 858   | 1.6  |
|                  | Belgium                 | 1326  | 2.4  |
|                  | Brazil                  | 1170  | 2.1  |
|                  | Cameroon                | 1144  | 2.1  |
|                  | Canada                  | 1248  | 2.3  |
|                  | Chile                   | 962   | 1.8  |
|                  | Colombia                | 1196  | 2.2  |
|                  | Costa Rica              | 1196  | 2.2  |
|                  | Croatia                 | 1274  | 2.3  |
|                  | Denmark                 | 1118  | 2.0  |
|                  | Ecuador                 | 1222  | 2.2  |
|                  | Egypt                   | 1170  | 2.1  |
|                  | England                 | 1144  | 2.1  |
|                  | France                  | 884   | 1.6  |
|                  | Germany                 | 884   | 1.6  |
|                  | Ghana                   | 1170  | 2.1  |
|                  | Iran                    | 988   | 1.8  |
|                  | Iraq                    | 1066  | 2.0  |
|                  | Italy                   | 1378  | 2.5  |
|                  | Jamaica                 | 1534  | 2.8  |
|                  | Japan                   | 936   | 1.7  |
|                  | Mexico                  | 1040  | 1.9  |
|                  | Morocco                 | 1430  | 2.6  |
|                  | Netherlands             | 1404  | 2.6  |
|                  | Nigeria                 | 1040  | 1.9  |
|                  | Panama                  | 1222  | 2.2  |
|                  | Peru                    | 1248  | 2.3  |
|                  | Poland                  | 1014  | 1.9  |
|                  | Portugal                | 1118  | 2.0  |
|                  | Qatar                   | 1716  | 3.1  |
|                  | Saudi Arabia            | 1352  | 2.5  |
|                  | Scotland                | 1274  | 2.3  |
|                  | Senegal                 | 988   | 1.8  |
|                  | Serbia                  | 1092  | 2.0  |
|                  | South Africa            | 910   | 1.7  |
|                  | South Korea             | 884   | 1.6  |
|                  | Spain                   | 1170  | 2.1  |
|                  | Sweden                  | 936   | 1.7  |
|                  | Switzerland             | 910   | 1.7  |
|                  | Tunisia                 | 1222  | 2.2  |
|                  | Turkey                  | 1300  | 2.4  |
|                  | Ukraine                 | 988   | 1.8  |
|                  | United States           | 988   | 1.8  |
|                  | Uruguay                 | 1170  | 2.1  |
|                  | Uzbekistan              | 1144  | 2.1  |
| position         | Defender                | 18900 | 34.6 |
|                  | Forward                 | 12600 | 23.1 |
|                  | Goalkeeper              | 6300  | 11.5 |
|                  | Midfielder              | 16800 | 30.8 |
| preferred_foot   | Left                    | 13944 | 25.5 |
|                  | Right                   | 40656 | 74.5 |
| stadium          | Arrowhead Stadium       | 3588  | 6.6  |
|                  | AT&T Stadium            | 3484  | 6.4  |
|                  | BC Place                | 3744  | 6.9  |
|                  | BMO Field               | 3172  | 5.8  |
|                  | Estadio Akron           | 3796  | 7.0  |
|                  | Estadio Azteca          | 2756  | 5.0  |
|                  | Estadio BBVA            | 4004  | 7.3  |
|                  | Gillette Stadium        | 2600  | 4.8  |
|                  | Hard Rock Stadium       | 3120  | 5.7  |
|                  | Levi’s Stadium          | 3432  | 6.3  |
|                  | Lincoln Financial Field | 4264  | 7.8  |
|                  | Lumen Field             | 3172  | 5.8  |
|                  | Mercedes-Benz Stadium   | 3692  | 6.8  |
|                  | MetLife Stadium         | 2808  | 5.1  |
|                  | NRG Stadium             | 3536  | 6.5  |
|                  | SoFi Stadium            | 3432  | 6.3  |
| city             | Atlanta                 | 3692  | 6.8  |
|                  | Boston                  | 2600  | 4.8  |
|                  | Dallas                  | 3484  | 6.4  |
|                  | East Rutherford         | 2808  | 5.1  |
|                  | Guadalajara             | 3796  | 7.0  |
|                  | Houston                 | 3536  | 6.5  |
|                  | Kansas City             | 3588  | 6.6  |
|                  | Los Angeles             | 3432  | 6.3  |
|                  | Mexico City             | 2756  | 5.0  |
|                  | Miami                   | 3120  | 5.7  |
|                  | Monterrey               | 4004  | 7.3  |
|                  | Philadelphia            | 4264  | 7.8  |
|                  | San Francisco           | 3432  | 6.3  |
|                  | Seattle                 | 3172  | 5.8  |
|                  | Toronto                 | 3172  | 5.8  |
|                  | Vancouver               | 3744  | 6.9  |
| opponent_team    | Algeria                 | 1066  | 2.0  |
|                  | Argentina               | 884   | 1.6  |
|                  | Australia               | 1222  | 2.2  |
|                  | Austria                 | 858   | 1.6  |
|                  | Belgium                 | 1326  | 2.4  |
|                  | Brazil                  | 1170  | 2.1  |
|                  | Cameroon                | 1144  | 2.1  |
|                  | Canada                  | 1248  | 2.3  |
|                  | Chile                   | 962   | 1.8  |
|                  | Colombia                | 1196  | 2.2  |
|                  | Costa Rica              | 1196  | 2.2  |
|                  | Croatia                 | 1274  | 2.3  |
|                  | Denmark                 | 1118  | 2.0  |
|                  | Ecuador                 | 1222  | 2.2  |
|                  | Egypt                   | 1170  | 2.1  |
|                  | England                 | 1144  | 2.1  |
|                  | France                  | 884   | 1.6  |
|                  | Germany                 | 884   | 1.6  |
|                  | Ghana                   | 1170  | 2.1  |
|                  | Iran                    | 988   | 1.8  |
|                  | Iraq                    | 1066  | 2.0  |
|                  | Italy                   | 1378  | 2.5  |
|                  | Jamaica                 | 1534  | 2.8  |
|                  | Japan                   | 936   | 1.7  |
|                  | Mexico                  | 1040  | 1.9  |
|                  | Morocco                 | 1430  | 2.6  |
|                  | Netherlands             | 1404  | 2.6  |
|                  | Nigeria                 | 1040  | 1.9  |
|                  | Panama                  | 1222  | 2.2  |
|                  | Peru                    | 1248  | 2.3  |
|                  | Poland                  | 1014  | 1.9  |
|                  | Portugal                | 1118  | 2.0  |
|                  | Qatar                   | 1716  | 3.1  |
|                  | Saudi Arabia            | 1352  | 2.5  |
|                  | Scotland                | 1274  | 2.3  |
|                  | Senegal                 | 988   | 1.8  |
|                  | Serbia                  | 1092  | 2.0  |
|                  | South Africa            | 910   | 1.7  |
|                  | South Korea             | 884   | 1.6  |
|                  | Spain                   | 1170  | 2.1  |
|                  | Sweden                  | 936   | 1.7  |
|                  | Switzerland             | 910   | 1.7  |
|                  | Tunisia                 | 1222  | 2.2  |
|                  | Turkey                  | 1300  | 2.4  |
|                  | Ukraine                 | 988   | 1.8  |
|                  | United States           | 988   | 1.8  |
|                  | Uruguay                 | 1170  | 2.1  |
|                  | Uzbekistan              | 1144  | 2.1  |
| tournament_stage | Final                   | 1092  | 2.0  |
|                  | Group Stage             | 30056 | 55.0 |
|                  | Quarter Finals          | 4368  | 8.0  |
|                  | Round of 16             | 6552  | 12.0 |
|                  | Round of 32             | 9256  | 17.0 |
|                  | Semi Finals             | 2184  | 4.0  |
|                  | Third Place Match       | 1092  | 2.0  |
| match_result     | D                       | 14352 | 26.3 |
|                  | L                       | 20124 | 36.9 |
|                  | W                       | 20124 | 36.9 |

``` r
# Y aún hay más:

install.packages("skimr", repos = "https://cloud.r-project.org")
```

``` r
library(skimr)

skim(df)
```

|                                                  |       |
|:-------------------------------------------------|:------|
| Name                                             | df    |
| Number of rows                                   | 54600 |
| Number of columns                                | 75    |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |       |
| Column type frequency:                           |       |
| character                                        | 13    |
| Date                                             | 1     |
| numeric                                          | 61    |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |       |
| Group variables                                  | None  |

Data summary

**Variable type: character**

| skim_variable    | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:-----------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| player_id        |         0 |             1 |   6 |   6 |     0 |     1248 |          0 |
| player_name      |         0 |             1 |   7 |  25 |     0 |     1245 |          0 |
| nationality      |         0 |             1 |   5 |  13 |     0 |       48 |          0 |
| team             |         0 |             1 |   4 |  13 |     0 |       48 |          0 |
| position         |         0 |             1 |   7 |  10 |     0 |        4 |          0 |
| preferred_foot   |         0 |             1 |   4 |   5 |     0 |        2 |          0 |
| club_name        |         0 |             1 |   3 |  24 |     0 |      122 |          0 |
| match_id         |         0 |             1 |   6 |   6 |     0 |     1050 |          0 |
| stadium          |         0 |             1 |   8 |  23 |     0 |       16 |          0 |
| city             |         0 |             1 |   5 |  15 |     0 |       16 |          0 |
| opponent_team    |         0 |             1 |   4 |  13 |     0 |       48 |          0 |
| tournament_stage |         0 |             1 |   5 |  17 |     0 |        7 |          0 |
| match_result     |         0 |             1 |   1 |   1 |     0 |        3 |          0 |

**Variable type: Date**

| skim_variable | n_missing | complete_rate | min | max | median | n_unique |
|:---|---:|---:|:---|:---|:---|---:|
| match_date | 0 | 1 | 2026-06-11 | 2026-07-31 | 2026-07-07 | 51 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate | mean | sd | p0 | p25 | p50 | p75 | p100 | hist |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|:---|
| age | 0 | 1 | 26.30 | 4.07 | 17.00 | 23.00 | 26.00 | 29.00 | 3.90e+01 | ▂▆▇▃▁ |
| jersey_number | 0 | 1 | 13.50 | 7.50 | 1.00 | 7.00 | 13.50 | 20.00 | 2.60e+01 | ▇▇▇▇▇ |
| height_cm | 0 | 1 | 181.65 | 6.28 | 163.00 | 177.00 | 182.00 | 186.00 | 2.00e+02 | ▁▃▇▅▁ |
| weight_kg | 0 | 1 | 75.75 | 3.95 | 65.00 | 73.00 | 76.00 | 78.00 | 8.70e+01 | ▁▃▇▃▁ |
| market_value_eur | 0 | 1 | 20084451.76 | 27188660.40 | 528822.00 | 4444778.00 | 10271107.00 | 23420128.00 | 2.00e+08 | ▇▁▁▁▁ |
| goals_team | 0 | 1 | 1.33 | 1.15 | 0.00 | 0.00 | 1.00 | 2.00 | 7.00e+00 | ▇▃▂▁▁ |
| goals_opponent | 0 | 1 | 1.33 | 1.15 | 0.00 | 0.00 | 1.00 | 2.00 | 7.00e+00 | ▇▃▂▁▁ |
| minutes_played | 0 | 1 | 36.20 | 36.42 | 0.00 | 0.00 | 24.00 | 75.00 | 9.00e+01 | ▇▂▁▂▅ |
| goals | 0 | 1 | 0.06 | 0.25 | 0.00 | 0.00 | 0.00 | 0.00 | 4.00e+00 | ▇▁▁▁▁ |
| assists | 0 | 1 | 0.05 | 0.24 | 0.00 | 0.00 | 0.00 | 0.00 | 3.00e+00 | ▇▁▁▁▁ |
| shots | 0 | 1 | 0.45 | 0.95 | 0.00 | 0.00 | 0.00 | 1.00 | 1.10e+01 | ▇▁▁▁▁ |
| shots_on_target | 0 | 1 | 0.05 | 0.24 | 0.00 | 0.00 | 0.00 | 0.00 | 5.00e+00 | ▇▁▁▁▁ |
| expected_goals_xg | 0 | 1 | 0.02 | 0.07 | 0.00 | 0.00 | 0.00 | 0.00 | 2.31e+00 | ▇▁▁▁▁ |
| expected_assists_xa | 0 | 1 | 0.02 | 0.07 | 0.00 | 0.00 | 0.00 | 0.00 | 2.20e+00 | ▇▁▁▁▁ |
| key_passes | 0 | 1 | 0.47 | 0.94 | 0.00 | 0.00 | 0.00 | 1.00 | 8.00e+00 | ▇▁▁▁▁ |
| successful_passes | 0 | 1 | 15.47 | 18.75 | 0.00 | 0.00 | 9.00 | 27.00 | 9.70e+01 | ▇▂▁▁▁ |
| total_passes | 0 | 1 | 19.18 | 22.62 | 0.00 | 0.00 | 11.00 | 33.00 | 1.00e+02 | ▇▃▂▁▁ |
| pass_accuracy | 0 | 1 | 0.81 | 0.07 | 0.42 | 0.76 | 0.82 | 0.86 | 9.70e-01 | ▁▁▃▇▃ |
| dribbles_attempted | 0 | 1 | 0.60 | 1.15 | 0.00 | 0.00 | 0.00 | 1.00 | 1.00e+01 | ▇▁▁▁▁ |
| successful_dribbles | 0 | 1 | 0.16 | 0.50 | 0.00 | 0.00 | 0.00 | 0.00 | 6.00e+00 | ▇▁▁▁▁ |
| crosses | 0 | 1 | 0.45 | 0.86 | 0.00 | 0.00 | 0.00 | 1.00 | 8.00e+00 | ▇▁▁▁▁ |
| successful_crosses | 0 | 1 | 0.02 | 0.14 | 0.00 | 0.00 | 0.00 | 0.00 | 2.00e+00 | ▇▁▁▁▁ |
| tackles | 0 | 1 | 0.80 | 1.34 | 0.00 | 0.00 | 0.00 | 1.00 | 8.00e+00 | ▇▂▁▁▁ |
| interceptions | 0 | 1 | 0.63 | 1.13 | 0.00 | 0.00 | 0.00 | 1.00 | 7.00e+00 | ▇▁▁▁▁ |
| clearances | 0 | 1 | 0.81 | 1.53 | 0.00 | 0.00 | 0.00 | 1.00 | 1.20e+01 | ▇▁▁▁▁ |
| blocks | 0 | 1 | 0.23 | 0.56 | 0.00 | 0.00 | 0.00 | 0.00 | 4.00e+00 | ▇▁▁▁▁ |
| aerial_duels_won | 0 | 1 | 0.75 | 1.17 | 0.00 | 0.00 | 0.00 | 1.00 | 7.00e+00 | ▇▁▁▁▁ |
| aerial_duels_lost | 0 | 1 | 0.36 | 0.71 | 0.00 | 0.00 | 0.00 | 1.00 | 5.00e+00 | ▇▁▁▁▁ |
| recoveries | 0 | 1 | 1.39 | 2.02 | 0.00 | 0.00 | 0.00 | 2.00 | 1.20e+01 | ▇▂▁▁▁ |
| defensive_actions | 0 | 1 | 2.47 | 3.64 | 0.00 | 0.00 | 0.00 | 4.00 | 2.30e+01 | ▇▂▁▁▁ |
| fouls_committed | 0 | 1 | 0.42 | 0.77 | 0.00 | 0.00 | 0.00 | 1.00 | 5.00e+00 | ▇▁▁▁▁ |
| fouls_suffered | 0 | 1 | 0.32 | 0.65 | 0.00 | 0.00 | 0.00 | 0.00 | 4.00e+00 | ▇▂▁▁▁ |
| yellow_cards | 0 | 1 | 0.10 | 0.30 | 0.00 | 0.00 | 0.00 | 0.00 | 1.00e+00 | ▇▁▁▁▁ |
| red_cards | 0 | 1 | 0.01 | 0.07 | 0.00 | 0.00 | 0.00 | 0.00 | 1.00e+00 | ▇▁▁▁▁ |
| offsides | 0 | 1 | 0.09 | 0.32 | 0.00 | 0.00 | 0.00 | 0.00 | 4.00e+00 | ▇▁▁▁▁ |
| saves | 0 | 1 | 0.12 | 0.70 | 0.00 | 0.00 | 0.00 | 0.00 | 1.10e+01 | ▇▁▁▁▁ |
| save_percentage | 0 | 1 | 0.03 | 0.14 | 0.00 | 0.00 | 0.00 | 0.00 | 1.00e+00 | ▇▁▁▁▁ |
| punches | 0 | 1 | 0.03 | 0.21 | 0.00 | 0.00 | 0.00 | 0.00 | 3.00e+00 | ▇▁▁▁▁ |
| clean_sheet | 0 | 1 | 0.01 | 0.10 | 0.00 | 0.00 | 0.00 | 0.00 | 1.00e+00 | ▇▁▁▁▁ |
| goals_conceded | 0 | 1 | 0.05 | 0.33 | 0.00 | 0.00 | 0.00 | 0.00 | 7.00e+00 | ▇▁▁▁▁ |
| penalty_saves | 0 | 1 | 0.00 | 0.06 | 0.00 | 0.00 | 0.00 | 0.00 | 1.00e+00 | ▇▁▁▁▁ |
| distance_covered_km | 0 | 1 | 4.00 | 4.02 | 0.00 | 0.00 | 3.00 | 7.90 | 1.40e+01 | ▇▂▃▃▁ |
| sprint_distance_km | 0 | 1 | 0.46 | 0.48 | 0.00 | 0.00 | 0.30 | 0.90 | 2.00e+00 | ▇▂▃▁▁ |
| top_speed_kmh | 0 | 1 | 18.65 | 16.02 | 0.00 | 0.00 | 29.90 | 32.70 | 3.70e+01 | ▆▁▁▁▇ |
| accelerations | 0 | 1 | 10.04 | 10.58 | 0.00 | 0.00 | 6.00 | 20.00 | 4.40e+01 | ▇▂▃▁▁ |
| decelerations | 0 | 1 | 8.85 | 9.39 | 0.00 | 0.00 | 6.00 | 17.00 | 4.00e+01 | ▇▂▃▁▁ |
| stamina_score | 0 | 1 | 81.89 | 10.77 | 50.00 | 74.60 | 82.60 | 90.00 | 9.90e+01 | ▁▂▆▇▆ |
| player_rating | 0 | 1 | 3.63 | 3.16 | 0.00 | 0.00 | 5.50 | 6.40 | 9.40e+00 | ▇▁▂▇▁ |
| performance_score | 0 | 1 | 36.83 | 31.07 | 0.00 | 0.70 | 54.10 | 64.10 | 9.77e+01 | ▇▁▃▇▁ |
| offensive_contribution | 0 | 1 | 44.72 | 24.87 | 1.00 | 25.67 | 41.60 | 60.80 | 9.90e+01 | ▅▇▇▃▃ |
| defensive_contribution | 0 | 1 | 52.73 | 23.90 | 5.00 | 33.10 | 49.90 | 71.30 | 9.90e+01 | ▃▇▇▅▅ |
| possession_impact | 0 | 1 | 2.85 | 4.23 | 0.00 | 0.00 | 1.10 | 4.00 | 3.70e+01 | ▇▁▁▁▁ |
| pressure_resistance | 0 | 1 | 60.32 | 20.23 | 15.00 | 43.70 | 57.00 | 75.10 | 9.90e+01 | ▁▇▇▅▅ |
| creativity_score | 0 | 1 | 46.14 | 22.42 | 5.00 | 29.80 | 40.50 | 59.00 | 9.90e+01 | ▂▇▅▂▂ |
| consistency_score | 0 | 1 | 63.75 | 19.86 | 25.00 | 46.50 | 61.10 | 78.90 | 9.90e+01 | ▂▇▅▅▅ |
| clutch_performance_score | 0 | 1 | 55.57 | 13.66 | 15.00 | 46.40 | 55.00 | 63.90 | 9.90e+01 | ▁▅▇▃▁ |
| total_goals_tournament | 0 | 1 | 0.64 | 1.09 | 0.00 | 0.00 | 0.00 | 1.00 | 1.00e+01 | ▇▁▁▁▁ |
| total_assists_tournament | 0 | 1 | 0.61 | 0.93 | 0.00 | 0.00 | 0.00 | 1.00 | 8.00e+00 | ▇▁▁▁▁ |
| total_minutes_tournament | 0 | 1 | 272.30 | 116.81 | 0.00 | 184.00 | 268.00 | 359.00 | 6.15e+02 | ▂▇▇▅▁ |
| player_of_match_awards | 0 | 1 | 0.03 | 0.21 | 0.00 | 0.00 | 0.00 | 0.00 | 4.00e+00 | ▇▁▁▁▁ |
| tournament_rating | 0 | 1 | 3.63 | 3.16 | 0.00 | 0.00 | 5.40 | 6.40 | 9.50e+00 | ▇▁▂▇▁ |

Pero vamos un paso más allá, revisando sólo lo que nos podría interesar:

``` r
library(dplyr)

df %>%
  group_by(position) %>%
  summarise(
    count = n(),
    mean_val = mean(age, na.rm = TRUE),
    sd_val = sd(age, na.rm = TRUE),
    median_val = median(age, na.rm = TRUE),
    IQR_val = IQR(age, na.rm = TRUE)
  )
```

    ## # A tibble: 4 × 6
    ##   position   count mean_val sd_val median_val IQR_val
    ##   <chr>      <int>    <dbl>  <dbl>      <dbl>   <dbl>
    ## 1 Defender   18900     26.7   3.87         27       5
    ## 2 Forward    12600     25.5   3.89         25       5
    ## 3 Goalkeeper  6300     28.7   4.03         28       7
    ## 4 Midfielder 16800     25.6   4.03         25       5

Otro ejemplo:

``` r
df %>%
  group_by(team) %>%
  summarise(
    count = n(),
    mean_val = mean(height_cm, na.rm = TRUE),
    sd_val = sd(height_cm, na.rm = TRUE),
    median_val = median(height_cm, na.rm = TRUE),
    IQR_val = IQR(height_cm, na.rm = TRUE)
  )
```

    ## # A tibble: 48 × 6
    ##    team      count mean_val sd_val median_val IQR_val
    ##    <chr>     <int>    <dbl>  <dbl>      <dbl>   <dbl>
    ##  1 Algeria    1066     183.   6.13       183        9
    ##  2 Argentina   884     181.   5.67       182.      10
    ##  3 Australia  1222     182.   7.17       182        6
    ##  4 Austria     858     181.   6.11       182        8
    ##  5 Belgium    1326     182.   8.14       184.      11
    ##  6 Brazil     1170     183.   5.42       183        7
    ##  7 Cameroon   1144     180.   6.05       180        8
    ##  8 Canada     1248     181.   6.26       182.       8
    ##  9 Chile       962     183.   6.19       186.       7
    ## 10 Colombia   1196     183    5.96       182       10
    ## # ℹ 38 more rows

``` r
df %>%
  group_by(team) %>%
  summarise(
    count = n(),
    mean_val = mean(weight_kg, na.rm = TRUE),
    sd_val = sd(weight_kg, na.rm = TRUE),
    median_val = median(weight_kg, na.rm = TRUE),
    IQR_val = IQR(weight_kg, na.rm = TRUE)
  )
```

    ## # A tibble: 48 × 6
    ##    team      count mean_val sd_val median_val IQR_val
    ##    <chr>     <int>    <dbl>  <dbl>      <dbl>   <dbl>
    ##  1 Algeria    1066     76.0   4.15       77         7
    ##  2 Argentina   884     75.2   3.05       75.5       5
    ##  3 Australia  1222     75.7   4.93       76         4
    ##  4 Austria     858     74.3   3.36       74.5       6
    ##  5 Belgium    1326     75.3   4.08       76         5
    ##  6 Brazil     1170     76.1   3.62       76         5
    ##  7 Cameroon   1144     74.8   4.34       74         6
    ##  8 Canada     1248     75.2   3.81       75         5
    ##  9 Chile       962     76.5   3.50       76.5       5
    ## 10 Colombia   1196     77.6   3.67       78         5
    ## # ℹ 38 more rows
