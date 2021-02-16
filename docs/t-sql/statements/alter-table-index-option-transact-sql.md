---
description: ALTER TABLE index_option (Transact-SQL)
title: " | Microsoft Docs"
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- index_option
ms.assetid: 8a14f12d-2fbf-4036-b8b2-8db3354e0eb7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ed0569b851d03e323164786d6b432c4cf78bf6dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348510"
---
# <a name="alter-table-index_option-transact-sql"></a>ALTER TABLE index_option (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica un conjunto de opciones que se pueden aplicar a un índice que forma parte de una definición de restricción creada con [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
{   
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | IGNORE_DUP_KEY = { ON | OFF }  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF } 
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF } 
  | SORT_IN_TEMPDB = { ON | OFF }   
  | ONLINE = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = { NONE |ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }   
      [ , ...n ] ) ]  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<range> ::=   
<partition_number_expression> TO <partition_number_expression>  
  
<single_partition_rebuild__option> ::=  
{  
    SORT_IN_TEMPDB = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = {NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                           ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 PAD_INDEX **=** { ON | **OFF** }  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica el relleno del índice. El valor predeterminado es OFF.  
  
 ACTIVAR  
 El porcentaje de espacio disponible especificado por FILLFACTOR se aplica a páginas de nivel intermedio del índice.  
  
 No se especifica OFF ni *fillfactor*.  
 Las páginas de nivel intermedio se llenan casi al máximo de su capacidad, dejando espacio suficiente para al menos una fila del tamaño máximo que admite el índice, en función del conjunto de claves de las páginas intermedias.  
  
 FILLFACTOR **=** _fillfactor_  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica un porcentaje que indica cuánto debe llenar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] el nivel hoja de cada página de índice durante la creación o modificación de los índices. El valor especificado debe ser un entero de 1 a 100. El valor predeterminado es 0.  
  
> [!NOTE]  
>  Los valores de factor de relleno 0 y 100 son idénticos en todos los sentidos.  
  
 IGNORE_DUP_KEY **=** { ON | **OFF** }  
 Especifica el tipo de respuesta cuando una operación de inserción intenta insertar valores de clave duplicados en un índice único. La opción IGNORE_DUP_KEY se aplica solamente a operaciones de inserción realizadas tras crear o volver a generar el índice. La opción no tiene efecto cuando se ejecutan [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) o [UPDATE](../../t-sql/queries/update-transact-sql.md). El valor predeterminado es OFF.  
  
 ACTIVAR  
 Se produce un mensaje de advertencia cuando se inserten valores de clave duplicados en un índice único. Solo dan error las filas que infringen la restricción de unicidad.  
  
 Apagado  
 Se produce un mensaje de error cuando se insertan valores de clave duplicados en un índice único. Se revierte toda la operación INSERT.  
  
 IGNORE_DUP_KEY no se puede establecer en ON para los índices creados en una vista, los índices que no sean únicos, los índices XML, los índices espaciales y los índices filtrados.  
  
 Para ver IGNORE_DUP_KEY, use [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 En la sintaxis compatible con versiones anteriores, WITH IGNORE_DUP_KEY es equivalente a WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | **OFF** }  
 Especifica si se vuelven a calcular las estadísticas. El valor predeterminado es OFF.  
  
 ACTIVAR  
 Las estadísticas obsoletas no se vuelven a calcular automáticamente.  
  
 Apagado  
 Se habilita la actualización automática de las estadísticas.  
  
 ALLOW_ROW_LOCKS **=** { **ON** | OFF }  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica si se permiten los bloqueos de fila. El valor predeterminado es ON.  
  
 ACTIVAR  
 Los bloqueos de fila se admiten al obtener acceso al índice. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina cuándo se usan los bloqueos de fila.  
  
 Apagado  
 No se usan los bloqueos de fila.  
  
 ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica si se permiten bloqueos de página. El valor predeterminado es ON.  
  
 ACTIVAR  
 Los bloqueos de página se permiten al obtener acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina el momento en que se usan los bloqueos de página.  
  
 Apagado  
 No se utilizan bloqueos de página.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }

