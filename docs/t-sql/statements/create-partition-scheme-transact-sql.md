---
description: CREATE PARTITION SCHEME (Transact-SQL)
title: CREATE PARTITION SCHEME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE PARTITION SCHEME
- SCHEME
- PARTITION SCHEME
- CREATE_PARTITION_SCHEME_TSQL
- SCHEME_TSQL
- PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- partitioned indexes [SQL Server], schemes
- partitioned tables [SQL Server], schemes
- CREATE PARTITION SCHEME statement
- partition schemes [SQL Server], creating
- filegroups [SQL Server], partitions
- partitioned indexes [SQL Server], filegroups
- partitioned tables [SQL Server], filegroups
- mapping partitions [SQL Server]
ms.assetid: 5b21c53a-b4f4-4988-89a2-801f512126e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 079bd2ee428b706230e8d4283355603a09300d3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188554"
---
# <a name="create-partition-scheme-transact-sql"></a>CREATE PARTITION SCHEME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un esquema en la base de datos actual que asigna a grupos de archivos las particiones de una tabla o índice con particiones. El número y el dominio de las particiones de una tabla o índice con particiones se determinan en una función de partición. Antes de crear un esquema de partición, es necesario crear una función de partición de una instrucción [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md).  

>[!NOTE]
>En Azure SQL Database solamente se admiten grupos de archivos principales.  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE PARTITION SCHEME partition_scheme_name  
AS PARTITION partition_function_name  
[ ALL ] TO ( { file_group_name | [ PRIMARY ] } [ ,...n ] )  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *partition_scheme_name*  
 Es el nombre del esquema de partición. Los nombres de esquema de partición deben ser únicos en la base de datos y cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 *partition_function_name*  
 Es el nombre de la función de partición que utiliza el esquema de partición. Las particiones creadas por la función de partición se asignan a los grupos de archivos especificados en el esquema de partición. *partition_function_name* ya debe existir en la base de datos. Una única partición no puede contener grupos de archivos FILESTREAM y no FILESTREAM a la vez.  
  
 ALL  
 Especifica que todas las particiones se asignan al grupo de archivos suministrado en *file_group_name* o al grupo de archivos principal si se especifica **[** PRIMARY **]**. Si se especifica ALL, solo se puede especificar un valor de *file_group_name*.  
  
 *file_group_name* |  **[** PRIMARY **]** [ **,** _...n_]  
 Especifica los nombres de los grupos de archivos que almacenarán las particiones especificadas por *partition_function_name*. *file_group_name* ya debe existir en la base de datos.  
  
 Si se especifica **[** PRIMARY **]**, la partición se almacena en el grupo de archivos principal. Si se especifica ALL, solo se puede especificar un valor de *file_group_name*. A partir de la partición 1, las particiones se asignan a los grupos de archivos en el orden en que éstos aparecen en [ **,** _...n_]. Se puede especificar el mismo valor de *file_group_name* más de una vez en [ **,** _...n_]. Si *n* no es suficiente para contener el número de particiones especificadas en *partition_function_name*, CREATE PARTITION SCHEME generará un error.  
  
 Si *partition_function_name* genera menos particiones que grupos de archivos, el primer grupo de archivos sin asignar se marca como NEXT USED y se muestra un mensaje informativo donde se indica el grupo de archivos NEXT USED. Si se especifica ALL, solo *file_group_name* mantiene su propiedad NEXT USED para este *partition_function_name*. El grupo de archivos NEXT USED recibirá una partición adicional si se crea una en una instrucción ALTER PARTITION FUNCTION. Si desea crear más grupos de archivos sin asignar para almacenar las nuevas particiones, utilice ALTER PARTITION SCHEME.  
  
 Al especificar el grupo de archivos principal en *file_group_name* [ 1 **,** _...n_], PRIMARY debe estar delimitado, como en **[** PRIMARY **]** , porque es una palabra clave.  
  
 Solo se admite PRIMARY para [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]. Vea el ejemplo E de abajo. 
  
