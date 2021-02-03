---
description: Guía de diseño de índices de almacén de columnas
title: Guía de diseño de índices de almacén de columnas | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: fc3e22c2-3165-4ac9-87e3-bf27219c820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8dd4d753f4195a1cf4ae6a92ea8bd617675ba4f7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237594"
---
# <a name="columnstore-indexes---design-guidance"></a>Guía de diseño de índices de almacén de columnas
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Recomendaciones de alto nivel para diseñar índices de almacén de columnas. Unas pocas decisiones adecuadas en cuanto al diseño le pueden ayudar a lograr el alto rendimiento de compresión de datos y de consultas para el que se han diseñado los índices de almacén de columnas. 

## <a name="prerequisites"></a>Requisitos previos

En este artículo se supone que está familiarizado con la terminología y arquitectura de almacenes de columna. Para obtener más información, vea [Introducción a los índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md) y [Arquitectura de índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index).

### <a name="know-your-data-requirements"></a>Conocer los requisitos de datos
Antes de diseñar un índice de almacén de columnas, comprenda tanto como sea posible los requisitos de datos. Por ejemplo, piense las respuestas a estas preguntas:

- ¿Qué tamaño tiene mi tabla?
- ¿Mis consultas principalmente realizan análisis que recorren grandes rangos de valores?  Los índices de almacén de columnas están diseñados para funcionar bien con recorridos de rangos grandes en lugar de buscar valores específicos.
- ¿La carga de trabajo lleva a cabo una gran cantidad de actualizaciones y eliminaciones? Los índices de almacén de columnas funcionan bien cuando los datos son estables. Las consultas se deben actualizar y eliminar menos del 10 % de las filas.
- ¿Tengo tablas de hechos y dimensiones para un almacenamiento de datos?
- ¿Es necesario realizar análisis en una carga de trabajo transaccional? Si es así, vea la guía de diseño del almacén de columnas para análisis operativo en tiempo real.

Podría no necesitar un índice de almacén de columnas. Las tabas de almacén de filas con montones o índices agrupados tienen un mejor rendimiento en las consultas que buscan en datos (intentando encontrar un valor determinado), o en las consultas realizadas en un rango de valores muy acotado. Use índices de almacén de filas en las cargas de trabajo transaccionales, ya que tienden a necesitar búsquedas de tabla, más que recorridos de tabla de rangos grandes.  

## <a name="choose-the-best-columnstore-index-for-your-needs"></a>Elegir el mejor índice de almacén de columnas para sus necesidades

Un índice de almacén de columnas puede ser agrupado o no agrupado.  Un índice de almacén de columnas agrupado puede tener uno o varios índices de árbol B no agrupados. Los índices de almacén de columnas son fáciles de probar. Si crea una tabla como un índice de almacén de columnas, puede convertir fácilmente la tabla en una tabla de almacén de filas quitando el índice de almacén de columnas. 

A continuación se muestra un resumen de las opciones y recomendaciones. 

| Opción de almacén de columnas | Recomendaciones sobre cuándo usar | Compresión |
| :----------------- | :------------------- | :---------- |
| Índice de almacén de columnas agrupado | Se utiliza para:<br></br>1) Carga de trabajo de almacenamiento de datos tradicional con un esquema de estrella o copo de nieve<br></br>2) Cargas de trabajo de Internet de las cosas (IoT) que insertan grandes volúmenes de datos con mínimas actualizaciones y eliminaciones. | Promedio de 10 veces |
| Índices de árbol B no agrupados en un índice de almacén de columnas agrupado | Se utiliza para:<br></br>    1. Exigir restricciones de clave principal y clave externa en un índice de almacén de columnas agrupado.<br></br>    2. Acelerar consultas que buscan valores específicos o pequeños rangos de valores.<br></br>    3. Acelerar actualizaciones y eliminaciones de filas específicas.| Promedio de 10 veces más algún tipo de almacenamiento adicional para las NCI.|
| Índice de almacén de columnas no agrupado en un montón basado en disco o índice de árbol B | Se utiliza para: <br></br>1) Una carga de trabajo OLTP que tiene algunas consultas de análisis. Puede quitar los índices de árbol B creados para el análisis y reemplazarlos por un índice de almacén de columnas no agrupado.<br></br>2) Muchas cargas de trabajo OLTP tradicionales que realizan operaciones de extracción, transformación y carga (ETL) para mover datos a un almacenamiento de datos independiente. Puede eliminar ETL y un almacenamiento de datos independiente mediante la creación de un índice de almacén de columnas no agrupado en algunas de las tablas OLTP. | NCCI es un índice adicional que requiere un 10 % más de almacenamiento de promedio.|
| Índice de almacén de columnas en una tabla en memoria | Se aplican las mismas recomendaciones que para el índice de almacén de columnas no agrupado en una tabla basada en disco, salvo que la tabla base es una tabla en memoria. | El índice de almacén de columnas es un índice adicional.|

