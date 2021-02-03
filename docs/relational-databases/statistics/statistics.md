---
title: Estadísticas
description: El Optimizador de consultas utiliza estadísticas para crear planes de consulta que mejoren el rendimiento de las consultas. Obtenga más información sobre los conceptos y las directrices para usar la optimización de consultas.
ms.custom: ''
ms.date: 1/7/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], query optimization
- query performance [SQL Server], statistics
- query optimization statistics [SQL Server]
- statistical information [SQL Server], database options
- query optimization statistics [SQL Server], about query optimization statistics
- statistical information [SQL Server], guidelines
- statistical information [SQL Server]
- using statistics [SQL Server]
- statistical information [SQL Server], indexes
- index statistics [SQL Server]
- query optimizer [SQL Server], statistics
- statistics [SQL Server]
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 89ebfbb70c7c50729ebbfb5de6a8551e00927bc6
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233237"
---
# <a name="statistics"></a>Estadísticas

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  El Optimizador de consultas utiliza estadísticas para crear planes de consulta que mejoren el rendimiento de las consultas. En el caso de la mayoría de las consultas, el Optimizador de consultas genera ya las estadísticas necesarias para un plan de consulta de alta calidad. En algunos casos, para obtener los mejores resultados, es necesario crear estadísticas adicionales o modificar el diseño de la consulta. En este artículo se explican los conceptos relativos a la estadística y se proporcionan directrices para emplear la estadística de optimización de consultas de forma eficaz.  
  
##  <a name="components-and-concepts"></a><a name="DefinitionQOStatistics"></a> Componentes y conceptos  
### <a name="statistics"></a>Estadísticas  
 Las estadísticas para la optimización de consulta son objetos binarios grandes (BLOB) que contienen información estadística sobre la distribución de valores en una o más columnas de una tabla o vista indizada. El Optimizador de consultas utiliza estas estadísticas para estimar la *cardinalidad*, es decir, el número de filas, en el resultado de la consulta. Estas *estimaciones de cardinalidad* permiten al Optimizador de consultas crear un plan de consulta de alta calidad. Por ejemplo, en función de los predicados, el Optimizador de consultas podría usar las estimaciones de cardinalidad para elegir el operador Index Seek en lugar del operador Index Scan, que requiere un uso intensivo de los recursos, si eso mejora el rendimiento de la consulta.  
  
 Cada objeto de estadísticas se crea en una lista de una o más columnas de la tabla e incluye un *histograma* que muestra la distribución de valores en la primera columna. Los objetos de estadísticas en varias columnas también almacenan la información estadística relativa a la correlación de valores entre las columnas. Estas estadísticas de la correlación, o *densidades*, derivan del número de filas distintas de valores de columna. 

#### <a name="histogram"></a><a name="histogram"></a> Histograma  
Un **histograma** mide la frecuencia de aparición de cada valor distinto en un conjunto de datos. El optimizador de consultas calcula un histograma de los valores de la primera columna de clave del objeto de estadísticas; para ello, selecciona los valores de la columna tomando una muestra estadística de las filas o realizando un análisis completo de todas las filas de la tabla o vista. Si el histograma se crea a partir de muestras de un conjunto de filas, los totales almacenados para el número de filas y el número de valores distintos son las estimaciones y no es necesario que sean números enteros.

> [!NOTE]
> <a name="frequency"></a> Los histogramas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo se crean para una única columna: la primera del conjunto de columnas de clave del objeto de estadísticas.
  
Para crear el histograma, el optimizador de consultas ordena los valores de la columna, calcula el número de valores que coinciden con cada valor distinto de esta y, luego, agrega los valores de la columna en un máximo de 200 pasos contiguos del histograma. Cada paso del histograma incluye un rango de valores de columna seguido de un valor de columna de límite superior. El intervalo incluye todos los valores de columna posibles comprendidos entre los valores límite (sin incluir los propios valores límite). El valor de columna ordenado más pequeño es el valor del límite superior del primer paso del histograma.

Más concretamente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea el **histograma** del conjunto ordenado de valores de columna en tres pasos:

- **Inicialización del histograma**: en el primer paso, se procesa una secuencia de valores desde el principio del conjunto ordenado y se recopila un máximo de 200 valores de *range_high_key*, *equal_rows*, *range_rows* y *distinct_range_rows* (*range_rows* y *distinct_range_rows* son siempre cero durante este paso). El primer paso finaliza cuando se han agotado todas las entradas o cuando se han encontrado 200 valores. 
- **Examen con combinación de depósito**: cada valor adicional de la columna inicial de la clave de estadísticas se procesa en el segundo paso, de forma ordenada; cada valor sucesivo se agrega al último rango o se crea un rango al final (esto es posible porque los valores de entrada están ordenados). Si se crea un rango nuevo, un par de rangos existentes colindantes se contrae en un solo rango. Este par de rangos se selecciona para minimizar la pérdida de información. Este método usa un algoritmo de *diferencias máximas* para minimizar el número de pasos del histograma a la vez que maximiza las diferencias entre los valores de límite. El número de pasos después de contraer los rangos permanece en 200 a lo largo de este paso.
- **Consolidación del histograma**: en el tercer paso, es posible que se contraigan más rangos si no se pierde una cantidad significativa de información. El número de pasos del histograma puede ser menor que el número de valores distintos, incluso para las columnas con menos de 200 puntos de límite. Por lo tanto, incluso aunque la columna tenga más de 200 valores únicos, es posible que el histograma tenga menos de 200 pasos. Para una columna formada solamente por valores únicos, el histograma consolidado tendrá un mínimo de tres pasos.

> [!NOTE]
> Si se ha generado el histograma con un ejemplo en lugar de hacerlo mediante fullscan, los valores de *equal_rows*, *range_rows*, *distinct_range_rows* y  *average_range_rows* son estimados y, por tanto, no es necesario que sean números enteros.

