---
description: Funciones de categoría (Transact-SQL)
title: Funciones de categoría (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- ranking functions
- row ranking [SQL Server]
- functions [SQL Server], ranking
- ranking rows
ms.assetid: e7f917ba-bf4a-4fe0-b342-a91bcf88a71b
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0be4ad27b55f5edbf5615adbf8ebfb5890be56d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182319"
---
# <a name="ranking-functions-transact-sql"></a>Funciones de categoría (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Las funciones de categoría devuelven un valor de categoría para cada fila de una partición. Según la función que se utilice, algunas filas pueden recibir el mismo valor que otras. Las funciones de categoría son no deterministas.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] proporciona las siguientes funciones de categoría:  

:::row:::
    :::column:::
        [RANK](../../t-sql/functions/rank-transact-sql.md)
    :::column-end:::
    :::column:::
        [NTILE](../../t-sql/functions/ntile-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [DENSE_RANK](../../t-sql/functions/dense-rank-transact-sql.md)
    :::column-end:::
    :::column:::
        [ROW_NUMBER](../../t-sql/functions/row-number-transact-sql.md)
    :::column-end:::
:::row-end:::
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestran las cuatro funciones de categoría usadas en la misma consulta. Consulte cada función de categoría para ver ejemplos específicos de las funciones.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS Rank  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS Quartile  
    ,s.SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|FirstName|Apellidos|Row Number|Rank|Dense Rank|Quartile|SalesYTD|PostalCode|  
|---------------|--------------|----------------|----------|----------------|--------------|--------------|----------------|  
|Michael|Blythe|1|1|1|1|4 557 045,0459|98027|  
|Linda|Mitchell|2|1|1|1|5 200 475,2313|98027|  
|Jillian|Carson|3|1|1|1|3 857 163,6332|98027|  
|Garrett|Vargas|4|1|1|1|1 764 938,9859|98027|  
|Tsvi|Reiter|5|1|1|2|2 811 012,7151|98027|  
|Shu|Ito|6|6|2|2|3 018 725,4858|98055|  
|José|Saraiva|7|6|2|2|3 189 356,2465|98055|  
|David|Campbell|8|6|2|3|3 587 378,4257|98055|  
|Tete|Mensa Annan|9|6|2|3|1 931 620,1835|98055|  
|Lynn|Tsoflias|10|6|2|3|1 758 385,926|98055|  
|Rachel|Valdez|11|6|2|4|2 241 204,0424|98055|  
|Jae|Pak|12|6|2|4|5 015 682,3752|98055|  
|Ranjit|Varkey Chudukatil|13|6|2|4|3 827 950,238|98055|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [OVER &#40;cláusula de Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