## <a name="use-a-clustered-columnstore-index-for-large-data-warehouse-tables"></a>Utilizar un índice de almacén de columnas agrupado para tablas de almacenamiento de datos de gran tamaño
El índice de almacén de columnas agrupado es más que un índice: es el almacenamiento de la tabla principal. Consigue una compresión de datos alta y una mejora significativa en el rendimiento de las consultas en las tablas de dimensiones y hechos de almacenamiento de datos de gran tamaño. Los índices de almacén de columnas agrupados son más adecuados para las consultas de análisis que para las consultas transaccionales, puesto que las consultas de análisis tienden a realizar operaciones en grandes rangos de valores en lugar de buscar valores específicos. 

Considere el uso de un índice de almacén de columnas agrupado cuando:

- Cada partición tenga al menos un millón de filas. Los índices almacén de columnas tengan grupos de filas dentro de cada partición. Si la tabla es demasiado pequeña para rellenar un grupo de filas dentro de cada partición, no obtendrá las ventajas de la compresión del almacén de columnas y el rendimiento de consultas.
- Principalmente, las consultas realizan análisis en rangos de valores. Por ejemplo, para buscar el valor medio de una columna, la consulta debe recorrer todos los valores de columna. A continuación, agrega los valores sumándolos para determinar el promedio.
- La mayoría de las inserciones se realizan en grandes volúmenes de datos con actualizaciones y eliminaciones mínimas. Muchas cargas de trabajo, como Internet de las cosas (IoT), insertan grandes volúmenes de datos con actualizaciones y eliminaciones mínimas. Estas cargas de trabajo pueden aprovechar la compresión y las mejoras del rendimiento de consultas que proviene del uso de un índice de almacén de columnas agrupado.

No utilice un índice de almacén de columnas agrupado cuando:

* La tabla requiera tipo de datos varchar(max), nvarchar(max) o varbinary(max). O bien, diseñe el índice de almacén de columnas para que no incluya estas columnas.
* Los datos de tabla no sean permanentes. Considere el uso de un montón o una tabla temporal cuando necesite almacenar y eliminar los datos rápidamente.
* La tabla tenga menos de un millón de filas por partición. 
* Más de un 10 % de las operaciones en la tabla son actualizaciones y eliminaciones. Una gran cantidad de actualizaciones y eliminaciones provoca fragmentación. La fragmentación afecta a las tasas de compresión y al rendimiento de consultas hasta que se ejecuta una operación denominada reorganizar que obliga a todos los datos a permanecer en el almacén de columnas y elimina la fragmentación. Para más información, vea [Minimizing index fragmentation in columnstore index](/archive/blogs/sqlserverstorageengine/columnstore-index-defragmentation-using-reorganize-command) (Minimizar la fragmentación en índices de almacén de columnas).

Para más información, vea [Columnstore indexes - data warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md) (Almacenamiento de datos de índices de almacén de columnas).

## <a name="add-b-tree-nonclustered-indexes-for-efficient-table-seeks"></a>Agregar índices no agrupados de árbol B para búsquedas eficientes de tabla

