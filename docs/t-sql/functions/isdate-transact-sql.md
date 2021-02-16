---
description: ISDATE (Transact-SQL)
title: ISDATE (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISDATETIME
- ISDATE_TSQL
- ISDATE
- ISDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], ISDATE
- validate dates times [SQL Server]
- formats [SQL Server], time
- dates [SQL Server], validate
- verify dates times [SQL Server]
- functions [SQL Server], time
- formats [SQL Server], dates
- functions [SQL Server], date and time
- time [SQL Server], functions
- time [SQL Server], validate
- ISDATE function [SQL Server]
ms.assetid: 8e2c9ee7-388a-432f-b2c9-7b398f26bf85
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d43b4ab5bbd5c029022fe7a73bf7b55848f0464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350399"
---
# <a name="isdate-transact-sql"></a>ISDATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve 1 si *expression* es un valor válido de **date**, **time** o **datetime**; en caso contrario, devuelve 0.  
  
 ISDATE devuelve 0 si *expression* es un valor **datetime2**.  
  
 Para ver información general sobre todos los tipos de datos y funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)], vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md). Tenga en cuenta que el intervalo de datos de fecha y hora es de 01-01-1753 a 31-12-9999, mientras que el intervalo de datos de fecha es de 01-01-0001 a 31-12-9999.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ISDATE ( expression )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es una cadena de caracteres o una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) que se puede convertir en una cadena de caracteres. La expresión debe tener menos de 4.000 caracteres. No se permiten tipos de datos de fecha y hora, excepto datetime y smalldatetime, como argumento para ISDATE.  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 ISDATE solo es determinista si se usa con la función [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md), se especifica el parámetro de estilo CONVERT y el estilo no es igual a 0, 100, 9 ni 109.  
  
 El valor devuelto de ISDATE depende de los valores establecidos por [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md), [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) y la [opción de configuración del servidor Idioma predeterminado](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).  
  
## <a name="isdate-expression-formats"></a>Formatos de expresión ISDATE  
 Para obtener ejemplos de formatos válidos para los que ISDATE devolverá 1, vea la sección "Formatos de literales de cadena compatibles para datetime" en los temas [datetime](../../t-sql/data-types/datetime-transact-sql.md) y [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md). Para obtener más ejemplo, vea también la columna Entrada/salida de la sección "Argumentos" de [CAST y CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 En la tabla siguiente se resumen los formatos de expresión de entrada que no son válidos y devuelven 0 o un error.  
  
|Expresión ISDATE|Valor devuelto de ISDATE|  
|-----------------------|-------------------------|  
|NULL|0|  
|Valores de tipos de datos incluidos en la lista [Tipos de datos](../../t-sql/data-types/data-types-transact-sql.md) en cualquier categoría de tipo de datos distinta de las cadenas de caracteres, cadenas de caracteres Unicode o fecha y hora.|0|  
|Valores de los tipos de datos **text**, **ntext** o **image**.|0|  
|Cualquier valor que tiene una escala de precisión por segundos mayor que 3 (.0000 a.0000000... n) ISDATE devuelve 0 si *expression* es un valor **datetime2**, pero devolverá 1 si *expression* es un valor **datetime** válido.|0|  
|Cualquier valor que mezcla una fecha válida con un valor no válido, por ejemplo 1995-10-1a.|0|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-isdate-to-test-for-a-valid-datetime-expression"></a>A. Utilizar ISDATE para probar si una expresión de fecha y hora es válida  
 En este ejemplo se muestra cómo usar `ISDATE` para probar si una cadena de caracteres es un tipo **datetime** válido.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    PRINT 'VALID'  
ELSE  
    PRINT 'INVALID';  
```  
  
### <a name="b-showing-the-effects-of-the-set-dateformat-and-set-language-settings-on-return-values"></a>B. Mostrar los efectos de los parámetros SET DATEFORMAT y SET LANGUAGE en los valores devueltos  
 Las instrucciones siguientes muestran los valores que se devuelven al establecer `SET DATEFORMAT` y `SET LANGUAGE`.  
  
```sql  
/* Use these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
/* Expression in mdy dateformat */  
SELECT ISDATE('04/15/2008'); --Returns 1.  
/* Expression in mdy dateformat */  
SELECT ISDATE('04-15-2008'); --Returns 1.   
/* Expression in mdy dateformat */  
SELECT ISDATE('04.15.2008'); --Returns 1.   
/* Expression in myd  dateformat */  
SELECT ISDATE('04/2008/15'); --Returns 1.  
  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET DATEFORMAT dmy;  
SELECT ISDATE('15/04/2008'); --Returns 1.  
SET DATEFORMAT dym;  
SELECT ISDATE('15/2008/04'); --Returns 1.  
SET DATEFORMAT ydm;  
SELECT ISDATE('2008/15/04'); --Returns 1.  
SET DATEFORMAT ymd;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET LANGUAGE English;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET LANGUAGE Hungarian;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET LANGUAGE Swedish;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET LANGUAGE Italian;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
/* Return to these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-isdate-to-test-for-a-valid-datetime-expression"></a>C. Utilizar ISDATE para probar si una expresión de fecha y hora es válida  
 En este ejemplo se muestra cómo usar `ISDATE` para probar si una cadena de caracteres es un tipo **datetime** válido.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    SELECT 'VALID';  
ELSE  
    SELECT 'INVALID';  
```  
  
## <a name="see-also"></a>Consulte también  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
