---
description: Guía de arquitectura de páginas y extensiones
title: Guía de arquitectura de páginas y extensiones | Microsoft Docs
ms.custom: ''
ms.date: 03/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- page and extent architecture guide
- guide, page and extent architecture
ms.assetid: 83a4aa90-1c10-4de6-956b-7c3cd464c2d2
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae2204f4d4562b35cde84b66608efee9dc727562
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236479"
---
# <a name="pages-and-extents-architecture-guide"></a>Guía de arquitectura de páginas y extensiones
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

La página es la unidad fundamental del almacenamiento de datos en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Una extensión es una colección de ocho páginas físicamente contiguas. Las extensiones ayudan a administrar las páginas con eficacia. En esta guía se describen las estructuras de datos que se utilizan para administrar páginas y extensiones en todas las versiones de SQL Server. Comprender la arquitectura de las páginas y las extensiones es importante para diseñar y desarrollar bases de datos que funcionen eficazmente.

## <a name="pages-and-extents"></a>Páginas y extensiones

La unidad fundamental del almacenamiento de datos en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es la página. El espacio en disco asignado a un archivo de datos (.mdf o .ndf) en una base de datos se divide lógicamente en páginas numeradas de forma contigua de 0 a n. Las operaciones de E/S de disco se realizan en el nivel de página. Es decir, SQL Server lee o escribe páginas de datos enteras.

Las extensiones son una colección de ocho páginas físicamente contiguas; se utilizan para administrar las páginas de forma eficaz. Todas las páginas se organizan en extensiones.

### <a name="pages"></a>Páginas

Pongamos el ejemplo de un libro normal: todo su contenido está escrito en las páginas. De forma similar a un libro, en SQL Server todas las filas de datos están escritas en las páginas. En un libro, todas las páginas tienen el mismo tamaño físico. Del mismo modo, en SQL Server todas las páginas de datos tienen el mismo tamaño: 8 kilobytes. En un libro, la mayoría de las páginas contienen los datos (el contenido principal del libro) y algunas páginas contienen metadatos sobre el contenido (por ejemplo, tabla de contenido e índice). De nuevo, SQL Server no es distinto: la mayoría de las páginas contienen filas de datos reales almacenadas por los usuarios. Se denominan páginas de datos y páginas de texto e imagen (para casos especiales). Las páginas de índice contienen referencias de índice sobre dónde están los datos y, por último, hay páginas del sistema que almacenan varios metadatos sobre la organización de los datos (páginas PFS, GAM, SGAM, IAM, DCM y BCM). Consulte la tabla siguiente para ver los tipos de página y su descripción.

Tal como se ha mencionado, en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], el tamaño de página es de 8 KB. Esto significa que las bases de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tienen 128 páginas por megabyte. Cada página empieza con un encabezado de 96 bytes, que se utiliza para almacenar la información del sistema acerca de la página. Esta información incluye el número de página, el tipo de página, el espacio disponible en la página y el Id. de unidad de asignación del objeto propietario de la página.

En la siguiente tabla se muestran los tipos de página utilizados en los archivos de datos de una base de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

|Tipo de página | Contenido |
|-------|-------|
|data |Filas de datos con todos los datos, excepto los datos de text, ntext, image, nvarchar(max), varchar(max), varbinary(max) y xml, cuando el texto de la fila está configurado en ON. |
|Índice |Entradas de índice. |
|Texto o imagen |Tipos de datos de objetos grandes: (datos de text, ntext, image, nvarchar(max), varchar(max), varbinary(max) y xml) <br> Columnas de longitud variable cuando la fila de datos supera los 8 KB: (varchar, nvarchar, varbinary y sql_variant) |
|Mapa de asignación global, Mapa de asignación global compartido |Información acerca de si se han asignado las extensiones. |
|Espacio disponible en páginas (PFS) |Información acerca de la asignación de páginas y el espacio disponible disponible en las páginas. |
|Mapa de asignación de índices |Información acerca de las extensiones utilizadas por una tabla o un índice por unidad de asignación. |
|mapa cambiado masivamente |Información acerca de las extensiones modificadas por operaciones masivas desde la última instrucción BACKUP LOG por unidad de asignación. |
|Mapa cambiado diferencial |Información acerca de las extensiones que han cambiado desde la última instrucción BACKUP DATABASE por unidad de asignación. |

