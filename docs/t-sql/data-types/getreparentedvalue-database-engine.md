---
description: GetReparentedValue (motor de base de datos)
title: GetReparentedValue (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Reparent_TSQL
- Reparent
dev_langs:
- TSQL
helpviewer_keywords:
- Reparent [Database Engine]
ms.assetid: f47f8e25-08ef-498b-84f4-a317aca1f358
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c71c30acd6832ed2f53941bd3e841eb235c7b1cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165162"
---
# <a name="getreparentedvalue-database-engine"></a>GetReparentedValue (motor de base de datos)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve un nodo cuya ruta de acceso desde la raíz es la que va a _newRoot_, seguida de la ruta de acceso desde _oldRoot_.
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Transact-SQL syntax  
node. GetReparentedValue ( oldRoot, newRoot )  
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetReparentedValue ( SqlHierarchyId oldRoot , SqlHierarchyId newRoot )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
_oldRoot_  
**hierarchyid** que es el nodo que representa el nivel de la jerarquía que se va a modificar.
  
_newRoot_  
**hierarchyid** que representa el nodo. Reemplace la sección _oldRoot_ del nodo actual para mover el nodo.
  
## <a name="return-types"></a>Tipos de valor devuelto  
**Tipo de valor devuelto de SQL Server: hierarchyid**
  
**Tipo devuelto de CLR: SqlHierarchyId**
  
## <a name="remarks"></a>Observaciones  
Se usa para modificar el árbol, de forma que los nodos se mueven de _oldRoot_ a _newRoot_. GetReparentedValue se usa para mover un nodo de la jerarquía a una nueva ubicación de la jerarquía. El tipo de datos **hierarchyid** representa la estructura jerárquica, pero no la exige. Los usuarios deben asegurarse de que el identificador hierarchyid se estructura de forma apropiada para la nueva ubicación. Un índice único en el tipo de datos **hierarchyid** puede ayudar a evitar las entradas duplicadas. Para obtener un ejemplo de cómo mover un subárbol completo, vea [Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md).
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-comparing-two-node-locations"></a>A. Comparar dos ubicaciones de nodo  
En el ejemplo siguiente se muestra el identificador hierarchyid actual de un nodo. También se muestra cuál sería el valor de **hierarchyid** del nodo si este se moviera para convertirse en descendiente del nodo **\@NewParent**. Utiliza el método `ToString()` para mostrar las relaciones jerárquicas.
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ;  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- who is /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- who is /2/3/  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
(@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) ).ToString() AS Proposed_OrgNode_AS_Text,  
OrgNode AS Current_OrgNode,  
@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) AS Proposed_OrgNode,  
*  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode = @SubjectEmployee ;  
GO  
```  
  
### <a name="b-updating-a-node-to-a-new-location"></a>B. Actualizar un nodo a una ubicación nueva  
En el ejemplo siguiente se utiliza `GetReparentedValue()` en una instrucción UPDATE para mover un nodo de una ubicación anterior a una ubicación nueva en la jerarquía:
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ; -- Node /1/1/2/  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- Node /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- Node /2/3/  
  
UPDATE HumanResources.EmployeeDemo  
SET OrgNode = @SubjectEmployee. GetReparentedValue(@OldParent, @NewParent)   
WHERE OrgNode = @SubjectEmployee ;  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
*  
FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\gail0' ; -- Now node /2/3/2/  
```  
  
### <a name="c-clr-example"></a>C. Ejemplo de CLR  
En el siguiente fragmento de código se llama al método GetReparentedValue():
  
```sql
this. GetReparentedValue(oldParent, newParent)  
```  
  
## <a name="see-also"></a>Consulte también
[Referencia de los métodos del tipo de datos hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