En el diagrama siguiente se muestra un histograma con seis pasos. El área a la izquierda del primer valor límite superior es el primer paso.
  
![Histograma](../../relational-databases/system-dynamic-management-views/media/histogram-2.svg "Histograma") 
  
En cada paso del histograma anterior:
-   La línea gruesa representa el valor de límite superior (*range_high_key*) y el número de veces que tiene lugar (*equal_rows*).  
  
-   El área de color sólido situada a la izquierda de *range_high_key* representa el rango de valores de columna y el número medio de veces que tiene lugar cada valor de columna (*average_range_rows*). El valor de *average_range_rows* en el primer paso del histograma siempre es 0.  
  
-   Las líneas de puntos representan los valores de las muestras utilizados para estimar el número total de valores distintos que hay en el rango (*distinct_range_rows*) y el número total de valores que hay en el rango (*range_rows*). El optimizador de consultas utiliza *range_rows* y *distinct_range_rows* para calcular *average_range_rows* y no almacena los valores de las muestras.   
  
#### <a name="density-vector"></a><a name="density"></a>Vector de densidad  
**Densidad** es la información sobre el número de duplicados en una determinada columna o combinación de columnas, y se calcula como 1/(número de valores distintos). El optimizador de consultas utiliza las densidades para mejorar las estimaciones de cardinalidad de las consultas que devuelven varias columnas de la misma tabla o vista indizada. A medida que disminuye la densidad, aumenta la selectividad de un valor. Por ejemplo, en una tabla que representa automóviles, muchos automóviles tienen el mismo fabricante, pero cada uno dispone de un único número de identificación de vehículo (NIV). Un índice del NIV es más selectivo que un índice del fabricante, porque NIV tiene una densidad inferior a la del fabricante. 

> [!NOTE]
> Frecuencia es la información sobre la aparición de cada valor distinto en la primera columna de clave del objeto de estadísticas y se calcula como el recuento de filas * densidad. Puede encontrarse una frecuencia máxima de 1 en las columnas con valores únicos.

El vector de densidad contiene una densidad para cada prefijo de columnas del objeto de estadísticas. Por ejemplo, si un objeto de estadísticas tiene las columnas de clave `CustomerId`, `ItemId` y `Price`, la densidad se calcula en cada uno de los siguientes prefijos de columna.
  
|Prefijo de columna|Densidad calculada en|  
|---|---|
|(IdCliente)|Filas con valores que se corresponden con IdCliente|  
|(IdCliente, IdArtículo)|Filas con valores que se corresponden con IdCliente e IdArtículo|  
|(IdCliente, IdArtículo, Precio)|Filas con valores que se corresponden con IdCliente, IdArtículo y Precio| 