A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear índices no agrupados de árbol B como índices secundarios en un índice de almacén de columnas agrupado. El índice no agrupado de árbol B se actualiza cuando se producen cambios en el índice de almacén de columnas. Se trata de una característica eficaz que puede usar en su propio beneficio. 

Mediante el uso del índice de árbol B secundario, puede buscar eficazmente filas específicas sin recorrer todas las filas.  También habrá disponibles otras opciones. Por ejemplo, puede aplicar una restricción de clave principal o externa mediante una restricción UNIQUE en el índice de árbol B. Puesto que un valor que no es único no se insertará en el índice de árbol B, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no podrá insertar ese valor en el almacén de columnas. 

Considere la posibilidad de usar un índice de árbol B en un índice de almacén de columnas para:
* Ejecutar consultas que busquen valores específicos o rangos pequeños de valores.
* Exigir una restricción, como puede ser una restricción de clave principal o clave externa.
* Realizar las operaciones de actualización y eliminación eficientemente. El índice de árbol B puede localizar rápidamente las filas específicas para las actualizaciones y eliminaciones sin recorrer la tabla completa o una partición de una tabla.
* Cuando tenga almacenamiento adicional disponible para almacenar el índice de árbol B.

## <a name="use-a-nonclustered-columnstore-index-for-real-time-analytics"></a>Usar un índice de almacén de columnas no agrupado para análisis operativo en tiempo real

A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede tener un índice de almacén de columnas no agrupado en una tabla basada en disco de almacén de filas o en una tabla OLTP en memoria. Esto permite ejecutar el análisis en tiempo real en una tabla transaccional. Mientras las transacciones se produzcan en la tabla subyacente, puede ejecutar el análisis en el índice de almacén de columnas. Puesto que una tabla administra ambos índices, los cambios están disponibles en tiempo real en los índices de almacén de filas y de almacén de columnas.

Puesto que un índice de almacén de columnas consigue una compresión de datos 10 veces mejor que un índice de almacén de filas, dicho índice solo necesita una pequeña cantidad de almacenamiento adicional. Por ejemplo, si la tabla de almacén de filas comprimida tarda 20 GB, el índice de almacén de columnas podría requerir 2 GB adicionales. El espacio adicional necesario también depende del número de columnas del índice de almacén de columnas no agrupado. 

 Considere el uso de un índice de almacén de columnas no agrupado cuando:

* Ejecute el análisis en tiempo real en una tabla de almacén de filas transaccional. Puede reemplazar los índices existentes de árbol B que estén diseñados para realizar análisis por un índice de almacén de columnas no agrupado. 
  
*   Elimine la necesidad de un almacenamiento de datos independiente. Tradicionalmente, las empresas ejecutan transacciones en una tabla de almacén de filas y luego cargan los datos en un almacenamiento de datos independiente para ejecutar el análisis. Para muchas cargas de trabajo, puede eliminar el proceso de carga y el almacenamiento de datos independiente mediante la creación de un índice de almacén de columnas no agrupado en tablas transaccionales.

  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ofrece varias estrategias para hacer que este escenario sea más eficaz. Es muy fácil probarlo, ya que puede habilitar un índice de almacén de columnas no agrupado sin realizar cambios en su aplicación OLTP. 

Para agregar recursos de procesamiento adicionales, puede ejecutar el análisis en un lugar secundario legible. El uso de un elemento secundario legible separa el procesamiento de la carga de trabajo transaccional y la carga de trabajo de análisis. 

Para más información, vea [Get started with columnstore indexes for real-time operational analytics](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)(Introducción a los índices de almacén de columnas para análisis operativo en tiempo real).

Para más información sobre cómo elegir el mejor índice de almacén de columnas, vea el blog de Sunil Agarwal [Which columnstore index is right for my workload?](/archive/blogs/sql_server_team/columnstore-index-which-columnstore-index-is-right-for-my-workload) (¿Qué índice de almacén de columnas es adecuado para mi carga de trabajo?).