**Válido para** : [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.

Especifica si se deben optimizar la contención de inserción de la última página. El valor predeterminado es OFF. Consulte la sección [Claves secuenciales](./create-index-transact-sql.md#sequential-keys) de la página CREATE INDEX para obtener más información.
 
 SORT_IN_TEMPDB **=** { ON | **OFF** }  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica si los resultados de orden se almacenan en **tempdb**. El valor predeterminado es OFF.  
  
 ACTIVAR  
 Los resultados de ordenación intermedios utilizados para generar el índice se almacenan en **tempdb**. Esto puede reducir el tiempo necesario para crear un índice si **tempdb** y la base de datos de usuarios están en conjuntos de discos distintos. Sin embargo, esto aumenta la cantidad de espacio en disco utilizado durante la generación del índice.  
  
 Apagado  
 Los resultados de orden intermedios se almacenan en la misma base de datos que el índice.  
  
 ONLINE **=** { ON | **OFF** }  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica si las tablas subyacentes y los índices asociados están disponibles para realizar consultas y modificar datos durante la operación de índice. El valor predeterminado es OFF. REBUILD se puede realizar como una operación ONLINE.  
  
> [!NOTE]  
>  No es posible crear índices clúster únicos en línea. Entre estos se incluyen los índices creados a causa de una restricción UNIQUE o KEY PRIMARY.  
  
 ACTIVAR  
 Los bloqueos de tabla de larga duración no se mantienen durante la operación de indización. Durante la fase principal de la operación de índice, solo se mantiene un bloqueo preventivo en la tabla de origen. Esto habilita las consultas o actualizaciones en la tabla subyacente y en los índices. Al inicio de la operación, se mantiene un bloqueo compartido (S) en el objeto de origen durante un período de tiempo muy corto. Al final de la operación, se adquiere un bloqueo S (compartido) sobre el origen durante un corto período, si se está creando un índice no clúster; o bien, se adquiere un bloqueo SCH-M (modificación del esquema) cuando se crea o se quita un índice clúster en línea, y cuando se vuelve a crear un índice clúster o no clúster. Aunque los bloqueos de índice en línea son bloqueos de metadatos cortos, especialmente el bloqueo Sch-M debe esperar a que todas las transacciones de bloqueo se completen en esta tabla. Durante el tiempo de espera, bloqueo Sch-M bloquea las demás transacciones que esperen a este bloqueo al tener acceso a la misma tabla. ONLINE no se puede establecer en ON cuando se crea un índice en una tabla temporal local.  
  
> [!NOTE]  
>  La regeneración de índice en línea puede establecer las opciones *low_priority_lock_wait* que se describen más adelante en esta sección. *low_priority_lock_wait* administra la prioridad de los bloqueos S y Sch-M durante la regeneración de índices en línea.  
  
 Apagado  
 Los bloqueos de tabla se aplican durante la operación de índice. Esto evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación. Una operación de índice sin conexión para crear, volver a crear o quitar un índice clúster, o para volver a crear o quitar un índice no clúster, adquiere un bloqueo de modificación del esquema (Sch-M) de la tabla. Esto evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación. Una operación de índice sin conexión que crea un índice no clúster adquiere un bloqueo compartido (S) en la tabla. Esto evita que se realicen actualizaciones en la tabla subyacente, pero permite la realización de operaciones de lectura, como instrucciones SELECT.  
  
 Para más información, vea [Cómo funcionan las operaciones de índice en línea](../../relational-databases/indexes/how-online-index-operations-work.md).  
  
> [!NOTE]
>  Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 MAXDOP **=** _max_degree_of_parallelism_  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Reemplaza la opción de configuración de **max_degree_of_parallelism** mientras dure la operación de índice. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
 *max_degree_of_parallelism* puede tener estos valores:  
  
 - 1 - Suprime la generación de planes paralelos.  
 - \>1 - Restringe el número máximo de procesadores usados en una operación de índice paralelo para el número especificado.  
 - 0 (valor predeterminado) - Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
 Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 DATA_COMPRESSION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica la opción de compresión de datos para la tabla, el número de partición o el intervalo de particiones especificados. Las opciones son las siguientes:  
  
 Ninguno  
 No se comprimen la tabla ni las particiones especificadas. Solo se aplica a tablas de almacén de filas; no se aplica a tablas de almacén de columnas.  
  
 ROW  
 La tabla o las particiones especificadas se comprimen utilizando la compresión de fila. Solo se aplica a tablas de almacén de filas; no se aplica a tablas de almacén de columnas.  
  
 PAGE  
 La tabla o las particiones especificadas se comprimen utilizando la compresión de página. Solo se aplica a tablas de almacén de filas; no se aplica a tablas de almacén de columnas.  
  
 COLUMNSTORE  
 **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
 Solo se aplica a tablas de almacén de columnas. COLUMNSTORE especifica que se descomprima una partición que se comprimió con la opción COLUMNSTORE_ARCHIVE. Cuando se restauran los datos, el índice COLUMNSTORE sigue comprimido con la compresión de almacén de columnas que se usa para todas las tablas de almacén de columnas.  
  
 COLUMNSTORE_ARCHIVE  
 **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
 Solo se aplica a las tablas de almacén de columnas almacenadas con un índice clúster de almacén de columnas. COLUMNSTORE_ARCHIVE comprime aún más la partición especificada a un tamaño más pequeño. Esto se puede usar para el archivado o para otras situaciones que requieran menos almacenamiento y en las que pueda permitirse más tiempo para el almacenamiento y recuperación.  
  
 Para más información sobre la compresión, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).  
  
ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...*n* ] **)** **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica las particiones a las que se aplica el valor DATA_COMPRESSION. Si la tabla no tiene particiones, el argumento ON PARTITIONS genera un error. Si no se proporciona la cláusula ON PARTITIONS, la opción DATA_COMPRESSION se aplica a todas las particiones de una tabla con particiones.  
  
