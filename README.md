
# COVID-19 en España

# Índice
1. [Project Title](#Project_Title)
2. [Descripción](#Descripción)
3. [Analisis visual de la evolución del COVID-19](#Analisis_visual)
    1. [Datos de España](#Dato_España)
		1. [Incrementos diarios](#incrementos)
		2. [Totales vs diarios](#tot_vs_dia)
		3. [Incrementos porcentuales](#porcentuales)
    2. [Comparación paises](#paises)
    3. [Datos de Comunidades Autonomas](#CCAA)
		1. [Número de tests PCR realizados en Cataluña](#Test_cat)
		2. [Como influye el nivel de renta en la probabilidad de contagiarse en la comunidad de Madrid](#Renta_mad)
	4. [Datos globales](#globales)

4. [Predicción de la evolución del COVID19 en España](#Predicción)
5. [Fuentes](#Fuentes)


## 1. Project Title <a name="Project_Title"></a>
COVID-19 en España - Análisis con python

## 2. Descripción <a name="Descripción"></a>
En este repositorio voy a ir incluyendo software para el análisis de los datos del Covid-19 en España. En esta primera versión el notebook de Jupyter está preparado para leer directamente los datos más recientes de las webs del Ministerio de Sanidad y de la Universidad Johns Hopkins. Se procesan los datos y se preparan para su posterior análisis gráfico. Se presentan diferentes visualizaciones de la evolución de la enfermedad en España. Cuando sólo se utilizan datos de España se usa el dataset del Ministerio, cuando se compara la evolución con otros países se utiliza la información de la Johns Hopkins University. Se incluye predicción de la evolución del número de fallecidos y casos basado en modelo ARIMA


## 3. Analisis visual de la evolución del COVID-19 <a name="Analisis_visual"></a>

### 3.1) Datos de España - **actualizados a 29/04/2020** <a name="Dato_España"></a>

#### 3.1.1) Incrementos diarios **actualizados a 29/04/2020** <a name="incrementos"></a>


En las siguientes gráficas se analiza la evolución del COVID-19 en España, incrementos diarios de las cifras de fallecidos, casos y recuperados, así como de casos activos.

![Evolución de fallecidos a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/fallecidos.png)

![Evolución de casos a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/casos.png)

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/recuperados.png)

![Evolución de casos activos: acumulado de casos menos el acumulado de falecidos y recuperados](/resources/imagenes/casos_activos.png)

#### 3.1.2) Totales VS diarios **actualizados a 29/04/2020** <a name="tot_vs_dia"></a>

A continuación tenemos otra forma de ver los datos, la evolución de los casos totales respecto a los diarios, con un ajuste polinomial de orden 5 de los puntos, simplemente para observar la tendencia, sin interés predictivo.

![Fallecidos VS incremento de fallecidos día anterior](/resources/imagenes/fallecidos_VS_incremento.png)

![Casos VS incremento de casos día anterior](/resources/imagenes/casos_VS_incremento.png)


#### 3.1.3) Incrementos porcentuales **actualizados a 29/04/2020** <a name="porcentuales"></a>

A continuación podemos ver el incremento porcentual de casos y fallecidos del dia respecto al total de casos. Como se puede observar lleva estancado días en torno al 3%.

![Incrementos porcentual](/resources/imagenes/incremento_porcentual.png)


### 3.2) Comparación entre paises de la evolución del COVID-19 - **actualizados a 29/04/2020** <a name="paises"></a>

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/paises.png)


### 3.3) Datos de interes Comunidades autonomas - **actualizados a 29/04/2020** <a name="CCAA"></a>

#### 3.3.1) Número de tests PCR realizados en Cataluña - **actualizados a 29/04/2020** <a name="Test_cat"></a>

Cataluña he hecho publico el número de tests PCR realizados y los que han sido positivos y negativos

| Dato| Número|
|------------ | -------------|
|Número total de tests | 141364 |
|Positivos | 47725 |
|Negativos | 93639 |

![Evolución del número de tests PCR hechos en Cataluña al día: totales, positivos y negativos](/resources/imagenes/tests.png)


#### 3.3.2) Como influye el nivel de renta en la probabilidad de contagiarse en la comunidad de Madrid - **actualizados a 22/04/2020** <a name="Renta_mad"></a>

A partir de los datos que ha hecho públicos la comunidad de Madrid de la tasa de incidencia acumulada por municipio/distrito y la renta neta de los municipios y distritos de Madrid, se ha hecho una analisis ANOVA de cómo influye el nivel de renta en la tasa de incidencia. Los municipios/distritos se han estratificado de la siguiente forma:

|Renta | Categorización|
|------------ | -------------|
|Renta Baja	| < 85% de la mediana|
|Renta Media-Baja	| > 85% < 100% de la mediana|
|Renta Media-Media	| > 100% < 125% de la mediana|
|Renta Media-Alta	| > 125% < 150% de la mediana|
|Renta Alta	| > 150% de la mediana|


Ejecutamos el test ANOVA para la mayor fecha disponible al ser datos acumulados. Observamos que a nivel de significación del 95% la varibale renta es significativa a la hora de contiagiarse de COVID-19 en la comunidad de Madrid:

```python
from statsmodels.formula.api import ols
data_mad= data_mad[data_mad["fecha_informe"] == data_mad["fecha_informe"].max()]
results = ols('tasa_incidencia_acumulada_total ~ data_mad["Renta Categorical"] ', data=data_mad).fit()
table = sm.stats.anova_lm(results)
table
```



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>df</th>
      <th>sum_sq</th>
      <th>mean_sq</th>
      <th>F</th>
      <th>PR(&gt;F)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>data_mad["Renta Categorical"]</th>
      <td>4.0</td>
      <td>8.338377e+05</td>
      <td>208459.432723</td>
      <td>2.816715</td>
      <td>0.029552</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>93.0</td>
      <td>6.882743e+06</td>
      <td>74007.986294</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
results.summary()
```



<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>    <td>tasa_incidencia_acumulada_total</td> <th>  R-squared:         </th> <td>   0.108</td>
</tr>
<tr>
  <th>Model:</th>                          <td>OLS</td>               <th>  Adj. R-squared:    </th> <td>   0.070</td>
</tr>
<tr>
  <th>Method:</th>                    <td>Least Squares</td>          <th>  F-statistic:       </th> <td>   2.817</td>
</tr>
<tr>
  <th>Date:</th>                    <td>Wed, 22 Apr 2020</td>         <th>  Prob (F-statistic):</th>  <td>0.0296</td> 
</tr>
<tr>
  <th>Time:</th>                        <td>20:33:52</td>             <th>  Log-Likelihood:    </th> <td> -685.87</td>
</tr>
<tr>
  <th>No. Observations:</th>             <td>    98</td>              <th>  AIC:               </th> <td>   1382.</td>
</tr>
<tr>
  <th>Df Residuals:</th>                 <td>    93</td>              <th>  BIC:               </th> <td>   1395.</td>
</tr>
<tr>
  <th>Df Model:</th>                     <td>     4</td>              <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>             <td>nonrobust</td>            <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
                           <td></td>                             <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th>                                          <td>  791.6047</td> <td>   65.980</td> <td>   11.998</td> <td> 0.000</td> <td>  660.581</td> <td>  922.629</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Baja]</th>        <td>   90.3053</td> <td>  102.571</td> <td>    0.880</td> <td> 0.381</td> <td> -113.380</td> <td>  293.990</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Media-Alta]</th>  <td> -123.4083</td> <td>   98.182</td> <td>   -1.257</td> <td> 0.212</td> <td> -318.378</td> <td>   71.562</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Media-Baja]</th>  <td> -129.6947</td> <td>   83.645</td> <td>   -1.551</td> <td> 0.124</td> <td> -295.798</td> <td>   36.408</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Media-Media]</th> <td> -185.9529</td> <td>   84.229</td> <td>   -2.208</td> <td> 0.030</td> <td> -353.214</td> <td>  -18.692</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 4.162</td> <th>  Durbin-Watson:     </th> <td>   1.481</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.125</td> <th>  Jarque-Bera (JB):  </th> <td>   4.138</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 0.495</td> <th>  Prob(JB):          </th> <td>   0.126</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 2.813</td> <th>  Cond. No.          </th> <td>    6.36</td>
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.




### 3.4) Datos globales - **actualizados a 18/04/2020** <a name="globales"></a>

Tabla por fecha y país con los 10 días con más nuevos casos. Los 10 días con más nuevos casos han sido en Estados Unidos:

|Fecha | País| Incremento de casos|
|------------ | -------------| -------------|
|2020-04-10	| US	| 35098|
|2020-04-04	| US	| 33267|
|2020-04-08	| US	| 32829|
|2020-04-09	| US	| 32385|
|2020-04-17	| US	| 31905|
|2020-04-03	| US	| 31824|
|2020-04-02	| US	| 30390|
|2020-04-11	| US	| 29861|
|2020-04-06	| US	| 29595|
|2020-04-07	| US	| 29556|

Tabla por fecha y país con los 10 días con mayor incremento de fallecidos:

|Fecha | País| Incremento de fallecidos|
|------------ | -------------| -------------|
|2020-04-16|US|4591|
|2020-04-17|US|3857|
|2020-04-15|US|2494|
|2020-04-14|US|2303|
|2020-04-10|US|2108|
|2020-04-08|US|1973|
|2020-04-07|US|1939|
|2020-04-11|US|1876|
|2020-04-09|US|1783|
|2020-04-12|US|1557|


¿Qué paises están en el inicio de la curva? Es decir, vamos a ver los paises que doblan su número de fallecidos y casos más rápido, para paises que tengan más de 25 fallecidos acumulados **actualizados a 21/04/2020**



<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
	  <th>Fecha</th>
      <th>Country/Region</th>
      <th>Fallecidos</th>
      <th>Incremento porcentual de fallecidos respecto al total</th>
      <th>Días que tarda en doblar fallecidos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2020-04-21</td>
      <td>Finland</td>
      <td>141</td>
      <td>30.496</td>
      <td>3.279</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Pakistan</td>
      <td>201</td>
      <td>12.438</td>
      <td>8.040</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Russia</td>
      <td>456</td>
      <td>11.184</td>
      <td>8.941</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Sweden</td>
      <td>1765</td>
      <td>10.482</td>
      <td>9.541</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Japan</td>
      <td>263</td>
      <td>10.266</td>
      <td>9.741</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>India</td>
      <td>645</td>
      <td>8.217</td>
      <td>12.170</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Bangladesh</td>
      <td>110</td>
      <td>8.182</td>
      <td>12.222</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Peru</td>
      <td>484</td>
      <td>8.058</td>
      <td>12.410</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Argentina</td>
      <td>147</td>
      <td>7.483</td>
      <td>13.364</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Panama</td>
      <td>136</td>
      <td>7.353</td>
      <td>13.600</td>
    </tr>
  </tbody>
</table>
</div>


A continuación se muestran los 10 paises con más de 100 caos que menos tardan en doblar su número de casos.

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
	  <th>Fecha</th>
      <th>Country/Region</th>
      <th>Casos</th>
      <th>Incremento porcentual de casos respecto al total</th>
      <th>Días que tarda en doblar casos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2020-04-21</td>
      <td>Gabon</td>
      <td>156</td>
      <td>23.077</td>
      <td>4.333</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Somalia</td>
      <td>286</td>
      <td>17.133</td>
      <td>5.837</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Bangladesh</td>
      <td>3382</td>
      <td>12.833</td>
      <td>7.793</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Singapore</td>
      <td>9125</td>
      <td>12.175</td>
      <td>8.213</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Pakistan</td>
      <td>9565</td>
      <td>11.992</td>
      <td>8.339</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Russia</td>
      <td>52763</td>
      <td>10.693</td>
      <td>9.352</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Djibouti</td>
      <td>945</td>
      <td>10.476</td>
      <td>9.545</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Venezuela</td>
      <td>285</td>
      <td>10.175</td>
      <td>9.828</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Saudi Arabia</td>
      <td>11631</td>
      <td>9.862</td>
      <td>10.140</td>
    </tr>
    <tr>
      <td>2020-04-21</td>
      <td>Guinea</td>
      <td>688</td>
      <td>9.593</td>
      <td>10.424</td>
    </tr>
  </tbody>
</table>
</div>


## 4. Predicción de la evolución del COVID19 en España <a name="Predicción"></a>


```python
mod = sm.tsa.statespace.SARIMAX(np.log(data_es["Diferencia fallecidos dia anterior"]),
                                order=(1, 1, 1),
                                seasonal_order=(1, 1, 0, 12),
                                enforce_stationarity=False,
                                enforce_invertibility=False)
results = mod.fit()
print(results.summary())
```

                                           SARIMAX Results                                        
    ==============================================================================================
    Dep. Variable:     Diferencia fallecidos dia anterior   No. Observations:                   37
    Model:                SARIMAX(1, 1, 1)x(1, 1, [], 12)   Log Likelihood                   3.297
    Date:                                Tue, 14 Apr 2020   AIC                              1.405
    Time:                                        17:13:23   BIC                              2.997
    Sample:                                    03-08-2020   HQIC                             0.402
                                             - 04-13-2020                                         
    Covariance Type:                                  opg                                         
    ==============================================================================
                     coef    std err          z      P>|z|      [0.025      0.975]
    ------------------------------------------------------------------------------
    ar.L1          0.8252      0.162      5.083      0.000       0.507       1.143
    ma.L1         -1.0000    4.3e+04  -2.33e-05      1.000   -8.43e+04    8.43e+04
    ar.S.L12      -0.0095      0.065     -0.146      0.884      -0.137       0.118
    sigma2         0.0268   1153.003   2.33e-05      1.000   -2259.817    2259.871
    ===================================================================================
    Ljung-Box (Q):                        8.34   Jarque-Bera (JB):                 3.18
    Prob(Q):                              0.60   Prob(JB):                         0.20
    Heteroskedasticity (H):               0.28   Skew:                             1.24
    Prob(H) (two-sided):                  0.24   Kurtosis:                         3.88
    ===================================================================================
    
    Warnings:
    [1] Covariance matrix calculated using the outer product of gradients (complex-step).
    

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/predicción.png)

Está predición está hecha sobre el incremento de fallecidos diario, por lo tanto el dato observado se publica en D+1.

Fecha | Incremento de fallecidos predicho | Incremento de fallecidos observado
------------ | ------------- | -------------
13/04/2020 | 555 | 567 
14/04/2020 | 544 | 523 
15/04/2020 | 447 | 551
16/04/2020 | 452 | 585 
17/04/2020 | 433 | 438
18/04/2020 | 506 | 356
19/04/2020 | 360 | 399 
20/04/2020 | 334 | 430 
21/04/2020 | 302 | 435
22/04/2020 | 364 | 
23/04/2020 | 420 | 

A continuación la predicción de la evolución de la media movil semanal:

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/predicción_media_movil.png)




## 5. Fuentes <a name="Fuentes"></a>

[Web del Ministerio de Sanidad](https://covid19.isciii.es/)

[Web de la Johns Hopkins University](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/)

[Datos Cataluña transparenciacatalunya](https://analisi.transparenciacatalunya.cat/)

[Datos Madrid](https://datos.comunidad.madrid/)



## Contacto

[Linkedin](https://www.linkedin.com/in/guillermo-arg%C3%BCello-gonz%C3%A1lez-599437b9?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_contact_details%3BNofztgFzReqI491IQYH5Zg%3D%3D)
