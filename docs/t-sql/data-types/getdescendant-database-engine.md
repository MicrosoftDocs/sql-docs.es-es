---
description: GetDescendant (motor de base de datos)
title: GetDescendant (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetDescendant_TSQL
- GetDescendant
dev_langs:
- TSQL
helpviewer_keywords:
- GetDescendant [Database Engine]
ms.assetid: f5f39596-033e-4243-acbc-caa188b45b03
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 21c00b305f4bc31782307f7abb779c46ffe97e6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165176"
---
# <a name="getdescendant-database-engine"></a>GetDescendant (motor de base de datos)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve el nodo secundario del nodo primario.
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Transact-SQL syntax  
parent.GetDescendant ( child1 , child2 )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetDescendant ( SqlHierarchyId child1 , SqlHierarchyId child2 )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*child1*  
NULL o el **hierarchyid** de un nodo secundario del nodo actual.
  
*child2*  
NULL o el **hierarchyid** de un nodo secundario del nodo actual.
  
## <a name="return-types"></a>Tipos de valor devuelto  
**Tipo de valor devuelto de SQL Server: hierarchyid**
  
**Tipo devuelto de CLR: SqlHierarchyId**
  
## <a name="remarks"></a>Observaciones  
Devuelve un nodo secundario que es un descendiente del nodo primario.
-   Si el elemento primario es NULL, devolverá NULL.  
-   Si el elemento primario no es NULL, y child1 y child2 son NULL, devuelve un elemento secundario del elemento primario.  
-   Si el elemento primario y child1 no son NULL y, a su vez, child2 es NULL, devuelve un elemento secundario del elemento primario mayor que child1.  
-   Si el elemento primario y child2 no son NULL y, a su vez, child1 es NULL, devuelve un elemento secundario del elemento primario menor que child2.  
-   Si el elemento primario, child1 y child2 no son NULL, devuelve un elemento secundario del elemento primario mayor que child1 y menor que child2.  
-   Si child1 o child2 no son NULL y no son un elemento secundario del elemento primario, se producirá una excepción.  
-   Si child2 no es NULL ni es un elemento secundario del elemento primario, se producirá una excepción.  
-   Si child1 >= child2, se producirá una excepción.  
  
GetDescendant es determinista. Por lo tanto, si se llama a GetDescendant con las mismas entradas, siempre producirá la misma salida. Sin embargo, la identidad exacta del nodo secundario producido puede variar en función de su relación con los demás nodos, como se muestra en el ejemplo C.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-inserting-a-row-as-the-least-descendant-node"></a>A. Insertar una fila como el nodo menos descendiente  
Se contrata a un nuevo empleado, que es subordinado de un empleado ya existente en el nodo `/3/1/`. Ejecute el siguiente código para insertar la nueva fila usando el método GetDescendant sin argumentos para especificar el nodo de la nueva fila como `/3/1/1/`:
  
```sql
DECLARE @Manager hierarchyid;   
SET @Manager = CAST('/3/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(NULL, NULL),  
'adventure-works\FirstNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="b-inserting-a-row-as-a-greater-descendant-node"></a>B. Insertar una fila como un nodo más descendiente  
Se contrata a otro empleado nuevo, que es subordinado del mismo jefe que en el ejemplo A. Ejecute el siguiente código para insertar la fila nueva con el método GetDescendant, usando el argumento child1 para especificar que el nodo de la nueva fila vendrá después del nodo del ejemplo A, convirtiéndose en `/3/1/2/`:
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, NULL),  
'adventure-works\SecondNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="c-inserting-a-row-between-two-existing-nodes"></a>C. Insertar una fila entre dos nodos existentes  
Se contrata a un tercer empleado, subordinado del mismo jefe que en el ejemplo A. Este ejemplo inserta la nueva fila en un nodo mayor que el nodo `FirstNewEmployee` del ejemplo A y menor que el nodo `SecondNewEmployee` del ejemplo B. Ejecute el siguiente código con el método GetDescendant. Use los argumentos child1 y child2 para especificar que el nodo de la nueva fila se convertirá en el nodo `/3/1/1.1/`:
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid, @Child2 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
SET @Child2 = CAST('/3/1/2/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, @Child2),  
'adventure-works\ThirdNewEmployee', 'Application Intern', '3/11/07') ;  
  
```  
  
Después de completar los ejemplos A, B y C, los nodos agregados a la tabla estarán en el mismo nivel que los siguientes valores **hierarchyid**:
  
`/3/1/1/`
  
`/3/1/1.1/`
  
`/3/1/2/`
  
El nodo `/3/1/1.1/` es mayor que el nodo `/3/1/1/`, pero está en el mismo nivel de la jerarquía.
  
### <a name="d-scalar-examples"></a>D. Ejemplos escalares  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite inserciones y eliminaciones arbitrarias de cualquier nodo **hierarchyid**. Si usa GetDescendant(), siempre podrá generar un nodo entre dos nodos **hierarchyid** cualesquiera. Ejecute el código siguiente para generar nodos de ejemplo mediante `GetDescendant`:
  
```sql
DECLARE @h hierarchyid = hierarchyid::GetRoot();  
DECLARE @c hierarchyid = @h.GetDescendant(NULL, NULL);  
SELECT @c.ToString();  
DECLARE @c2 hierarchyid = @h.GetDescendant(@c, NULL);  
SELECT @c2.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
SET @c = @h.GetDescendant(@c, @c2);  
SELECT @c.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
```  
  
### <a name="e-clr-example"></a>E. Ejemplo de CLR  
En el fragmento de código siguiente se llama al método `GetDescendant()`:
  
```sql
SqlHierarchyId parent, child1, child2;  
parent = SqlHierarchyId.GetRoot();  
child1 = parent.GetDescendant(SqlHierarchyId.Null, SqlHierarchyId.Null);  
child2 = parent.GetDescendant(child1, SqlHierarchyId.Null);  
Console.Write(parent.GetDescendant(child1, child2).ToString());  
```  
  
## <a name="see-also"></a>Consulte también
[Referencia de los métodos del tipo de datos hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
