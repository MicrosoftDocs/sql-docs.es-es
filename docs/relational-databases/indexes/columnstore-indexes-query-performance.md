---
description: Rendimiento de las consultas de índices de almacén de columnas
title: Rendimiento de las consultas de índices de almacén de columnas | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 83acbcc4-c51e-439e-ac48-6d4048eba189
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6501f3148ab906c8ac719407a489db6a4be1e62b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236711"
---
# <a name="columnstore-indexes---query-performance"></a>Rendimiento de las consultas de índices de almacén de columnas

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Recomendaciones para lograr el rendimiento de las consultas muy rápido que se espera que proporcionen los índices de almacén de columnas.    
    
 Los índices de almacén de columnas pueden lograr un rendimiento hasta 100 veces mayor en las cargas de almacenamiento de datos y análisis y hasta 10 veces mejor en la compresión de datos que los índices de almacén de filas tradicionales. Estas recomendaciones ayudan a que las consultas consigan el rendimiento de las consultas muy rápido que se espera que proporcionen los índices de almacén de columnas. Al final hay más explicaciones sobre el rendimiento del almacén de columnas.    
    
## <a name="recommendations-for-improving-query-performance"></a>Recomendaciones para mejorar el rendimiento de las consultas    
 Aquí se proporcionan algunas recomendaciones para lograr el rendimiento alto que se espera que proporcionen los índices de almacén de columnas.    
    
### <a name="1-organize-data-to-eliminate-more-rowgroups-from-a-full-table-scan"></a>1. Organización de los datos para eliminar más grupos de filas de un recorrido de tabla completo    
    
-   **Aproveche el orden de inserción.** Normalmente, en el almacén de datos tradicional, los datos se insertan realmente en orden cronológico y el análisis se realiza en la dimensión de tiempo. Por ejemplo, en los análisis de ventas por trimestre. Para este tipo de carga de trabajo, se produce la eliminación del grupo de filas automáticamente. En [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede encontrar que se ha omitido una serie de grupos de filas como parte del procesamiento de consulta.    
    
-   **Aproveche el índice agrupado del almacén de filas.** Si el predicado de consulta común está en una columna (por ejemplo, C1) que no está relacionada con el orden de inserción de la fila, puede crear un índice agrupado de almacenamiento de filas en la columna C1 y, después, crear un índice agrupado de almacén de columnas al quitar el índice agrupado de almacenamiento de filas. Si crea el índice de almacén de columnas agrupado explícitamente con `MAXDOP = 1`, el índice de almacén de columnas agrupado se ordena perfectamente en la columna C1. Si especifica `MAXDOP = 8`, verá la superposición de los valores en los ocho grupos de filas. Un caso común de esta estrategia es cuando crea inicialmente el índice de almacén de columnas con un conjunto grande de datos. Tenga en cuenta que, para el índice de almacén de columnas no agrupado (NCCI), si la tabla de base del almacén de filas tiene un índice agrupado, las filas ya están ordenadas. En este caso, el índice de almacén de columnas no agrupado resultante se ordenará automáticamente. Un punto importante a tener en cuenta es que ese índice de almacén de columnas no mantiene de manera inherente el orden de las filas. Como se insertan filas nuevas o se actualizan las filas más antiguas, deberá repetir el proceso, ya que el rendimiento de la consulta de análisis puede deteriorarse.    
    
-   **aprovechamiento de la partición de tablas.** Puede dividir el índice de almacén de columnas y, después, usar la eliminación de particiones para reducir el número de grupos de filas que se van a analizar. Por ejemplo, una tabla de hechos almacena las compras realizadas por los clientes y un patrón de consulta común va a encontrar las compras trimestrales realizadas por un cliente específico. Para esto, puede combinar el orden de inserción con la partición en la columna del cliente. Cada partición contendrá filas en orden cronológico para el cliente específico. Además, considere la posibilidad de usar la creación de particiones de tabla si es necesario quitar datos del almacén de columnas. Desactivar y truncar las particiones que ya no son necesarias es una estrategia eficaz para eliminar datos sin generar la fragmentación ocasionada por tener grupos de filas más pequeños.    

-   **Evite la eliminación de grandes cantidades de datos**. Quitar las filas comprimidas de un grupo de filas no es una operación sincrónica. Resultaría caro descomprimir un grupo de filas, eliminar la fila y, a continuación, volver a comprimirla. Por lo tanto, si elimina datos de grupos de filas comprimidos, estos grupos de filas se seguirán analizando aunque devuelvan menos filas. Si el número de filas eliminadas para varios grupos de filas es lo suficientemente grande como para combinar estos en menos grupos de filas, la reorganización del almacén de columnas mejora la calidad del índice y el rendimiento de las consultas. Si el proceso de eliminación de datos normalmente vacía grupos de filas completos, considere la posibilidad de usar la creación de particiones de tabla, desactivar las particiones que ya no son necesarias y truncarlas en lugar de eliminarlas. 

    > [!NOTE]
    > A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], el motor de tupla cuenta con la ayuda de una tarea de combinación en segundo plano que comprime automáticamente los grupos de filas delta OPEN que han existido durante algún tiempo, según lo determinado por un umbral interno, o combina los grupos de filas COMPRESSED desde donde se ha eliminado un gran número de filas. De este modo, se mejora la calidad del índice de almacén de columnas a lo largo del tiempo.   
    > Si se requiere la eliminación de grandes cantidades de datos del índice de almacén de columnas, considere la posibilidad de dividir esa operación en lotes de eliminación más pequeños a lo largo del tiempo. De este modo, se permite que la tarea de combinación en segundo plano administre la tarea de combinar grupos de filas más pequeños y mejorar la calidad del índice y se elimina la necesidad de programar ventanas de mantenimiento de reorganización de índices tras la eliminación de datos.    
    > Para obtener más información sobre los términos y conceptos de almacén de columnas, vea [Índices de almacén de columnas: Información general](../../relational-databases/indexes/columnstore-indexes-overview.md).
    
