---
description: ToString (motor de base de datos)
title: ToString (motor de base de datos)
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ToString
- ToString_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ToString [Database Engine]
ms.assetid: 5fc11ca5-c26d-4518-9512-67aa0270f110
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: fc4af80c3e3c05009eb488a87caa0107f3185e87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179634"
---
# <a name="tostring-database-engine"></a>ToString (motor de base de datos)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una cadena con la representación lógica de *this*. Se llama implícitamente a ToString cuando se produce una conversión de **hierarchyid** en un tipo de cadena. Tiene el efecto contrario de [Parse &#40;motor de base de datos&#41;](../../t-sql/data-types/parse-database-engine.md).
  
## <a name="syntax"></a>Sintaxis  

```syntaxsql
-- Transact-SQL syntax
node.ToString  ( )
-- This is functionally equivalent to the following syntax  
-- which implicitly calls ToString():  
CAST(node AS nvarchar(4000))  
```  
  
```syntaxsql
-- CLR syntax
string ToString  ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valores devueltos

**Tipo de valor devuelto de SQL Server: nvarchar(4000)**
  
**Tipo de valor devuelto de CLR: String**
  
## <a name="remarks"></a>Observaciones  
Devuelve la ubicación lógica en la jerarquía. Por ejemplo, `/2/1/` representa la cuarta fila ([!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) de la siguiente estructura jerárquica de un sistema de archivos:
  
```sql
/        C:\  
/1/      C:\Database Files  
/2/      C:\Program Files  
/2/1/    C:\Program Files\Microsoft SQL Server  
/2/2/    C:\Program Files\Microsoft Visual Studio  
/3/      C:\Windows  
```  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-transact-sql-example-in-a-table"></a>A. Ejemplo de Transact-SQL en una tabla  
En el siguiente ejemplo se devuelve la columna `OrgNode` como tipo de datos **hierarchyid** y en el formato de cadena, que es más legible:
  
```sql
SELECT OrgNode,  
OrgNode.ToString() AS Node  
FROM HumanResources.EmployeeDemo  
ORDER BY OrgNode ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
OrgNode   Node  
0x        /  
0x58      /1/  
0x5AC0    /1/1/  
0x5B40    /1/2/  
0x5BC0    /1/3/  
0x5C20    /1/4/  
...  
```  
  
### <a name="b-converting-transact-sql-values-without-a-table"></a>B. Convertir valores de Transact-SQL sin una tabla  
En el siguiente ejemplo de código se usa `ToString` para convertir un valor **hierarchyid** en una cadena, y `Parse` para convertir un valor de cadena en **hierarchyid**.
  
```sql
DECLARE @StringValue AS nvarchar(4000), @hierarchyidValue AS hierarchyid  
SET @StringValue = '/1/1/3/'  
SET @hierarchyidValue = 0x5ADE  
  
SELECT hierarchyid::Parse(@StringValue) AS hierarchyidRepresentation,  
@hierarchyidValue.ToString() AS StringRepresentation ;
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
hierarchyidRepresentation    StringRepresentation
-------------------------    -----------------------
0x5ADE                       /1/1/3/
```
  
### <a name="c-clr-example"></a>C. Ejemplo de CLR  
En el siguiente fragmento de código se llama al método ToString():
  
```sql
this.ToString()  
```  
  
## <a name="see-also"></a>Consulte también
[Referencia de los métodos del tipo de datos hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