> [!NOTE]
> Los archivos de registro no contienen páginas, contienen series de registros.

Las filas de datos se colocan en las páginas una a continuación de otra, empezando inmediatamente después del encabezado. Al final de la página, comienza una tabla de desplazamiento de fila y cada una de esas tablas contiene una entrada para cada fila de la página. Cada entrada de desplazamiento de fila registra la distancia del primer byte de la fila desde el inicio de la página. Por lo tanto, la función de la tabla de desplazamiento de filas es ayudar a SQL Server buscar filas en una página muy rápidamente. Las entradas de la tabla de desplazamiento de fila están en orden inverso a la secuencia de las filas de la página.

![page_architecture](../relational-databases/media/page-architecture.gif)

#### <a name="large-row-support"></a>Compatibilidad con filas largas  

Las filas no pueden abarcar páginas; no obstante, se pueden apartar de la página de la fila ciertas partes de la fila para que ésta pueda tener un tamaño mucho mayor. La cantidad máxima de datos y de sobrecarga que está contenida en una única fila de una página es de 8060 bytes (8 KB). Sin embargo, esto no incluye los datos almacenados en el tipo de página Texto o imagen. 

Esta restricción es menos estricta para tablas que contienen columnas varchar, nvarchar, varbinary o sql_variant. Cuando el tamaño de fila total de todas las columnas variables y fijas de una tabla excede el límite de 8060 bytes, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mueve dinámicamente una o más columnas de longitud variable a páginas de la unidad de asignación ROW_OVERFLOW_DATA, empezando por la columna con el mayor ancho. 