### <a name="2-plan-for-enough-memory-to-create-columnstore-indexes-in-parallel"></a>2. Planear para que haya suficiente memoria para crear índices de almacén de columnas en paralelo    
 La creación un índice de almacén de columnas es de forma predeterminada una operación paralela a menos que se restrinja la memoria. Crear el índice en paralelo requiere más memoria que crear el índice en serie. Cuando hay suficiente memoria, la creación de un índice de almacén de columnas tarda aproximadamente 1,5 más que generar un árbol B en las mismas columnas.    
    
 La memoria necesaria para crear un índice de almacén de columnas depende del número de columnas, el número de columnas de cadena, el grado de paralelismo (DOP) y las características de los datos. Por ejemplo, si la tabla tiene menos de un millón de filas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo usará un subproceso para crear el índice de almacén de columnas.    
    
 Si la tabla tiene más de un millón de filas pero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede obtener una concesión de memoria suficiente para crear el índice mediante MAXDOP, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reducirá automáticamente el valor de `MAXDOP` según sea necesario para ajustarse a la concesión de memoria disponible.  En algunos casos, se debe reducir a uno el DOP para generar el índice cuando la memoria está restringida.    
    
 A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la consulta siempre funcionará en modo por lotes. En versiones anteriores, la ejecución por lotes solo se utiliza cuando DOP es mayor que uno.    
    
## <a name="columnstore-performance-explained"></a>Explicación del rendimiento del almacén de columnas    
 Los índices de almacén de columnas logran un rendimiento de consulta elevado mediante la combinación del procesamiento en modo por lotes en memoria de alta velocidad con técnicas que reducen en gran medida los requisitos de E/S. Puesto que las consultas de análisis examinan grandes cantidades de filas, tienen normalmente un enlace de optimización de la infraestructura y, por tanto, la reducción de la E/S durante la ejecución de la consulta es fundamental para el diseño de los índices de almacén de columnas. Una vez que se han leído los datos en la memoria, es fundamental reducir el número de operaciones en memoria.    
    
 Los índices de almacén de columnas reducen la E/S y optimizan las operaciones en memoria a través de una alta compresión de datos, la eliminación del almacén de columnas, la eliminación del grupo de filas y el procesamiento por lotes.    
    
