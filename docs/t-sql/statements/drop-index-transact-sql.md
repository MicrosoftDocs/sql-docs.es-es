---
description: DROP INDEX (Transact-SQL)
title: DROP INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_INDEX_TSQL
- DROP INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- nonclustered indexes [SQL Server], removing
- MAXDOP index option, DROP INDEX statement
- index removal [SQL Server]
- spatial indexes [SQL Server], dropping
- removing indexes
- deleting indexes
- dropping indexes
- MOVE TO clause
- clustered indexes, removing
- indexes [SQL Server], dropping
- filtered indexes [SQL Server], dropping
- ONLINE option
- indexes [SQL Server], moving
- XML indexes [SQL Server], dropping
- DROP INDEX statement
ms.assetid: 2b1464c8-934c-405f-8ef7-2949346b5372
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 163b64d3ecd5f4f21cfbbc5fef6944f7610f78ea
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237206"
---
# <a name="drop-index-transact-sql"></a>DROP INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quita uno o más índices XML, filtrados, espaciales o relacionales de la base de datos actual. Puede quitar un índice clúster y mover la tabla resultante a otro grupo de archivos o esquema de partición en una sola transacción especificando la opción MOVE TO.  
  
 La instrucción DROP INDEX no es aplicable a los índices creados mediante la definición de restricciones PRIMARY KEY y UNIQUE. Para quitar la restricción y el índice correspondiente, use [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) con la cláusula DROP CONSTRAINT.  
  
> [!IMPORTANT]
>  La sintaxis definida en `<drop_backward_compatible_index>` dejará de incluirse en una futura versión de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar esta sintaxis en nuevos trabajos de programación y tenga previsto modificar las aplicaciones que actualmente la utilizan. En su lugar, utilice la sintaxis especificada en `<drop_relational_or_xml_index>`. Los índices XML no se pueden quitar utilizando la sintaxis compatible con versiones anteriores.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server (All options except filegroup and filestream apply to Azure SQL Database.)  
  
DROP INDEX [ IF EXISTS ]   
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
| <drop_backward_compatible_index> [ ,...n ]  
}  
  
<drop_relational_or_xml_or_spatial_index> ::=  
    index_name ON <object>   
    [ WITH ( <drop_clustered_index_option> [ ,...n ] ) ]  
  
<drop_backward_compatible_index> ::=  
    [ owner_name. ] table_or_view_name.index_name  
  
<object> ::=  
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
  
<drop_clustered_index_option> ::=  
{  
    MAXDOP = max_degree_of_parallelism  
  | ONLINE = { ON | OFF }  
  | MOVE TO { partition_scheme_name ( column_name )   
            | filegroup_name  
            | "default"   
            }  
  [ FILESTREAM_ON { partition_scheme_name   
            | filestream_filegroup_name   
            | "default" } ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database  
  
DROP INDEX  
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
}  
  
<drop_relational_or_xml_or_spatial_index> ::=   
    index_name ON <object>  
  
<object> ::=   
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP INDEX index_name ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *IF EXISTS*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Quita el índice condicionalmente solo si ya existe.  
  
 *index_name*  
 Es el nombre del índice que se va a quitar.  
  
 *database_name*  
 Es el nombre de la base de datos.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la tabla o la vista.  
  
 *table_or_view_name*  
 Es el nombre de la tabla o vista asociada al índice. Los índices espaciales solo se admiten en tablas.  
  
 Para mostrar un informe de los índices en un objeto, use la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 Azure SQL Database admite el formato de nombre de tres partes nombre_basededatos.[nombre_esquema].nombre_objeto cuando nombre_basededatos es la base de datos actual o tempdb y nombre_objeto comienza con #.  
  
 \<drop_clustered_index_option>  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Controla las opciones de los índices clúster. Estas opciones no se pueden utilizar con otros tipos de índices.  
  
 MAXDOP = *max_degree_of_parallelism*  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] (solo los niveles de rendimiento P2 y P3)  
  
 Reemplaza la opción de configuración de **max_degree_of_parallelism** mientras dure la operación de índice. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
