---
description: Consultas
title: Consultas | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5ff02a32-e8d3-479c-ae8b-07581e41f5f8
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: abe6b791139a4fbc110b4aeb43e0de267a927e96
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235996"
---
# <a name="queries"></a>Consultas

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  El lenguaje de manipulación de datos (DML) es un vocabulario usado para recuperar y trabajar con datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y en SQL Database. La mayoría funciona también en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] (revise cada instrucción individual para más información). Use estas instrucciones para agregar, modificar, consultar o quitar datos de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>En esta sección  
 En la siguiente tabla se describen las instrucciones DML que usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

:::row:::
    :::column:::
        [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)
    :::column-end:::
    :::column:::
        [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
    :::column-end:::
    :::column:::
        [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
    :::column-end:::
    :::column:::
        [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)
    :::column-end:::
    :::column:::
        [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

 En la siguiente lista se enumeran las cláusulas que se usan en varias instrucciones o cláusulas DML.  
  
|Cláusula|Se puede utilizar en estas instrucciones|  
|------------|-------------------------------------|  
|[FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)|DELETE, SELECT, UPDATE|  
|[Sugerencias &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)|DELETE, INSERT, SELECT, UPDATE|  
|[OPTION Clause &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md) (Cláusula OPTION [Transact-SQL])|DELETE, SELECT, UPDATE|  
|[OUTPUT Clause &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)|DELETE, INSERT, MERGE, UPDATE|  
|[Search Condition &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md) (Condición de búsqueda [Transact-SQL])|DELETE, MERGE, SELECT, UPDATE|  
|[Table Value Constructor &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md) (Constructor con valores de tabla [Transact-SQL])|FROM, INSERT, MERGE|  
|[TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md)|DELETE, INSERT, MERGE, SELECT, UPDATE|  
|[WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)|DELETE, SELECT, UPDATE, MATCH|  
|[WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)|DELETE, INSERT, MERGE, SELECT, UPDATE|  
| &nbsp; | &nbsp; |
