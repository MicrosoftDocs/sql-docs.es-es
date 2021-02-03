---
description: GetAncestor (motor de base de datos)
title: GetAncestor (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetAncestor_TSQL
- GetAncestor
dev_langs:
- TSQL
helpviewer_keywords:
- GetAncestor [Database Engine]
ms.assetid: b96a986f-d5e4-4034-8013-de7974594ee9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0a306814f0146fdb0dc1833d0d6f98d84cae8f1a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172544"
---
# <a name="getancestor-database-engine"></a>GetAncestor (motor de base de datos)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve un objeto **hierarchyid** que representa el antepasado *número* de *this*.
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Transact-SQL syntax  
child.GetAncestor ( n )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetAncestor ( int n )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*n*  
Un tipo **int**, que representa el número de niveles que subir en la jerarquía.
  
## <a name="return-types"></a>Tipos de valores devueltos
**Tipo de valor devuelto de SQL Server: hierarchyid**
  
**Tipo devuelto de CLR: SqlHierarchyId**
  
## <a name="remarks"></a>Observaciones  
Se usa para probar si cada nodo de la salida tiene el nodo actual como antecesor en el nivel especificado.
  
Si se pasa un número mayor que [GetLevel()](../../t-sql/data-types/getlevel-database-engine.md), se devuelve NULL.
  
Si se pasa un número negativo, se produce una excepción.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-finding-the-child-nodes-of-a-parent"></a>A. Encontrar los nodos secundarios de un elemento primario  
`GetAncestor(1)` devuelve los empleados que tienen `david0` como su antecesor inmediato (su elemento primario). En el ejemplo siguiente se utiliza `GetAncestor(1)`.
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\david0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(1) = @CurrentEmployee ;  
```  
  
### <a name="b-returning-the-grandchildren-of-a-parent"></a>B. Devolución de los elementos descendientes del secundario de un elemento primario  
`GetAncestor(2)` devuelve los empleados que están dos niveles por debajo en la jerarquía del nodo actual. Estos empleados son los subelementos secundarios del nodo actual. En el ejemplo siguiente se utiliza `GetAncestor(2)`.
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\ken0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(2) = @CurrentEmployee ;  
```  
  
### <a name="c-returning-the-current-row"></a>C. Devolución de la fila actual  
Ejecute el siguiente código para devolver el nodo actual usando `GetAncestor(0)`.
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\david0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(0) = @CurrentEmployee ;  
```  
  
### <a name="d-returning-a-hierarchy-level-if-a-table-isnt-present"></a>D. Devolución de un nivel de jerarquía si una tabla no está presente  
`GetAncestor` devuelve el nivel seleccionado en la jerarquía, incluso si una tabla no está presente. Por ejemplo, el código siguiente designa a un empleado actual y devuelve el identificador `hierarchyid` del antecesor del empleado actual sin hacer referencia a una tabla.
  
```sql
DECLARE @CurrentEmployee hierarchyid ;  
DECLARE @TargetEmployee hierarchyid ;  
SELECT @CurrentEmployee = '/2/3/1.2/5/3/' ;  
SELECT @TargetEmployee = @CurrentEmployee.GetAncestor(2) ;  
SELECT @TargetEmployee.ToString(), @TargetEmployee ;  
```  
  
### <a name="e-calling-a-common-language-runtime-method"></a>E. Llamada de un método del Common Language Runtime  
En el fragmento de código siguiente se llama al método `GetAncestor()`.
  
```sql
this.GetAncestor(1)  
```  
  
## <a name="see-also"></a>Consulte también
[IsDescendantOf &#40;motor de base de datos&#41;](../../t-sql/data-types/isdescendantof-database-engine.md)  
[Referencia de los métodos del tipo de datos hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
