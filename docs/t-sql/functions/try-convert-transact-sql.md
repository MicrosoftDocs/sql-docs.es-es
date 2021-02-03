---
description: TRY_CONVERT (Transact-SQL)
title: TRY_CONVERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRY_CONVERT_TSQL
- TRY_CONVERT
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CONVERT function
ms.assetid: 3e6e7825-6482-4cb2-a8c2-9abc99e265a6
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016 ||>= sql-server-linux-2017||>= aps-pdw-2016||= azure-sqldw-latest
ms.openlocfilehash: 65d082cc3e4758688c7405e7bfc337dfb28d525d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159712"
---
# <a name="try_convert-transact-sql"></a>TRY_CONVERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una conversión de valor al tipo de datos especificado si la conversión se realiza correctamente; de lo contrario, devuelve NULL.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
TRY_CONVERT ( data_type [ ( length ) ], expression [, style ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *data_type [ ( length ) ]*  
 Tipo de datos al que se va a convertir *expression*.  
  
 *expression*  
 Valor que se va a convertir.  
  
 *style*  
 Expresión opcional de tipo entero que especifica el modo en que la función **TRY_CONVERT** va a traducir *expression*.  
  
 *style* acepta los mismos valores que el parámetro *style* de la función **CONVERT**. Para obtener más información, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 El intervalo de valores aceptables está determinado por el valor de *data_type*. Si *style* es NULL, **TRY_CONVERT** devuelve NULL.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve una conversión de valor al tipo de datos especificado si la conversión se realiza correctamente; de lo contrario, devuelve NULL.  
  
## <a name="remarks"></a>Observaciones  
 **TRY_CONVERT** toma el valor que se le ha pasado e intenta convertirlo al *data_type* especificado. Si la conversión se realiza correctamente, **TRY_CONVERT** devuelve el valor como el *data_type* especificado; si se produce un error, se devuelve NULL. Pero si se solicita una conversión que no se permite explícitamente, **TRY_CONVERT** generará un error.  
  
 **TRY_CONVERT** es una palabra clave reservada en el nivel de compatibilidad 110 y posteriores.  
  
 Esta función se puede enviar de forma remota a servidores que tengan una versión de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y superior. No se puede enviar de forma remota a servidores que tengan una versión inferior a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-try_convert-returns-null"></a>A. TRY_CONVERT devuelve NULL  
 En el ejemplo siguiente se muestra que TRY_CONVERT devuelve NULL cuando se produce un error en la conversión.  
  
```sql  
SELECT   
    CASE WHEN TRY_CONVERT(float, 'test') IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------------  
Cast failed  
  
(1 row(s) affected)  
```  
  
 En el ejemplo siguiente se demuestra que la expresión debe tener el formato esperado.  
  
```sql  
SET DATEFORMAT dmy;  
SELECT TRY_CONVERT(datetime2, '12/31/2010') AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-try_convert-fails-with-an-error"></a>B. TRY_CONVERT genera un error  
 En el ejemplo siguiente, se muestra que TRY_CONVERT devuelve un error cuando la conversión no está explícitamente permitida.  
  
```sql  
SELECT TRY_CONVERT(xml, 4) AS Result;  
GO  
```  
  
 El resultado de esta instrucción es un error, ya que un entero no se puede convertir en un tipo de datos xml.  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-try_convert-succeeds"></a>C. TRY_CONVERT se realiza correctamente  
 Este ejemplo demuestra que la expresión debe tener el formato esperado.  
  
```sql
SET DATEFORMAT mdy;  
SELECT TRY_CONVERT(datetime2, '12/31/2010') AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------------------  
2010-12-31 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