> [!IMPORTANT]  
>  MAXDOP no se admite en índices espaciales o índices XML.  
  
 *max_degree_of_parallelism* puede tener estos valores:  
  
 1  
 Suprime la generación de planes paralelos.  
  
 \>1  
 Restringe el número máximo de procesadores utilizados en una operación de índice paralelo para el número especificado.  
  
 0 (predeterminado)  
 Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
 Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]  
>  Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ONLINE = ON | **OFF**  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Especifica si las tablas subyacentes y los índices asociados están disponibles para realizar consultas y modificar datos durante la operación de índice. El valor predeterminado es OFF.  
  
 ACTIVAR  
 No se mantienen los bloqueos de tabla a largo plazo. Esto permite que continúen las consultas o actualizaciones en la tabla subyacente.  
  
 Apagado  
 Se aplican bloqueos de tabla y la tabla deja de estar disponible mientras dure la operación con índices.  
  
 La opción ONLINE solo se puede especificar cuando se quitan índices clúster. Para obtener más información, vea la sección Comentarios.  
  
> [!NOTE]  
>  Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ **)**  | _filegroup\_name_ |  **"** default **"**  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores. [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] admite "default" como el nombre del grupo de archivos.  
  
 Especifica la ubicación donde se moverán las filas de datos que están actualmente en el nivel hoja del índice clúster. Los datos se mueven a la nueva ubicación en forma de montón. Se puede especificar un esquema de partición o un grupo de archivos como la nueva ubicación, pero es necesario que ya existan. MOVE TO no es válido para vistas indizadas o índices no clúster. Si no se especifica ningún esquema de partición o grupo de archivos, la tabla resultante se colocará en el mismo esquema de partición o grupo de archivos que se definió para el índice clúster.  
  
 Si se quita un índice clúster mediante MOVE TO, se vuelven a generar todos los índices no clúster de la tabla base, pero permanecen en sus grupos de archivos o esquemas de partición originales. Si la tabla base se mueve a un grupo de archivos o esquema de partición diferente, los índices no clúster no se mueven para hacerlos coincidir con la nueva ubicación de la tabla base (montón). Por lo tanto, aunque los índices no clúster estuvieran previamente alineados con el índice clúster, es posible que ya no lo estén con el montón. Para más información sobre la alineación de índices con particiones, vea [Tablas e índices con particiones](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 _partition_scheme_name_ **(** _column_name_ **)**  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Especifica un esquema de partición como la ubicación de la tabla resultante. Es necesario que el esquema de partición se haya creado previamente ejecutando [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) o [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Si no se especifica ninguna ubicación y la tabla tiene particiones, la tabla se incluye en el mismo esquema de partición que el índice clúster existente.  
  
 El nombre de columna del esquema no se limita a las columnas de la definición del índice. Se puede especificar cualquier columna de la tabla base.  
  
 *filegroup_name*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica un grupo de archivos como la ubicación de la tabla resultante. Si no se especifica ninguna ubicación y la tabla no tiene particiones, la tabla resultante se incluye en el mismo grupo de archivos que el índice clúster. El grupo de archivos debe existir previamente.  
  
 **"** default **"**  
 Especifica la ubicación predeterminada de la tabla resultante.  
  
> [!NOTE]
>  En este contexto, el valor predeterminado no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en MOVE TO **"** default **"** o MOVE TO **[** default **]** . Si se especifica el parámetro **"** default **"** , la opción QUOTED_IDENTIFIER debe establecerse en ON para la sesión actual. Esta es la configuración predeterminada. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 FILESTREAM_ON { *partition_scheme_name* | *filestream_filegroup_name* |  **"** default **"** }  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica la ubicación donde se moverá la tabla FILESTREAM que está actualmente en el nivel hoja del índice clúster. Los datos se mueven a la nueva ubicación en forma de montón. Se puede especificar un esquema de partición o un grupo de archivos como la nueva ubicación, pero es necesario que ya existan. FILESTREAM ON no es válida para vistas indizadas ni índices no clúster. Si no se especifica un esquema de partición, los datos se ubicarán en el mismo esquema de partición que se definió para el índice clúster.  
  
 *partition_scheme_name*  
 Especifica un esquema de partición de los datos FILESTREAM. Es necesario que el esquema de partición se haya creado previamente ejecutando [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) o [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). Si no se especifica ninguna ubicación y la tabla tiene particiones, la tabla se incluye en el mismo esquema de partición que el índice clúster existente.  
  
 Si especifica un esquema de partición para MOVE TO, se debe utilizar el mismo esquema de partición para FILESTREAM ON.  
  
 *filestream_filegroup_name*  
 Especifica un grupo de archivos FILESTREAM de los datos FILESTREAM. Si no se especifica ninguna ubicación y la tabla no tiene particiones, los datos se incluyen en el grupo de archivos FILESTREAM predeterminado.  
  
 **"** default **"**  
 Especifica la ubicación predeterminada de los datos FILESTREAM.  
  
> [!NOTE]
>  En este contexto, el valor predeterminado no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en MOVE TO **"** default **"** o MOVE TO **[** default **]** . Si se especifica "default", la opción QUOTED_IDENTIFIER debe tener el valor ON para la sesión actual. Esta es la configuración predeterminada. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
 Cuando se quita un índice no clúster, se quita la definición del índice de los metadatos y las páginas de datos del índice (árbol b) se quitan de los archivos de la base de datos. Cuando se quita un índice clúster, se quita la definición del índice de los metadatos y las filas de datos que se almacenaron en el nivel hoja del índice clúster se almacenan en la tabla resultante no ordenada, un montón. Se recuperará todo el espacio anteriormente ocupado por el índice. Después, se puede utilizar este espacio para cualquier objeto de base de datos.  
  
 Un índice no se puede quitar si el grupo de archivos en el que se encuentra está sin conexión o se ha definido como de solo lectura.  
  
 Cuando se quita el índice clúster de una vista indizada, automáticamente se quitan todos los índices no clúster y las estadísticas creadas automáticamente en la misma vista. Las estadísticas creadas manualmente no se quitan.  
  
 La sintaxis _table\_or\_view\_name_ **.** _index\_name_ se mantiene por compatibilidad con versiones anteriores. Los índices XML o espaciales no se pueden quitar utilizando la sintaxis compatible con versiones anteriores.  
  
 Cuando se quitan índices con 128 extensiones o más, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] difiere las cancelaciones de asignación de páginas reales y sus bloqueos asociados hasta que se confirma la transacción.  
  
 A veces, los índices se quitan y se vuelven crear para reorganizarlos o volver a generarlos, por ejemplo, para aplicar un nuevo valor de factor de relleno o para reorganizar los datos después de una carga masiva. Para ello es más eficaz usar [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md), especialmente en el caso de los índices clúster. ALTER INDEX REBUILD incluye optimizaciones para impedir la sobrecarga que representa la regeneración de los índices no clúster.  
  
## <a name="using-options-with-drop-index"></a>Usar opciones con DROP INDEX  
 Puede establecer las siguientes opciones de índice cuando quite un índice clúster: MAXDOP, ONLINE y MOVE TO.  
  
 Utilice MOVE TO para quitar el índice clúster y mover la tabla resultante a otro grupo de archivos o esquema de partición en una sola transacción.  
  
 Cuando se especifica ONLINE = ON, las consultas y modificaciones de los datos subyacentes e índices no clúster asociados no se bloquean con la transacción DROP INDEX. No se puede quitar más de un índice clúster en línea al mismo tiempo. Para obtener una descripción completa de la opción ONLINE, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 No se puede quitar un índice clúster en línea si el índice está deshabilitado en una vista o contiene columnas de tipo **text**, **ntext**, **image**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** o **xml** en las filas de datos de nivel hoja.  
  
 Para utilizar las opciones ONLINE = ON y MOVE TO se requiere más espacio temporal en el disco.  
  
 Después de quitar un índice, el montón resultante aparece en la vista de catálogo **sys.indexes** con NULL en la columna **name**. Para ver el nombre de la tabla, combine **sys.indexes** con **sys.tables** en **object_id**. Dispone de una consulta de ejemplo en el ejemplo D.  
  
 En los equipos con varios procesadores que ejecutan [!INCLUDE[ssEnterpriseEd2005](../../includes/ssenterpriseed2005-md.md)] o posterior, DROP INDEX puede utilizar más procesadores para realizar las operaciones de recorrido y ordenación asociadas a la eliminación del índice clúster, al igual que hacen otras consultas. Puede configurar de forma manual el número de procesadores que se utilizan para ejecutar la instrucción DROP INDEX especificando la opción de índice MAXDOP. Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
 Cuando se quita un índice clúster, las particiones del montón correspondientes retienen su valor de compresión de datos, a menos que se modifique el esquema de partición. Si se cambia el esquema de la partición, todas las particiones se generaran en un estado sin comprimir (DATA_COMPRESSION = NONE). Para quitar un índice clúster y cambiar el esquema de la partición, es necesario llevar a cabo los dos pasos siguientes:  
  
1.  Quitar el índice clúster.  
  
2.  Modificar la tabla utilizando una opción ALTER TABLE... REBUILD... que especifique la opción de compresión.  
  
Cuando se quita un índice clúster OFFLINE, solo se quitan los niveles superiores de índices clústeres, por lo que la operación es realmente rápida. Cuando se quita un índice clúster ONLINE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve a generar el montón dos veces, una vez para el paso 1 y otra para el paso 2. Para más información sobre la compresión de datos, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).  
  