Esto se realiza cuando una operación de inserción o actualización aumenta el tamaño total de la fila más allá del límite de 8060 bytes. Cuando una columna se mueve a una página de la unidad de asignación ROW_OVERFLOW_DATA, se mantiene un puntero de 24 bytes de la página original de la unidad de asignación IN_ROW_DATA. Si una operación posterior reduce el tamaño de la fila ,[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vuelve a mover las columnas dinámicamente a la página de datos original. 

##### <a name="row-overflow-considerations"></a>Consideraciones acerca del desbordamiento de fila 

Como se mencionó anteriormente, una fila no puede residir en varias páginas y puede desbordarse si el tamaño combinado de los campos de tipo de datos de longitud variable supera el límite de 8060 bytes. Para ilustrar esto, se puede crear una tabla con dos columnas: una varchar(7000) y otra varchar(2000). De forma individual, ninguna de las columnas excede los 8060 bytes, pero si se combinan sí que podrían hacerlo, si se rellena todo el ancho de cada columna. SQL Server puede trasladar dinámicamente la columna de longitud variable varchar (7000) a las páginas de la unidad de asignación ROW_OVERFLOW_DATA. Al combinar las columnas varchar, nvarchar, varbinary, sql_variant o de tipo definido por el usuario CLR que superen los 8060 bytes por fila, tenga en cuenta lo siguiente:
-  El movimiento de registros grandes a otra página tiene lugar de forma dinámica a medida que la longitud de los registros aumenta en función de las operaciones de actualización. Las operaciones de actualización que reducen la longitud de los registros pueden hacer que los registros se vuelvan a colocar en la página original de la unidad de asignación IN_ROW_DATA. El hecho de consultar y realizar otras operaciones de selección, como puedan ser ordenaciones o combinaciones en registros grandes que contengan datos de desbordamiento de fila, ralentiza el tiempo de procesamiento, ya que estos registros se procesan sincrónicamente en lugar de asincrónicamente.   
   Por lo tanto, al diseñar una tabla con varias columnas varchar, nvarchar, varbinary, sql_variant o de tipo definido por el usuario CLR, tenga en cuenta el porcentaje de filas que tienen posibilidades de desbordarse y la frecuencia con la que se consultarán estos datos de desbordamiento. Si es muy probable que se realicen consultas con frecuencia en muchas filas de los datos de desbordamiento de fila, considere la posibilidad de normalizar la tabla para que algunas columnas se muevan a otra tabla. Más adelante, puede realizar la consulta en una operación JOIN asincrónica. 
-  La longitud de las columnas individuales debe estar comprendida en el límite de 8000 bytes para las columnas varchar, nvarchar, varbinary, sql_variant y de tipo definido por el usuario CLR. Solo la combinación de sus longitudes puede superar el límite de fila de 8.060 bytes de una tabla.
-  La suma de otras columnas de tipos de datos, incluidos los datos char y nchar, debe estar comprendida en el límite de fila de 8060 bytes. Los datos de objeto grande no están sometidos al límite de fila de 8.060 bytes. 
-  La clave de índice de un índice agrupado no puede contener columnas varchar con datos existentes en la unidad de asignación ROW_OVERFLOW_DATA. Si se crea un índice agrupado en una columna varchar y los datos existentes están en la unidad de asignación IN_ROW_DATA, no se realizarán correctamente las siguientes acciones de inserción o actualización en la columna que intenten insertar los datos de manera no consecutiva. Para obtener más información acerca de las unidades de asignación, vea Organización de tablas e índices.
-  Puede incluir columnas que contengan datos de desbordamiento de fila como columnas de clave o sin clave de un índice no clúster.
-  El tamaño máximo del registro para las tablas que usan columnas dispersas es de 8.018 bytes. Si los datos convertidos más los datos de registro existentes superan los 8018 bytes, se devolverá el error [MSSQLSERVER ERROR 576](../relational-databases/errors-events/database-engine-events-and-errors.md). Si las columnas pasan de dispersas a no dispersas y viceversa, el Motor de base de datos mantendrá una copia de los datos del registro actuales. Esto duplica temporalmente el almacenamiento necesario para el registro.
-  Para obtener más información sobre las tablas o los índices que pueden contener datos de desbordamiento de fila, use la función de administración dinámica [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).

### <a name="extents"></a>Extents 

Las extensiones son la unidad básica en la que se administra el espacio. Una extensión consta de ocho páginas contiguas físicamente, es decir 64 KB. Esto significa que las bases de datos de SQL Server tienen 16 extensiones por megabyte.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tiene dos tipos de extensiones: 

* Las extensiones **uniformes** son propiedad de un único objeto; solo el objeto propietario puede usar las ocho páginas de la extensión.
* Las extensiones **mixtas**, que pueden estar compartidas por hasta ocho objetos. Cada una de las 8 páginas de la extensión puede ser propiedad de un objeto diferente.

![Extensiones uniformes y mixtas](../relational-databases/media/extents.gif)

Hasta [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] (incluido), [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no asigna extensiones completas a tablas con pequeñas cantidades de datos. A las tablas o índices nuevos se les suelen asignar páginas de extensiones mixtas. Cuando la tabla o el índice crecen hasta el punto de ocupar ocho páginas, se pasan a extensiones uniformes para las posteriores asignaciones. Si crea un índice de una tabla existente que dispone de filas suficientes para generar ocho páginas en el índice, todas las asignaciones del índice están en extensiones uniformes. 

A partir de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], el valor predeterminado para la mayoría de asignaciones en una base de datos de usuario y tempdb es usar extensiones uniformes, salvo en el caso de las asignaciones que pertenezcan a las primeras ocho páginas de una [cadena IAM](#IAM). Las asignaciones relativas a bases de datos modelo, master y msdb conservarán el comportamiento anterior. 

> [!NOTE]
> Hasta [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] (incluido), puede usarse la marca de seguimiento 1118 para cambiar la asignación predeterminada para que siempre use extensiones uniformes. Para más información sobre esta marca de seguimiento, vea [DBCC TRACEON: marcas de seguimiento (Transact-SQL)](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).   
>   
> A partir de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], la función proporcionada por TF 1118 se habilita automáticamente para tempdb y para todas las bases de datos de usuario. Para las bases de datos de usuario, este comportamiento se controla mediante la opción `SET MIXED_PAGE_ALLOCATION` de `ALTER DATABASE`, con el valor predeterminado establecido en Desactivado, y la marca de seguimiento 1118 no tiene ningún efecto. Para más información, vea [Opciones de ALTER DATABASE SET (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-set-options.md).

A partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], la función del sistema `sys.dm_db_database_page_allocations` puede notificar información de asignación de páginas relativa a una base de datos, tabla, índice o partición.

> [!IMPORTANT]
> La función del sistema `sys.dm_db_database_page_allocations` no está documentada y está sujeta a cambios. la compatibilidad no está garantizada. 

A partir de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], la función del sistema [sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) está disponible y devuelve información sobre una página de una base de datos. La función devuelve una fila que contiene la información del encabezado de la página, incluidos object_id, index_id y partition_id. Esta función reemplaza la necesidad de usar `DBCC PAGE` en la mayoría de los casos.

