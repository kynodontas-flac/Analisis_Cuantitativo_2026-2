# Visualizaciones en R
En este archivo podrán ver de manera más comprensiva (a través de plantillas antes que código real) cómo hacer visualizaciones con la librería "ggplot2" del paquete "tidyverse".

Tengan en cuenta que estos *chunks* de código NO correrán a menos de que se reemplacen los argumentos que estén entre *brackets* ("[]") en minúsculas y sin tildes; se encuentran EN MAYÚSCULAS para que sean más fáciles de identificar.

**DEBEN TENER EN CUENTA:
1. **PONER SUS PROPIOS ARGUMENTOS EN MINÚSCULAS**
2. **BORRAR LOS BRACKES ("[]")**
3. **SI ES UNA VARIABLE DE R NO PUEDEN TENER ESPACIOS Y SE VE CUANDO EL ARGUMENTO TIENE GUIONES BAJOS Y SI ES UN TÍTULO ESCRIBIRLO SÓLO EN TEXTO (CUANDO EL ARGUMENTO NO TIENE GUIONES BAJOS)**

Tengan en cuenta tener instalado "tidiverse" con
`install.packages("tidiverse")`

Igualmente, recuerden que tienen que "llamar" a la librería antes de correr sus *scripts* con
`library(ggplot2)`
#### Visualizaciones de barras:
```r
# Hacer el gráfico
[NOMBRE_DEL_GRÁFICO] <- ggplot([NOMBRE_DEL_DATASET_CARGADO], aes(x = [VARIABLE_CATEGÓRICA_1], y = [VARIABLE_NUMÉRICA_1], fill = [VARIABLE_CATEGÓRICA_1])) +
  geom_col(width = 0.6, show.legend = FALSE) +
  geom_text(aes(label = [NOMBRE_VARIABLE_NUMÉRICA_1]), vjust = -0.5, fontface = "bold") +
  scale_fill_brewer(palette = "Blues") +
  scale_y_continuous(limits = c(0, 350)) +
  labs(
    title = "[TÍTULO DEL GRÁFICO]",
    x = "[VARIABLE NUMÉRICA 1 EN TEXTO]",
    y = "[VARIABLE CATEGÓRICA 1 EN TEXTO]"
  ) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))

# Guardar imagen especificando tamaño. Son tamaños de prueba, pueden cambiar los números para hacer la visualización más larga y/o ancha. El "dpi" es estándar para impresiones.
ggsave("grafico_barras.png", plot = p, width = 7, height = 4.5, dpi = 300)
```

#### Visualizaciones de tortas
```r
# Hacer el gráfico
[NOMBRE_DEL_GRÁFICO] <- ggplot([NOMBRE_DEL_DATASET_CARGADO], aes(x = "", y = [VARIABLE_NUMÉRICA_1], fill = [VARIABLE_CATEGÓRICA_1])) +
  geom_col(width = 1, color = "white") +                   # Barras apiladas
  coord_polar(theta = "y") +                               # Transforma a coordenadas polares
  geom_text(aes(label = [VARIABLE_NUMÉRICA_1]), 
            position = position_stack(vjust = 0.5),        # Centra los números en cada porción
            color = "[NOMBRECOLORENINGLÉSSINESPACIOS]", fontface = "bold") +
  scale_fill_brewer(palette = "Blues") +
  labs(title = "[TÍTULO DEL GRÁFICO]", fill = "[VARIABLE CATEGÓRICA 1 EN TEXTO]") +
  theme_void() +                                           # Elimina ejes, fondo y cuadrícula
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))

# Guardar imagen especificando tamaño
ggsave("grafico_torta.png", plot = p, width = 6, height = 5, dpi = 300)
```

#### Visualización de histograma:
```r
# Hacer el gráfico
[NOMBRE_DEL_GRÁFICO] <- ggplot([NOMBRE_DEL_DATASET_CARGADO], aes(x = [VARIABLE_NUMÉRICA_1])) +
  geom_histogram(bins = 15, fill = "#4682b4", color = "[NOMBRECOLORENINGLÉSSINESPACIOS]") + # El argumento bins = n ajusta el número de intervalos de ser una variable continua o de querer reducir el número de mediciones discretas.
  labs(
    title = "[TÍTULO DEL GRÁFICO]",
    x = "[VARIABLE NUMÉRICA 1 EN TEXTO]",
    y = "Frecuencia"
  ) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))

# Guardar imagen especificando tamaño
ggsave("histograma.png", plot = p, width = 7, height = 4.5, dpi = 300)
```

#### Visualización de dispersión:
```r
# Hacer el gáfico
[NOMBRE_DEL_GRÁFICO] <- ggplot([NOMBRE_DEL_DATASET_CARGADO], aes(x = [VARIABLE_NUMÉRICA_1], y = [VARIABLE_NUMÉRICA_2])) +
  geom_point(color = "#2b5c8f", size = 3, alpha = 0.8) +          # Puntos
  geom_smooth(method = "lm", color = "red", se = FALSE) +         # Línea de tendencia lineal
  labs(
    title = "[TÍTULO DEL GRÁFICO]",
    x = "[VARIABLE NUMÉRICA 1 EN TEXTO]",
    y = "[VARIABLE NUMÉRICA 2 EN TEXTO]"
  ) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))

# Guardar imagen especificando tamaño
ggsave("grafico_dispersion.png", plot = p, width = 7, height = 4.5, dpi = 300)
```

