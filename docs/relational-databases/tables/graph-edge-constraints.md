---
description: Restricciones perimetrales de grafos
title: Restricciones perimetrales de grafos | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- edge constraints [SQL Server]
- CONNECTION constraints
- edge constraints [Azure SQL Database]
- graph edge constraints
- SQL Graph
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 972e0a1cf391f321426398e94997740712580305
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346716"
---
# <a name="edge-constraints"></a>Restricciones perimetrales

[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

Las restricciones perimetrales se pueden usar para aplicar la integridad de datos y una semántica específica en las tablas perimetrales de una base de datos de grafos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="edge-constraints"></a><a name="Connection"></a> Restricciones perimetrales

En la primera versión de las características de los gráficos, las tablas perimetrales no aplicaban nada para los puntos de conexión del perímetro. Es decir, los perímetros de una base de datos de gráficos no podían conectar ningún nodo con ninguno otro, independientemente del tipo.

Esta versión introduce las restricciones perimetrales, con las que los usuarios pueden agregar restricciones a sus tablas perimetrales, de manera que pueden aplicar una semántica específica y mantener la integridad de datos. Al agregar un perímetro nuevo a una tabla perimetral que tiene restricciones perimetrales, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] exige que los nodos que está intentando conectar el perímetro existan en las tablas de nodo adecuadas. También se garantiza que no se pueda quitar un nodo si un perímetro sigue haciendo referencia a él.

### <a name="edge-constraint-clauses"></a>Cláusulas de restricciones perimetrales

Cada restricción perimetral consta de una o varias cláusulas de restricción perimetral. Una cláusula de restricción perimetral es el par de nodos FROM y TO a los que el perímetro indicado se ha podido conectar.

Imagínese que tiene los nodos `Product` y `Customer` en su gráfico y usa el perímetro `Bought` para conectar estos nodos. La cláusula de restricción perimetral especifica el par de nodos FROM y TO y la dirección del perímetro. En este caso, la cláusula de restricción perimetral será `Customer` TO `Product`. Es decir, se permitirá insertar un perímetro `Bought` que vaya de un `Customer` a un `Product`. Si se intenta insertar un perímetro que vaya de `Product` a `Customer`, se producirá un error.

- Una cláusula de restricción perimetral contiene un par de tablas de nodos FROM y TO donde se aplica la restricción perimetral.
- Los usuarios pueden especificar varias cláusulas de restricción perimetral por restricción perimetral, que se aplicarán como disyunción.
- Si se crean varias restricciones perimetrales en una sola tabla perimetral, los perímetros deberán cumplir TODAS las restricciones para poder ser admitidos.

### <a name="indexes-on-edge-constraints"></a>Índices en las restricciones perimetrales

La creación de una restricción perimetral no crea automáticamente un índice en las columnas `$from_id` y `$to_id` de la tabla perimetral. Se recomienda crear manualmente un índice en un par de columnas `$from_id` y `$to_id` si tiene consultas de búsqueda de puntos o una carga de trabajo OLTP.

### <a name="on-delete-referential-actions-on-edge-constraints"></a>Acciones referenciales ON DELETE en restricciones perimetrales
Las acciones en cascada en una restricción perimetral permiten a los usuarios definir las acciones que toma el motor de base de datos cuando un usuario elimina los nodos, a los que se conecta el perímetro especificado. Se pueden definir las acciones referenciales siguientes:  
*NO ACTION*   
El motor de base de datos genera un error al intentar eliminar un nodo que tiene perímetros conectados.  

*CASCADE*   
Cuando se elimina un nodo de la base de datos, se eliminan los perímetros conectados.  

## <a name="working-with-edge-constraints"></a>Trabajo con restricciones perimetrales

Puede definir una restricción perimetral en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../includes/tsql-md.md)]. Las restricciones perimetrales solo se pueden definir en tablas perimetrales de gráficos. Para crear, eliminar o modificar una restricción perimetral, debe contar con el permiso **ALTER** en la tabla.

### <a name="create-edge-constraints"></a>Creación de restricciones perimetrales

En los ejemplos siguientes se muestra cómo crear restricciones perimetrales en tablas nuevas o existentes.

#### <a name="to-create-an-edge-constraint-on-a-new-edge-table"></a>Para crear una restricción perimetral en una nueva tabla perimetral

En el ejemplo siguiente se crea una restricción perimetral en la tabla perimetral **bought**.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE NO ACTION
   )
   AS EDGE;
   ```

#### <a name="defining-referential-actions-on-a-new-edge-table"></a>Definición de acciones referenciales en una tabla perimetral nueva 

En el ejemplo siguiente se crea una restricción perimetral en la tabla perimetral **bought** y se define la acción referencial ON DELETE CASCADE. 

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE CASCADE
   )
   AS EDGE;
   ```

#### <a name="to-add-edge-constraint-to-an-existing-edge-table"></a>Para agregar una restricción perimetral a una tabla perimetral existente