## <a name="managing-extent-allocations-and-free-space"></a>Administrar las asignaciones de extensiones y el espacio disponible 

Las estructuras de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que administran asignaciones de extensiones y realizan un seguimiento del espacio disponible tienen una estructura relativamente sencilla. Esto tiene las siguientes ventajas: 

* La información del espacio libre está densamente empaquetada, de modo que esta información está contenida en relativamente pocas páginas.   
  Esto aumenta la velocidad al reducir la cantidad de lecturas del disco necesarias para recuperar la información de asignación. También incrementa la posibilidad de que las páginas de asignación permanezcan en la memoria, lo que elimina aún más lecturas. 

* La mayor parte de la información de asignación no está encadenada. Esto simplifica el mantenimiento de la información de asignación.    
  La asignación o cancelación de asignación de las páginas se puede hacer con más rapidez. Esto reduce el conflicto entre las tareas simultáneas que necesiten asignar o cancelar la asignación de páginas. 

### <a name="managing-extent-allocations"></a>Administrar las asignaciones de extensiones

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa dos tipos de mapas de asignación para registrar la asignación de las extensiones: 

- **Mapa de asignación global (GAM)**    
  Las páginas GAM registran las extensiones que han sido asignadas. Cada GAM cubre 64 000 extensiones o casi 4 GB de datos. La página GAM tiene un bit por cada extensión del intervalo que cubre. Si el bit es 1, la extensión está disponible; si el bit es 0, la extensión está asignada. 

- **Mapa de asignación global compartido (SGAM)**    
  Las páginas SGAM registran las extensiones que actualmente se están utilizando como extensiones mixtas y además tienen al menos una página sin utilizar. Cada SGAM cubre 64 000 extensiones o casi 4 GB de datos. La SGAM tiene un bit por cada extensión del intervalo que cubre. Si el bit es 1, la extensión se está utilizando como extensión mixta y tiene una página disponible. Si el bit es 0, la extensión no se utiliza como extensión mixta o se trata de una extensión mixta cuyas páginas están todas en uso. 

Todas las extensiones tienen establecidos los siguientes patrones de bits en las página GAM y SGAM, basados en su uso actual. 

|Uso actual de la extensión | Bit en GAM | Bit en SGAM |
|---------|----------|------| 
|Disponible, no se utiliza |1 |0 |
|Extensión uniforme o extensión mixta completa |0 |0 |
|Extensión mixta con páginas disponibles |0 |1 |
 
Esto hace que los algoritmos de administración de las extensiones sean sencillos. 
-   Para asignar una extensión uniforme, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] busca un bit 1 en la página GAM y lo establece en 0. 
-   Para buscar una extensión mixta con páginas disponibles, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] busca un bit 1 en la página SGAM. 
-   Para asignar una extensión mixta, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] busca 1 bit en la página GAM, lo establece en 0 y, a continuación, establece también en 1 el bit correspondiente de la página SGAM. 
-   Para desasignar una extensión, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] se asegura de que el bit de la página GAM esté establecido en 1 y el bit de la página SGAM en 0. Los algoritmos internos usados realmente por [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] son más complejos que los mencionados en este artículo, puesto que [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] distribuye los datos en la base de datos de manera uniforme. Sin embargo, incluso los algoritmos reales quedan simplificados al no tener que administrar cadenas de información de asignación de extensiones.

### <a name="tracking-free-space"></a>Realizar un seguimiento del espacio libre

Las páginas **Espacio disponible en páginas (PFS)** registran el estado de asignación de cada página, si una página concreta está asignada y la cantidad de espacio libre en cada página. La PFS tiene un byte por página, que registra si la página está asignada y, en tal caso, si está vacía, llena entre el 1 y el 50 %, entre el 51 y el 80 %, entre el 81 y el 95 % o entre el 96 y el 100 %.

Una vez que se ha asignado una extensión a un objeto, el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza las páginas PFS para registrar las páginas de la extensión que están asignadas o libres. Esta información se utiliza cuando el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] tiene que asignar una nueva página. La cantidad de espacio libre de una página solo se mantiene en páginas de texto e imagen y de montón. Se utiliza cuando el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] tiene que buscar una página con espacio libre disponible para almacenar una fila recién insertada. Los índices no necesitan que se haga un seguimiento del espacio libre en páginas, porque el punto en el que se inserta una nueva fila se establece mediante los valores de clave del índice.