## <a name="use-table-partitions-for-data-management-and-query-performance"></a>Utilizar particiones de tabla para la administración de datos y el rendimiento de consultas
Los índices de columna admiten la creación de particiones, que es una buena manera de administrar y archivar datos. La creación de particiones mejora el rendimiento de consultas limitando las operaciones a una o más particiones.

### <a name="use-partitions-to-make-the-data-easier-to-manage"></a>Utilizar particiones para facilitar la administración de los datos
Para tablas grandes, el único método práctico para administrar rangos de datos es mediante el uso de particiones. Las ventajas de las particiones para tablas de almacén de filas también se aplican a los índices de almacén de columnas. 

Por ejemplo, tanto las tablas de almacén de filas como de almacén de columnas utilizan particiones para:

- Controlar el tamaño de las copias de seguridad incrementales. Puede hacer una copia de seguridad de las particiones para separar grupos de archivos y luego marcarlas como de solo lectura. De esta forma, las futuras copias de seguridad omitirán los grupos de archivos de solo lectura. 
- Ahorrar costos de almacenamiento moviendo una partición más antigua a un almacenamiento menos costoso. Por ejemplo, podría utilizar la conmutación de partición para mover una partición a una ubicación de almacenamiento más económica.
- Realizar operaciones de forma eficaz limitando las operaciones a una partición. Por ejemplo, puede centrarse únicamente en las particiones fragmentadas para el mantenimiento de índices.

Además, con un índice de almacén de columnas, la creación de particiones se utiliza para:

* Ahorrar un 30 % adicional en los costos de almacenamiento. Puede comprimir las particiones más antiguas con las opciones de compresión COLUMNSTORE_ARCHIVE. Los datos serán más lentos para el rendimiento de consultas, lo cual es aceptable si la partición se consulta con poca frecuencia.

### <a name="use-partitions-to-improve-query-performance"></a>Usar particiones para mejorar el rendimiento de consultas

Mediante el uso de particiones, puede limitar las consultas para recorrer solo las particiones específicas, lo que limita el número de filas para recorrer. Por ejemplo, si el índice tiene particiones por año y la consulta está analizando los datos del año pasado, solo necesita recorrer los datos de una partición. 

### <a name="use-fewer-partitions-for-a-columnstore-index"></a>Usar menos particiones para un índice de almacén de columnas

A menos que tenga un tamaño de datos suficientemente grande, un índice de almacén de columnas funciona mejor con menos particiones de las que podría usar para un índice de almacén de filas. Si no tiene al menos un millón de filas por partición, la mayoría de las filas podrían ir al almacén delta donde no disfrutan de la ventaja de rendimiento de la compresión del almacén de columnas. Por ejemplo, si carga un millón de filas en una tabla con particiones de 10 y cada partición recibe 100 000 filas, todas las filas irán a grupos de filas delta. 

Ejemplo:
* Cargue 1 000 000 de filas en una partición o una tabla sin particiones. Se obtiene un grupo de filas comprimido con 1 000 000 de filas. Esto es fantástico para compresión de datos alta y un rendimiento de consultas rápido.
* Cargue 1 000 000 de filas equitativamente en 10 particiones. Cada partición obtiene 100 000 filas, que es menos que el umbral mínimo para la compresión del almacén de columnas. Como resultado, el índice de almacén de columnas podría tener 10 grupos de filas delta con 100 000 filas en cada uno. Existen varias maneras de obligar a los grupos de filas delta al almacén de columnas. Aun así, si estas son las únicas filas del índice de almacén de columnas, los grupos de filas comprimidos serán demasiado pequeños para la mejor compresión y rendimiento de consultas.

Para más información sobre la creación de particiones, vea la publicación de blog de Sunil Agarwal, [Should I partition my columnstore index?](/archive/blogs/sqlserverstorageengine/columnstore-index-should-i-partition-my-columnstore-index) (¿Debo particionar mi índice de almacén de columnas?).