En el ejemplo siguiente se usa ALTER TABLE para agregar una restricción perimetral a la tabla perimetral **bought**.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY,
      , CustomerName VARCHAR(100)
   )
   AS NODE;
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product);
```  

#### <a name="create-a-new-edge-constraint-on-existing-edge-table-with-additional-edge-constraint-clauses"></a>Creación de una restricción perimetral en una tabla perimetral existente con cláusulas de restricción perimetral adicionales

En el ejemplo siguiente se usa el comando `ALTER TABLE` para agregar una restricción perimetral nueva con cláusulas de restricción perimetral adicionales en la tabla perimetral **bought**.
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
-- User ALTER TABLE to create a new edge constraint.
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product, Supplier TO Product);
```  

En el ejemplo siguiente, hay dos cláusulas de restricción perimetral en la restricción *EC_BOUGHT1*, una que conecta **Customer** (Cliente) con **Product** (Producto) y otra que conecta **Supplier** (Proveedor) con **Product** (Producto). Estas dos cláusulas se aplican como disyunción; es decir, un perímetro determinado tiene que cumplir cualquiera de estas dos cláusulas para poder estar permitido en la tabla perimetral.

#### <a name="creating-a-new-edge-constraint-on-existing-edge-table-with-new-edge-constraint-clause"></a>Creación de una restricción perimetral en una tabla perimetral existente con una cláusula de restricción perimetral nueva

En el ejemplo siguiente se usa el comando `ALTER TABLE` para agregar una restricción perimetral nueva con una cláusula de restricción perimetral nueva en la tabla perimetral **bought**.
  
 ```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT,
         CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Supplier TO Product);
 ```  

En el ejemplo anterior, creamos dos restricciones perimetrales independientes en la tabla perimetral **bought**: *EC_BOUGHT* y *EC_BOUGHT1*. Ambas restricciones perimetrales tienen cláusulas de restricción perimetral diferentes. Si una tabla perimetral contiene más de una restricción perimetral, un perímetro concreto tendrá que cumplir **TODAS** las restricciones perimetrales para poder estar permitido en la tabla perimetral. Puesto que en este caso ningún perímetro podrá cumplir *EC_BOUGHT* y *EC_BOUGHT1*, la tabla perimetral **bought** deberá permanecer vacía.

Para que las inserciones se efectúen correctamente en esta tabla perimetral, se debe quitar una de las restricciones perimetrales o bien las dos y crear una restricción perimetral que contenga ambas cláusulas de restricción perimetral.

```sql
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT1;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT_NEW CONNECTION (Customer TO Product, Supplier TO Product);
GO
```  

Para que se permita un perímetro concreto en la tabla perimetral **bought**, tiene que cumplir cualquiera de las cláusulas de restricción perimetral de la restricción *EC_BOUGHT_NEW*. Por lo tanto, se permitirá cualquier perímetro que esté intentando unos nodos **Customer**-**Product** (Cliente-Producto) o **Supplier**-**Product** (Proveedor-Producto) válidos.

### <a name="delete-edge-constraints"></a>Eliminación de restricciones perimetrales

En el ejemplo siguiente se identifica primero el nombre de la restricción perimetral y, luego, se elimina.  
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   ) AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
    )
    AS EDGE;
GO
-- Return the name of edge constraint.
SELECT name  
   FROM sys.edge_constraints  
   WHERE type = 'EC' AND parent_object_id = OBJECT_ID('bought');  
GO  
-- Delete the primary key constraint.  
ALTER TABLE bought
DROP CONSTRAINT EC_BOUGHT;
```  

### <a name="modify-edge-constraints"></a>Modificación de restricciones perimetrales

Para modificar una restricción perimetral mediante Transact-SQL, deberá eliminar la restricción perimetral existente y, a continuación, volver a crearla con la nueva definición.


### <a name="view-edge-constraints"></a>Visualización de restricciones perimetrales

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

El ejemplo devuelve todas las restricciones perimetrales y sus propiedades para la tabla perimetral `bought` en la base de datos tempdb.  

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
   GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;

-- CREATE edge table with edge constraints.
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product, Supplier TO Product)
   )
   AS EDGE;

-- Query sys.edge_constraints and sys.edge_constraint_clauses to view
-- edge constraint properties.
SELECT
   EC.name AS edge_constraint_name
   , OBJECT_NAME(EC.parent_object_id) AS edge_table_name
   , OBJECT_NAME(ECC.from_object_id) AS from_node_table_name
   , OBJECT_NAME(ECC.to_object_id) AS to_node_table_name
   , is_disabled
   , is_not_trusted
FROM sys.edge_constraints EC
   INNER JOIN sys.edge_constraint_clauses ECC
   ON EC.object_id = ECC.object_id
WHERE EC.parent_object_id = object_id('bought');
```  

## <a name="related-tasks"></a>Tareas relacionadas

[CREATE TABLE (SQL Graph)](../../t-sql/statements/create-table-sql-graph.md)  
[ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  

Para información sobre la tecnología de grafos en SQL Server, consulte el artículo sobre el [procesamiento de gráficos con SQL Server y Azure SQL Database](../graphs/sql-graph-overview.md).
