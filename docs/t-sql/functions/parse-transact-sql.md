---
description: PARSE (Transact-SQL)
title: PARSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PARSE
- PARSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PARSE function
ms.assetid: 6a2dbf10-f692-471b-9458-24d246963049
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: daa87ffc035733c415d1c5a4c66bfc7a4e1278f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99100686"
---
# <a name="parse-transact-sql"></a>PARSE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el resultado de una expresión, traducido al tipo de datos solicitado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
PARSE ( string_value AS data_type [ USING culture ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *string_value*  
 Valor **nvarchar**(4000) que representa el valor con formato que se va a analizar en el tipo de datos especificado.  
  
 *string_value* debe ser una representación válida del tipo de datos solicitado; de lo contrario, PARSE produce un error.  
  
 *data_type*  
 Valor literal que representa el tipo de datos solicitado para el resultado.  
  
 *culture*  
 Cadena opcional que identifica la referencia cultural en la que se da formato a *string_value*.  
  
 Si no se proporciona el argumento *culture*, se usará el idioma de la sesión actual. Este idioma se establece implícitamente, o explícitamente mediante la instrucción SET LANGUAGE. *culture* acepta cualquier referencia cultural compatible con .NET Framework; no se limita a los idiomas admitidos explícitamente por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el argumento *culture* no es válido, PARSE desencadena un error.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve el resultado de la expresión, traducido al tipo de datos solicitado.  
  
## <a name="remarks"></a>Comentarios  
 Los valores NULL que se pasan como argumentos a PARSE se tratan de dos maneras:  
  
1.  Si se pasa una constante NULL, se produce un error. Un valor NULL no se puede analizar en otro tipo de datos diferente teniendo en cuenta la referencia cultural.  
  
2.  Si se pasa un parámetro con un valor NULL en tiempo de ejecución, se devuelve un valor NULL para evitar que se cancele todo el lote.  
  
 Use PARSE solo para convertir de tipos de cadena a tipos de fecha y hora y de número. Para las conversiones de tipos generales, siga usando CAST o CONVERT. Tenga en cuenta que hay cierta sobrecarga de rendimiento al analizar el valor de cadena.  
  
 PARSE se basa en la presencia de Common Language Runtime (CLR) de .NET Framework.  
  
 Esta función no se enviará de forma remota puesto que depende de la presencia del CLR. El envío remoto de una función que necesita el CLR produciría un error en el servidor remoto.  
  
 **Más información sobre el parámetro data_type**  
  
 Los valores del parámetro *data_type* están restringidos a los tipos que aparecen en la tabla siguiente, junto con estilos. La información de estilo se proporciona como ayuda para determinar los tipos de modelos permitidos. Para más información sobre los estilos, vea la documentación de .NET Framework para las enumeraciones **System.Globalization.NumberStyles** y **DateTimeStyles** .  
  
|Category|Tipo|Tipo de .NET Framework|Estilos usados|  
|--------------|----------|-------------------------|-----------------|  
|Numeric|bigint|Int64|NumberStyles.Number|  
|Numeric|int|Int32|NumberStyles.Number|  
|Numeric|SMALLINT|Int16|NumberStyles.Number|  
|Numeric|TINYINT|Byte|NumberStyles.Number|  
|Numeric|Decimal|Decimal|NumberStyles.Number|  
|Numeric|NUMERIC|Decimal|NumberStyles.Number|  
|Numeric|FLOAT|Double|NumberStyles.Float|  
|Numeric|real|Single|NumberStyles.Float|  
|Numeric|SMALLMONEY|Decimal|NumberStyles.Currency|  
|Numeric|money|Decimal|NumberStyles.Currency|  
|Fecha y hora|date|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Fecha y hora|time|TimeSpan|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Fecha y hora|datetime|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Fecha y hora|smalldatetime|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Fecha y hora|datetime2|DateTime|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
|Fecha y hora|datetimeoffset|DateTimeOffset|DateTimeStyles.AllowWhiteSpaces &#124; DateTimeStyles.AssumeUniversal|  
  
 **Más información sobre el parámetro culture**  
  
 En la tabla siguiente se muestran las asignaciones de los idiomas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a las referencias culturales de .NET Framework.  
  
|Nombre completo|Alias|LCID|Referencia cultural específica|  
|---------------|-----------|----------|----------------------|  
|us_english|Inglés|3082|es-ES|  
|Deutsch|Alemán|1031|de-DE|  
|Français|Francés|1036|fr-FR|  
|日本語|Japonés|1041|ja-JP|  
|Dansk|Danés|1030|da-DK|  
|Español|Español|3082|es-ES|  
|Italiano|Italiano|1040|it-IT|  
|Nederlands|Neerlandés|1043|nl-NL|  
|Norsk|Noruego|2068|nn-NO|  
|Português|Portugués|2070|pt-PT|  
|Suomi|Finés|1035|fi-FI|  
|Svenska|Sueco|1053|sv-SE|  
|čeština|Checo|1029|Cs-CZ|  
|magyar|Húngaro|1038|Hu-HU|  
|polski|Polaco|1045|Pl-PL|  
|română|Rumano|1048|Ro-RO|  
|hrvatski|Croata|1050|hr-HR|  
|slovenčina|Eslovaco|1051|Sk-SK|  
|slovenski|Esloveno|1060|Sl-SI|  
|ελληνικά|Griego|1032|El-GR|  
|български|Búlgaro|1026|bg-BG|  
|русский|Ruso|1049|Ru-RU|  
|Türkçe|Turco|1055|Tr-TR|  
|British|British English|2057|en-GB|  
|eesti|Estonio|1061|Et-EE|  
|latviešu|Letón|1062|lv-LV|  
|lietuvių|Lituano|1063|lt-LT|  
|Português (Brasil)|Portugués (Brasil)|1046|pt-BR|  
|繁體中文|Chino tradicional|1028|zh-TW|  
|한국어|Coreano|1042|Ko-KR|  
|简体中文|Chino simplificado|2052|zh-CN|  
|Árabe|Árabe|1025|ar-SA|  
|ไทย|Tailandés|1054|Th-TH|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-parse-into-datetime2"></a>A. PARSE en datetime2  
  
```sql  
SELECT PARSE('Monday, 13 December 2010' AS datetime2 USING 'en-US') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
2010-12-13 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-parse-with-currency-symbol"></a>B. PARSE con símbolo de moneda  
  
```sql  
SELECT PARSE('€345,98' AS money USING 'de-DE') AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
345.98  
  
(1 row(s) affected)  
```  
  
### <a name="c-parse-with-implicit-setting-of-language"></a>C. PARSE con configuración implícita de idioma  
  
```sql  
-- The English language is mapped to en-US specific culture  
SET LANGUAGE 'English';  
SELECT PARSE('12/16/2010' AS datetime2) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
---------------  
2010-12-16 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
  
