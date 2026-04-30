# **Código de automatización de Performance Review.**



**El servicio de telefonía virtual CallMeMaybe necesita una nueva función que brindará a los supervisores y las supervisoras información sobre los operadores menos eficaces.** 



Con una muestra de comportamiento de usuarios y operadores  del último trimestre de 2019 se desarrolló el sistema de clasificación. A continuación encontrará los resultados del análisis de los datos, la explicación del modelo propuesto y sus resultados.



¿Cuándo se considera que un operador es ineficaz?

* Cuando tiene una gran cantidad de llamadas entrantes perdidas (internas y externas).



* Cuando tiene un tiempo de espera prolongado para las llamadas entrantes.



* Cuando tiene un número reducido de llamadas salientes.





Se desarrolló una función para catalogar el trabajo general de cada operador. Se crearon 4 subcategorías porque no es lo mismo ser ineficaz en una métrica que serlo en todas. La regla buscada es que un operador es ineficiente si cumple al tiempo las tres condiciones: tiempos de espera altos, muchas llamadas perdidas y llamadas muy largas. Las cuatro sub categorías son: 



1\. Ineficaz: Esta subcategoría es la principal. Si hay 4 strikes se cumple con el criterio para pertener a este grupo.



2\. Parcialmente ineficaz: No se cumplen todas las condiciones al tiempo, pero se cumplen 3. Es decir, si hay 3 strikes, se entra a este grupo y se está en riesgo de pasar a ser un operador ineficaz. Esta métrica debe ser de especial atención para los supervisores.



3\. Parcialmente eficaz: Se cumplen 2 condiciones al tiempo. Es decir, si hay 2 strikes, se entra a este grupo y se está en riesgo de pasar a ser un operador parcialmente ineficaz. Esta métrica es de especial atención para los supervisores.



4\. Eficaz: La métrica premio. Si alguien tiene un strike o menos se considera como un operador eficiente ya que no cumple varias condiciones simultaneas y un usuario disgustado puede dañar en cualquier momento sus métricas.





Se observó una distribución para entender cuál es la tendencia. Se concluye que más de la mitad son  eficaces o parcialmente eficaces, una buena noticia. Sin embargo, una minoría se cuenta como ineficaz.



**Descripción técnica del proyecto:**



1\. Se utilizaron librerías como Pandas y Numpy  que sirvieron para manipular, calcular y convertir los datos de las bases de datos.  También se utilizaron las librerías Matplotlib y Seaborn especializadas en visualización de datos, con ellas se hicieron los gráficos; y scipy, math y statsmodel que contienen modelos matemáticos y sirven para realizar pruebas de hipótesis con elevados cálculos estadísticos. 



2\. Para el desarrollo de esta prueba se aportaron dos DataFrames: A. clients y B. Eventos de llamada.



A. contiene todos sus valores pero no fue relevante para las  pruebas hechas. Solo se concluyó la existencia de un dataframe con valores y formatos apropiados y completos. 



B. Se encontró un DataFrame con los valores de sus columnas como se esperaba, sin embargo viene con valores ausentes en la columna clave: 'operator\_id', esto quiere decir que la prueba pudo tener sesgos pues hay un operador (o varios eventos de varios operadores) que no fueron estudiados. También faltan algunos valores de la columna internal. Por último, no se especifíca el formato de la duración de las llamadas (minutos, segundos o milisegundos).



3\. Dado que las columnas de duración de llamadas son medidas continúas se hizo una agrupación categórica para saber cuánto se están demorando las llamadas y cuánto se demoran con tiempo de espera.  Este es el resultado. 



4\. Tras una exploración profunda de todo el DataFrame, se concluyó que los elementos más relevantes tienen que ver con la duración de las llamadas y la duración más el tiempo de espera.

5\. Luego de revisar proporciones, se concluyó que hay una relación entre el tiempo de espera y el número de llamadas rechazadas por lo que se recomienda hacer un estudio a detalle enfocado en esta métrica para establecer si hay o no causalidad.

6\. El procedimiento utilizado para calificar a los operadores fue el siguiente:

&#x20;       A. Se limpió el DataFrame de valores ausentes convirtiendolos en la categoría unknown. Se dejaron porque de otra forma se desconoce si ese valor ausente importa, así que es mejor contarlo bajo una etiqueta. Era necesario procesarlos porque podía generar errores en cálculos de conversión de formato.

&#x20;       B. Se encontraron valores atípicos que se dejaron porque esas anomalías igual representan una demora de un cliente o de un operador e ignorarlos significaría desconocerlos.

&#x20;       C. Se agruparon los datos por id de operador.

&#x20;       D. Se filtraron los datos por llamadas entrantes y salientes.

&#x20;       E. Se calculó el promedio de tiempo de espera, número de llamadas y llamadas perdidas por operador para las llamadas entrantes y promedio de llamadas para las salientes por operador y posterior mente se sacó un promedio general.

&#x20;       F. Se hizo un nuevo DataFrame en donde estaban todos los operadores y por cada uno si el promedio era:

&#x20;           ▪ El tiempo de espera de llamadas entrantes era superior a la media general.

&#x20;           ▪ El número de llamadas entrantes era inferior a la media general.

&#x20;           ▪ El número de llamadas perdidas es superior a la media general.

&#x20;           ▪ El número de llamadas salientes es inferior a la media general.

Se calificaba con un valor de True o False.

&#x20;       E. Se generó una condición si la persona tenía true en más de 3 de estás condiciones previas se consideraba ineficaz; 3 es parcialmente eineficaz, 2 parcialmente eficaz; 1 o ninguna eficaz.





CONCLUSIONES Y RECONENDACIONES:

Los operadores suelen ser eficaces o parcialmente eficaces. Una buena noticia. Cerca de 600 son eficaces y cerca de 350 son parcialmente eficaces. Esto quiere decir que de las 4 condiciones que se evalúan para clasi€icarlos como ineficaces, por mucho cumplen con 2 - para el caso de los parcialmente eficaces- o una o ninguna para el caso de los eficaces.



Se sugiere revisar el performance en el tablero porque la calificación puede estar sesgada porque hay usuarios que solo atienden llamadas salientes, así pasan como eficaces en los filtros de las llamadas entrantes. Esto también ocurre en el caso contrario, es decir operadores que solo trabajan con llamadas entrantes. Además, el sesgo puede estar dado por una Ilamada larga que afecte el promedio del operador.

Para	la	revisión	del	performance	de	cada	operador,	visite: https://public.tabIeau.com

/app/pro€iIe/david.alejandro.guar.n.barrero/viz

/Dashboard Callme Maybe/Dashboard1



Hay correlación entre el tiempo de espera y el número de llamadas rechazadas, se sugiere un estudio profundo para evaluar causalidad. En las llamadas en general el tiempo de espera alto afecta las llamadas, en las llamadas entrantes, al parecer los usuarios con el tiempo de espera corto pueden ser sutilmente propensos a irse.





El código se estructuró con comentarios, de forma que solo debe descargar el notebook y los datasets, reemplazar la ruta de los archivos que contienen los datos y correr el código en el lugar de su preferencia.