### <a name="filtered-statistics"></a>Estadísticas filtradas  
 Las estadísticas filtradas pueden mejorar el rendimiento de las consultas que se seleccionan desde subconjuntos de datos bien definidos. Las estadísticas filtradas utilizan un predicado de filtro para seleccionar el subconjunto de datos que se incluye en las estadísticas. Las estadísticas filtradas bien diseñadas pueden mejorar el plan de ejecución de la consulta en comparación con las estadísticas de tabla completa. Para obtener más información sobre el predicado de filtro, vea [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md). Para obtener más información sobre los casos en los que conviene crear estadísticas filtradas, consulte la sección [Cuándo crear las estadísticas](#CreateStatistics) de este artículo.  
 
### <a name="statistics-options"></a>Opciones de estadísticas  
 Hay tres opciones que afectan al momento y al modo en los que se crean y actualizan las estadísticas. Estas opciones se pueden configurar únicamente en el nivel de la base de datos.  
  
#### <a name="auto_create_statistics-option"></a><a name="AutoUpdateStats"></a>Opción AUTO_CREATE_STATISTICS  
 Cuando está activada la opción automática de creación de estadísticas, [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics), el optimizador de consultas crea las estadísticas en columnas individuales en el predicado de consulta, según sea necesario, para mejorar las estimaciones de cardinalidad para el plan de consulta. Estas estadísticas de columna única se crean en las columnas que aún no tienen un [histograma](#histogram) en un objeto de estadísticas existente. La opción AUTO_CREATE_STATISTICS no determina si las estadísticas se crean para los índices. Esta opción tampoco genera estadísticas filtradas. Se aplica estrictamente a estadísticas de columna única para la tabla completa.  
  
 Cuando el Optimizador de consultas crea las estadísticas como resultado de usar la opción AUTO_CREATE_STATISTICS, el nombre de las estadísticas comienza con `_WA`. Puede utilizar la consulta siguiente para determinar si el Optimizador de consultas ha creado estadísticas para una columna de predicado de consulta.  
  
```sql  
SELECT OBJECT_NAME(s.object_id) AS object_name,  
    COL_NAME(sc.object_id, sc.column_id) AS column_name,  
    s.name AS statistics_name  
FROM sys.stats AS s 
INNER JOIN sys.stats_columns AS sc  
    ON s.stats_id = sc.stats_id AND s.object_id = sc.object_id  
WHERE s.name like '_WA%'  
ORDER BY s.name;  
```  
  
#### <a name="auto_update_statistics-option"></a>Opción AUTO_UPDATE_STATISTICS  
 Cuando está activada la opción automática de actualización de estadísticas, [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics), el optimizador de consultas determina cuándo las estadísticas pueden quedar obsoletas y las actualiza cuando una consulta las usa. Las estadísticas se vuelven obsoletas después de que operaciones de inserción, actualización, eliminación o combinación cambien la distribución de los datos en la tabla o la vista indizada. El Optimizador de consultas determina cuándo las estadísticas pueden quedar obsoletas contando el número de modificaciones de datos desde la actualización más reciente de las estadísticas, comparando el número de modificaciones con respecto a un umbral. El umbral se basa en el número de filas de la tabla o la vista indizada.  
  
* Hasta [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa un umbral basado en el porcentaje de filas modificadas. Esto es así independientemente del número de filas de la tabla. El umbral es el siguiente:
    * Si la cardinalidad de tabla era de 500 o menos en el momento de la evaluación de las estadísticas, se actualizará cada 500 modificaciones.
    * Si la cardinalidad de tabla estaba por encima de 500 en el momento de la evaluación de las estadísticas, se actualizará cada 500 modificaciones, más el 20 % pertinente.

* A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y en el [nivel de compatibilidad de base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) 130, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa un umbral de actualización de estadísticas dinámico y decreciente que se ajusta según el número de filas de la tabla. Esto se calcula como la raíz cuadrada de 1000 multiplicado por la cardinalidad de tabla actual. Por ejemplo, si la tabla contiene 2 millones de filas, entonces, el cálculo es sqrt (1000 * 2000000) = 44721,359. Con este cambio, las estadísticas en tablas de gran tamaño se actualizarán con más frecuencia. Sin embargo, si una base de datos tiene un nivel de compatibilidad inferior a 130, se aplicará el umbral de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. 

> [!IMPORTANT]
> En [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] hasta [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], o bien en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores en el [nivel de compatibilidad de la base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) 120 e inferior, habilite la [marca de seguimiento 2371](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) para que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] use un umbral de actualización de estadísticas descendente y dinámico.

Puede usar las siguientes instrucciones para habilitar la marca de seguimiento 2371 en su entorno anterior a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]:

 - Si no ha observado problemas de rendimiento debido a las estadísticas obsoletas, no es necesario habilitar esta marca de seguimiento.
 - Si está en sistemas SAP, habilite esta marca de seguimiento.  Consulte este [blog](/archive/blogs/saponsqlserver/changes-to-automatic-update-statistics-in-sql-server-traceflag-2371) para más información.
 - Si tiene que confiar en el trabajo nocturno para actualizar las estadísticas porque la actualización automática actual no se desencadena con la frecuencia suficiente, considere la posibilidad de habilitar la marca de seguimiento 2371 para reducir el umbral.
  
El Optimizador de consultas comprueba que hay estadísticas obsoletas antes de compilar una consulta y antes de ejecutar un plan de consulta almacenado en la memoria caché. Antes de compilar una consulta, el Optimizador de consultas utiliza las columnas, tablas y vistas indexadas en el predicado de consulta para determinar qué estadísticas podrían estar obsoletas. Antes de ejecutar un plan de consulta almacenado en la memoria caché, [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba que el plan de consulta hace referencia a las estadísticas actualizadas.  
  
La opción AUTO_UPDATE_STATISTICS se aplica a los objetos de estadísticas creados para los índices, columnas únicas de predicados de consulta y las estadísticas creadas con la instrucción [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) . Esta opción también se aplica a las estadísticas filtradas.  
 
Puede usar [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) para realizar un seguimiento de forma precisa del número de filas cambiadas en una tabla y decidir si desea actualizar las estadísticas manualmente.



  
#### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC  
La opción de actualización asincrónica de estadísticas [AUTO_UPDATE_STATISTICS_ASYNC](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics_async) determina si el optimizador de consultas usa actualizaciones sincrónicas o asincrónicas de las estadísticas. La opción de actualización asincrónica de las estadísticas está desactivada de forma predeterminada y el optimizador de consultas actualiza las estadísticas sincrónicamente. La opción AUTO_UPDATE_STATISTICS_ASYNC se aplica a los objetos de estadísticas creados para índices y columnas únicas de los predicados de consulta, así como a las estadísticas creadas con la instrucción [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) .  
 
> [!NOTE]
> Para establecer la opción de actualización asincrónica de las estadísticas en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en la página *Opciones* de la ventana *Propiedades de la base de datos*, las opciones *Actualizar estadísticas automáticamente* y *Actualizar estadísticas automática y asincrónicamente* deben establecerse en **True**.
  
Las actualizaciones de las estadísticas pueden ser sincrónicas (el valor predeterminado) o asincrónicas. 

* Con las actualizaciones sincrónicas de las estadísticas, las consultas siempre se compilan y ejecutan con estadísticas actualizadas. Si las estadísticas no están actualizadas, el optimizador de consultas esperará a que lo estén antes de compilar y ejecutar la consulta. 

* En el caso de las actualizaciones asincrónicas de las estadísticas, las consultas se compilan con estadísticas existentes, incluso aunque estas no estén actualizadas. Si, al compilar la consulta, las estadísticas no están actualizadas, el optimizador de consultas podría elegir un plan de consultas poco óptimo. Las estadísticas suelen actualizarse poco después. Como de costumbre, las consultas que se compilen tras completarse las actualizaciones de las estadísticas se beneficiarán del hecho de poder usar estadísticas actualizadas.   

Considere la posibilidad de usar las estadísticas sincrónicas al realizar las operaciones que cambian la distribución de los datos, como truncar una tabla o realizar una actualización masiva de un gran porcentaje de las filas. Si no actualiza manualmente las estadísticas después de finalizar la operación, el uso de estadísticas sincrónicas garantizará que las estadísticas estén actualizadas antes de ejecutar las consultas en los datos modificados.  
  
Considere el uso de estadísticas asincrónicas para lograr tiempos de respuesta a la consulta más predecibles en los escenarios siguientes:  
  
* Su aplicación ejecuta frecuentemente la misma consulta, consultas similares o los planes de consulta almacenados en memoria caché similares. Sus tiempos de respuesta a la consulta podrían ser más predecibles con actualizaciones asincrónicas de las estadísticas que con actualizaciones sincrónicas, porque el Optimizador de consultas puede ejecutar las consultas de entrada sin esperar a que las estadísticas se actualicen. Esto evita que se retrasen algunas consultas, pero no otras.  
  
* Su aplicación ha experimentado tiempos de espera de solicitud de cliente causados por una o varias consultas que aguardaban la actualización de estadísticas. En algunos casos, la espera por las estadísticas sincrónicas podría causar errores en aplicaciones con tiempos de espera agresivos.  

> [!NOTE]
> Las estadísticas de las tablas temporales locales siempre se actualizan de forma sincrónica, independientemente de la opción AUTO_UPDATE_STATISTICS_ASYNC. Las estadísticas de las tablas temporales globales se actualizan de forma sincrónica o asincrónica, según la opción AUTO_UPDATE_STATISTICS_ASYNC establecida para la base de datos de usuario.

La actualización asincrónica de las estadísticas se realiza mediante una solicitud en segundo plano. Cuando la solicitud está lista para escribir estadísticas actualizadas en la base de datos, intenta adquirir un bloqueo de modificación del esquema en el objeto de metadatos de estadísticas. Si una sesión diferente ya mantiene un bloqueo en el mismo objeto, la actualización asincrónica de las estadísticas se bloqueará hasta que se pueda adquirir el bloqueo de modificación del esquema. Del mismo modo, la sesión en segundo plano de actualización asincrónica de las estadísticas, que ya contiene o está esperando a adquirir el bloqueo de modificación del esquema, puede bloquear las sesiones que necesiten adquirir un bloqueo de estabilidad de esquema en el objeto de metadatos de estadísticas para compilar una consulta. Por lo tanto, para las cargas de trabajo con compilaciones de consultas muy frecuentes y las actualizaciones frecuentes de las estadísticas, el uso de estadísticas asincrónicas puede aumentar la probabilidad de que se produzcan incidencias de simultaneidad debido a los bloqueos.

En Azure SQL Database y Azure SQL Managed Instance, puede evitar posibles incidencias de simultaneidad mediante la actualización asincrónica de las estadísticas. Para ello, habilite la [configuración de ámbito de la base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY. Con esta configuración habilitada, la solicitud en segundo plano esperará a adquirir el bloqueo de modificación del esquema (Sch-M) en una cola independiente de prioridad baja, lo que permitirá que otras solicitudes sigan compilando consultas con estadísticas existentes. Cuando ninguna otra sesión mantenga un bloqueo en el objeto de metadatos de estadísticas, la solicitud en segundo plano adquirirá el bloqueo de modificación del esquema y la actualización de estadísticas. En el caso improbable de que la solicitud en segundo plano no pueda adquirir el bloqueo en un período de tiempo de expiración de varios minutos, se anulará la actualización asincrónica de las estadísticas y estas no se actualizarán hasta que se desencadene otra actualización automática de las estadísticas o se [actualicen manualmente](update-statistics.md).

> [!Note]
> La opción de configuración del ámbito de la base de datos ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY ahora está disponible en Azure SQL Database y Azure SQL Managed Instance, y está previsto que también se incluya en SQL Server vNext. 

#### <a name="incremental"></a>INCREMENTAL  
 Cuando la opción INCREMENTAL de CREATE STATISTICS está establecida en ON, se generan estadísticas por partición. Si se establece en OFF, el árbol de estadísticas se quita y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcula las estadísticas. El valor predeterminado es OFF. Este valor invalida la propiedad INCREMENTAL de la base de datos. Para obtener más información sobre cómo crear estadísticas incrementales, vea [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md). Para obtener más información sobre cómo crear estadísticas por partición automáticamente, vea [Propiedades de la base de datos &#40;Página Opciones&#41;](../../relational-databases/databases/database-properties-options-page.md#automatic) y [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) (Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;). 
  
 Cuando se agregan particiones a una tabla grande, las estadísticas deben actualizarse para que incluyan las particiones nuevas. Sin embargo, el tiempo necesario para examinar la tabla completa (opción FULLSCAN o SAMPLE) puede ser bastante largo. Además, no es necesario examinar toda la tabla porque puede que solo se necesiten las estadísticas de las particiones nuevas. La opción incremental crea y almacena estadísticas sobre cada partición y, cuando se actualiza, solo actualiza las estadísticas de las particiones que necesitan estadísticas nuevas.  
  
 Si no se admiten las estadísticas por partición, la opción se omite y se genera una advertencia. Las estadísticas incrementales no se admiten para los siguientes tipos de estadísticas:  
  
* Estadísticas creadas con índices que no están alineados por partición con la tabla base.  
* Estadísticas creadas sobre bases de datos secundarias legibles AlwaysOn.  
* Estadísticas creadas sobre bases de datos de solo lectura.  
* Estadísticas creadas sobre índices filtrados.  
* Estadísticas creadas sobre vistas.  
* Estadísticas creadas sobre tablas internas.  
* Estadísticas creadas con índices espaciales o índices XML.  
  
**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores. 
  
## <a name="when-to-create-statistics"></a><a name="CreateStatistics"></a> Cuándo crear las estadísticas  
 El Optimizador de consultas ya permite crear las estadísticas de las siguientes formas:  
  
1.  El Optimizador de consultas crea las estadísticas para índices en tablas o vistas cuando se crea el índice. Estas estadísticas se crean en las columnas de clave del índice. Si el índice es un índice filtrado, el Optimizador de consultas crea las estadísticas filtradas en el mismo subconjunto de filas especificado para el índice filtrado. Para obtener más información sobre los índices filtrados, vea [Crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md) y [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  

    > [!NOTE]
    > A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], las estadísticas no se crean examinando todas las filas de la tabla cuando se crea o se vuelve a compilar un índice con particiones. En su lugar, el Optimizador de consultas usa el algoritmo de muestreo predeterminado para generar estadísticas. Después de actualizar una base de datos con índices con particiones, puede observar una diferencia en los datos del histograma para estos índices. Este cambio de comportamiento puede no afectar al rendimiento de las consultas. Para obtener estadísticas sobre índices con particiones examinando todas las filas de la tabla, use `CREATE STATISTICS` o `UPDATE STATISTICS` con la cláusula `FULLSCAN`. 
  
2.  El optimizador de consultas crea las estadísticas para las columnas únicas de predicados de consulta cuando [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) está activada.  

En la mayoría de las consultas, estos dos métodos de creación de estadísticas aseguran un plan de consulta de alta calidad; en unos casos, puede mejorar los planes de consulta creando estadísticas adicionales con la instrucción [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) . Estas estadísticas adicionales pueden capturar correlaciones estadísticas que el Optimizador de consultas no tiene en cuenta cuando crea estadísticas para índices o columnas únicas. Su aplicación podría tener las correlaciones estadísticas adicionales en los datos de la tabla que, si se calcula en un objeto de estadísticas, podrían habilitar el Optimizador de consultas para mejorar los planes de consulta. Por ejemplo, las estadísticas filtradas en un subconjunto de filas de datos o las estadísticas de varias columnas en columnas de predicado de consulta podrían mejorar el plan de consulta.  
  
Al crear las estadísticas con la instrucción CREATE STATISTICS, recomendamos mantener la opción AUTO_CREATE_STATISTICS para que el Optimizador de consultas continúe creando rutinariamente estadísticas de columna única para las columnas de predicado de consulta. Para obtener más información sobre los predicados de consulta, vea [Condición de búsqueda &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
Considere la creación de estadísticas con la instrucción CREATE STATISTICS cuando se aplique cualquiera de los escenarios siguientes:  

* El Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] sugiere crear las estadísticas. 
* El predicado de consulta contiene varias columnas correlacionadas que ya no están en el mismo índice.  
* La consulta realiza la selección entre un subconjunto de datos.  
* La consulta ha perdido estadísticas.  
  
### <a name="query-predicate-contains-multiple-correlated-columns"></a>Predicado de consulta con varias columnas correlacionadas  
Cuando un predicado de consulta contiene varias columnas que tienen relaciones y dependencias entre columnas, las estadísticas sobre esas columnas podrían mejorar el plan de consulta. Las estadísticas sobre varias columnas contienen estadísticas de correlación entre columnas, llamadas *densidades*, que no están disponibles en las estadísticas de columna única. Las densidades pueden mejorar las estimaciones de cardinalidad cuando los resultados de la consulta dependen de relaciones de los datos entre varias columnas.  
  
Si las columnas ya están en el mismo índice, el objeto de estadísticas de varias columnas ya existe y no es necesario crearlo manualmente. Si las columnas no están ya en el mismo índice, puede crear las estadísticas de varias columnas creando un índice en las columnas o usando la instrucción [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md). Se necesitan más recursos del sistema para mantener un índice que para mantener un objeto de estadísticas. Si la aplicación no requiere el índice de varias columnas, puede economizar en recursos del sistema creando el objeto de estadísticas sin crear el índice.  
  
Al crear las estadísticas de varias columnas, el orden de las columnas en la definición del objeto de estadísticas afecta a la efectividad de las densidades para realizar las estimaciones de cardinalidad. El objeto de estadísticas almacena las densidades correspondientes a cada prefijo de las columnas de clave en la definición del objeto de estadísticas. Para más información sobre las densidades, vea la sección [Densidad](#density) en esta página.  
  
Para crear densidades que sean útiles para las estimaciones de cardinalidad, las columnas del predicado de consulta deben coincidir con uno de los prefijos de columnas de la definición del objeto de estadísticas. Por ejemplo, en el siguiente caso se crea un objeto de estadísticas de varias columnas en las columnas `LastName`, `MiddleName` y `FirstName`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.stats  
    WHERE name = 'LastFirst'  
    AND object_ID = OBJECT_ID ('Person.Person'))  
DROP STATISTICS Person.Person.LastFirst;  
GO  
CREATE STATISTICS LastFirst ON Person.Person (LastName, MiddleName, FirstName);  
GO  
```  
  
En este ejemplo, el objeto de estadísticas `LastFirst` tiene densidades para los siguientes prefijos de columna: `(LastName)`, `(LastName, MiddleName)` y `(LastName, MiddleName, FirstName)`. La densidad no está disponible para `(LastName, FirstName)`. Si la consulta utiliza `LastName` y `FirstName` sin utilizar `MiddleName`, la densidad no está disponible para las estimaciones de cardinalidad.  
  
### <a name="query-selects-from-a-subset-of-data"></a>La consulta realiza la selección entre un subconjunto de datos  
Cuando el Optimizador de consultas crea las estadísticas para las columnas únicas e índices, crea las estadísticas para los valores de todas las filas. Cuando las consultas realizan la selección de entre un subconjunto de filas, y ese subconjunto de filas tiene una distribución de datos única, las estadísticas filtradas pueden mejorar los planes de consulta. Puede crear estadísticas filtradas usando la instrucción [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) con la cláusula [WHERE](../../t-sql/queries/where-transact-sql.md) para definir la expresión del predicado de filtro.  
  
Por ejemplo, con [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], cada producto de la tabla `Production.Product` pertenece a una de las cuatro categorías de la tabla `Production.ProductCategory`: Bikes, Components, Clothing y Accessories. Cada una de las categorías tiene una distribución de datos diferente en función del peso: el peso de las bicicletas (bikes) va de 13,77 a 30,0, el de los componentes (components) de 2,12 a 1050,00 con algunos valores NULL, todos los pesos de la ropa (clothing) son NULL, lo mismo que los de los accesorios (accessories).  
  
Utilizando las bicicletas (Bikes) como ejemplo, las estadísticas filtradas para todos los pesos de las bicicletas proporcionarán estadísticas más precisas al Optimizador de consultas y podrán mejorar la calidad del plan de consulta en comparación con las estadísticas de tabla completa o las estadísticas no existentes en la columna del peso (Weight). La columna de peso de bicicleta es una buena candidata para las estadísticas filtradas, pero no necesariamente para un índice filtrado si el número de búsquedas de peso es relativamente pequeño. La ganancia de rendimiento para las búsquedas que proporciona un índice filtrado no podría ser mayor que el mantenimiento adicional y el costo de almacenamiento de agregar un índice filtrado a la base de datos.  
  
La instrucción siguiente crea las estadísticas filtradas `BikeWeights` en todas las subcategorías de Bikes. La expresión de predicado filtrado define las bicicletas enumerando todas las subcategorías de bicicleta con la comparación `Production.ProductSubcategoryID IN (1,2,3)`. El predicado no puede utilizar el nombre de categoría Bikes porque está almacenado en la tabla Production.ProductCategory, mientras que todas las columnas de la expresión del filtro deben estar en la misma tabla.  
  
[!code-sql[StatisticsDDL#FilteredStats2](../../relational-databases/statistics/codesnippet/tsql/statistics_1.sql)]  
  
El Optimizador de consultas puede utilizar estadísticas filtradas de `BikeWeights` para mejorar el plan de consulta correspondiente a la consulta siguiente. Esta segunda selecciona todas las bicicletas cuyo peso es superior a `25`.  
  
```sql  
SELECT P.Weight AS Weight, S.Name AS BikeName  
FROM Production.Product AS P  
    JOIN Production.ProductSubcategory AS S   
    ON P.ProductSubcategoryID = S.ProductSubcategoryID  
WHERE P.ProductSubcategoryID IN (1,2,3) AND P.Weight > 25  
ORDER BY P.Weight;  
GO  
```  
  
### <a name="query-identifies-missing-statistics"></a>Consulta que identifica las estadísticas que faltan  
Si un error u otro evento evitan que el Optimizador de consultas cree las estadísticas, el Optimizador de consultas creará el plan de consulta sin utilizar las estadísticas. El Optimizador de consultas marca las estadísticas como perdidas e intenta regenerar las estadísticas la siguiente vez que se ejecuta la consulta.  
  
Las estadísticas perdidas se indican mediante advertencias (el nombre de la tabla aparece en rojo) cuando el plan de ejecución de una consulta se representa gráficamente mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Además, la supervisión de la clase de eventos **Missing Column Statistics** con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] indica cuándo se han perdido las estadísticas. Para obtener más información, vea [Errores y advertencias &#40;categoría de eventos del motor de base de datos&#41;](../../relational-databases/event-classes/errors-and-warnings-event-category-database-engine.md).  
  
 Si se han perdido estadísticas, siga estos pasos:  
  
* Compruebe que [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) y [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) están activadas.  
* Compruebe que la base de datos no es de solo lectura. Si la base de datos es de solo lectura, no se puede guardar un nuevo objeto de estadísticas.  
* Cree las estadísticas perdidas usando la instrucción [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).  
  
Cuando faltan las estadísticas de una base de datos de solo lectura o de una instantánea de solo lectura o son obsoletas, [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea y mantiene estadísticas temporales en **tempdb**. Cuando [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea estadísticas temporales, el nombre de las estadísticas se anexan con el sufijo *_readonly_database_statistic* para diferenciar las estadísticas temporales de las permanentes. El sufijo *_readonly_database_statistic* está reservado para las estadísticas que genera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los scripts para las estadísticas temporales se pueden crear y reproducir en una base de datos de lectura-escritura. Cuando se crea el script, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] cambia el sufijo del nombre de las estadísticas de *_readonly_database_statistic* a *_readonly_database_statistic_scripted*.  
  
Solo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede crear y actualizar las estadísticas temporales. No obstante, puede eliminar las estadísticas temporales y supervisar las propiedades de estadísticas mediante las mismas herramientas que se usan para las estadísticas permanentes:  
  
* Elimine las estadísticas temporales con la instrucción [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md).  
* Supervise las estadísticas con las vistas de catálogo **[sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)** y **[sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)** . **sys_stats** incluye la columna **is_temporary** para indicar las estadísticas que son permanentes y las que son temporales.  
  
 Debido a que las estadísticas temporales se almacenan en **tempdb**, el reinicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provoca que desaparezcan todas las estadísticas temporales.  
    
## <a name="when-to-update-statistics"></a><a name="UpdateStatistics"></a> Cuándo actualizar las estadísticas  
 El Optimizador de consultas determina cuándo las estadísticas podrían quedar obsoletas y, a continuación, las actualiza cuando es necesario para un plan de consulta. En algunos casos, puede mejorar el plan de consulta y, por consiguiente, mejorar su rendimiento. Para ello, puede actualizar las estadísticas con más frecuencia que la que se da cuando la opción [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) está activada. Puede actualizar las estadísticas con la instrucción UPDATE STATISTICS o con el procedimiento almacenado sp_updatestats.  
  
 La actualización de las estadísticas asegura que las consultas se compilan con estadísticas actualizadas. Sin embargo, la actualización de las estadísticas hace que las consultas se vuelvan a compilar. Recomendamos no actualizar las estadísticas con demasiada frecuencia, porque hay que elegir el punto válido entre la mejora de los planes de consulta y el tiempo empleado en volver a compilar las consultas. Las compensaciones específicas dependen de su aplicación.  
  
 Al actualizar las estadísticas con UPDATE STATISTICS o con sp_updatestats, recomendamos mantener la opción AUTO_UPDATE_STATISTICS activada para que el Optimizador de consultas continúe actualizando rutinariamente las estadísticas. Para obtener más información sobre cómo actualizar las estadísticas en una columna, un índice, una tabla o una vista indexada, vea [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md). Para obtener más información sobre cómo actualizar las estadísticas para todas las tablas internas y definidas por el usuario de la base de datos, vea el procedimiento almacenado [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md).  
  
 Para determinar cuándo se actualizaron por última vez las estadísticas, use las funciones [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) o [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md).  
  
 Considere la actualización de las estadísticas en las condiciones siguientes:  
  
* Los tiempos de ejecución de la consulta son lentos.  
* Se producen operaciones de inserción en columnas de clave ascendentes o descendentes.  
* Después de las operaciones de mantenimiento.  

### <a name="query-execution-times-are-slow"></a>Tiempos de ejecución de las consultas lentos  
 Si los tiempos de respuesta de la consulta son lentos o impredecibles, asegúrese de que las consultas tienen estadísticas actualizadas antes de realizar los pasos adicionales de la solución de problemas.  
  
### <a name="insert-operations-occur-on-ascending-or-descending-key-columns"></a>Operaciones de inserción en columnas de clave ascendentes o descendentes  
 Las estadísticas de columnas de clave ascendentes o descendentes, como IDENTITY o las columnas de marca de tiempo en tiempo real, podrían requerir actualizaciones de las estadísticas más frecuentes que las que realiza el Optimizador de consultas. Las operaciones de inserción anexan nuevos valores a las columnas ascendentes o descendentes. El número de filas agregado podría ser demasiado pequeño para desencadenar una actualización de las estadísticas. Si las estadísticas no están actualizadas y las consultas se seleccionan de las filas recientemente agregadas, las estadísticas actuales no tendrán estimaciones de cardinalidad para estos nuevos valores. Esto puede producir estimaciones de cardinalidad inexactas y un rendimiento lento de las consultas.  
  
 Por ejemplo, una consulta que selecciona entre las fechas de pedidos de venta más recientes tendrá estimaciones de cardinalidad inexactas si las estadísticas no se han actualizado para incluir las estimaciones de cardinalidad correspondientes a las fechas de pedidos de venta más recientes.  
  
### <a name="after-maintenance-operations"></a>Después de las operaciones de mantenimiento  
 Considere la actualización de las estadísticas después de haber realizado procedimientos de mantenimiento que cambian la distribución de los datos, como truncar una tabla o realizar una inserción masiva de un porcentaje grande de las filas. Esto puede evitar los retrasos futuros en el procesamiento de la consulta, mientras las consultas esperan las actualizaciones automáticas de las estadísticas.  
  
 Otras operaciones, como regenerar, desfragmentar o reorganizar un índice, no cambian la distribución de los datos. Por consiguiente, no necesita actualizar las estadísticas después de realizar las operaciones [ALTER INDEX REBUILD](../../t-sql/statements/alter-index-transact-sql.md#rebuilding-indexes), [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md), [DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md) o [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md#reorganizing-indexes). El Optimizador de consultas actualiza las estadísticas cuando regenera un índice en una tabla o vista con ALTER INDEX REBUILD o DBCC DBREINDEX. Sin embargo, esta actualización de las estadísticas es un subproducto de la recreación del índice. El Optimizador de consultas no actualiza las estadísticas después de las operaciones DBCC INDEXDEFRAG o ALTER INDEX REORGANIZE. 
 
> [!TIP]
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4, utilice la opción PERSIST_SAMPLE_PERCENT de [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md) o [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md) para establecer y mantener un porcentaje de muestreo específico para las actualizaciones de estadísticas posteriores en las que no se especifica explícitamente un porcentaje de muestreo.

### <a name="automatic-index-and-statistics-management"></a>Administración automática de índice y estadísticas

Aproveche soluciones como la [desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) para administrar automáticamente las actualizaciones de estadísticas y la desfragmentación de índices para una o varias bases de datos. Este procedimiento elige automáticamente si se debe volver a generar o reorganizar un índice según su nivel de fragmentación, entre otros parámetros y actualiza las estadísticas con un umbral lineal.
  
##  <a name="queries-that-use-statistics-effectively"></a><a name="DesignStatistics"></a> Consultas que usan eficazmente las estadísticas  
 Algunas implementaciones de consulta, como las variables locales y las expresiones complejas en el predicado de consulta, pueden conducir a planes de consulta que no son óptimos. Las siguientes instrucciones de diseño de consulta para el uso eficaz de las estadísticas pueden evitarlo. Para obtener más información sobre los predicados de consulta, vea [Condición de búsqueda &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 Puede mejorar los planes de consulta aplicando instrucciones de diseño de consulta que utilicen las estadísticas con eficacia para mejorar las *estimaciones de cardinalidad* en las expresiones, variables y funciones utilizadas en los predicados de consulta. Cuando el Optimizador de consultas no conoce el valor de una expresión, variable o función, no conoce qué valor ha de buscar en el histograma y, por consiguiente, no puede recuperar del histograma la mejor estimación de cardinalidad. En cambio, el Optimizador de consultas basa la estimación de cardinalidad en el número medio de filas por valor distinto para todas las filas buscadas en el histograma. El resultado son estimaciones de cardinalidad poco óptimas, además de dañar el rendimiento de la consulta. Para más información sobre los histogramas, vea la sección [Histograma](#histogram) en esta página o [sys.dm_db_stats_histogram](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md).
  
 Las instrucciones siguientes describen cómo escribir las consultas para mejorar los planes de consulta mediante la mejora de las estimaciones de cardinalidad.  
  
### <a name="improving-cardinality-estimates-for-expressions"></a>Mejora de las estimaciones de cardinalidad en las expresiones  
Para mejorar las estimaciones de cardinalidad en las expresiones, siga estas instrucciones:  
  
* Siempre que sea posible, simplifique las expresiones utilizando constantes. El Optimizador de consultas no evalúa todas las funciones y expresiones que contienen constantes antes de determinar las estimaciones de cardinalidad. Por ejemplo, simplifique la expresión `ABS(-100)` en `100`.  
  
* Si la expresión utiliza varias variables, considere la creación de una columna calculada para la expresión y, a continuación, cree estadísticas o un índice en la columna calculada. Por ejemplo, el predicado de consulta `WHERE PRICE + Tax > 100` podría tener una mejor estimación de cardinalidad si crea una columna calculada para la expresión `Price + Tax`.  
  
### <a name="improving-cardinality-estimates-for-variables-and-functions"></a>Mejora de las estimaciones de cardinalidad en las variables y funciones  
Para mejorar las estimaciones de cardinalidad en las variables y funciones, siga estas instrucciones:  
  
* Si el predicado de consulta utiliza una variable local, considere volver a escribir la consulta usando un parámetro en lugar de una variable local. No se conoce el valor de una variable local cuando el Optimizador de consultas crea el plan de ejecución de la consulta. Cuando una consulta utiliza un parámetro, el Optimizador de consultas utiliza la estimación de cardinalidad para el primer valor de parámetro real que se pasa al procedimiento almacenado.  
  
* Le recomendamos que use una tabla estándar o una tabla temporal para retener los resultados de las funciones con valores de tabla de múltiples instrucciones (mstvf). El Optimizador de consultas no crea estadísticas para las funciones con valores de tabla de múltiples instrucciones. Con este enfoque, el Optimizador de consultas puede crear estadísticas sobre las columnas de tabla y utilizarlas para crear un plan de consulta mejor.  
  
* Considere el uso de una tabla estándar o una tabla temporal como un reemplazo para las variables de tabla. El Optimizador de consultas no crea estadísticas para las variables de tabla. Con este enfoque, el Optimizador de consultas puede crear estadísticas sobre las columnas de tabla y utilizarlas para crear un plan de consulta mejor. Hay que determinar si se utilizar una tabla temporal o una variable de tabla; las variables de tabla utilizadas en los procedimientos almacenados causan menos recompilaciones del procedimiento almacenado que las tablas temporales. Dependiendo de la aplicación, el uso de una tabla temporal en lugar de una variable de tabla no mejora el rendimiento.  
  
* Si un procedimiento almacenado contiene una consulta que utiliza un parámetro pasado, evite cambiar el valor del parámetro dentro del procedimiento almacenado antes de utilizarlo en la consulta. Las estimaciones de cardinalidad para la consulta se basan en el valor de parámetro pasado y no en el valor actualizado. Para evitar cambiar el valor del parámetro, puede reescribir la consulta para utilizar los dos procedimientos almacenados.  
  
     Por ejemplo, el procedimiento almacenado siguiente `Sales.GetRecentSales` cambia el valor del parámetro `@date` cuando `@date` es NULL.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
    AS BEGIN  
        IF @date IS NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
     Si la primera llamada al procedimiento almacenado `Sales.GetRecentSales` pasa un NULL para el parámetro `@date`, el Optimizador de consultas compilará el procedimiento almacenado con la estimación de cardinalidad para `@date = NULL` aunque no se llame al predicado de consulta con `@date = NULL`. Esta estimación de cardinalidad podría ser significativamente diferente del número de filas del resultado de la consulta real. Como resultado, el Optimizador de consultas podría elegir un plan de consulta poco óptimo. Para ayudar a evitar esto, puede rescribir el procedimiento almacenado en dos procedimientos del modo siguiente:  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNullRecentSales (@date datetime)  
    AS BEGIN  
        IF @date is NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        EXEC Sales.GetNonNullRecentSales @date;  
    END  
    GO  
    IF OBJECT_ID ( 'Sales.GetNonNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNonNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNonNullRecentSales (@date datetime)  
    AS BEGIN  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
### <a name="improving-cardinality-estimates-with-query-hints"></a>Mejora de las estimaciones de cardinalidad con sugerencias de consulta  
 Para mejorar las estimaciones de cardinalidad para las variables locales, puede usar las sugerencias de consulta `OPTIMIZE FOR <value>` o `OPTIMIZE FOR UNKNOWN` con RECOMPILE. Para obtener más información, vea [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
 En algunas aplicaciones, volver a compilar la consulta cada vez que la ejecuta podría tardar demasiado tiempo. La sugerencia de consulta `OPTIMIZE FOR` puede servir de ayuda incluso si no usa la opción `RECOMPILE`. Por ejemplo, podría agregar una opción `OPTIMIZE FOR` al procedimiento almacenado Sales.GetRecentSales para especificar una fecha concreta. En el ejemplo siguiente se agrega la opción `OPTIMIZE FOR` al procedimiento Sales.GetRecentSales.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetRecentSales;  
GO  
CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
AS BEGIN  
    IF @date is NULL  
        SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
    SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
    WHERE h.SalesOrderID = d.SalesOrderID  
    AND h.OrderDate > @date  
    OPTION ( OPTIMIZE FOR ( @date = '2004-05-01 00:00:00.000'))  
END;  
GO  
```  
  
### <a name="improving-cardinality-estimates-with-plan-guides"></a>Mejora de las estimaciones de cardinalidad con guías de plan  
 En algunos casos, las directrices de diseño de consultas podrían no aplicarse, bien porque no pueda cambiar la consulta, bien porque la sugerencia de consulta RECOMPILE cause demasiadas recompilaciones. Puede utilizar las guías de plan para especificar otras sugerencias, como USE PLAN, para controlar el comportamiento de la consulta mientras investiga los cambios de la aplicación con el proveedor de la aplicación. Para obtener más información acerca de las guías de plan, vea [Plan Guides](../../relational-databases/performance/plan-guides.md).  
  
  
## <a name="see-also"></a>Consulte también  
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [Creación de índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md)   
 [Controlar el comportamiento de Autostat (AUTO_UPDATE_STATISTICS) en SQL Server](https://support.microsoft.com/help/2754171)   
 [STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)   
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)  
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)  
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)    
 [Desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)