En el archivo de datos se agrega una nueva página PFS, GAM o SGAM para cada intervalo adicional del que se realiza el seguimiento. Por tanto, hay una nueva página PFS 8088 páginas después de la primera página PFS, y páginas PFS adicionales en intervalos posteriores de 8088 páginas. Como ejemplo, el id. de página 1 es una página PFS, el id. de página 8088 es una página PFS, el id. de página 16176 es una página PFS, etc. Hay una nueva página GAM de 64 000 extensiones después de la primera página GAM, y realiza un seguimiento de las 64 000 extensiones que la siguen; la secuencia continúa en intervalos de 64 000 extensiones. Del mismo modo, hay una nueva página de SGAM 64 000 extensiones después de la primera página SGAM y páginas SGAM adicionales en intervalos de 64 000 extensiones posteriores. En la siguiente ilustración se muestra la secuencia de páginas que usa [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] para asignar y administrar extensiones.

![manage_extents](../relational-databases/media/manage-extents.gif)

## <a name="managing-space-used-by-objects"></a><a name="IAM"></a> Administración del espacio utilizado por los objetos 

Una página del **Mapa de asignación de índices (IAM)** asigna las extensiones en una parte de 4 GB de un archivo de base de datos usado por una unidad de asignación. Existen tres tipos de unidades de asignación:

- IN_ROW_DATA   
    Contiene una partición de un montón o un índice.

- LOB_DATA   
   Contiene tipos de objetos grandes (LOB), como XML, VARBINARY(max) y VARCHAR(max).

- ROW_OVERFLOW_DATA   
   Contiene datos de longitud variable almacenados en columnas VARCHAR, NVARCHAR, VARBINARY o SQL_VARIANT que superan el límite de tamaño de fila de 8060 bytes. 

Cada partición de un montón o un índice contiene al menos una unidad de asignación IN_ROW_DATA. También puede contener una unidad de asignación LOB_DATA o ROW_OVERFLOW_DATA en función del esquema del montón o el índice.

Una página IAM cubre un intervalo de 4 GB en un archivo, lo que equivale a la cobertura de una página GAM o SGAM. Si la unidad de asignación contiene extensiones de más de un archivo o más de un intervalo de 4 GB de un archivo, se vincularán varias páginas IAM en una cadena IAM. Por lo tanto, cada unidad de asignación tiene como mínimo una página IAM para cada archivo en el que tiene extensiones. También puede haber más de una página IAM en un archivo si el intervalo de las extensiones del archivo asignado a la unidad de asignación supera el intervalo que una sola página IAM puede registrar. 

![iam_pages](../relational-databases/media/iam-pages.gif)

Las páginas IAM se asignan según se necesitan para cada unidad de asignación y se ubican en el archivo de forma aleatoria. La vista del sistema `sys.system_internals_allocation_units` apunta a la primera página IAM de una unidad de asignación. Todas las páginas IAM de esa unidad de asignación se vinculan en una cadena IAM.

> [!IMPORTANT]
> La vista de sistema `sys.system_internals_allocation_units` se ha diseñado para uso interno y está sujeta a cambios, por lo que la compatibilidad no está garantizada. Esta vista no está disponible en [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]. 

![iam_chain](../relational-databases/media/iam-chain.gif)
 
Páginas IAM vinculadas en una cadena por unidad de asignación Una página IAM tiene un encabezado que indica la extensión inicial del intervalo de extensiones asignado por la página IAM. La página IAM también tiene un mapa de bits grande en el que cada bit representa una extensión. El primer bit del mapa representa la primera extensión del intervalo, el segundo bit representa la segunda extensión, etc. Si un bit es 0, la extensión que representa no está asignada a la unidad de asignación propietaria de IAM. Si el bit es 1, la extensión que representa está asignada a la unidad de asignación propietaria de la página IAM.

Si [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] necesita insertar una fila nueva y no hay espacio disponible en la página actual, utiliza las páginas IAM y PFS para buscar una página para la asignación o, en el caso de un montón o una página de texto o imagen, una página con espacio suficiente para contener la fila. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utiliza las páginas IAM para buscar las extensiones asignadas a la unidad de asignación. Para cada extensión, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] busca las páginas PFS para ver si existe una página que se pueda utilizar. Cada página IAM y PFS cubre muchas páginas de datos, por lo que en una base de datos hay pocas páginas IAM y PFS. Esto significa que las páginas IAM y PFS están generalmente en el grupo de búferes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y se pueden buscar con rapidez. Para los índices, el punto de inserción de una fila nueva se establece mediante la clave de índice, pero cuando se necesita una página nueva, se produce el proceso descrito anteriormente.