\<partition_number_expression> se puede especificar de estas maneras:  
  
-   Proporcionar el número de una partición, por ejemplo: EN PARTICIONES (2).  
-   Proporcionar los números de partición para varias particiones individuales separadas por comas, por ejemplo: EN PARTICIONES (1, 5).  
-   Proporcione ambos rangos y las particiones individuales, por ejemplo: EN PARTICIONES (2, 4, 6 A 8).  
  
\<range> se puede especificar como números de partición separados por la palabra TO, como por ejemplo: EN PARTICIONES (6 A 8).  
  
 Para establecer diferentes tipos de compresión de datos para distintas particiones, especifique la opción DATA_COMPRESSION más de una vez, por ejemplo:  
  
```sql  
--For rowstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
)  
  
--For columnstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = COLUMNSTORE ON PARTITIONS (1, 3, 5),   
DATA_COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (2, 4, 6 TO 8)  
)  
```  
  
**\<single_partition_rebuild__option>**  
 En la mayoría de los casos, la regeneración de un índice hace que se vuelvan a generar todas las particiones de un índice con particiones. Cuando las opciones siguientes se aplican a una partición única, no vuelven a generar todas las particiones.  
  
-   SORT_IN_TEMPDB  
-   MAXDOP  
-   DATA_COMPRESSION  
  
**low_priority_lock_wait**  
 **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
 Se realiza una operación **SWITCH** o una regeneración de índices en línea en cuanto no hay operaciones de bloqueo para esta tabla. *WAIT_AT_LOW_PRIORITY* indica que esperará si **SWITCH** o la regeneración de índices en línea no se pueden realizar inmediatamente. La operación mantiene bloqueos de prioridad baja y permite que continúen otras operaciones que mantienen bloqueos en conflicto con la instrucción DDL. Omitir la opción **WAIT AT LOW PRIORITY** equivale a `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.  
  
MAX_DURATION = *time* [**MINUTES** ]  
 El tiempo de espera (valor entero especificado en minutos) durante el cual espera el bloqueo de **SWITCH** o la regeneración de índices en línea, al ejecutar el comando DDL. SWITCH o la operación de regeneración de índices en línea intenta completarse inmediatamente. Si la operación se bloquea durante el tiempo de **MAX_DURATION**, se ejecutará una de las acciones de **ABORT_AFTER_WAIT**. El tiempo de **MAX_DURATION** siempre es en minutos y la palabra **MINUTES** puede omitirse.  
  
ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
 Ninguno  
 Continua la operación **SWITCH** o la regeneración de índices en línea sin cambiar la prioridad de bloqueo (con prioridad normal).  
  
SELF  
 Sale de la operación DDL **SWITCH** o regeneración de índices en línea que se está ejecutando actualmente sin realizar ninguna acción.  
  
BLOCKERS  
 Elimina todas las transacciones de usuario que están bloqueando la operación DDL **SWITCH** o la regeneración de índices en línea de forma que la operación pueda continuar.  
 BLOCKERS necesita el permiso **ALTER ANY CONNECTION**.  
  
## <a name="remarks"></a>Observaciones  
 Para le interesa una descripción completa de las opciones de índice, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-constraint-transact-sql.md)   
 [computed_column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-computed-column-definition-transact-sql.md)   
 [table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  
  
 
