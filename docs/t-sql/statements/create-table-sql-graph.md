---
description: CREATE TABLE (SQL Graph)
title: CREATE TABLE (SQL Graph) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_GRAPH_TSQL
- TABLE
- CREATE_TABLE_TSQL
- NODE
- NODE_TSQL
- AS_NODE
- AS_NODE_TSQL
- EDGE
- EDGE_TSQL
- AS_EDGE
- AS_EDGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- graph
- SQL graph
- CREATE TABLE SQL graph
- NODE
- EDGE
- SQL graph, CREATE TABLE statement
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d946a2c834fa910a0a7b76b321a55bfbdd91198
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177898"
---
# <a name="create-table-sql-graph"></a>CREATE TABLE (SQL Graph)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Crea una tabla de SQL Graph como una tabla `NODE` o `EDGE`. 
  
> [!NOTE]   
>  Para instrucciones Transact-SQL estándar, vea [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } 
       | <computed_column_definition>
       | <column_set_definition>
       | [ <table_constraint> ] [ ,... n ]
       | [ <table_index> ] }
          [ ,...n ]
    )   
    AS [ NODE | EDGE ]
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
[ ; ] 

< table_constraint > ::=
[ CONSTRAINT constraint_name ]
{
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        (column [ ASC | DESC ] [ ,...n ] )
        [
            WITH FILLFACTOR = fillfactor
           |WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name (partition_column_name)
            | filegroup | "default" } ]
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
    | CONNECTION
        ( { node_table TO node_table } 
          [ , {node_table TO node_table }]
          [ , ...n ]
        )
        [ ON DELETE { NO ACTION | CASCADE } ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
```  
  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
En este documento solo se enumeran los argumentos pertenecientes a SQL Graph. Para obtener una lista completa y una descripción de los argumentos admitidos, vea [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).

 *database_name*    
 Es el nombre de la base de datos en la que se crea la tabla. *database_name* debe especificar el nombre de una base de datos existente. Si no se especifica, *database_name* usa de manera predeterminada la base de datos actual. El inicio de sesión de la conexión actual debe estar asociado a un identificador de usuario existente en la base de datos especificada por *database_name*, y ese identificador de usuario debe tener permisos CREATE TABLE.  
  
 *schema_name*    
 Es el nombre del esquema al que pertenece la nueva tabla.  
  
 *table_name*      
 Es el nombre de la tabla de nodo o perimetral. Los nombres de las tablas deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). *table_name* puede contener un máximo de 128 caracteres, excepto para los nombres de tablas temporales locales (nombres precedidos de un único signo de número, #), que no pueden superar los 116 caracteres.  
  
 NODE   
 Crea una tabla de nodo.

 EDGE  
 Crea una tabla perimetral.  
 
 *table_constraint*   
 Especifica las propiedades de una restricción PRIMARY KEY, UNIQUE, FOREIGN KEY, CONNECTION o CHECK, o bien una definición DEFAULT agregada a una tabla.
 
 > [!NOTE]   
 > La restricción CONNECTION solo se aplica a un tipo de tabla perimetral.
 
 ON { esquema_de_partición | grupo_de_archivos | "default" }    
 Especifica el esquema de partición o el grupo de archivos en que se almacena la tabla. Si se especifica esquema_de_partición, la tabla será una tabla con particiones cuyas particiones se almacenan en un conjunto de uno o más grupos de archivos especificados en esquema_de_partición. Si se especifica grupo_de_archivos, la tabla se almacena en el grupo de archivos indicado. El grupo de archivos debe existir en la base de datos. Si se especifica "default", o bien si ON no se especifica en ninguna parte, la tabla se almacena en el grupo de archivos predeterminado. El mecanismo de almacenamiento de una tabla según se especifica en CREATE TABLE no se puede modificar posteriormente.

 ON {esquema_de_partición | grupo_de_archivos | "default"}    
 También se puede especificar en una restricción PRIMARY KEY o UNIQUE. Estas restricciones crean índices. Si se especifica grupo_de_archivos, el índice se almacena en el grupo de archivos con nombre. Si se especifica "default", o bien si ON no se especifica en ninguna parte, el índice se almacena en el mismo grupo de archivos que la tabla. Si la restricción PRIMARY KEY o UNIQUE crea un índice clúster, las páginas de datos de la tabla se almacenan en el mismo grupo de archivos que el índice. Si se especifica CLUSTERED o la restricción crea un índice agrupado, y se especifica un valor esquema_de_partición distinto del valor esquema_de_partición o grupo_de_archivos de la definición de tabla, o viceversa, únicamente se respeta la definición de restricción y se omite el resto.
  
## <a name="remarks"></a>Observaciones

No se admite la creación de una tabla temporal como una tabla de nodo o perimetral.  

No se admite la creación de una tabla de nodo o perimetral como una tabla temporal.

No se admite Stretch Database para una tabla de nodo o perimetral.

Las tablas de nodo o perimetrales no pueden ser tablas externas (no hay compatibilidad de PolyBase con las tablas de Graph). 

No se puede modificar un nodo de gráfico o una tabla perimetral sin particiones en una tabla de nodo de gráfico o una tabla perimetral con particiones. 
  
 
## <a name="examples"></a>Ejemplos  
  
### <a name="a-create-a-node-table"></a>A. Creación de una tabla `NODE`
 En el ejemplo siguiente se muestra cómo se crea una tabla `NODE`.

```sql
 CREATE TABLE Person (
        ID INTEGER PRIMARY KEY, 
        name VARCHAR(100), 
        email VARCHAR(100)
 ) AS NODE;
```

### <a name="b-create-an-edge-table"></a>B. Creación de una tabla `EDGE`
En el ejemplo siguiente se muestra cómo se crean tablas `EDGE`.

```sql
 CREATE TABLE friends (
    id INTEGER PRIMARY KEY,
    start_date DATe
 ) AS EDGE;
```

```sql
 -- Create a likes edge table, this table does not have any user defined attributes   
 CREATE TABLE likes AS EDGE;
```

En el ejemplo siguiente se modela una regla por la que **solo** las personas pueden ser amigas de otras personas, lo que significa que este borde no permite hacer referencia a ningún nodo distinto de Person.

```
/* Create friend edge table with CONSTRAINT, restricts for nodes and it direction */
CREATE TABLE dbo.FriendOf(
  CONSTRAINT cnt_Person_FriendOf_Person
    CONNECTION (dbo.Person TO dbo.Person) 
)AS EDGE;
```


## <a name="see-also"></a>Consulte también 
 [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Graph processing with SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md) (Procesamiento de gráficos con SQL Server 2017)

