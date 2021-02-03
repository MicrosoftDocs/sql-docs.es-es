---
description: DATALENGTH (Transact-SQL)
title: DATALENGTH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATALENGTH_TSQL
- DATALENGTH
dev_langs:
- TSQL
helpviewer_keywords:
- number of bytes representing expression
- data types [SQL Server], length
- DATALENGTH function
- expressions [SQL Server], length
- lengths [SQL Server], data
ms.assetid: 00f377f1-cc3e-4eac-be47-b3e3f80267c9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a047c6178f9defea3abca3a1526c4b11a8fce88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183561"
---
# <a name="datalength-transact-sql"></a>DATALENGTH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve el número de bytes usados para representar cualquier expresión.

> [!NOTE]
> Para devolver el número de caracteres de una expresión de cadena, utilice la función [LEN](../../t-sql/functions/len-transact-sql.md).
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATALENGTH ( expression )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*expression*  
Una [expression](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo de datos.
  
## <a name="return-types"></a>Tipos de valores devueltos
**bigint** si *expression* tiene el tipo de datos **nvarchar(max)**, **varbinary(max)** o **varchar(max)**; en caso contrario, **int**.
  
## <a name="remarks"></a>Observaciones  
`DATALENGTH` resulta muy útil cuando se usa con tipos de datos que pueden almacenar datos de longitud variable, como:
- **image**
- **ntext**
- **nvarchar**
- **text**
- **varbinary**
- **varchar**
  
Para un valor NULL, `DATALENGTH` devuelve NULL.
  
> [!NOTE]  
> Los niveles de compatibilidad pueden afectar a los valores devueltos. Vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) para más información sobre los niveles de compatibilidad.  

> [!NOTE]
> Use [LEN](../../t-sql/functions/len-transact-sql.md) para devolver el número de caracteres codificados en una expresión de cadena determinada y [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) para devolver el tamaño en bytes para una expresión de cadena determinada. Estos resultados pueden diferir en función del tipo de datos y del tipo de codificación utilizado en la columna. Para obtener más información sobre las diferencias de almacenamiento entre los distintos tipos de codificación, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).

## <a name="examples"></a>Ejemplos  
En este ejemplo se busca la longitud de la columna `Name` en la tabla `Product`:
  
```sql
USE AdventureWorks2016  
GO
SELECT length = DATALENGTH(EnglishProductName), EnglishProductName  
FROM dbo.DimProduct  
ORDER BY EnglishProductName;  
GO  
```  
  
## <a name="see-also"></a>Consulte también
[LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
