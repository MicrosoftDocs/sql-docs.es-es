---
title: INSERT (gráfico SQL) | Microsoft Docs
description: Obtenga información sobre la sintaxis, los permisos y los argumentos de la instrucción INSERT, que agrega una o más filas a un nodo de SQL Graph o a una tabla perimetral en SQL Server.
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- INSERT statement [SQL Server], SQL graph
- SQL graph, INSERT statement
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 497b22e6305be8d986c6c8b4b1b66de2d2dd771e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204892"
---
# <a name="insert-sql-graph"></a>INSERT (gráfico SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

Agrega una o varias filas a una tabla `node` o `edge` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

> [!NOTE]   
>  Para obtener instrucciones Transact-SQL estándar, vea [INSERT TABLE (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md).
  
![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="insert-into-node-table-syntax"></a>Sintaxis INSERT en tabla de nodo 
La sintaxis para insertar en una tabla de nodo es la misma que para una tabla normal. 

```syntaxsql
[ WITH <common_table_expression> [ ,...n ] ]  
INSERT   
{  
        [ TOP ( expression ) [ PERCENT ] ]   
        [ INTO ]   
        { <object> | rowset_function_limited   
          [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
        }  
    {  
        [ (column_list) ] | [(<edge_table_column_list>)]  
        [ <OUTPUT Clause> ]  
        { VALUES ( { DEFAULT | NULL | expression } [ ,...n ] ) [ ,...n     ]   
        | derived_table   
        | execute_statement  
        | <dml_table_source>  
        | DEFAULT VALUES   
        }  
    }  
}  
[;]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
      | database_name .[ schema_name ] .   
      | schema_name .   
    ]  
    node_table_name  | edge_table_name
}  
  
<dml_table_source> ::=  
    SELECT <select_list>  
    FROM ( <dml_statement_with_output_clause> )   
      [AS] table_alias [ ( column_alias [ ,...n ] ) ]  
    [ WHERE <on_or_where_search_condition> ]  
        [ OPTION ( <query_hint> [ ,...n ] ) ]  

<on_or_where_search_condition> ::=
    {  <search_condition_with_match> | <search_condition> }

<search_condition_with_match> ::=
    { <graph_predicate> | [ NOT ] <predicate> | ( <search_condition> ) }
    [ AND { <graph_predicate> | [ NOT ] <predicate> | ( <search_condition> ) } ]
    [ ,...n ]

<search_condition> ::=
    { [ NOT ] <predicate> | ( <search_condition> ) }
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition> ) } ]
    [ ,...n ]

<graph_predicate> ::=
    MATCH( <graph_search_pattern> [ AND <graph_search_pattern> ] [ , ...n] )

<graph_search_pattern>::=
    <node_alias> { { <-( <edge_alias> )- | -( <edge_alias> )-> } <node_alias> }

<edge_table_column_list> ::=
    ($from_id, $to_id, [column_list])

```  
  
 
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
En este documento se describen los argumentos relacionados con un gráfico SQL. Para obtener una lista completa y una descripción de los argumentos admitidos en una instrucción INSERT, vea [INSERT TABLE (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md).

INTO  
Es una palabra clave opcional que se puede usar entre `INSERT` y la tabla de destino.  
  
*search_condition_with_match*   
La cláusula `MATCH` se puede usar en una subconsulta mientras se realiza una operación de inserción en una tabla de nodo o perimetral. Para obtener la sintaxis la instrucción `MATCH`, vea [GRAPH MATCH (Transact-SQL)](../../t-sql/queries/match-sql-graph.md).

*graph_search_pattern*   
Patrón de búsqueda proporcionado a la cláusula `MATCH` como parte del predicado del gráfico.

*edge_table_column_list*   
Los usuarios deben proporcionar valores en `$from_id` y `$to_id` al realizar una operación de inserción en una tabla perimetral. Se devolverá un error si no se proporciona un valor o se insertan valores NULL en esas columnas. 
  

## <a name="remarks"></a>Observaciones  
Insertar en un nodo es lo mismo que insertar en cualquier tabla relacional. Los valores de la columna $node_id se generan automáticamente.

Al insertar en una tabla perimetral, los usuarios deben proporcionar valores para las columnas `$from_id` y `$to_id`.   

La inserción con BULK en una tabla de nodo es igual que para una tabla relacional.

Antes de insertar con BULK en una tabla perimetral, las tablas de nodo se deben importar. Así, los valores de `$from_id` y `$to_id` se podrán extraer de la columna `$node_id` de la tabla de nodo y se insertarán en la tabla perimetral. 

  
### <a name="permissions"></a>Permisos  
El permiso INSERT es obligatorio en la tabla de destino.  
  
Los permisos INSERT corresponden de forma predeterminada a los miembros del rol fijo de servidor **sysadmin**, de los roles fijos de base de datos **db_owner** y **db_datawriter** y al propietario de la tabla. Los miembros de los roles **sysadmin**, **db_owner** y **db_securityadmin** y el propietario de la tabla pueden transferir permisos a otros usuarios.  
  
Para ejecutar INSERT con la opción BULK de la función OPENROWSET, debe ser miembro de los roles fijos de servidor **sysadmin** o **bulkadmin**.  
  

## <a name="examples"></a>Ejemplos  
  
#### <a name="a--insert-into-node-table"></a>A.  Insertar en una tabla de nodo  
En el ejemplo siguiente se crea una tabla de nodo Person y se insertan dos filas en ella.

```sql
-- Create person node table
CREATE TABLE dbo.Person (ID integer PRIMARY KEY, name varchar(50)) AS NODE;
 
-- Insert records for Alice and John
INSERT INTO dbo.Person VALUES (1, 'Alice');
INSERT INTO dbo.Person VALUES (2,'John');
```
  
#### <a name="b--insert-into-edge-table"></a>B.  Insertar en una tabla perimetral  
En el siguiente ejemplo se crea una tabla perimetral friend y se inserta un borde en ella.

```sql
-- Create friend edge table
CREATE TABLE dbo.friend (start_date DATE) AS EDGE;

-- Create a friend edge, that connect Alice and John
INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'John'), '9/15/2011');
```

  
## <a name="see-also"></a>Consulte también  
[INSERT TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
[Graph processing with SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md) (Procesamiento de gráficos con SQL Server 2017)  


