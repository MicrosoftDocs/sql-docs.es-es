---
description: sp_data_source_objects (Transact-SQL)
title: sp_data_source_objects | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_objects
helpviewer_keywords:
- PolyBase
ms.assetid: 48066431-fed2-4a8a-85af-ac704689e183
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f3aa43750cebfbbac77e3807845f1ed154af1ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067220"
---
# <a name="sp_data_source_objects-transact-sql"></a>sp_data_source_objects (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Devuelve la lista de objetos de tabla que están disponibles para su virtualización.

> [!NOTE]
> Este procedimiento se presentó en [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintaxis  

```syntaxsql
sp_data_source_objects  
         [ @data_source = ] 'data_source'
     [ , [ @object_root_name = ] 'object_root_name' ]
     [ , [ @max_search_depth = ] max_search_depth ]
     [ , [ @search_options = ] 'search_options' ]
[ ; ]
```  
  
## <a name="arguments"></a>Argumentos  

`[ @data_source = ] 'data_source'`   
Nombre del origen de datos externo del que se van a obtener los metadatos. `@data_source` es `sysname`.  

`[ @object_root_name = ] 'object_root_name'`   
Este parámetro es la raíz del nombre de los objetos que se van a buscar. `@object_root_name` es `nvarchar(max)` de, con un valor predeterminado de `NULL` .

Esta llamada solo devuelve objetos externos que comienzan por el valor establecido para `@object_root_name` .

Si un origen de datos ODBC se conecta a un sistema de administración de bases de datos relacionales (RDBMS) que usa nombres de tres partes, `@object_root_name` no puede contener un nombre de base de datos parcial. En estos casos, el parámetro `@object_root_name` debe contener las tres partes, donde la tercera parte es el nombre del objeto que se va a buscar.
> [!CAUTION]
> Debido a las diferencias entre las plataformas de datos externos, algunas plataformas no devuelven ningún resultado si se proporciona el valor predeterminado de `NULL` . Algunos tratan `NULL` como la falta de un filtro. Por ejemplo, Oracle RDMBS no devolverá resultados si `NULL` se proporciona para `@object_root_name` .

`[ @max_search_depth = ] max_search_depth`   
Este valor especifica la profundidad máxima (en partes) más allá de la `@object_root_name` que se desea buscar. `@max_search_depth` es `int` de con un valor predeterminado de 1.

Por ejemplo, un `@max_search_depth` de 1, con un `@object_root_name` que es el nombre de una base de datos de SQL Server, devolverá esquemas incluidos en la base de datos.

Un `@max_search_depth` de `NULL` devolverá información sobre `@object_root_name` si existe y no está vacío, en el caso de catálogo o esquema.

`[ @search_options = ] 'search_options'`   
El `search_options` parámetro es de tipo nvarchar (Max) y su valor predeterminado es `NULL` .

Este parámetro no se utiliza, pero puede implementarse en el futuro.

## <a name="result-sets"></a>Conjuntos de resultados

| Nombre de columna | Tipo de datos | Descripción |
|--|--|--|
| Object_Type | nvarchar(200) | Tipo del objeto (por ejemplo: tabla o base de datos). |
| OBJECT_NAME | nvarchar(max) | Nombre completo del objeto. Se usa para usar el carácter de comilla específico de back-end. |
| OBJECT_LEAF_NAME | nvarchar(max) | Nombre de objeto no calificado. |
| TABLE_LOCATION | nvarchar(max) | Cadena de ubicación de tabla válida que se puede usar para una instrucción CREATE EXTERNAL TABLE. Será `NULL` si no es aplicable. |
  
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

La noción de vacío frente a no vacío se relaciona con el comportamiento del controlador ODBC y la [ `SQLTables` función](../native-client-odbc-api/sqltables.md). No vacío indica que un objeto contiene tablas, no filas. Por ejemplo, un esquema vacío no contiene ninguna tabla en SQL Server. Una base de datos vacía contiene sin tablas en Teradata.

Los tipos de objeto se determinan mediante el controlador ODBC del origen de datos externo. Cada origen de datos externo determina lo que califica como una tabla. Esto puede incluir objetos de base de datos como funciones de TeraData o sinónimos en Oracle. Polybase no se puede conectar a algunos objetos ODBC como tablas externas y, por lo tanto, no tiene un valor en la columna TABLE_LOCATION. A pesar de eso, la presencia de uno de estos objetos puede hacer que una base de datos o un esquema no estén vacíos.

Use `sp_data_source_objects` y [`sp_data_source_table_columns`](sp-data-source-table-columns.md) para detectar objetos externos. Estos procedimientos almacenados del sistema devuelven el esquema de las tablas que están disponibles para su virtualización. Azure Data Studio usa estos dos procedimientos almacenados para admitir la [virtualización de datos](../../azure-data-studio/extensions/data-virtualization-extension.md). Use [sp_data_source_table_columns](sp-data-source-table-columns.md) para detectar esquemas de tabla externos representados en tipos de datos de SQL Server.

### <a name="data-source-type-specific-remarks"></a>Comentarios específicos del tipo de origen de datos

* Teradata

   Las vistas del sistema de Teradata no usan seguridad de nivel de fila (RLS), por lo que los usuarios pueden ver la existencia de tablas que no pueden consultar.

* MongoDB

   Algunas versiones anteriores de MongoDB restringen la capacidad de enumerar todas las bases de datos para usuarios similares a administradores. Los usuarios sin este permiso pueden obtener errores de autenticación intentando ejecutar este procedimiento con un valor null `@object_root_name` .

## <a name="examples"></a>Ejemplos  

### <a name="sql-server"></a>SQL Server

En el ejemplo siguiente se devuelven todas las bases de datos, esquemas y tablas/vistas

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 3;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| SCHEMA | "base de datos". DBO | dbo | NULL |
| TABLE | "base de datos". DBO "." cliente | cliente | [base de datos]. [dbo]. cliente |
| TABLE | "base de datos". DBO "." movs | item | [base de datos]. [dbo]. movs |
| TABLE | "base de datos". DBO "." nación | nación | [base de datos]. [dbo]. nación |

En el ejemplo siguiente se devuelven todas las bases de datos

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "UserDatabase" | UserDatabase | NULL |
| DATABASE | "master" | maestro | NULL |
| DATABASE | msdb | msdb | NULL |
| DATABASE | tempdb | tempdb | NULL |
| DATABASE | "database" | database | NULL |

En el ejemplo siguiente se devuelven todos los esquemas de una base de datos

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| SCHEMA | "base de datos". DBO | dbo | NULL |
| SCHEMA | "base de datos". INFORMATION_SCHEMA " | INFORMATION_SCHEMA | NULL |
| SCHEMA | "base de datos". Sist | sys | NULL |

En el ejemplo siguiente se devuelven todas las tablas del esquema 

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database].[dbo]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| TABLE | "base de datos". DBO "." cliente | cliente | [base de datos]. [dbo]. cliente |
| TABLE | "base de datos". DBO "." movs | item | [base de datos]. [dbo]. movs |
| TABLE | "base de datos". DBO "." nación | nación | [base de datos]. [dbo]. nación |
| TABLE | "base de datos". DBO "." pedidos | pedidos | [base de datos]. [dbo]. pedidos |
| TABLE | "base de datos". DBO "." referencia | referencia | [base de datos]. [dbo]. referencia |