### <a name="data-compression"></a>Compresión de datos    
 Los índices de almacén de columnas logran una compresión de datos hasta 10 veces mayor que los índices de almacén de filas. Esto reduce en gran medida la E/S necesaria para ejecutar las consultas de análisis y, por tanto, mejora el rendimiento de las consultas.    
    
-   Los índices de almacén de columnas leen los datos comprimidos del disco, lo que significa que deben leerse menos bytes de datos en la memoria.    
    
-   Los índices de almacén de columnas almacenan datos en un formato comprimido en la memoria, lo que reduce la E/S con la disminución del número de veces que se leen los mismos datos en memoria. Por ejemplo, con la compresión 10 veces mayor, los índices de almacén de columnas pueden mantener 10 veces más datos en memoria en comparación con el almacenamiento de datos en formato sin comprimir. Con más datos en memoria, es más probable que el índice de almacén de columnas busque los datos que necesita en la memoria sin generar lecturas adicionales del disco.    
    
-   Los índices de almacén de columnas comprimen los datos por columnas en lugar de por filas, lo que genera altas tasas de compresión y reduce el tamaño de los datos almacenados en disco. Se comprime y almacena cada columna de forma independiente.  Los datos de una columna siempre tienen el mismo tipo y suelen tener valores similares. Las técnicas de compresión de datos son muy buenas para lograr las mayores índices de compresión cuando los valores son similares.    
    
-   Por ejemplo, si una tabla de hechos almacena direcciones de clientes y tiene una columna de país, el número total de valores posibles es inferior a 200. Algunos de esos valores se repetirá muchas veces. Si la tabla de hechos tiene 100 millones de filas, la columna de país se comprimirá fácilmente y requerirá muy poco almacenamiento. La compresión de fila por fila no puede aprovechar la similitud de los valores de columna de esta manera y usará más bytes para comprimir los valores de la columna de país.    
    
### <a name="column-elimination"></a>Eliminación de la columna    
 Los índices de almacén de columnas omiten la lectura de las columnas que no son necesarias para el resultado de la consulta. Esta capacidad, denominada eliminación de la columna, reduce aún más la E/S para la ejecución de la consulta y, por tanto, mejora el rendimiento de las consultas.    
    
-   La eliminación de la columna es posible porque los datos se organizan y comprimen columna por columna. En cambio, cuando los datos se almacenan fila por fila, los valores de columna de cada fila se almacenan físicamente juntos y no se pueden separar fácilmente. El procesador de consultas tiene que leerse en una fila completa para recuperar valores de columna específicos, lo que aumenta la E/S porque se leen datos adicionales innecesariamente en la memoria.    
    
-   Por ejemplo, si una tabla tiene 50 columnas y la consulta usa solo 5 de esas columnas, el índice de almacén de columnas solo captura las 5 columnas del disco. Omite la lectura en las demás 45 columnas. Esto reduce la E/S en otro 90 % suponiendo que todas las columnas son de tamaño similar. Si los mismos datos se almacenan en un almacén de filas, el procesador de consultas necesita leer las 45 columnas adicionales.    
    