[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] solo asigna una nueva extensión a una unidad de asignación cuando no puede encontrar rápidamente una página en una extensión existente con espacio suficiente para almacenar la fila que se va a insertar. 

<a name="ProportionalFill"></a>[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] asigna las extensiones entre las que están disponibles en el grupo de archivos siguiendo un **algoritmo de asignación de relleno proporcional**. Si en el mismo grupo de archivos con dos archivos, uno de ellos tiene el doble de espacio disponible que el otro, se asignarán dos páginas del archivo con más espacio disponible por cada página asignada del otro archivo. Esto significa que los archivos de un grupo de archivos tienen un porcentaje de espacio utilizado similar. 

## <a name="tracking-modified-extents"></a>Realizar un seguimiento de las extensiones modificadas 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa dos estructuras de datos internas para realizar un seguimiento de las extensiones modificadas mediante operaciones de copia masiva y de las extensiones modificadas desde la última copia de seguridad completa. Esas estructuras de datos aceleran en gran medida las copias de seguridad diferenciales. También aceleran el registro de las operaciones de copia masiva cuando una base de datos utiliza el modelo de recuperación optimizado para cargas masivas de registros. Al igual que las páginas del mapa de asignación global (GAM) y del mapa de asignación global compartido (SGAM), estas nuevas estructuras son mapas de bits en los que cada bit representa una única extensión. 

- **Mapa cambiado diferencial (DCM)**    
   Realiza un seguimiento de las extensiones que han cambiado desde la última instrucción `BACKUP DATABASE`. Si el bit de una extensión es 1, esta se ha modificado desde la última instrucción `BACKUP DATABASE`. Si el bit es 0, la extensión no se ha modificado. Las copias de seguridad diferenciales solo leen las páginas DCM para determinar las extensiones que se han modificado. Esto reduce enormemente el número de páginas que debe recorrer una copia de seguridad diferencial. El tiempo de ejecución de una copia de seguridad diferencial es proporcional al número de extensiones modificadas desde la última instrucción BACKUP DATABASE y no al tamaño global de la base de datos. 

- **Mapa cambiado masivamente (BCM)**    
   Realiza un seguimiento de las extensiones que se han modificado mediante operaciones de registro masivo desde la última instrucción `BACKUP LOG`. Si el bit de una extensión es 1, esta se ha modificado mediante una operación de registro masivo después de la última instrucción `BACKUP LOG`. Si el bit es 0, la extensión no se ha modificado mediante operaciones de registro masivo. Aunque las páginas BCM aparecen en todas las bases de datos, son relevantes únicamente cuando la base de datos está utilizando el modelo de recuperación optimizado para cargas masivas de registros. En este modelo de recuperación, cuando se ejecuta `BACKUP LOG`, el proceso de copia de seguridad recorre los BCM buscando extensiones que se hayan modificado. A continuación incluye dichas extensiones en la copia de seguridad del registro. Esto permite recuperar operaciones de registro masivo si se restaura la base de datos a partir de una copia de seguridad y una secuencia de copias de seguridad de registro de transacciones. Las páginas BCM no son relevantes en una base de datos que está utilizando el modelo de recuperación simple, porque no se registran las operaciones de registro masivo. No son relevantes en una base de datos que utiliza el modelo de recuperación completa, porque este modelo trata las operaciones de registro masivo como operaciones de registro completo. 

El intervalo entre las páginas DCM y BCM es el mismo que el intervalo entre las páginas GAM y SGAM, es decir, 64.000 extensiones. Las páginas DCM y BCM se colocan después de las páginas GAM y SGAM en un archivo físico:

![special_page_order](../relational-databases/media/special-page-order.gif)

## <a name="see-also"></a>Consulte también
[sys.allocation_units &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)     
[Montones &#40;tablas sin índices agrupados&#41;](../relational-databases/indexes/heaps-tables-without-clustered-indexes.md#heap-structures)       
[sys.dm_db_page_info](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)     
[Leer páginas](../relational-databases/reading-pages.md)   
[Escribir páginas](../relational-databases/writing-pages.md)   
