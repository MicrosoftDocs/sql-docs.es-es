---
description: STR (Transact-SQL)
title: STR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STR
- STR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- converting numbers to characters
- characters [SQL Server], converting
- character data [SQL Server]
- STR function
ms.assetid: de03531b-d9e7-4c3c-9604-14e582ac20c6
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cd56db7440b13baa144340ccccf53f0e1e6155a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198464"
---
# <a name="str-transact-sql"></a>STR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve datos de caracteres convertidos de datos numéricos. Los datos de caracteres están justificados a la derecha, con una longitud y precisión decimal especificadas. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
STR ( float_expression [ , length [ , decimal ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *float_expression*  
 Es una expresión de tipo de datos numéricos aproximados (**float**) con separador decimal.  
  
 *length*  
 Es la longitud total. Ésta incluye el separador decimal, el signo, los dígitos y los espacios. El valor predeterminado es 10.  
  
 *decimal*  
 Es el número de posiciones a la derecha del separador decimal. *decimal* debe ser inferior o igual a 16. Si el parámetro *decimal* es mayor que 16, el resultado se trunca en dieciséis posiciones a la derecha del separador decimal.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **varchar**  
  
## <a name="remarks"></a>Observaciones  
 Si se especifican, los valores de los parámetros *length* y *decimal* de STR deben ser positivos. El número se redondea a un entero de forma predeterminada o si el parámetro decimal es 0. La longitud especificada debe ser mayor o igual que la parte del número situada delante del separador decimal más el signo del número (si lo hay). Una *float_expression* corta se justifica a la derecha según la longitud especificada y una *float_expression* larga se trunca según el número de decimales especificados. Por ejemplo, STR(12, 10) da como resultado 12. Se justifica a la derecha en el conjunto de resultados. Pero STR(1223, 2) trunca el conjunto de resultados a \*\*. Las funciones de cadena se pueden anidar.  
  
> [!NOTE]  
>  Para convertir a datos Unicode, utilice STR en una función de conversión CONVERT o [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se convierte una expresión formada por un máximo de cinco dígitos y un separador decimal en una cadena de caracteres de seis posiciones. La parte fraccionaria del número se redondea a un lugar decimal.  
  
```sql
SELECT STR(123.45, 6, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------  
 123.5  
  
(1 row(s) affected)  
```  
  
 Cuando la expresión excede la longitud especificada, la cadena devuelve `**` para esa longitud.  
  
```sql
SELECT STR(123.45, 2, 2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--  
**  
  
(1 row(s) affected)  
```  
  
 Incluso cuando se anidan datos numéricos en `STR`, el resultado son datos de tipo carácter con el formato especificado.  
  
```sql
SELECT STR (FLOOR (123.45), 8, 3);
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
 123.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
 [FORMAT (Transact-SQL)](../../t-sql/functions/format-transact-sql.md)  
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