### <a name="rowgroup-elimination"></a>Eliminación del grupo de filas    
 Para el análisis de una tabla completa, un gran porcentaje de los datos no coincide normalmente con los criterios de predicado de consulta. Con los metadatos, el índice de almacén de columnas puede omitir la lectura de los grupos de filas que no contienen los datos necesarios para el resultado de la consulta, sin necesidad de la E/S real. Esta capacidad, denominada eliminación del grupo de filas, reduce la E/S para los análisis de tabla completa y, por tanto, mejora el rendimiento de las consultas.    
    
 **¿Cuándo un índice de almacén de columnas tiene que realizar un análisis de tabla completa?**    
    
 A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear uno o varios índices no agrupados normales de árbol B en un índice agrupado de almacén de columnas exactamente igual que en un montón de almacén de filas. Los índices no agrupados de árbol B pueden acelerar una consulta que tenga un predicado de igualdad o un predicado con un pequeño intervalo de valores. Para predicados más complicados, el optimizador de consultas puede elegir un análisis de tabla completa. Sin la capacidad para omitir los grupos de filas, un análisis de tabla completa sería muy lento, especialmente para las tablas grandes.    
    
 **¿Cuándo se beneficia una consulta de análisis de la eliminación del grupo de filas para un análisis de tabla completa?**    
    
 Por ejemplo, una empresa minorista ha modelado sus datos de ventas con una tabla de hechos con un índice agrupado de almacén de columnas. Cada nueva venta almacena varios atributos de la transacción, incluida la fecha de venta de un producto. Curiosamente, aunque los índices de almacén de columnas no garantizan un criterio de ordenación, las filas de la tabla se cargarán ordenadas por fecha. Esta tabla crecerá con el tiempo. Aunque la empresa minorista puede conservar los datos de ventas de los últimos 10 años, una consulta de análisis solo necesita calcular un agregado para el último trimestre. Los índices de almacén de columnas pueden eliminar el acceso a los datos para los 39 trimestres anteriores con solo mirar los metadatos de la columna de fecha. Se trata de una reducción adicional del 97 % en la cantidad de datos que se lee en la memoria y que se procesa.    
    
 **¿Qué grupos de filas se omiten en un análisis de tabla completa?**    
    
 Para determinar qué grupos de filas eliminar, el índice de almacén de columnas usa metadatos para almacenar los valores mínimos y máximos de cada segmento de columna para cada grupo de filas. Cuando ninguno de los intervalos del segmento de columna cumple los criterios de predicado de consulta, se omite el grupo de filas completo sin realizar el aprovisionamiento de la infraestructura real. Esto funciona porque los datos se cargan normalmente de forma ordenada y aunque no se garantiza que las filas se ordenen, los valores de datos similares a menudo se encuentran en el mismo grupo de filas o un grupo de filas adyacente.    
    
 Para más información sobre grupos de filas, vea [Directrices para diseñar índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index).    
    
### <a name="batch-mode-execution"></a>Ejecución del modo por lotes    
 La [ejecución del modo por lotes](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) hace referencia al procesamiento de un conjunto de filas, normalmente hasta 900 filas juntas, para obtener la eficacia de la ejecución. Por ejemplo, la consulta `SELECT SUM (Sales) FROM SalesData` agrega las ventas totales de la tabla SalesData. En la ejecución del modo por lotes, el motor de ejecución de consultas calcula el agregado en el grupo de 900 valores. Esto incluye metadatos, los costos de acceso y otros tipos de sobrecarga sobre todas las filas de un lote, en lugar de pagar el costo para cada fila. Por lo tanto, se reduce significativamente la ruta de acceso del código. El procesamiento de modo por lotes funciona en los datos comprimidos cuando sea posible y elimina algunos de los operadores de intercambio utilizados por el procesamiento del modo de fila. Esto acelera la ejecución de las consultas de análisis por órdenes de magnitud.    
    
 No todos los operadores de ejecución de consultas se pueden ejecutar en el modo por lotes. Por ejemplo, las operaciones de DML Insert, Delete o Update ejecutan una fila cada vez. Los operadores de modo por lote destinan operadores para la aceleración del rendimiento de las consultas como Scan, Join, Aggregate, sort, etc. Dado que el índice de almacén de columnas se introdujo en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], hay un esfuerzo sostenido para expandir los operadores que se pueden ejecutar en el modo por lotes. En la tabla siguiente se muestran los operadores que se ejecutan en el modo por lotes según la versión del producto.    
    
