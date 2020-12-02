---
description: Parse (motor de base de datos)
title: Parse (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Parse
- Parse_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Parse [Database Engine]
ms.assetid: b37e28b6-6e2e-470a-945b-ce5252da743a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8ebfca7738f8310108aab9ba988e658e7a5c1e17
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "92038368"
---
# <a name="parse-database-engine"></a>Parse (motor de base de datos)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Parse convierte la representación de cadena canónica de **hierarchyid** en un valor **hierarchyid**. Se llama implícitamente a Parse cuando se produce una conversión de un tipo de cadena a **hierarchyid**. Tiene el efecto contrario de [ToString](../../t-sql/data-types/tostring-database-engine.md). Parse() es un método estático.
  
## <a name="syntax"></a>Sintaxis  
  
```sql
-- Transact-SQL syntax  
hierarchyid::Parse ( input )  
-- This is functionally equivalent to the following syntax   
-- which implicitly calls Parse():  
CAST ( input AS hierarchyid )  
```  
  
```csharp
-- CLR syntax  
static SqlHierarchyId Parse ( SqlString input )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*input*  
[!INCLUDE[tsql](../../includes/tsql-md.md)]: Valor de tipo de datos de carácter que se va a convertir.
  
CLR: Valor de cadena que se va a evaluar.
  
## <a name="return-types"></a>Tipos de valor devuelto  
**Tipo de valor devuelto de SQL Server: hierarchyid**
  
**Tipo devuelto de CLR: SqlHierarchyId**
  
## <a name="remarks"></a>Observaciones  
Si Parse recibe un valor que no es una representación de cadena válida de un **hierarchyid**, se producirá una excepción. Por ejemplo, si los tipos de datos **char** contienen espacios finales, se producirá una excepción.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-converting-transact-sql-values-without-a-table"></a>A. Convertir valores de Transact-SQL sin una tabla  
En el siguiente ejemplo de código se usa `ToString` para convertir un valor **hierarchyid** en una cadena, y `Parse` para convertir un valor de cadena en **hierarchyid**.
  
```sql
DECLARE @StringValue AS NVARCHAR(4000), @hierarchyidValue AS hierarchyid  
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
  
### <a name="b-clr-example"></a>B. Ejemplo de CLR  
En el fragmento de código siguiente se llama al método Parse():
  
```csharp
string input = "/1/2/";  
SqlHierarchyId.Parse(input);  
```  
  
## <a name="see-also"></a>Consulte también
[Referencia de los métodos del tipo de datos hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Datos jerárquicos &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
