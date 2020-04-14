
# COVID-19 en España

# Índice
1. [Project Title](#Project_Title)
2. [Descripción](#Descripción)
3. [Analisis visual de la evolución del COVID-19](#Analisis_visual)
    1. [Datos de España](#Dato_España)
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

### 3.1) Datos de España - **actualizados a 14/04/2020** <a name="Dato_España"></a>

En las siguientes gráficas se analiza la evolución del COVID-19 en España:

![Evolución de fallecidos a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/fallecidos.png)

![Evolución de casos a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/casos.png)

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/recuperados.png)


### 3.2) Comparación entre paises de la evolución del COVID-19 - **actualizados a 14/04/2020** <a name="paises"></a>

![Evolución de recuperados a partir del primer día con 10 fallecidos acumulados](/resources/imagenes/paises.png)


### 3.3) Datos de interes Comunidades autonomas - **actualizados a 14/04/2020** <a name="CCAA"></a>

#### 3.3.1) Número de tests PCR realizados en Cataluña - **actualizados a 14/04/2020** <a name="Test_cat"></a>

Cataluña he hecho publico el número de tests PCR realizados y los que han sido positivos y negativos

 Dato| Número
------------ | -------------
Número total de tests | 70446
Positivos | 29396
Negativos | 41050

![Evolución del número de tests PCR hechos en Cataluña al día: totales, positivos y negativos](/resources/imagenes/tests.png)


#### 3.3.2) Como influye el nivel de renta en la probabilidad de contagiarse en la comunidad de Madrid - **actualizados a 14/04/2020** <a name="Renta_mad"></a>

A partir de los datos que ha hecho públicos la comunidad de Madrid de la tasa de incidencia acumulada por municipio/distrito y la renta neta de los municipios y distritos de Madrid, se ha hecho una analisis ANOVA de cómo influye el nivel de renta en la tasa de incidencia. Los municipios/distritos se han estratificado de la siguiente forma:

Renta | Categorización
------------ | -------------
Renta Baja	| < 85% de la mediana
Renta Media-Baja	| > 85% < 100% de la mediana
Renta Media-Media	| > 100% < 125% de la mediana
Renta Media-Alta	| > 125% < 150% de la mediana
Renta Alta	| > 150% de la mediana


Ejecutamos el test ANOVA para la mayor fecha disponible al ser datos acumulados. Observamos que a nivel de significación del 95% la varibale renta es significativa a la hora de contiagiarse de COVID-19 en la comunidad de Madrid:

```python
from statsmodels.formula.api import ols
data_mad= data_mad[data_mad["fecha_informe"] == data_mad["fecha_informe"].max()]
results = ols('tasa_incidencia_acumulada_total ~ data_mad["Renta Categorical"] ', data=data_mad).fit()
table = sm.stats.anova_lm(results)
table
```



<div>
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
      <td>6.476488e+05</td>
      <td>161912.189810</td>
      <td>2.684711</td>
      <td>0.036816</td>
    </tr>
    <tr>
      <th>Residual</th>
      <td>84.0</td>
      <td>5.065955e+06</td>
      <td>60308.984845</td>
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
  <th>Dep. Variable:</th>    <td>tasa_incidencia_acumulada_total</td> <th>  R-squared:         </th> <td>   0.113</td>
</tr>
<tr>
  <th>Model:</th>                          <td>OLS</td>               <th>  Adj. R-squared:    </th> <td>   0.071</td>
</tr>
<tr>
  <th>Method:</th>                    <td>Least Squares</td>          <th>  F-statistic:       </th> <td>   2.685</td>
</tr>
<tr>
  <th>Date:</th>                    <td>Sun, 12 Apr 2020</td>         <th>  Prob (F-statistic):</th>  <td>0.0368</td> 
</tr>
<tr>
  <th>Time:</th>                        <td>13:38:51</td>             <th>  Log-Likelihood:    </th> <td> -613.53</td>
</tr>
<tr>
  <th>No. Observations:</th>             <td>    89</td>              <th>  AIC:               </th> <td>   1237.</td>
</tr>
<tr>
  <th>Df Residuals:</th>                 <td>    84</td>              <th>  BIC:               </th> <td>   1250.</td>
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
  <th>Intercept</th>                                          <td>  640.3465</td> <td>   59.562</td> <td>   10.751</td> <td> 0.000</td> <td>  521.902</td> <td>  758.791</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Baja]</th>        <td>  124.4390</td> <td>   95.027</td> <td>    1.310</td> <td> 0.194</td> <td>  -64.534</td> <td>  313.412</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Media-Alta]</th>  <td>  -54.4895</td> <td>   90.481</td> <td>   -0.602</td> <td> 0.549</td> <td> -234.420</td> <td>  125.441</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Media-Baja]</th>  <td>  -41.2813</td> <td>   78.548</td> <td>   -0.526</td> <td> 0.601</td> <td> -197.482</td> <td>  114.919</td>
</tr>
<tr>
  <th>data_mad["Renta Categorical"][T.Renta Media-Media]</th> <td> -153.8337</td> <td>   77.201</td> <td>   -1.993</td> <td> 0.050</td> <td> -307.356</td> <td>   -0.312</td>
</tr>
</table>



### 3.4) Datos globales - **actualizados a 14/04/2020** <a name="globales"></a>

Tabla por fecha y país con los 10 días con más nuevos casos. Los 10 días con más nuevos casos han sido en Estados Unidos:

Fecha | País| Incremento de casos
------------ | -------------| -------------
2020-04-10	| US	| 35098
2020-04-04	| US	| 33267
2020-04-08	| US	| 32829
2020-04-09	| US	| 32385
2020-04-03	| US	| 31824
2020-04-02	| US	| 30390
2020-04-11	| US	| 29861
2020-04-06	| US	| 29595
2020-04-07	| US	| 29556
2020-04-12	| US	| 28917

Tabla por fecha y país con los 10 días con mayor incremento de fallecidos:

Fecha | País| Incremento de fallecidos
------------ | -------------| -------------
2020-04-10	|US	|2108
2020-04-08	|US	|1973
2020-04-07	|US	|1939
2020-04-11	|US	|1877
2020-04-09	|US	|1783
2020-04-12	|US	|1557
2020-04-13	|US	|1509
2020-04-07	|France	|1417
2020-04-02	|France	|1355
2020-04-09	|France	|1341



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
14/04/2020 | 544 |  
15/04/2020 | 464 |  
16/04/2020 | 382 |  


## 5. Fuentes <a name="Fuentes"></a>

[Web del Ministerio de Sanidad](https://covid19.isciii.es/)

[Web de la Johns Hopkins University](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/)

[Datos Cataluña transparenciacatalunya](https://analisi.transparenciacatalunya.cat/)

[Datos Madrid](https://datos.comunidad.madrid/)



## Contacto

[Linkedin](https://www.linkedin.com/in/guillermo-arg%C3%BCello-gonz%C3%A1lez-599437b9?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_contact_details%3BNofztgFzReqI491IQYH5Zg%3D%3D)
