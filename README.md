# Banca-Satander
Challenge

### Respuestas:

1) Desde un primer momento me sentaría a ver con el equipo los requerimientos del negocio.

Luego, empezaría evaluando la tecnología del negocio y el origen de datos.

Considerando que el origen se encuentra es una tabla relacional alojada en Big-Query sugiero montar un entorno DEV, cargar el data set y empezar a realizar consultas y conocer más a detalle el dataset. Por otro lado, comienzo a delinear el tratamiento de datos que vamos a ejecutar posteriormente una vez tengamos definido el modelo de datos. Este debe llevarse a cabo en un entorno sql, para ir ganando tiempo y confianza. Pero también podría hacerse usando Python y algunas librerías.

Ya una vez, nos encontremos trabajando con el set de datos, comenzamos a diseñar el modelo de datos dimensional. Para el cual vamos a tener que crear los elementos (tablas) en una base de datos SQL en instancia stage, intermedias y dw. Las cuales van a recargarse por medio de stored procedure que serán ejecutados por paquetes de ETLs/ELTLs con alguna herramienta amigable al entorno.

Como resultado de las querys de consulta, con las que alimentaremos las tablas, terminamos por diseñar el modelo de datos. Este será decidido con el quipo, y validado con los agentes del negocio.

2) El modelo de datos a presentar es el siguiente:

![image](https://user-images.githubusercontent.com/105885683/199309454-cc29a576-1566-4678-9334-309a93030d93.png)



3) Querys de consulta para cargar datos en las tablas del modelo:



----------------///////////-------------

/*
Query: "Fact_Mov_Loqueos_HB"

Autor: Ferrer, Franco.

Fecha: 01/11/2022

*/

SELECT DISTINCT

table1.SESSION_ID

,usr.USER_ID

,evento.EVENT_ID

,table1.USER_CITY

,table1.SERVER_TIME

,table1.DEVICE_BROWSER

,table1.DEVICE_OS

,table1.DEVICE_MOBILE

,table1.TIME_SPENT

FROM "datos_data_engineer" AS table1

LEFT JOIN "Dim_Usuaarios_HB" AS usr

ON table1.USER_ID = usr.USER_ID

LEFT JOIN "Dim_Tipo_Evento" AS evento

table1.EVENTE_ID = evento.EVENT_ID



----------------///////////-------------



/*

Query: "Fact_Historica_Logueos_HB"

Autor: Ferrer, Franco.

Fecha: 01/11/2022

*/



SELECT 

SESSION_ID

,MIN(SERVER_TIME) AS HISTORY_LOGIN

,USER_ID

,SEGMENT_ID

,EVENT_ID

,USER_CITY

,SERVER_TIME

,DEVICE_BROWSER

,DEVICE_OS

,DEVICE_MOBILE

,TIME_SPENT

FROM "Fact_Mov_Loguin_HB"

GROUP BY SESSION_ID

,USER_ID

,SEGMENT_ID

,EVENT_ID

,USER_CITY

,SERVER_TIME

,DEVICE_BROWSER

,DEVICE_OS

,DEVICE_MOBILE

,TIME_SPENT



----------------///////////-------------

/*


Query: "Dim_Tipo_Evento"

Autor: Ferrer, Franco.

Fecha: 01/11/2022

*/



SELECT DISTINCT

,EVENT_ID

,EVENT_DESCRIPTION

,CRASH_DETECTION

FROM "datos_data_engineer"

LEFT JOIN  AS usr

ON table1.USER_ID = usr.USER_ID

LEFT JOIN "Dim_Tipo_Evento" AS evento

table1.EVENTE_ID = evento.EVENT_ID



----------------///////////-------------

/*

Query: "Dim_Tipo_Segmento"

Autor: Ferrer, Franco.

Fecha: 01/11/2022

*/



SELECT DISTINCT

,SEGMENT_ID


,SEGMENT_DESCRIPTION


FROM "datos_data_engineer"



----------------///////////-------------

/*

Query: "Dim_Usuarios_HB"

Autor: Ferrer, Franco.

Fecha: 01/11/2022

*/




SELECT DISTINCT

,USER_ID

,SEGEMENT_ID

FROM "datos_data_engineer"


/*


"Es necesario hacer un JOIN con alguna tabla maestro para asegurar la unicidad de los datos del modelo"

*/


4) 
 
La Query para el KPI retencion. Este genera los datos para levantar con una herramienta de visuaización.


SELECT TOP 10

USER_ID

,COUNT DISTINCT(SESSION_ID) AS q_logueos

,SUM(TIME_SPENT) AS tiempo_session

FROM "Fact_Mov_Logueos_HB"

WHERE

BETWEEN CAST(SERVER_TIME, AS DATE) 

AND DATEADD("M", -1, CAST(SERVER_TIME, AS DATE))

GROUP BY USER_ID, SERVER_TIME

ORDER BY SUM(TIME_SPENT) DESC



5) 

Utilziando googleColab realizamos el script para cargar el archivo tsv y transformarlo en csv.

import pandas as pd

df_tsv = pd.read_csv('datos_data_engineer.tsv', encoding='utf-16-LE',sep='\t',index_col="id", parse_dates=True).dropna()

df_tsv

df_tsv.to_csv('new_datos_banca_santander.csv', sep=',')