### <a name="oracle"></a>Oracle

En el ejemplo siguiente se devuelven los esquemas y tablas, funciones, vistas, etc. completos.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[OracleObjectRoot]'; 
DECLARE @max_search_depth INT = 2; 
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| VIEW | "SYS". ALL_SQLSET_STATEMENTS " | ALL_SQLSET_STATEMENTS | [ORACLEOBJECTROOT]. [SYS]. [ALL_SQLSET_STATEMENTS] |
| SYSTEM TABLE | "SYS". BOOTSTRAP $ " | BOOTSTRAP $ | [ORACLEOBJECTROOT]. [SYS]. [BOOTSTRAP $] |
| SYNONYM | "PÚBLICO". ALL_ALL_TABLES " | ALL_ALL_TABLES | NULL |
| SCHEMA | "database" | database | NULL |
| TABLE | "base de datos". cliente | cliente | [ORACLEOBJECTROOT]. [base de datos]. cliente |

### <a name="teradata"></a>Teradata

En el ejemplo siguiente se devuelven todas las bases de datos y tablas, funciones, vistas, etc.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |  
|--|--|--|--|
| FUNCTION | "SYSLIB"." ExtractRoles" | ExtractRoles | NULL |  
| SYSTEM TABLE | "DBC". UDTCast" | UDTCast | [DBC]. [UDTCast] |  
| TYPE | "SYSUDTLIB"." LENGUAJE | XML | NULL |  
| DATABASE | "database" | database | NULL |
| TABLE | "base de datos". cliente | cliente | [base de datos]. cliente |  

### <a name="mongo-db"></a>Mongo DB

En el ejemplo siguiente se devuelven todas las bases de datos y tablas.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| TABLE | "base de datos". cliente | cliente | [base de datos]. cliente |
| TABLE | "base de datos". movs | item | [base de datos]. movs |
| TABLE | "base de datos". nación | nación | [base de datos]. nación |
| TABLE | "base de datos". pedidos | pedidos | [base de datos]. pedidos |

## <a name="see-also"></a>Consulte también

- [sp_data_source_columns](./sp-data-source-table-columns.md)   
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)
- [Extensión de virtualización de datos para Azure Data Studio](../../azure-data-studio/extensions/data-virtualization-extension.md)   
- [Introducción a PolyBase](../polybase/polybase-guide.md)   
- [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)