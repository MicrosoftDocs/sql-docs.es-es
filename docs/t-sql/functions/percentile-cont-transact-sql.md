---
description: PERCENTILE_CONT (Transact-SQL)
title: PERCENTILE_CONT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENTILE_CONT_TSQL
- PERCENTILE_CONT
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, PERCENTILE_CONT
- PERCENTILE_CONT function
ms.assetid: d019419e-5297-4994-97d5-e9c8fc61bbf4
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2e2ab7fa86b8888f105428847bdd751ac749bba5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186059"
---
# <a name="percentile_cont-transact-sql"></a>PERCENTILE_CONT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Calcula un percentil basándose en una distribución continua de valores de columna en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El resultado se interpola y puede no ser igual que ninguno de los valores concretos de la columna.  
  
 ![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
PERCENTILE_CONT ( numeric_literal )   
    WITHIN GROUP ( ORDER BY order_by_expression [ ASC | DESC ] )  
    OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *numeric_literal*  
 El percentil que se va a calcular. El valor debe estar entre 0,0 y 1,0.  
  
 WITHIN GROUP **(** ORDER BY *order_by_expression* [ **ASC** | DESC ] **)**  
 Especifica una lista de valores numéricos para ordenar y cuyo percentil se va a calcular. Solo se permite una *order_by_expression*. La expresión necesita dar como resultado un tipo numérico exacto o aproximado, y no se permiten otros tipos de datos. Los tipos numéricos exactos con **int**, **bigint**, **smallint**, **tinyint**, **numeric**, **bit**, **decimal**, **smallmoney** y **money**. Los tipos numéricos aproximados son **float** y **real**. El criterio de ordenación predeterminado es ascendente.  
  
 OVER **(** \<partition_by_clause> **)**  
 Divide el conjunto de resultados generado por la cláusula FROM en particiones a las que se aplica la función de percentil. Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md). Los parámetros \<ORDER BY clause> y \<rows or range clause> de la sintaxis OVER no se pueden especificar en una función PERCENTILE_CONT.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **float(53)**  
  
## <a name="compatibility-support"></a>Soporte de compatibilidad  
 En el nivel de compatibilidad 110 y posteriores, WITHIN GROUP es una palabra clave reservada. Para obtener más información, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="general-remarks"></a>Notas generales  
 Se omite cualquier valor NULL del conjunto de datos.  
  
 PERCENTILE_CONT es no determinista. Para obtener más información, consulte [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-basic-syntax-example"></a>A. Ejemplo de sintaxis básica  
 En el ejemplo siguiente se usa PERCENTILE_CONT y PERCENTILE_DISC para buscar el salario medio de los empleados de cada departamento. Tenga en cuenta que puede que estas funciones no devuelvan el mismo valor. Esto se debe a que PERCENTILE_CONT interpola el valor adecuado, tanto si existe en el conjunto de datos como si no existe, mientras que PERCENTILE_DISC siempre devuelve un valor real del conjunto.  
  
```sql  
USE AdventureWorks2012;  
  
SELECT DISTINCT Name AS DepartmentName  
      ,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianCont  
      ,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianDisc  
FROM HumanResources.Department AS d  
INNER JOIN HumanResources.EmployeeDepartmentHistory AS dh   
    ON dh.DepartmentID = d.DepartmentID  
INNER JOIN HumanResources.EmployeePayHistory AS ph  
    ON ph.BusinessEntityID = dh.BusinessEntityID  
WHERE dh.EndDate IS NULL;  
```  
  
 A continuación se muestra un conjunto parcial de resultados.  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.8269      16.8269
Engineering            34.375       32.6923
Executive              54.32695     48.5577
Human Resources        17.427850    16.5865
```  

### <a name="b-basic-syntax-example"></a>B. Ejemplo de sintaxis básica  
 En el ejemplo siguiente se usa PERCENTILE_CONT y PERCENTILE_DISC para buscar el salario medio de los empleados de cada departamento. Tenga en cuenta que puede que estas funciones no devuelvan el mismo valor. Esto se debe a que PERCENTILE_CONT interpola el valor adecuado, tanto si existe en el conjunto de datos como si no existe, mientras que PERCENTILE_DISC siempre devuelve un valor real del conjunto.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT DepartmentName  
,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianCont  
,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianDisc  
FROM dbo.DimEmployee; 
```  
  
 A continuación se muestra un conjunto parcial de resultados.  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.826900    16.8269
Engineering            34.375000    32.6923
Human Resources        17.427850    16.5865
Shipping and Receiving 9.250000      9.0000
```  
  
## <a name="see-also"></a>Consulte también  
 [PERCENTILE_DISC &#40;Transact-SQL&#41;](../../t-sql/functions/percentile-disc-transact-sql.md)  
  
 