#### Visualización de cajas y bigotes:
```r
# Hacer el gráfico
[NOMBRE_DEL_GRÁFICO] <-ggplot([NOMBRE_DEL_DATASET_CARGADO], aes(x = [VARIABLE_CATEGÓRICA_1], y = [VARIABLE_NUMÉRICA_1], fill = [VARIABLE_CATEGÓRICA_1])) +
  geom_boxplot(alpha = 0.7, outlier.colour = "red") +
  theme_minimal() +
  labs(
    title = "[TÍTULO DEL GRÁFICO]",
    subtitle = "[SUBTÍTULO DEL GRÁFICO]",
    x = "VARIABLE CATEGÓRICA EN TEXTO",
    y = "VARIABLE NUMÉRICA EN TEXTO"
  ) +
  theme(legend.position = "none")
  
# Guardar imagen especificando tamaño
ggsave("grafico_cajas_bigotes.png", plot = p, width = 7, height = 4.5, dpi = 300)
```
#### **EJEMPLO:**
```r
# Instalar los paquetes instalados (sólo si no los tienen)
install.packages(c("tidyverse", "readr", "psych", "skimr"))

# Llamar a la librería para importar el dataset en .csv (comma separated values)
library(readr)
# Importar el dataset. En este caso suponemos que es un dataset de una empresa de publicidad y se verá reflejado en las variables que usan
df <- read_csv("datos_de_prueba.csv")  # Este nombre RESPONDE AL NOMBRE DEL ARCHIVO QUE QUIERAN IMPORTAR Y SI NO SE ENCUENTRA EN EL WORKING DIRECTORY SE TIENE QUE ESPECIFICAR EL PATH, por ejemplo "c:/Users/Documentos/Analisis Cuantitativo/datos_de_prueba.csv"

# Vemos las primeras 5 líneas de nuestro dataset para ver que cargo bien. También podríamos dar doble click a df (o el nombre que hayamos usado) en el envirnoment
head(df)

# Podemos correr unos descriptivos básicos para familiarizarnos con las variables
summary(df)

# Si queremos descriptivos más complejos podemos correr
library(psych)
describe(df)

# O incluso
library(skimr)
skim(df)

# Hacemos un gráfico de barras
library(ggplot2)
ggplot(df, aes(x = Mes, y = Ventas, fill = Mes)) +
  geom_col(width = 0.6, show.legend = FALSE) +
  geom_text(aes(label = Ventas), vjust = -0.5, fontface = "bold") +
  scale_fill_brewer(palette = "Blues") +
  scale_y_continuous(limits = c(0, 350)) +
  labs(
    title = "Rendimiento Mensual de Ventas",
    x = "Meses del Año",
    y = "Monto (USD)"
  ) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))

# Hacemos un gráfico de torta
library(ggplot2)
ggplot(df, aes(x = "", y = Ventas, fill = Mes)) +
  geom_col(width = 1, color = "white") +                   # Barras apiladas
  coord_polar(theta = "y") +                               # Transforma a coordenadas polares
  geom_text(aes(label = Ventas), 
            position = position_stack(vjust = 0.5),        # Centra los números en cada porción
            color = "white", fontface = "bold") +
  scale_fill_brewer(palette = "Blues") +
  labs(title = "Distribución Mensual de Ventas", fill = "Meses") +
  theme_void() +                                           # Elimina ejes, fondo y cuadrícula
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))

# Hacemos un histograma
library(ggplot2)

ggplot(datos, aes(x = Ventas)) +
  geom_histogram(bins = 15, fill = "#4682b4", color = "white") + # Se ajusta el nº de intervalos
  labs(
    title = "Distribución de Ventas",
    x = "Monto de Venta (USD)",
    y = "Frecuencia"
  ) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))

# Hacemos un gráfico de dispersión
library(ggplot2)

ggplot(datos, aes(x = Publicidad, y = Ventas)) +
  geom_point(color = "#2b5c8f", size = 3, alpha = 0.8) +          # Puntos
  geom_smooth(method = "lm", color = "red", se = FALSE) +         # Línea de tendencia lineal
  labs(
    title = "Relación entre Publicidad y Ventas",
    x = "Inversión en Publicidad (USD)",
    y = "Ventas Totales (USD)"
  ) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))
  
# Hacemos un gráfico de cajas y bigotes
ggplot(datos, aes(x = Marca, y = Ventas, fill = Marca)) +
  geom_boxplot(alpha = 0.7, outlier.colour = "red") +
  theme_minimal() +
  labs(
    title = "Ventas totales (USD) por cada marca",
    x = "Marcas",
    y = "Ventas Totales (USD)"
  ) +
  theme(legend.position = "none")
```
