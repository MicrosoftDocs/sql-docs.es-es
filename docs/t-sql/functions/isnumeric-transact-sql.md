---
description: ISNUMERIC (Transact-SQL)
title: ISNUMERIC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISNUMERIC
- ISNUMERIC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], valid numeric type
- numeric data
- ISNUMERIC function
- verifying valid numeric type
- valid numeric type [SQL Server]
- checking valid numeric type
ms.assetid: 7aa816de-529a-4f6c-a99f-4d5a9ef599eb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9fa722dde0889cf9e75ffa9261686a137d53af84
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200004"
---
# <a name="isnumeric-transact-sql"></a>ISNUMERIC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Determina si una expresión es un tipo numérico válido.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
ISNUMERIC ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es la [expresión](../../t-sql/language-elements/expressions-transact-sql.md) que se va a evaluar.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 ISNUMERIC devuelve 1 cuando la expresión de entrada se evalúa para un tipo de datos numérico válido; de lo contrario, devuelve 0. Entre los [tipos de datos numéricos](../../t-sql/data-types/numeric-types.md) válidos se encuentran los siguientes:  

| Área | Tipos de datos numéricos |
|-|-|
| [Valores numéricos exactos](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) | **bigint**, **int**, **smallint**, **tinyint**, **bit** |
| [Precisión fija](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) | **decimal**, **numeric** |
| [Aproximado](../../t-sql/data-types/float-and-real-transact-sql.md) | **float**, **real** |
| [Valores monetarios](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) | **money**, **smallmoney** |

> [!NOTE]  
> ISNUMERIC devuelve 1 para algunos caracteres que no son números, como el signo más (+), menos (-) y símbolos de moneda válidos, como el signo de dólar ($). Para obtener una lista completa de los símbolos de moneda, vea [money y smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo utiliza `ISNUMERIC` para devolver todos los códigos postales que no son valores numéricos.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT City, PostalCode  
FROM Person.Address   
WHERE ISNUMERIC(PostalCode) <> 1;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 El siguiente ejemplo utiliza `ISNUMERIC` para devolver todos los códigos postales que no son valores numéricos.  
  
```sql
USE master;  
GO  
SELECT name, ISNUMERIC(name) AS IsNameANumber, database_id, ISNUMERIC(database_id) AS IsIdANumber   
FROM sys.databases;  
GO  
```  
  
## <a name="see-also"></a>Consulte también

- [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)
- [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