|Operadores del modo por lotes|¿Cuándo se usa?|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<sup>1</sup>|Comentarios|    
|---------------------------|------------------------|---------------------|---------------------|---------------------------------------|--------------|    
|Operaciones de DML (insert, delete, update y merge)||no|no|no|DML no es una operación de modo por lotes porque no es paralela. Incluso cuando se habilita el procesamiento por lotes del modo serie, no vemos importantes mejoras si se permite que DML se procese en el modo por lotes.|    
|Exploración de índice de almacén de columnas|SCAN|N/D|sí|sí|Para los índices de almacén de columnas, podemos insertar el predicado en el nodo SCAN.|    
|Exploración de índice de almacén de columnas (no agrupado)|SCAN|sí|sí|sí|sí|    
|Index Seek||N/D|N/D|no|Se lleva a cabo una operación de búsqueda a través de un índice no agrupado de árbol B en el modo de fila.|    
|Compute Scalar|Expresión que se evalúa en un valor escalar.|sí|sí|sí|Sin embargo, hay algunas restricciones sobre el tipo de datos. Esto es cierto para todos los operadores de modo por lotes.|    
|concatenación|UNION y UNION ALL|no|sí|sí||    
|filter|Aplicación de predicados|sí|sí|sí||    
|Hash Match|Funciones de agregación basada en hash, combinación hash exterior, combinación hash derecha, combinación hash izquierda, combinación derecha interna, combinación interna izquierda|sí|sí|sí|Restricciones para la agregación: sin min/max para las cadenas. Funciones de agregación disponibles: sum/count/avg/min/max.<br />Restricciones de combinación: no combinaciones de tipo no coincidente en tipos no enteros.|    
|Merge Join||no|no|no||    
|Consultas multiproceso||sí|sí|sí||    
|bucles anidados||no|no|no||    
|Consultas uniproceso que se ejecutan en MAXDOP 1||no|no|sí||    
|Consultas uniproceso con un plan de consulta en serie||no|no|sí||    
|sort|Cláusula Order by en SCAN con índice de almacén de columnas|no|no|sí||    
|Top Sort||no|no|sí||    
|Agregados de ventana||N/D|N/D|sí|Nuevo operador en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].|    
    
<sup>1</sup> se aplica a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)] a los niveles Premium, Standard, S3 y superiores, a todos los niveles de núcleo virtual y a [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]    

Para más información, vea la [Guía de arquitectura de procesamiento de consulta](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution).
    
