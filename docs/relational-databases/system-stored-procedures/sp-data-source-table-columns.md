---
title: sp_data_source_table_columns
description: sp_data_source_table_columns (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_table_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
author: MikeRayMSFT
ms.author: mikeray
ms.custom: ''
ms.date: 11/10/2020
ms.openlocfilehash: 31b0dedb8544cc6752eac8a9934aa92d74d424ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081886"
---
# <a name="sp_data_source_table_columns-transact-sql"></a>sp_data_source_table_columns (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Devuelve la lista de columnas de la tabla de origen de datos externos.
  
> [!NOTE]
> Este procedimiento se presentó en [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sqlsyntax
sp_data_source_table_columns
         [ @data_source = ] 'data_source'
       , [ @table_location = ] 'table_location'
[ ; ]
```  

## <a name="arguments"></a>Argumentos

`[ @data_source = ] 'data_source'`   
Nombre del origen de datos externo del que se van a obtener los metadatos. El tipo es `sysname` .

`[ @table_location = ] 'table_location'`   
Cadena de ubicación de la tabla que identifica la tabla. `table_location` el tipo es `nvarchar(max)` .

## <a name="returns"></a>Devoluciones

El procedimiento almacenado devuelve la siguiente información:

|Nombre de columna |Tipo de datos |Descripción|
|---|---|---|
|NOMBRE|nvarchar(max)|El nombre de la columna.
|TYPE|nvarchar(200)|Nombre del tipo de SQL Server
|LENGTH|int|Longitud de la columna
|PRECISION|int|Precisión de la columna
|SCALE|int|Escala de la columna
|COLLATION|nvarchar(200)|SQL Server intercalación de la columna
|IS_NULLABLE|bit|Es esta columna que acepta valores NULL.
|SOURCE_TYPE_NAME|nvarchar(max)|Nombre del tipo específico de back-end. Se utiliza principalmente para la depuración. En el caso de los orígenes ODBC, se corresponderá con la columna de resultados TYPE_NAME de SQLColumns ().
|COMENTARIOS|nvarchar(max)|Comentarios generales o descripción de la columna. Actualmente siempre es NULL.|

## <a name="permissions"></a>Permisos  

Requiere el permiso ALTER ANY EXTERNAL DATA SOURCE.
  
## <a name="remarks"></a>Observaciones  

La instancia de SQL Server debe tener instalada la característica  [polybase](../../relational-databases/polybase/polybase-guide.md) .

Este procedimiento almacenado admite conectores para:

- SQL Server
- Oracle
- Teradata
- MongoDB
- CosmosDB

El procedimiento almacenado no admite conectores de origen ODBC DTA genéricos.

La noción de vacío frente a no vacío se relaciona con el comportamiento del controlador ODBC y la [ `SQLTables` función](../native-client-odbc-api/sqltables.md). No vacío indica que un objeto contiene tablas, no filas. Por ejemplo, un esquema vacío no contiene ninguna tabla en SQL Server. Una base de datos vacía contiene sin tablas en Teradata. los resultados son una representación SQL Server del esquema de back-end tal como la interpreta el conector de polybase para el back-end. La distinción es que, en lugar de simplemente pasar los resultados de la llamada ODBC al back-end, los resultados se basan en el resultado del código de asignación de tipo de polybase.

Use [`sp_data_source_objects`](sp-data-source-objects.md) y `sp_data_source_table_columns` para detectar objetos externos. Estos procedimientos almacenados del sistema devuelven el esquema de las tablas que están disponibles para su virtualización. Azure Data Studio usa estos dos procedimientos almacenados para admitir la [virtualización de datos](../../azure-data-studio/extensions/data-virtualization-extension.md). Use `sp_data_source_table_columns` para detectar esquemas de tabla externos representados en SQL Server tipos de datos.

Debido a las diferencias entre las intercalaciones en los datos de origen de Hadoop y las intercalaciones admitidas en SQL Server 2019, las longitudes de tipo de datos recomendadas para las columnas de tipo de datos VARCHAR en tablas externas pueden ser mucho mayores de lo esperado. es así por diseño.

## <a name="example"></a>Ejemplo  

En el ejemplo siguiente se devuelven las columnas de una tabla externa en una SQL Server denominada `server` , que pertenece a un esquema denominado `schema` .
  
```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @table_location NVARCHAR(400) = N'[database].[schema].[table]';
EXEC sp_data_source_table_columns @data_source, @table_location
```  
  
## <a name="see-also"></a>Consulte también

- [Introducción a PolyBase](../polybase/polybase-guide.md)
- [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)