## <a name="choose-the-appropriate-data-compression-method"></a>Elija el método de compresión de datos adecuado
El índice de almacén ofrece dos opciones para la compresión de datos: almacén de columnas compresión de archivo y compresión.El índice de almacén de columnas ofrece dos opciones para la compresión de datos: compresión de almacén de columnas y compresión de archivo. Puede elegir la opción de compresión al crear el índice, o cambiarla más adelante con [ALTER INDEX ... REBUILD](../../t-sql/statements/alter-index-transact-sql.md).

### <a name="use-columnstore-compression-for-best-query-performance"></a>Utilizar la compresión de almacén de columnas para mejorar el rendimiento de consultas
La compresión de almacén de columnas suele alcanzar tasas de compresión 10 veces superiores a los índices de almacén de filas. Es el método de compresión estándar para los índices de almacén de columnas y permite un rendimiento de consultas rápido. 

### <a name="use-archive-compression-for-best-data-compression"></a>Utilice la compresión de archivo para la mejor compresión de datos
La compresión de archivos está diseñada para una compresión máxima cuando el rendimiento de consultas no es tan importante. Logra mayores tasas de compresión de datos que la compresión del almacén de columnas, pero tiene un precio. Tarda más tiempo en comprimir y descomprimir los datos, por lo que no es adecuada para el rendimiento de consultas rápido. 

## <a name="use-optimizations-when-you-convert-a-rowstore-table-to-a-columnstore-index"></a>Usar optimizaciones al convertir una tabla de almacén de filas en un índice de almacén de columnas

Si los datos ya están en una tabla de almacén de filas, puede utilizar [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md) para convertir la tabla en un índice de almacén de columnas agrupado. Hay algunas optimizaciones que mejorarán el rendimiento de consultas después de convertir la tabla, tal y como se describe a continuación.

### <a name="use-maxdop-to-improve-rowgroup-quality"></a>Utilice MAXDOP para mejorar la calidad del grupo de filas
Puede configurar el número máximo de procesadores para convertir un índice agrupado de árbol B o de montón en un índice de almacén de columnas. Para configurar los procesadores, utilice el grado máximo de la opción de paralelismo (MAXDOP). 

Si tiene grandes cantidades de datos, MAXDOP 1 probablemente será demasiado lento.  El aumento de MAXDOP a 4 funciona bien. Si el resultado es que algunos grupos de filas no tienen el número óptimo de filas, puede ejecutar [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md) para mezclarlos en segundo plano.

### <a name="keep-the-sorted-order-of-a-b-tree-index"></a>Mantener el orden de clasificación de un índice de árbol B
Dado que el índice de árbol B ya almacena filas en un orden determinado, conservar ese orden cuando las filas se comprimen en el índice de almacén de columnas puede mejorar el rendimiento de consultas.

El índice del almacén de columnas no ordena los datos, pero utiliza metadatos para realizar el seguimiento de los valores mínimo y máximo de cada segmento de columna en cada grupo de filas.  Al recorrer un rango de valores, puede calcular rápidamente cuándo omitir el grupo de filas. Cuando los datos se ordenan, se pueden omitir más grupos de filas. 

Para conservar el orden de clasificación durante la conversión:
* Utilice [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md) con la cláusula DROP_EXISTING. Esto también conserva el nombre del índice. Si tiene scripts que ya utilizan el nombre del índice del almacén de filas, no necesitará actualizarlos. 

    En este ejemplo se convierte un índice de almacén de filas agrupado en una tabla denominada `MyFactTable` en un índice clúster. El nombre del índice, `ClusteredIndex_d473567f7ea04d7aafcac5364c241e09`, permanece igual.

    ```sql
    CREATE CLUSTERED COLUMNSTORE INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    ON MyFactTable  
    WITH (DROP_EXISTING = ON);  
    ```

## <a name="related-tasks"></a>Related Tasks  
Se trata de tareas para crear y mantener índices de almacén de columnas. 
  
