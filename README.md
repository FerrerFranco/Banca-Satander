# Banca-Satander
Challenge

### Respuestas:

1) Desde un primer momento me sentaría a ver con el equipo los requerimientos del negocio.

Luego, empezaría evaluando la tecnología del negocio y el origen de datos.

Considerando que el origen se encuentra es una tabla relacional alojada en Big-Query sugiero montar un entorno DEV, cargar el data set y empezar a realizar consultas y conocer más a detalle el dataset. Por otro lado, comienzo a delinear el tratamiento de datos que vamos a ejecutar posteriormente una vez tengamos definido el modelo de datos. Este debe llevarse a cabo en un entorno sql, para ir ganando tiempo y confianza. Pero también podría hacerse usando Python y algunas librerías.

Ya una vez, nos encontremos trabajando con el set de datos, comenzamos a diseñar el modelo de datos dimensional. Para el cual vamos a tener que crear los elementos (tablas) en una base de datos SQL en instancia stage, intermedias y dw. Las cuales van a recargarse por medio de stored procedure que serán ejecutados por paquetes de ETLs/ELTLs con alguna herramienta amigable al entorno.

Como resultado de las querys de consulta, con las que alimentaremos las tablas, terminamos por diseñar el modelo de datos. Este será decidido con el quipo, y validado con los agentes del negocio.

![image](https://user-images.githubusercontent.com/105885683/199309454-cc29a576-1566-4678-9334-309a93030d93.png)