### <a name="aggregate-pushdown"></a>Aplicación de agregados    
 Ruta de acceso de ejecución normal para el cálculo de agregados para capturar las filas calificadas desde el nodo SCAN y agregar los valores en el modo por lotes. Aunque esto proporciona un buen rendimiento, con [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la operación de agregación se puede insertar en el nodo SCAN para mejorar el rendimiento del cálculo de la agregación mediante órdenes de magnitud en la parte superior de la ejecución en el modo por lotes una vez que se cumplan las condiciones siguientes: 
 
-    Los agregados son `MIN`, `MAX`, `SUM`, `COUNT` y `COUNT(*)`. 
-  El operador de agregación debe estar en la parte superior del nodo SCAN o el nodo SCAN con `GROUP BY`.
-  Este agregado no es un agregado Distinct.
-  La columna de agregado no es una columna de cadena.
-  La columna de agregado no es una columna virtual. 
-  El tipo de datos de entrada y salida debe ser uno de los siguientes y debe tener 64 bits o menos.
    -  `tinyint`, `int`, `bigint`, `smallint`, `bit`
    -  `smallmoney`, `money`, `decimal` y `numeric` con precisión < = 18
    -  `smalldate`, `date`, `datetime`, `datetime2`, `time`
    
 La aplicación de la agregación se acelera aún más con la agregación eficaz en datos comprimidos/codificados en la ejecución compatible con la caché y el aprovechamiento de SIMD    
    
 ![Aplicación de agregado](../../relational-databases/indexes/media/aggregate-pushdown.jpg "Aplicación de agregados")    
    
Por ejemplo, la aplicación de la agregación se realiza en las dos consultas siguientes:    
    
```sql     
SELECT  productkey, SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI    
GROUP BY productkey;    
    
SELECT  SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI;    
```    
    
### <a name="string-predicate-pushdown"></a>Aplicación del predicado de la cadena    
Cuando se diseña un esquema de almacenamiento de datos, el modelado del esquema recomendado es usar un esquema de estrella o copo de nieve que conste de una o varias tablas de hechos y muchas tablas de dimensión. La [tabla de hechos](https://wikipedia.org/wiki/Fact_table) almacena las transacciones o medidas empresariales y la [tabla de dimensiones](https://wikipedia.org/wiki/Dimension_table) almacena las dimensiones en las que tienen que analizarse los hechos.    
    
Por ejemplo, un hecho puede ser un registro que representa una venta de un producto determinado en una región específica, mientras que la dimensión representa un conjunto de regiones, productos y así sucesivamente. Las tablas de hechos y dimensiones están conectadas a través de la relación de clave principal o externa. Las consultas de análisis de uso más frecuente unen una o varias tablas de dimensiones con la tabla de hechos.    
    
Consideremos un elemento `Products` de tabla de dimensiones. Una clave principal típica será `ProductCode`, que normalmente se representa como tipo de datos de cadena. Para el rendimiento de las consultas, es aconsejable crear una clave suplente, normalmente una columna de enteros, para hacer referencia a la fila en la tabla de dimensiones de la tabla de hechos. 
    
El índice de almacén de columnas ejecuta las consultas de análisis con combinaciones y predicados que implican claves basadas en enteros o números de forma muy eficiente. Pero en muchas cargas de trabajo de cliente, encontramos el uso de las columnas basadas en cadenas que se vinculan con tablas de dimensiones/hechos, lo que, como resultado, genera un rendimiento de la consulta del índice del almacén de columna diferente del esperado. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] mejora el rendimiento de las consultas de análisis con las columnas basadas en cadenas notablemente mediante la aplicación de los predicados con las columnas de cadena en el nodo SCAN.    
    
La aplicación del predicado de cadena aprovecha el diccionario principal y secundario creado para que las columnas mejoren el rendimiento de las consultas. Por ejemplo, consideremos el segmento de columna de cadena dentro de un grupo de filas que consta de 100 valores de cadena distintos. Esto significa que se hace referencia a cada valor de cadena distinto 10 000 veces de media, lo que supone un millón de filas.    
    
Con la aplicación del predicado de la cadena, la ejecución de la consulta computa el predicado con los valores del diccionario, y si son aptos, todas las cadenas referentes al valor del diccionario se aceptan automáticamente. Esto mejora el rendimiento de dos maneras:
1.  Solo se devuelve la fila apta, lo que reduce el número de filas que fluye fuera del nodo SCAN. 
2.  Se reduce considerablemente el número de comparaciones de cadenas. En este ejemplo, se requieren solo 100 comparaciones de cadena en 1 millón de comparaciones. Existen algunas limitaciones, como se describe a continuación:    

    -   No aplicación del predicado de la cadena para grupos de filas delta. No hay ningún diccionario para las columnas en grupos de filas delta.    
    -   No aplicación del predicado de la cadena si el diccionario supera las 64 KB de entradas.    
    -   No se admiten valores NULL de la evaluación de expresión.    
    
## <a name="see-also"></a>Consulte también    
 [Directrices para diseñar índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index) [Índices de almacén de columnas: Guía de carga de datos](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)     
 [Índices de almacén de columnas para el almacenamiento de datos](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)    
 [Diseño de los índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)    
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)     
  
