---
description: CONCAT (Transact-SQL)
title: CONCAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONCAT
- CONCAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT function
ms.assetid: fce5a8d4-283b-4c47-95e5-4946402550d5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 68589c27d999bdddc0998773f80dcf17c4dcfec8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188777"
---
# <a name="concat-transact-sql"></a>CONCAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve una cadena resultante de la concatenación, o la combinación, de dos o más valores de cadena de una manera integral. (Para agregar un valor de separación durante la concatenación, vea [CONCAT_WS](../../t-sql/functions/concat-ws-transact-sql.md)).
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CONCAT ( string_value1, string_value2 [, string_valueN ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*string_value*  
Valor de cadena que se va a concatenar con los demás valores. La función `CONCAT` requiere al menos dos argumentos *valor_cadena* y no más de 254 argumentos *valor_cadena*.
  
## <a name="return-types"></a>Tipos de valores devueltos  
*string_value*  
Un valor de cadena cuya longitud y tipo dependen de la entrada.
  
## <a name="remarks"></a>Observaciones  
`CONCAT` toma un número variable de argumentos de cadena y los concatena (o combina) en una sola cadena. Necesita un mínimo de dos valores de entrada; de lo contrario, se produce un error en `CONCAT`. `CONCAT` convierte implícitamente todos los argumentos en tipos de cadena antes de la concatenación. `CONCAT` convierte implícitamente los valores NULL en cadenas vacías. Si `CONCAT` recibe argumentos en los que todos los valores son **NULL**, devolverá una cadena vacía de tipo **varchar**(1). La conversión implícita de cadenas sigue las reglas existentes para las conversiones de tipos de datos. Vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md) para obtener más información sobre las conversiones de tipo de datos.
  
El tipo devuelto depende del tipo de los argumentos. En esta tabla se muestra la asignación:
  
|Tipo de entrada|Tipo de salida y longitud de datos|  
|---|---|
|1. Cualquier argumento de<br><br />un tipo de sistema de CLR de SQL<br><br />un UDT de CLR de SQL<br><br />o bien<br><br />`nvarchar(max)`|**nvarchar(max)**|  
|2. De lo contrario, cualquier argumento de tipo<br><br />**varbinary(max)**<br><br />o bien<br><br />**ntext**|**varchar(max)**, a menos que uno de los parámetros sea un tipo **nvarchar** de cualquier longitud. En este caso, `CONCAT` devuelve un resultado de tipo **nvarchar (max)**.|  
|3. De lo contrario, cualquier argumento de tipo **nvarchar** de 4000 caracteres como máximo<br><br />( **nvarchar**(<= 4000) )|**nvarchar**(<= 4000)|  
|4. En todos los demás casos|**varchar**(<= 8000) (un tipo **varchar** de 8 000 caracteres como máximo) a menos que uno de los parámetros sea un tipo nvarchar de cualquier longitud. En ese caso, `CONCAT` devuelve un resultado de tipo **nvarchar (max)**.|  
  
Cuando `CONCAT` recibe argumentos de entrada **nvarchar** de longitud <= 4 000 caracteres, o bien argumentos de entrada **varchar** de longitud <= 8 000 caracteres, las conversiones implícitas pueden afectar a la longitud del resultado. Otros tipos de datos tienen otras longitudes cuando se convierten implícitamente a cadenas. Por ejemplo, un tipo **int** (14) tiene una longitud de cadena de 12, mientras que un tipo **float** tiene una longitud de 32. Por tanto, una concatenación de dos enteros devuelve un resultado con una longitud de no menos de 24.
  
Si ninguno de los argumentos de entrada tiene un tipo de objeto grande (LOB) admitido, el tipo devuelto se trunca a una longitud de 8 000 caracteres, independientemente del tipo de valor devuelto. Este truncamiento ahorra espacio y admite la eficacia de la generación de planes.
  
La función CONCAT se puede ejecutar de forma remota en un servidor vinculado de la versión [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o versiones posteriores. Para servidores vinculados anteriores, la operación de CONCAT se realizará localmente, después de que el servidor vinculado devuelva los valores no concatenados.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-concat"></a>A. Usar CONCAT  
  
```sql
SELECT CONCAT ( 'Happy ', 'Birthday ', 11, '/', '25' ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
-------------------------  
Happy Birthday 11/25  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-concat-with-null-values"></a>B. Usar CONCAT con valores NULL  
  
```sql
CREATE TABLE #temp (  
    emp_name NVARCHAR(200) NOT NULL,  
    emp_middlename NVARCHAR(200) NULL,  
    emp_lastname NVARCHAR(200) NOT NULL  
);  
INSERT INTO #temp VALUES( 'Name', NULL, 'Lastname' );  
SELECT CONCAT( emp_name, emp_middlename, emp_lastname ) AS Result  
FROM #temp;  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
------------------  
NameLastname  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vea también
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)   
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  