|Tarea|Temas de referencia|Notas|  
|----------|----------------------|-----------|  
|Crear una tabla como un almacén de columnas.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear la tabla como un índice de almacén de columnas agrupado. No es necesario crear primero una tabla de almacén de filas y, luego, convertirla en almacén de columnas.|  
|Crear una tabla de memoria con un índice de almacén de columnas.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear una tabla optimizada para memoria con un índice de almacén de columnas. El índice de almacén de columnas también se puede agregar una vez creada la tabla mediante la sintaxis de ALTER TABLE ADD INDEX.|  
|Convertir una tabla de almacén de filas en un almacén de columnas.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Convierta un árbol binario o un montón existentes en un almacén de columnas. Los ejemplos muestran cómo tratar los índices existentes, así como el nombre del índice, al realizar esta conversión.|  
|Convertir una tabla de almacén de columnas en un almacén de filas.|[CREATE CLUSTERED INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) o [Volver a convertir una tabla de almacén de columnas en un montón de almacén de filas](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |Habitualmente, esta conversión no es necesaria pero puede haber ocasiones en las que necesite realizarla. Los ejemplos muestran cómo convertir un almacén de columnas en un montón o un índice agrupado.|   
|Crear un índice de almacén de columnas en una tabla de almacén de filas.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Una tabla de almacén de filas puede tener un índice de almacén de columnas.  Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], los índices de almacén de columnas pueden tener una condición de filtrado. En los ejemplos se usa la sintaxis básica.|  
|Crear índices de rendimiento para análisis operativos.|[Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|Se describe cómo crear índices de almacén de columnas y de árbol B complementarios para que las consultas OLTP usen los índices de árbol B y, las consultas de análisis, los índices de almacén de columnas.|  
|Crear índices de almacén de columnas de rendimiento para el almacenamiento de datos.|[Almacenamiento de datos de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Se describe cómo usar índices de árbol B en las tablas de almacén de columnas para crear consultas de almacenamiento de datos de rendimiento.|  
|Usar un índice de árbol B para aplicar una restricción de clave principal en un índice de almacén de columnas.|[Almacenamiento de datos de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Se muestra cómo combinar índices de árbol B y de almacén de columnas para aplicar restricciones de clave principal en el índice de almacén de columnas.|  
|Eliminar un índice de almacén de columnas.|[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)|Para eliminar un índice de almacén de columnas, se usa la sintaxis de DROP INDEX estándar que usan los índices de árbol B. Si se elimina un índice de almacén de columnas agrupado, la tabla de almacén de columnas se convertirá en un montón.|  
|Eliminar una fila de un índice de almacén de columnas.|[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Use [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md) para eliminar una fila.<br /><br /> fila **almacén de columna** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marca la fila como eliminada lógicamente, pero no recupera el almacenamiento físico de la fila hasta que se vuelva a generar el índice.<br /><br /> fila **almacén delta** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elimina la fila lógica y físicamente.|  
|Actualizar una fila en el índice de almacén de columnas.|[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)|Use [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md) para actualizar una fila.<br /><br /> fila **almacén de columna** :  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marca la fila como eliminada lógicamente y, después, inserta la fila actualizada en el almacén delta.<br /><br /> fila **almacén delta** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] updates the row in the almacén delta.|  
|Forzar que todas las filas del almacén delta vayan al almacén de columnas.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) ... REBUILD<br /><br /> [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|ALTER INDEX con la opción REBUILD hace que todas las filas vayan al almacén de columnas.|  
|Desfragmentar un índice de almacén de columnas.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)|ALTER INDEX ... REORGANIZE desfragmenta los índices de almacén de columnas en línea.|  
|Combinar tablas con índices de almacén de columnas.|[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)|

## <a name="next-steps"></a>Pasos siguientes
Para crear un índice de almacén de columnas vacío para:

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consulte [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).
* [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], consulte [CREATE TABLE (Azure Synapse Analytics)](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md).

Para obtener más información sobre cómo convertir un índice de montón o de árbol B de almacén de filas existente en un índice de almacén de columnas agrupado, o para crear un índice de almacén de columnas, consulte [CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md).