## <a name="xml-indexes"></a>Índices XML  
 No se pueden especificar opciones al quitar un índice XML. Además, no se puede usar la sintaxis _table\_or\_view\_name_ **.** _index\_name_. Cuando se quita un índice XML principal, todos los índices XML secundarios asociados se quitan automáticamente. Para obtener más información, consulte [Índices XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
## <a name="spatial-indexes"></a>Índices espaciales  
 Los índices espaciales solo se admiten en tablas. Cuando se quita un índice espacial, no se puede especificar ninguna opción ni usar **.** _index\_name_. La sintaxis correcta es la siguiente:  
  
 DROP INDEX *spatial_index_name* ON *spatial_table_name*;  
  
 Para más información sobre los índices espaciales, vea [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar DROP INDEX, se requiere, como mínimo, el permiso ALTER en la tabla o vista. Este permiso se concede de forma predeterminada al rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-dropping-an-index"></a>A. Quitar un índice  
 En el siguiente ejemplo se elimina el índice `IX_ProductVendor_VendorID` en la tabla `ProductVendor` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX IX_ProductVendor_BusinessEntityID   
    ON Purchasing.ProductVendor;  
GO  
```  
  
### <a name="b-dropping-multiple-indexes"></a>B. Quitar varios índices  
 En el ejemplo siguiente se eliminan dos índices en una sola transacción de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX  
    IX_PurchaseOrderHeader_EmployeeID ON Purchasing.PurchaseOrderHeader,  
    IX_Address_StateProvinceID ON Person.Address;  
GO  
```  
  
### <a name="c-dropping-a-clustered-index-online-and-setting-the-maxdop-option"></a>C. Quitar un índice clúster en línea y establecer la opción MAXDOP  
 En el ejemplo siguiente se elimina un índice clúster con la opción `ONLINE` establecida en `ON` y `MAXDOP` establecida en `8`. Dado que no se ha especificado la opción MOVE TO, la tabla resultante se almacena en el mismo grupo de archivos que el índice. En este ejemplo se usa la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
```sql  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials WITH (ONLINE = ON, MAXDOP = 2);  
GO  
```  
  
### <a name="d-dropping-a-clustered-index-online-and-moving-the-table-to-a-new-filegroup"></a>D. Quitar un índice clúster en línea y mover la tabla a un nuevo grupo de archivos  
 En el ejemplo siguiente se elimina un índice clúster en línea y se mueve la tabla resultante (montón) al grupo de archivos `NewGroup` mediante la cláusula `MOVE TO` . Las vistas de catálogo `sys.indexes`, `sys.tables`y `sys.filegroups` se consultan para comprobar la ubicación del índice y la tabla en los grupos de archivos antes y después del desplazamiento. (A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] se puede usar la sintaxis DROP INDEX IF EXISTS).  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql  
--Create a clustered index on the PRIMARY filegroup if the index does not exist.  
CREATE UNIQUE CLUSTERED INDEX  
    AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
        ON Production.BillOfMaterials (ProductAssemblyID, ComponentID,   
        StartDate)  
    ON 'PRIMARY';  
GO  
-- Verify filegroup location of the clustered index.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U')  
GO  
--Create filegroup NewGroup if it does not exist.  
IF NOT EXISTS (SELECT name FROM sys.filegroups  
                WHERE name = N'NewGroup')  
    BEGIN  
    ALTER DATABASE AdventureWorks2012  
        ADD FILEGROUP NewGroup;  
    ALTER DATABASE AdventureWorks2012  
        ADD FILE (NAME = File1,  
            FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\File1.ndf')  
        TO FILEGROUP NewGroup;  
    END  
GO  
--Verify new filegroup  
SELECT * from sys.filegroups;  
GO  
-- Drop the clustered index and move the BillOfMaterials table to  
-- the Newgroup filegroup.  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials   
    WITH (ONLINE = ON, MOVE TO NewGroup);  
GO  
-- Verify filegroup location of the moved table.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U');  
GO  
```  
  
### <a name="e-dropping-a-primary-key-constraint-online"></a>E. Quitar una restricción PRIMARY KEY en línea  
 Los índices creados como resultado de la creación de restricciones PRIMARY KEY o UNIQUE no se pueden quitar mediante DROP INDEX. Se quitan con la instrucción ALTER TABLE DROP CONSTRAINT. Para más información, vea [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 En el ejemplo siguiente se elimina un índice clúster con una restricción PRIMARY KEY al quitar la restricción. La tabla `ProductCostHistory` no tiene restricciones FOREIGN KEY. Si lo hiciera, sería necesario quitar esas restricciones primero.  
  
```sql  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
ALTER TABLE Production.TransactionHistoryArchive  
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID  
WITH (ONLINE = ON);  
```  
  
### <a name="f-dropping-an-xml-index"></a>F. Quitar un índice XML  
 En el siguiente ejemplo se quita un índice XML de la tabla `ProductModel` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DROP INDEX PXML_ProductModel_CatalogDescription   
    ON Production.ProductModel;  
```  
  
### <a name="g-dropping-a-clustered-index-on-a-filestream-table"></a>G. Quitar un índice clúster en una tabla FILESTREAM  
 En el ejemplo siguiente se elimina un índice clúster en línea y se mueven la tabla resultante (montón) y los datos FILESTREAM al esquema de partición `MyPartitionScheme` mediante las cláusulas `MOVE TO` y `FILESTREAM ON`.  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql  
DROP INDEX PK_MyClusteredIndex   
    ON dbo.MyTable   
    WITH (MOVE TO MyPartitionScheme,  
          FILESTREAM_ON MyPartitionScheme);  
GO  
```  

  
## <a name="see-also"></a>Consulte también  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-index-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)  
