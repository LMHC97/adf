# TITULO
## CONTEXTO DEL PROYECTO 
## Call Center
![N|Solid](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSyWEMAsO2fStc8YIGr9f-co5h7D84aCB_E0A&usqp=CAU)

Analizar las operaciones de un Call Center de un Banco, para proponer mejoras en:
* Eficiencia operativa, proponiendo mejoras operativas.
* Mejorar la satisfacción del cliente, cumpliendo los SLA comprometidos.
* Brindar una herramienta para la gestión y la toma de decisiones a los managers del Call Center.

En conclusión se solicita definir, construir y presentar un Dashboard que permita medir los niveles de calidad de servicio, eficiencia y productividad del Call Center.
Para ello, se propone que definamos los KPIs adecuados para poder medir los objetivos propuestos, y definir nuevos niveles objetivos de manera de ofrecer esos niveles de SLA a terceras partes, o generar un nuevo servicio Premium para los clientes mas importantes del banco.

**Preguntas a responder**
- ¿Cuál es el nivel de servicio para los clientes Prioritarios? 
- ¿Damos un mejor servicio que a los clientes normales?
- ¿Qué volumen de llamadas atendemos? 
- ¿Cuáles son los cuellos de botella? ¿En qué días? ¿En qué bandas horarias?
- ¿Cómo es la eficiencia y productividad de nuestros agentes?
- ¿Hay clientes recurrentes en el uso del servicio?
- ¿Cuáles son los tipos de servicio más recurrentes?
- ¿Podemos estimar la dotación necesaria para cumplir con una calidad de servicio determinada?  Ejemplo: si quiero que mi tiempo promedio de espera sea menor a 60 segundos?
### Para comenzar, se hace la importación de librerías y módulos. Inicialmente, trabajaremos con pandas y csv:
```
import pandas as pd
import csv
```
## Se importa el CSV que contiene los datos que serán limpiados y posteriormente analizados:
```
df = pd.read_csv('Call_Center_1999_DataSet.csv', delimiter=';', encoding='UTF-8') 
```
## Reporte de incidencias relacionadas con la primera vista de los datos y acciones tomadas para asegurar la integridad y limpieza de los datos:
- Se verifica la existencia de valores nulos y filas vacías que, de eliminarse, no perjudican la calidad de los datos, por lo que se procede a su eliminación.
```
df.dropna(inplace=True)
```
- Se verifica la existencia de una columna (startdate) que no contiene valores y no está descrita en el diccionario de datos, por lo que se procede a su eliminación.
```
df= df.drop('startdate', axis=1)
```
- Se identifica el valor "AA" dentro de la columna "Type" que no corresponde a un tipo de actividad indicada en el diccionario de datos, por lo que se procede a la eliminación de los valores "AA" 
```
df.drop(df[df['type']=='AA'].index,inplace=True)
```
- Se identifican outliers en los datos identificados como "Phantom" en la columna "Outcome". De acuerdo al diccionario de datos, el código "Phantom" marca datos de llamada que deben ser ignorados, por lo que se procede a su eliminación. 
```
df.drop(df[df['outcome']=='PHANTOM'].index,inplace=True)
```
- Se exporta el CSV que ha sido limpiado.
```
df.to_csv('callcentercorregido.csv', index=False, sep= ';')
```
### Análisis Exploratorio de Datos (EDA)
- Se observan los datos, tipo de datos y un ejemplo de su contenido.
```
df.head(3)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vru.line</th>
      <th>call_id</th>
      <th>customer_id</th>
      <th>priority</th>
      <th>type</th>
      <th>date</th>
      <th>vru_entry</th>
      <th>vru_exit</th>
      <th>vru_time</th>
      <th>q_start</th>
      <th>q_exit</th>
      <th>q_time</th>
      <th>outcome</th>
      <th>ser_start</th>
      <th>ser_exit</th>
      <th>ser_time</th>
      <th>server</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AA0101</td>
      <td>33116</td>
      <td>9664491.0</td>
      <td>2</td>
      <td>PS</td>
      <td>1999-01-01</td>
      <td>0:00:31</td>
      <td>0:00:36</td>
      <td>5</td>
      <td>0:00:36</td>
      <td>0:03:09</td>
      <td>153</td>
      <td>HANG</td>
      <td>0:00:00</td>
      <td>0:00:00</td>
      <td>0</td>
      <td>NO_SERVER</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AA0101</td>
      <td>33117</td>
      <td>0.0</td>
      <td>0</td>
      <td>PS</td>
      <td>1999-01-01</td>
      <td>0:34:12</td>
      <td>0:34:23</td>
      <td>11</td>
      <td>0:00:00</td>
      <td>0:00:00</td>
      <td>0</td>
      <td>HANG</td>
      <td>0:00:00</td>
      <td>0:00:00</td>
      <td>0</td>
      <td>NO_SERVER</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AA0101</td>
      <td>33118</td>
      <td>27997683.0</td>
      <td>2</td>
      <td>PS</td>
      <td>1999-01-01</td>
      <td>6:55:20</td>
      <td>6:55:26</td>
      <td>6</td>
      <td>6:55:26</td>
      <td>6:55:43</td>
      <td>17</td>
      <td>AGENT</td>
      <td>6:55:43</td>
      <td>6:56:37</td>
      <td>54</td>
      <td>MICHAL</td>
    </tr>
  </tbody>
</table>
</div>

- Se identifican las estadísticas descriptivas de la base de datos. Es importante considerar que sólo se toman en cuenta las columnas que contienen valor numérico.
```
df.describe()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>call_id</th>
      <th>priority</th>
      <th>vru_time</th>
      <th>q_time</th>
      <th>ser_time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>54022.000000</td>
      <td>54022.000000</td>
      <td>54022.000000</td>
      <td>54022.000000</td>
      <td>54022.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>28453.102717</td>
      <td>0.803469</td>
      <td>9.954259</td>
      <td>52.952612</td>
      <td>147.958887</td>
    </tr>
    <tr>
      <th>std</th>
      <td>15705.557074</td>
      <td>0.891397</td>
      <td>32.911385</td>
      <td>87.943279</td>
      <td>220.784324</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1169.000000</td>
      <td>0.000000</td>
      <td>-334.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>14860.250000</td>
      <td>0.000000</td>
      <td>6.000000</td>
      <td>0.000000</td>
      <td>13.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>28458.500000</td>
      <td>0.000000</td>
      <td>6.000000</td>
      <td>15.000000</td>
      <td>84.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>42061.750000</td>
      <td>2.000000</td>
      <td>10.000000</td>
      <td>71.000000</td>
      <td>185.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>55656.000000</td>
      <td>2.000000</td>
      <td>3639.000000</td>
      <td>3164.000000</td>
      <td>6454.000000</td>
    </tr>
  </tbody>
</table>
</div>
