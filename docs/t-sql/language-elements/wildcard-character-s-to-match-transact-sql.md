---
title: Carácter comodín [] para hacer coincidir caracteres
description: Use un carácter comodín para hacer coincidir uno o varios caracteres.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Match
- wildcard
- '[ ]'
- '[_]_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- '[ ] (wildcard - character(s) to match)'
ms.assetid: 57817576-0bf1-49ed-b05d-fac27e8fed7a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2c69fbf1900de88da7faf08e477eb4775187b964
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339801"
---
# <a name="--wildcard---characters-to-match-transact-sql"></a>\[ \] (caracteres comodín para coincidir) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Halla coincidencias con cada uno de los caracteres del intervalo o conjunto especificado entre corchetes `[ ]`. Estos caracteres comodín se pueden usar en comparaciones de cadenas donde se buscan coincidencias de patrón, como sucede con `LIKE` y `PATINDEX`.  

 
## <a name="examples"></a>Ejemplos  
### <a name="a-simple-example"></a>A. Ejemplo sencillo   
En el siguiente ejemplo se devuelven los nombres que comienzan por la letra `m`. `[n-z]` especifica que la segunda letra debe estar en alguna parte del intervalo entre `n` y `z`. El carácter comodín de porcentaje `%` permite cualquier carácter que comience por el tercer carácter. Las bases de datos `model` y `msdb` cumplen este criterio, pero no la base de datos `master`, de modo que se excluye del conjunto de resultados.
 
```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm[n-z]%';
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
name
-----
model
msdb
```   
 Es posible que tenga instaladas más bases de datos aplicables.


### <a name="b-more-complex-example"></a>B. Ejemplo más complejo   
 En el ejemplo siguiente se utiliza el operador [] para buscar los identificadores y nombres de todos los empleados de [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] cuyas direcciones tienen un código postal de cuatro dígitos.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.BusinessEntityID, p.FirstName, p.LastName, a.PostalCode  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
INNER JOIN Person.Address AS a ON a.AddressID = ea.AddressID  
WHERE a.PostalCode LIKE '[0-9][0-9][0-9][0-9]';  
```  
  
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  
  
```  
EmployeeID      FirstName      LastName      PostalCode  
----------      ---------      ---------     ----------  
290             Lynn           Tsoflias      3000  
```  

### <a name="c-using-a-set-that-combines-ranges-and-single-characters"></a>C. Uso de un conjunto que combine intervalos y caracteres únicos

Un conjunto de caracteres comodín puede incluír caracteres únicos e intervalos. El siguiente ejemplo usa el operador [] para buscar una cadena que empieza por un número o una serie de caracteres especiales.

```sql
SELECT [object_id], OBJECT_NAME(object_id) AS [object_name], name, column_id 
FROM sys.columns 
WHERE name LIKE '[0-9!@#$.,;_]%';
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
object_id     object_name                         name  column_id
---------     -----------                         ----  ---------
615673241     vSalesPersonSalesByFiscalYears      2002  5
615673241     vSalesPersonSalesByFiscalYears      2003  6
615673241     vSalesPersonSalesByFiscalYears      2004  7
1591676718    JunkTable                           _xyz  1
```
  
## <a name="see-also"></a>Consulte también  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
  [% &#40;caracteres comodín para coincidencia&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
 [&#91;^&#93; &#40;caracteres comodín para no coincidencia&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [\_ &#40;comodín, coincidir un carácter&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
