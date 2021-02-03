---
description: CHECKSUM_AGG (Transact-SQL)
title: CHECKSUM_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHECKSUM_AGG
- CHECKSUM_AGG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checksum values
- CHECKSUM_AGG function
- groups [SQL Server], checksum values
ms.assetid: cdede70c-4eb5-4c92-98ab-b07787ab7222
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9c55c317575e56f1a633033f7088a69231f21325
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202171"
---
# <a name="checksum_agg-transact-sql"></a>CHECKSUM_AGG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Esta función devuelve la suma de comprobación de los valores de un grupo. `CHECKSUM_AGG` omite los valores NULL. La [cláusula OVER](../../t-sql/queries/select-over-clause-transact-sql.md) puede seguir a `CHECKSUM_AGG`.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CHECKSUM_AGG ( [ ALL | DISTINCT ] expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
**ALL**  
Aplica la función de agregado a todos los valores. ALL es el argumento predeterminado.
  
DISTINCT  
Especifica que `CHECKSUM_AGG` devuelve la suma de comprobación de valores únicos.
  
*expression*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de entero. `CHECKSUM_AGG` no permite el uso de funciones de agregado ni subconsultas.
  
## <a name="return-types"></a>Tipos de valores devueltos
Devuelve la suma de comprobación de todos los valores de *expression* como **int**.
  
## <a name="remarks"></a>Comentarios  
`CHECKSUM_AGG` puede detectar cambios en una tabla.
  
El resultado `CHECKSUM_AGG` no depende del orden de las filas de la tabla. Asimismo, las funciones `CHECKSUM_AGG` permiten el uso de la palabra clave `DISTINCT` y de la cláusula `GROUP BY`.
  
Si se cambia un valor de una lista de expresiones, probablemente también cambiará la lista de valores de la suma de comprobación de lista, aunque hay una pequeña posibilidad de que la suma de comprobación calculada no cambie.
  
`CHECKSUM_AGG` tiene una funcionalidad similar a la de otras funciones de agregado. Para más información, vea [Funciones de agregado &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md).
  
## <a name="examples"></a>Ejemplos  
En estos ejemplos se usa `CHECKSUM_AGG` para detectar cambios en la columna `Quantity` de la tabla `ProductInventory` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
--Get the checksum value before the column value is changed.  

SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
262  
```  
  
```sql
UPDATE Production.ProductInventory   
SET Quantity=125  
WHERE Quantity=100;  
GO  

--Get the checksum of the modified column.  
SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
287  
```  
  
## <a name="see-also"></a>Vea también
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
[OVER Clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
