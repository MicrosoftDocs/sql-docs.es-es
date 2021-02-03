---
title: int, bigint, smallint y tinyint (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para los tipos de datos int, bigint, smallint y tinyint. Estos tipos de datos se usan para representar datos enteros.
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bigint_TSQL
- smallint
- bigint
- smallint_TSQL
- tinyint_TSQL
- int_TSQL
- int
- tinyint
dev_langs:
- TSQL
helpviewer_keywords:
- exact numeric data [SQL Server]
- numeric data
- tinyint data type
- int data type
- smallint data type
ms.assetid: 9bda5b0b-2380-4931-a1c8-f362fdefa99b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23242d075a453f244601ed573ed779d160d810bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165133"
---
# <a name="int-bigint-smallint-and-tinyint-transact-sql"></a>int, bigint, smallint y tinyint (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tipos de datos numéricos exactos que utilizan datos enteros. Para ahorrar espacio en la base de datos, use el tipo de datos más pequeño que puede contener todos los valores posibles de manera confiable. Por ejemplo, con tinyint bastaría en el caso de la edad de una persona, puesto que nadie vive más de 255 años, pero no sería suficiente en el caso de la antigüedad de un edificio, porque un edificio puede tener más de 255 años.
  
|Tipo de datos|Intervalo|Storage|  
|---|---|---|
|**bigint**|De -2^63 (-9.223.372.036.854.775.808) a 2^63-1 (9.223.372.036.854.775.807)|8 bytes|  
|**int**|De -2^31 (-2.147.483.648) a 2^31-1 (2.147.483.647)|4 bytes|  
|**smallint**|De -2^15 (-32.768) a 2^15-1 (32.767)|2 bytes|  
|**tinyint**|De 0 a 255|1 byte|  
  
## <a name="remarks"></a>Observaciones  
El tipo de datos **int** es el principal tipo de datos de valores enteros de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El tipo de datos **bigint** está pensado para usarse cuando los valores enteros pueden exceder el intervalo admitido por el tipo de datos **int**.
  
**bigint** se encuentra entre **smallmoney** y **int** en el gráfico de prioridad de tipo de datos.
  
Las funciones solo devuelven **bigint** si la expresión de parámetro es un tipo de datos **bigint**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no promueve automáticamente otros tipos de datos enteros (**tinyint**, **smallint** e **int**) en **bigint**.
  
> [!CAUTION]  
>  Cuando se usan los operadores aritméticos +, -, \*, / o % para llevar a cabo conversiones implícitas o explícitas de valores constantes **int**, **smallint**, **tinyint** o **bigint** en tipos de datos **float**, **real**, **decimal** o **numeric**, las reglas que aplica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al calcular el tipo de datos y la precisión de los resultados de la expresión varían dependiendo de si la consulta tiene parámetros automáticos o no.  
>   
>  Por lo tanto, expresiones similares en las consultas pueden generar resultados diferentes. Cuando una consulta no tiene parámetros automáticos, el valor constante primero se convierte en **numeric**, cuya precisión es lo suficientemente grande como para conservar el valor de la constante, antes de realizar la conversión al tipo de datos especificado. Por ejemplo, el valor constante 1 se convierte en **numeric (1, 0)** y el valor constante 250 se convierte en **numeric (3, 0)** .  
>   
>  Cuando una consulta tiene parámetros automáticos, el valor constante siempre se convierte en **numeric (10, 0)** antes de convertirse en el tipo de datos final. Cuando se utiliza el operador /, no solo puede diferir la precisión del tipo de los resultados entre consultas similares, sino que también puede variar el valor de los resultados. Por ejemplo, el valor de los resultados de una consulta con parámetros automáticos que incluye la expresión `SELECT CAST (1.0 / 7 AS float)` varía con respecto a la misma consulta cuando no tenga parámetros automáticos, puesto que los resultados de la primera se truncan para ajustarse al tipo de datos **numeric (10, 0)** .  
  
## <a name="converting-integer-data"></a>Convertir datos enteros
Cuando se convierten implícitamente enteros en un tipo de datos de caracteres, si el entero es demasiado grande para ajustarse al campo de carácter, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escribe el carácter ASCII 42, el asterisco (*).
  
Las constantes de enteros mayores que 2.147.483.647 se convierten en el tipo de datos **decimal**, no en el tipo de datos **bigint**. En el siguiente ejemplo se muestra que, cuando se supera el valor de umbral, el tipo de datos del resultado cambia de **int** a **decimal**.
  
```sql
SELECT 2147483647 / 2 AS Result1, 2147483649 / 2 AS Result2 ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result1      Result2  
1073741823   1073741824.500000  
```  
  
## <a name="examples"></a>Ejemplos  
En el ejemplo siguiente se crea una tabla con los tipos de datos **bigint**, **int**, **smallint** y **tinyint**. Se insertan valores en cada columna y se devuelven en la instrucción SELECT.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyBigIntColumn BIGINT  
,MyIntColumn  INT
,MySmallIntColumn SMALLINT
,MyTinyIntColumn TINYINT
);  
  
GO  
  
INSERT INTO dbo.MyTable VALUES (9223372036854775807, 2147483647,32767,255);  
 GO  
SELECT MyBigIntColumn, MyIntColumn, MySmallIntColumn, MyTinyIntColumn  
FROM dbo.MyTable;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
MyBigIntColumn       MyIntColumn MySmallIntColumn MyTinyIntColumn  
-------------------- ----------- ---------------- ---------------  
9223372036854775807  2147483647  32767            255  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  
