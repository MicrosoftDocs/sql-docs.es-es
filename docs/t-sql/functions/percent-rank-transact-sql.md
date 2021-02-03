---
description: PERCENT_RANK (Transact-SQL)
title: PERCENT_RANK (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENT_RANK_TSQL
- PERCENT_RANK
dev_langs:
- TSQL
helpviewer_keywords:
- PERCENT_RANK function
- analytic functions, PERCENT_RANK
ms.assetid: e361c2d4-c01f-4da4-8e89-1ddc724a2629
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 385d607f24c981bb3ee32ab434e7256ed685140d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237342"
---
# <a name="percent_rank-transact-sql"></a>PERCENT_RANK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Calcula el rango relativo de una fila dentro de un grupo de filas de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Use PERCENT_RANK para evaluar la situación relativa de un valor dentro de un conjunto de resultados de la consulta o de una partición. PERCENT_RANK es similar a la función CUME_DIST.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
PERCENT_RANK( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* divide el conjunto de resultados generado por la cláusula FROM en particiones a las que se aplica la función. Si no se especifica, la función trata todas las filas del conjunto de resultados de la consulta como un único grupo. _order\_by\_clause_ determina el orden lógico en el que se realiza la operación. *order_by_clause* es obligatorio. La \<rows or range clause\> de la sintaxis OVER no se puede especificar en una función PERCENT_RANK.  Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **float(53)**  
  
## <a name="general-remarks"></a>Notas generales  
 El intervalo de valores devueltos por PERCENT_RANK es mayor que 0 y menor o igual que 1. La primera fila de cualquier conjunto tiene un PERCENT_RANK de 0. Se incluyen valores NULL de forma predeterminada y se tratan como los posibles valores más bajos.  
  
 PERCENT_RANK es no determinista. Para obtener más información, consulte [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa la función CUME_DIST para calcular el percentil de salario de cada empleado dentro de un departamento determinado. El valor devuelto por la función CUME_DIST representa el porcentaje de empleados que tienen un salario menor o igual que el empleado actual del mismo departamento. La función PERCENT_RANK calcula el rango de salario del empleado dentro de un departamento como un porcentaje. La cláusula PARTITION BY se especifica para crear particiones de las filas del conjunto de resultados por departamento. La cláusula ORDER BY de la cláusula OVER ordena las filas de cada partición. La cláusula ORDER BY de la instrucción SELECT ordena las filas del conjunto de resultados completo.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [CUME_DIST &#40;Transact-SQL&#41;](../../t-sql/functions/cume-dist-transact-sql.md)  
  
  