## <a name="permissions"></a>Permisos  
 Se pueden utilizar los permisos siguientes para ejecutar CREATE PARTITION SCHEME:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se va a crear el esquema de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se va a crear el esquema de partición.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-partition-scheme-that-maps-each-partition-to-a-different-filegroup"></a>A. Crear un esquema de partición que asigne cada partición a un grupo de archivos diferente  
 En el ejemplo siguiente se crea una función de partición para crear cuatro particiones en una tabla o en un índice. Después, se crea un esquema de partición que especifica los grupos de archivos que van a contener cada una de las cuatro particiones. En este ejemplo se asume que los grupos de archivos ya existen en la base de datos.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
TO (test1fg, test2fg, test3fg, test4fg);  
```  
  
 Las particiones de una tabla que usa la función de partición `myRangePF1` en la columna de partición **col1** se asignarían como se muestra en la tabla siguiente.  
  
||||||  
|-|-|-|-|-|  
|**Grupo de archivos**|`test1fg`|`test2fg`|`test3fg`|`test4fg`|  
|**Partición**|1|2|3|4|  
|**Valores**|**col1** <= `1`|**col1** > `1` AND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="b-creating-a-partition-scheme-that-maps-multiple-partitions-to-the-same-filegroup"></a>B. Crear un esquema de partición que asigne varias particiones al mismo grupo de archivos  
 Si todas las particiones se asignan al mismo grupo de archivos, utilice la palabra clave ALL. Sin embargo, si se asignan varias particiones (no todas) al mismo grupo de archivos, el nombre del grupo de archivos debe repetirse, tal como se muestra en el ejemplo siguiente.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF2 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS2  
AS PARTITION myRangePF2  
TO ( test1fg, test1fg, test1fg, test2fg );  
```  
  
 Las particiones de una tabla que usa la función de partición `myRangePF2` en la columna de partición **col1** se asignarían como se muestra en la tabla siguiente.  
  
||||||  
|-|-|-|-|-|  
|**Grupo de archivos**|`test1fg`|`test1fg`|`test1fg`|`test2fg`|  
|**Partición**|1|2|3|4|  
|**Valores**|**col1** <= `1`|**col1** > 1 AND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="c-creating-a-partition-scheme-that-maps-all-partitions-to-the-same-filegroup"></a>C. Crear un esquema de partición que asigne todas las particiones al mismo grupo de archivos  
 En el ejemplo siguiente se crea la misma función de partición que en los ejemplos anteriores y se crea un esquema de partición que asigna todas las particiones al mismo grupo de archivos.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF3 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS3  
AS PARTITION myRangePF3  
ALL TO ( test1fg );  
```  
  
### <a name="d-creating-a-partition-scheme-that-specifies-a-next-used-filegroup"></a>D. Crear un esquema de partición que especifica un grupo de archivos NEXT USED  
 En el ejemplo siguiente se crea la misma función de partición que en los ejemplos anteriores y se crea un esquema de partición que especifica un número de grupos de archivos superior al número de particiones creadas por la función de partición asociada.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF4 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS4  
AS PARTITION myRangePF4  
TO (test1fg, test2fg, test3fg, test4fg, test5fg)  
```  
  
 Al ejecutar la instrucción se devuelve el mensaje siguiente.  
  
Se ha creado correctamente el esquema de partición "myRangePS4". "test5fg" tiene la marca de grupo de archivos usado a continuación en el esquema de partición "myRangePS4".  
  
  
 Si la función de partición `myRangePF4` se cambia para agregar una partición, el grupo de archivos `test5fg` recibe la partición que acaba de crearse.  

### <a name="e-creating-a-partition-scheme-only-on-primary"></a>E. Crear un esquema de partición solo en el grupo de archivos con el valor PRIMARY

 En Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], no se permite agregar archivos ni grupos de archivos, pero las tablas con particiones sí que se admiten mediante la creación de particiones únicamente en el grupo de archivos con el valor PRIMARY. 

En el ejemplo siguiente se crea una función de partición para crear cuatro particiones en una tabla o en un índice. Después, se crea un esquema de partición que especifica todas las particiones que se crean en el grupo de archivos PRIMARY.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
ALL TO ( [PRIMARY] );  
```
   
## <a name="see-also"></a>Vea también  
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Crear tablas e índices con particiones](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
