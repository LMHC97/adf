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
import pandas as pd
import csv
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
- 
