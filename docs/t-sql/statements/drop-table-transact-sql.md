---
description: DROP TABLE (Transact-SQL)
title: DROP TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_TABLE_TSQL
- DROP TABLE
dev_langs:
- TSQL
helpviewer_keywords:
- removing indexes
- table removal [SQL Server]
- deleting indexes
- dropping data
- deleting tables
- dropping indexes
- removing constraints
- removing permissions
- DROP TABLE statement
- removing tables
- deleting triggers
- removing data
- dropping tables
- deleting permissions
- dropping triggers
- deleting constraints
- removing triggers
- deleting data
- dropping constraints
- dropping permissions
ms.assetid: 0b6f2b6f-3aa3-4767-943f-43df3c3c5cfd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 01343ee8d31f086781f09be35b81155f5b9090eb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236993"
---
# <a name="drop-table-transact-sql"></a>DROP TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quita una o varias definiciones de tabla y todos los datos, índices, desencadenadores, restricciones y especificaciones de permisos de esas tablas. Las vistas o procedimientos almacenados que hagan referencia a la tabla quitada se deben quitar explícitamente con [DROP VIEW](../../t-sql/statements/drop-view-transact-sql.md) o [DROP PROCEDURE](../../t-sql/statements/drop-procedure-transact-sql.md). Para crear informes de las dependencias de una tabla, utilice [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP TABLE [ IF EXISTS ] { database_name.schema_name.table_name | schema_name.table_name | table_name } [ ,...n ]  
[ ; ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *database_name*  
 Es el nombre de la base de datos en la que se creó la tabla.  
  
 Azure SQL Database admite el formato de nombre de tres partes nombre_basededatos.[nombre_esquema].nombre_objeto cuando nombre_basededatos es la base de datos actual o tempdb y nombre_objeto comienza con #. Azure SQL Database no admite nombres de cuatro partes.  
  
 *IF EXISTS*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Quita la tabla condicionalmente solo si ya existe.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la tabla.  
  
 *table_name*  
 Es el nombre de la tabla que se va a quitar.  
  
## <a name="remarks"></a>Observaciones  
 No se puede utilizar DROP TABLE para quitar una tabla a la que haga referencia una restricción FOREIGN KEY. Primero se debe quitar la restricción FOREIGN KEY o la tabla de referencia. Si la tabla de referencia y la tabla que tiene la clave principal se van a quitar en la misma instrucción DROP TABLE, la tabla de referencia debe aparecer primero.  
  
 Se pueden quitar varias tablas de cualquier base de datos. Si la tabla que se va a quitar hace referencia a la clave principal de otra tabla que también se va a quitar, la tabla de referencia con la clave externa debe aparecer antes que la tabla que tiene la clave principal a la que se hace referencia.  
  
 Cuando se quita la tabla, las reglas o valores predeterminados de la tabla pierden sus enlaces y se quitan automáticamente las restricciones o desencadenadores asociados con la tabla. Si vuelve a crear una tabla, debe volver a enlazar las reglas y valores predeterminados apropiados, volver a crear los desencadenadores y agregar todas las restricciones necesarias.  
  
 Si elimina todas las filas de una tabla utilizando el *nombre de tabla* DELETE o utiliza la instrucción TRUNCATE TABLE, la tabla existe hasta que se quita.  
  
 Los índices y las tablas grandes que utilizan más de 128 extensiones se quitan en dos fases independientes: lógica y física. En la fase lógica, las unidades de asignación existentes que utiliza la tabla se marcan para la cancelación de asignación y se bloquean hasta que se confirme la transacción. En la fase física, las páginas IAM marcadas para cancelación de asignación se quitan físicamente por lotes.  
  
 Si quita una tabla que contiene una columna de tipo VARBINARY(MAX) con el atributo FILESTREAM, los datos almacenados en el sistema de archivos no se quitarán.  
  
> [!IMPORTANT]  
>  DROP TABLE y CREATE TABLE no se deberían ejecutar en la misma tabla en el mismo lote. De lo contrario, podría producirse un error inesperado.  
  
## <a name="permissions"></a>Permisos  
 Se requiere el permiso ALTER en el esquema al que pertenece la tabla, el permiso CONTROL en la tabla o la pertenencia al rol fijo de base de datos **db_ddladmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-dropping-a-table-in-the-current-database"></a>A. Quitar una tabla de la base de datos actual  
 En el siguiente ejemplo se quita la tabla `ProductVendor1`, y sus datos e índices de la base de datos actual.  
  
```sql  
DROP TABLE ProductVendor1 ;  
```  
  
### <a name="b-dropping-a-table-in-another-database"></a>B. Quitar una tabla de otra base de datos  
 En el siguiente ejemplo se quita la tabla `SalesPerson2` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. El ejemplo se puede ejecutar desde cualquier base de datos de la instancia de servidor.  
  
```sql  
DROP TABLE AdventureWorks2012.dbo.SalesPerson2 ;  
```  
  
### <a name="c-dropping-a-temporary-table"></a>C. Quitar una tabla temporal  
 En el siguiente ejemplo se crea una tabla temporal, se comprueba si existe, se quita y se comprueba de nuevo si existe. En este ejemplo no se utiliza la sintaxis **IF EXISTS**, que está disponible a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
  
```sql  
CREATE TABLE #temptable (col1 INT);  
GO  
INSERT INTO #temptable  
VALUES (10);  
GO  
SELECT * FROM #temptable;  
GO  
IF OBJECT_ID(N'tempdb..#temptable', N'U') IS NOT NULL   
DROP TABLE #temptable;  
GO  
--Test the drop.  
SELECT * FROM #temptable;  
```  
  
### <a name="d-dropping-a-table-using-if-exists"></a>D. Quitar una tabla con IF EXISTS  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 En el ejemplo siguiente se crea una tabla denominada T1. A continuación, la segunda instrucción quita la tabla. La tercera instrucción no realiza ninguna acción porque ya se ha eliminado la tabla, pero tampoco provoca ningún error.  
  
```sql  
CREATE TABLE T1 (Col1 INT);  
GO  
DROP TABLE IF EXISTS T1;  
GO  
DROP TABLE IF EXISTS T1;  
```  
  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)