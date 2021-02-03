---
description: DECOMPRESS (Transact-SQL)
title: DECOMPRESS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECOMPRESS
- DECOMPRESS_TSQL
helpviewer_keywords:
- DECOMPRESS function
ms.assetid: 738d56be-3870-4774-b112-3dce27becc11
author: cawrites
ms.author: chadam
ms.openlocfilehash: d79160f9ca9f87d67333797dd8198dc6c14678eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188647"
---
# <a name="decompress-transact-sql"></a>DECOMPRESS (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Esta función descomprimirá un valor de expresión de entrada mediante el algoritmo GZIP. `DECOMPRESS` devolverá una matriz de bytes (de tipo VARBINARY(MAX)).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
DECOMPRESS ( expression )  
```  
  
## <a name="arguments"></a>Argumentos
 *expression*  
Un valor **varbinary(** _n_ **)** , **varbinary(max)** o **binary(** _n_ **)** . Para más información, vea [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="return-types"></a>Tipos de valor devuelto  
Un valor de tipo de datos **varbinary (max)**. `DECOMPRESS` usará el algoritmo ZIP para descomprimir el argumento de entrada. El usuario debe convertir explícitamente el resultado en un tipo de destino si es necesario.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-decompress-data-at-query-time"></a>A. Descomprimir datos en el tiempo de consulta  
En este ejemplo se muestra cómo devolver datos de tabla comprimidos:  
  
```sql  
SELECT _id, name, surname, datemodified,  
             CAST(DECOMPRESS(info) AS NVARCHAR(MAX)) AS info  
FROM player;  
```  
  
### <a name="b-display-compressed-data-using-computed-column"></a>B. Mostrar los datos comprimidos con columna calculada  
En este ejemplo se muestra cómo crear una tabla para almacenar los datos descomprimidos:  
  
```sql  
CREATE TABLE example_table (  
    _id INT PRIMARY KEY IDENTITY,  
    name NVARCHAR(max),  
    surname NVARCHAR(max),  
    info VARBINARY(max),  
    info_json as CAST(DECOMPRESS(info) as NVARCHAR(max))  
);  
```  
  
## <a name="see-also"></a>Vea también  
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
 [COMPRESS &#40;Transact-SQL&#41;](../../t-sql/functions/compress-transact-sql.md)  
  
  
