#### Taller para familiarizarse con R antes del taller de 1er corte

R es un lenguaje de programación creado en 1993 especializado en estadística, el análisis de datos y la visualización de datos. Aunque más recientemente se ha abierto a aplicaciones de aprendizaje automático (*machine learning*). 

Este lenguaje es parte del sistema GNU (*GNU Not Unix*) y se distribuye como software de código libre bajo licencia GPL (*GNU Public License*) y corre en Windows, MacOS, GNU/Linux y Unix.

Este lenguaje debe ser instalado en la computadora y puede correrse desde el cualquier línea de comandos *shell*, sin embargo, frecuentemente se utiliza con distintos IDE (*Integrated Development Environment*) como RStudio (el que usaremos). Pero también puede correr dentro de IDEs generalistas como Emacs, VSCode o Vim/Neovim los cuales requieren más tiempo para configurar satisfactoriamente. Hasta el momento RStudio de la firma Posit es el de mayor uso.

#### 1. Debemos instalar lo que usaremos
Entramos a la página de R de la página de [CRAN R-Project](https://cran.dcc.uchile.cl/) (he escogido el servidor de Chile pero pueden escoger otro [aquí](https://cran.r-project.org/mirrors.html)). Deberán descargar e instalar dos archivos **en el orden que describo abajo**:
* Deberán escoger entre **Dowload R for Linux, macOS o Windows según su sistema operativo**.
* Una vez en la página de descarga verán "base", "contrib", "old contrib" y "Rtools". **Descargan base**. Este el lenguaje en sí. Al momento de la escritura de este tutorial la última versión estable es R-4.6.1
* Instalan el archivo **R-4.6.1-win.exe** (para **Windows**), **R-4.6.1-arm64.pkg** (para **Mac con chip M**) o **R-4.6.1-x86_64.pkg** (para **Mac con procesador Intel**). Para Linux deben seguir la documentación dependiendo del sabor de Linux (Debian, Fedora/Redhat, Ubuntu).
* **PARA WINDOWS**: Descargan [Rtools 4.5](https://cran.dcc.uchile.cl/bin/windows/Rtools/rtools45/files/rtools45-6768-6492.exe). **Se accede a través de la misma pantalla donde estaba "base"**. Rtools permite compilar los paquetes que utilizaremos.
* **PARA WINDOWS**: Instalan **rtools45-6768-6492.exe**
* **PARA MAC**: **Instalan Xcode desde la AppStore** o abren la aplicación "terminal" y corren el comando `sudo xcode-select --install`.
* **PARA MAC**: Descargan e instalan compilador desde Fortran [gfortran-14.2-universal.pkg](https://mac.r-project.org/tools/gfortran-14.2-universal.pkg)

Nota: Si no descargan e instalan Rtools (o Xcode y el compilador de Fortran en el caso de Mac) **DESPUÉS DE INSTALAR EL base PAQUETES COMO tidiverse O ggplot 2 no podrán ser usadas**.

Si hemos seguido los pasos en el orden correcto: 1) Descargar e instalar R base, 2) Descargar e instalar Rtools para Windows o Xcode y el compilador de Fortran para Mac debemos poder correr código en R desde *shell* o línea de comandos. 

Posteriormente descargaremos [RStudio](https://docs.posit.co/ide/user/#rstudio-ide-oss-downloads) para Windows 11 o macOS 14 y posterior (dependiendo de nuestro sistema operativo)

Una vez tenemos el archivo de instalación (.exe o .dmg) lo corremos para instalar RStudio poniendo **mucha atención a las pantallas de instalación** pues buscará dónde se aloja R base para poder utilizar el lenguaje de forma correcta.

#### 2) Familiarizarse con R
Siempre que necesiten hacer código en R deberán abrir la aplicación RStudio (si abren R-4.6.1 se abrirá una línea de comandos sin interfaz gráfica).

En RStudio tenemos 4 páneles en la pantalla y una cinta superior con "file", "edit", "code", "view", etc. El panel superior izquierdo será el lugar en donde escribirán y correran el código, el panel superior derecho será el lugar donde están registradas las *variables*, el panel inferior izquierdo corresponde a la "consola" el cual es el lugar donde veremos el resultado de nuestro código y en la esquina inferior derecha tenemos un panel que puede cambiar entre el directorio de trabajo (_working directory_) o la administración de paquetes instalados.

R es un lenguaje que corre en secuencia, línea por línea, por lo cual todo debe estar en orden. Podemos correr una línea a la vez. Hagan lo siguiente: 

Escriban `x <- 3` en el panel superior izquierdo y pueden hacer click en "Run", un botón en la parte superior derecha de ese panel con una flecha verde u oprimir Ctrl+Enter (Win) o Cmd+Enter (Mac) para correr la línea. Verán que en el panel inferior izquierdo aparecerá `> x <- 3`, lo cual quiere decir que el código corrió. En el panel superior izquierdo verán que apareció una tabla similar a esta:

| Values |     |
| ------ | --- |
| x      | 3   |

Esto quiere decir que exitosamente definimos la variable x y le asignamos el valor 3.

Ahora corramos la siguiente línea de código: `my_list <- list(10, 15, 18, 22, 54, 77, 128, 352, 1268)` y veremos que la consola muestra el texto de la línea de código en azul y en el panel superior derecho "Environment" veremos otra tabla así:

| Data    |           |
| ------- | --------- |
| my_list | List of 9 |

Si damos doble click en el "List of 9" podremos ver nuestra lista.

Complejicemos un poco la lista de dos formas complementarias:

*  Escribimos el código `names(my_list) <- c("Juan", "Estela", "Horacio", "Guillermo", "Lucrecio", "Arturo", "Mirella", "Gloria", "Beatriz")` y lo corremos. Al dar doble click sobre "List of 9" podemos ver que ahora tenemos los nombres que hemos puesto en vez de los anteriores IDs que eran [1], [2]. [3], etc. Este código depende de la línea anterior por lo que completo sería así 

`my_list <- list(10, 15, 18, 22, 54, 77, 128, 352, 1268) 
`names(my_list) <- c("Juan", "Estela", "Horacio", "Guillermo", "Lucrecio", "Arturo", "Mirella", "Gloria", "Beatriz")`

* Podemos escribirlo todo en la misma línea así:

```
my_list <- list(Juan = 10,
                Estela = 15,
                Horacio = 18,
                Guillermo = 22,
                Lucrecio = 54,
                Arturo = 77,
                Mirella = 128,
                Gloria = 352,
                Beatriz = 1268)
```

La "*indentation*" (que la segunda línea no empiece al nivel de sangría de la primera) indica que pertenecen a la misma línea, pero se escribe así para mejorar la legibilidad del código. Podemos comentarlo de esta manera: 

```
# Definimos una variable
x <- 3

# Hacemos una lista con valores y le aplicamos nombres
my_list <- list(10, 15, 18, 22, 54, 77, 128, 352, 1268)
names(my_list) <- c("Juan", "Estela", "Horacio", "Guillermo", "Lucrecio", "Arturo", "Mirella", "Gloria", "Beatriz")

# Hacemos una lista y le aplicamos valores en el mismo comando
my_list <- list(Juan = 10,
                Estela = 15,
                Horacio = 18,
                Guillermo = 22,
                Lucrecio = 54,
                Arturo = 77,
                Mirella = 128,
                Gloria = 352,
                Beatriz = 1268)
```
Podemos ver que los comentarios se hacen con el numeral "#".

Pero al dar doble click y ver los datos veremos que están guardados como "*double*", el cual si recuerdan corresponde a una variable continua, pero sólo vemos números enteros, discretos, por lo que tenemos que convertirlos a "*int*" (*integer* o entero). R por defecto guarda los números como *Doubles* (también llamados *floating point*).

* Una forma es corriendo cualquiera de las dos alternativas anteriores añadiendo una ele mayúscula "L" después de cada número, por ejemplo:
`my_list <- list(Juan = 10L,
                Estela = 15L,
                Horacio = 18L,
                Guillermo = 22L,
                Lucrecio = 54L,
                Arturo = 77L,
                Mirella = 128L,
                Gloria = 352L,
                Beatriz = 1268L)` 
Cuando hacemos doble click en "List of 9" en el environment veremos que ahora son enteros, *integers*. y cuando damos click al botón desplegable a la izquierda de "my_list" en el environment veremos lo siguiente:
```
my_list | List of 9

$ Juan : int 10
$ Estela : int 15
$ Horacio : int 18
...
...
...
$ Beatriz : int 1268
```
Y así sucesivamente para todos los datos de la lista.

* La otra forma consiste en convertir la lista con más código:
`my_list <- lapply(my_list, as.integer)`

Aquí llamamos a la función **lapply** y le especificamos los argumentos, que vuelva a my_list una lista de enteros.

Si cumplimos este pequeño tutorial para familiarizarnos con el entorno y su forma de trabajar, podemos pasar a la parte más compleja, la de aplicar la estadística descriptiva que hemos discutido en clase dentro de una base de datos real. Para ello tenemos que ingresar a la [carpeta en GitHub](https://github.com/kynodontas-flac/Analisis_Cuantitativo_2026-2/tree/fc0f1478e3a1c4eed469d105d1a0556fb69a73d8/1_Corte) donde se encuentra esta misma introducción y descargar los datos: [el archivo CSV](https://github.com/kynodontas-flac/Analisis_Cuantitativo_2026-2/blame/fc0f1478e3a1c4eed469d105d1a0556fb69a73d8/1_Corte/fifa_world_cup_2026_player_performance.csv) y [el archivo R Markdown](https://github.com/kynodontas-flac/Analisis_Cuantitativo_2026-2/blob/fc0f1478e3a1c4eed469d105d1a0556fb69a73d8/1_Corte/1Corte_Descriptiva.Rmd) (.Rmd) para abrirlo en RStudio.

Nota: TENGAN EN CUENTA QUE AMBOS ARCHIVOS DEBERÍAN ESTAR EN LA MISMA CARPETA CUANDO ABRAN EL ARCHIVO R MARKDOWN EN RSTUDIO, DE LO CONTRARIO HABRÁ QUE ESPECIFICAR EL PATH EN EL CÓDIGO.

Este archivo R Markdown funciona de manera similar a un script. La diferencia radica en que podemos guardar dentro del mismo archivo los resultados de cada "chunk" de código (como se suele hacer en documentos notebook de python). Por ello, verán muchos de los resultados del código sin haberlo corrido ustedes mismos, a excepción de pequeñas imágenes que un paquete puede generar y que no subí al GitHub por limpieza del repositorio.

Tengan en cuenta que como R es secuencial (corre línea por línea), el archivo tiene un orden. Muchas cosas pueden ser corridas sin la necesidad de otros chunks anteriores, pero la mayoría necesitará correr al menos los chunks en donde se instalan los paquetes, decididamente: 
```
# Instalamos los paquetes que necesitaremos

install.packages(c("tidyverse", "lubridate", "ggplot", "tibble", "tidyr", "ggplot2", "dplyr", "stringr"), repos = "https://cloud.r-project.org")
```

```
# Este paso no es necesario, pero sobre todo cuando trabajamos en computadores de mucho uso, que tienen versiones de paquetes de R con conflictos es útil

install.packages("devtools", repos = "https://cloud.r-project.org")
devtools::install_github("r-lib/conflicted")
```

```
library(devtools)
library(conflicted)
```

```
# Resolvemos los conflictos. En este caso 1 paquete tenían el mismo comando

conflicts_prefer(dplyr::filter)
```

```
# Instalamos readr para cargar nuestro dataset
install.packages("readr", repos = "https://cloud.r-project.org")
```

```
# Cargamos nuestros datos (preferiblemente en .csv)

library(readr)

df <- read_csv("fifa_world_cup_2026_player_performance.csv")

```
```{r}
# Vemos parcialmente los datos para cerciorarnos de que cargaron bien

head(df)
```

En estos chunks se instalan paquetes, se especifica de qué servidor deben descargarse estos, se importan librerías, ante la duda (cuando dos paquetes tienen una función que se llama igual) qué paquete debería elegir RStudio, y por último la importación de los datos que trabajaremos en CSV (archivo tabular de *comma separated values*).

Estos primeros chunks son necesarios para que el resto pueda correr sin ningún orden en específico. Lo que quiere decir que después de estos todo funciona independientemente del orden (a menos que se especifique otro `instal.packages` en un chunk inmediatamente anterior a un código que usa ese paquete).

Manos a la obra. Entre más "cacharreen" RStudio, vean tutoriales y se familiaricen con la herramienta más transparente será el taller de corte, pues consistirá menos en batallar con la herramienta y más con las conclusiones que podremos sacar de los distintos datos que podrán seleccionar para hacerlo.

Recuerden que el taller de corte es el **24 de agosto** y tendrán el tiempo de la clase para completarlo.