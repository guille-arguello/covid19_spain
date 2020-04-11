# COVID-19 en España

# Índice
1. [Project Title](#Project_Title)
2. [Descripción](#Descripción)
3. [Analisis visual de la evolución del COVID-19](#Analisis_visual)
   	1. [Datos de España](#Dato_España)
	2. [Comparación paises](#paises)
	3. [Datos de Comunidades Autonomas](#CCAA)

4. [Predicción de la evolución del COVID19 en España](#Predicción)
5. [Fuentes](#Fuentes)


## 1. Project Title <a name="Project_Title"></a>
COVID-19 en España - Análisis con python

## 2. Descripción <a name="Descripción"></a>
En este repositorio voy a ir incluyendo software para el análisis de los datos del Covid-19 en España. En esta primera versión el notebook de Jupyter está preparado para leer directamente los datos más recientes de las webs del Ministerio de Sanidad y de la Universidad Johns Hopkins. Se procesan los datos y se preparan para su posterior análisis gráfico. Se presentan diferentes visualizaciones de la evolución de la enfermedad en España. Cuando sólo se utilizan datos de España se usa el dataset del Ministerio, cuando se compara la evolución con otros países se utiliza la información de la Johns Hopkins University. Se incluye predicción de la evolución del número de fallecidos y casos basado en modelo ARIMA


## 3. Analisis visual de la evolución del COVID-19 <a name="Analisis_visual"></a>

### 3.1) Datos de España - **actualizados a 11/04/2020** <a name="Dato_España"></a>

![Evolución de fallecidos a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/fallecidos.png)

![Evolución de casos a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/casos.png)

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/recuperados.png)


### 3.2) Comparación entre paises de la evolución del COVID-19 - **actualizados a 11/04/2020** <a name="paises"></a>

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/paises.png)


### 3.3) Datos de interes Comunidades autonomas - **actualizados a 11/04/2020** <a name="CCAA"></a>

Cataluña he hecho publico el número de tests PCR realizados y los que han sido positovos y negativos

 Dato| Número
------------ | -------------
Número total de tests | 70446
Positivos | 29396
Negativos | 41050


![Evolución del número de tests PCR hechos en Cataluña al día: totales, positivos y negativos](/resources/imagenes/tests.png)


## 4. Predicción de la evolución del COVID19 en España <a name="Predicción"></a>

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/predicción.png)

Está predición está hecha sobre la media movil de fallecidos semanal, según lo cual el incremento diario de fallecidos los próximos días sería:

Fecha | Incremento de fallecidos
------------ | -------------
11/04/2020 | 734
12/04/2020 | 434
13/04/2020 | 582

## 5. Fuentes <a name="Fuentes"></a>

[Web del Ministerio de Sanidad](https://covid19.isciii.es/)

[Web de la Johns Hopkins University](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/)



