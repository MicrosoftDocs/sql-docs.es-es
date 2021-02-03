---
description: Constantes (Transact-SQL)
title: Constantes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- uniqueidentifier data type
- bit data type
- constants [SQL Server]
- scalar values
- money data type, constants
- binary [SQL Server], constants
- float data type
- Unicode [SQL Server], constants
- constants [Transact-SQL]
- integer constants
- decimal data type, constants
- character strings [SQL Server], constants
- positive values [SQL Server]
- formats [SQL Server], constants
- datetime data type [SQL Server]
- literals [SQL Server], constants
- real data type
- negative values
ms.assetid: 58ae3ff3-b1d5-41b2-9a2f-fc7ab8c83e0e
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 62c93d062c5903deb2e3dd291ba26953bcfb00be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172595"
---
# <a name="constants-transact-sql"></a>Constantes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Una constante, también conocida como valor literal o escalar, es un símbolo que representa un valor de datos específico. El formato de las constantes depende del tipo de datos del valor que representan.
  
## <a name="character-string-constants"></a>Constantes de cadena de caracteres
Las constantes de cadena de caracteres van entre comillas simples e incluyen caracteres alfanuméricos (a-z, A-Z y 0-9) y caracteres especiales, como el signo de exclamación (!), la arroba (@) y el signo de número (#). La intercalación predeterminada de la base de datos actual se asigna a las constantes de la cadena de caracteres. Si se utiliza la cláusula COLLATE, la conversión a la página de códigos predeterminada de la base de datos sigue teniendo lugar antes de la conversión a la intercalación especificada por la cláusula COLLATE. Las cadenas de caracteres escritas por los usuarios se evalúan a través de la página de códigos del equipo y, si es necesario, se traducen a la página de códigos predeterminada de la base de datos.

> [!NOTE]
> Cuando se especifica una [intercalación habilitada para UTF8](../../relational-databases/collations/collation-and-unicode-support.md#utf8) mediante la cláusula COLLATE, la conversión a la página de códigos predeterminada de la base de datos sigue teniendo lugar antes de la conversión a la intercalación especificada por la cláusula COLLATE. La conversión no se realiza directamente en la intercalación habilitada para Unicode especificada. Para obtener más información, vea [Cadena de Unicode](#unicode-strings).
  
Si la opción QUOTED_IDENTIFIER se ha establecido en OFF para una conexión, las cadenas de caracteres también pueden encerrarse entre comillas dobles, pero [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) y [ODBC Driver for SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md) usan automáticamente `SET QUOTED_IDENTIFIER ON`. Se recomienda el uso de comillas simples.
  
Si una cadena de caracteres entre comillas simples contiene una comilla, represente la comilla interna con dos comillas simples. Esto no es necesario en las cadenas incluidas entre comillas dobles.
  
Éstos son algunos ejemplos de cadenas de caracteres:
  
```
'Cincinnati'  
'O''Brien'  
'Process X is 50% complete.'  
'The level for job_id: %d should be between %d and %d.'  
"O'Brien"  
```  
  
Las cadenas vacías se representan como dos comillas simples sin nada entre ellas. En el modo de compatibilidad 6.x, una cadena vacía se trata como un espacio.
  
Las constantes de cadena de caracteres admiten [intercalaciones](../../relational-databases/collations/collation-and-unicode-support.md) mejoradas.
  
> [!NOTE]  
> Las constantes de caracteres con más de 8000 bytes se consideran como tipos de datos **varchar(max)**.  
  
## <a name="unicode-strings"></a>Cadenas Unicode
Las cadenas Unicode tienen un formato similar al de las cadenas de caracteres, pero están precedidas por el identificador N (N es el idioma nacional en el estándar SQL-92). 

> [!IMPORTANT]  
> El prefijo N tiene que estar en mayúsculas. 

Por ejemplo, `'Michél'` es una constante de caracteres, mientras que `N'Michél'` es una constante Unicode. Las constantes Unicode se interpretan como datos Unicode y no se evalúan mediante una página de códigos. Las constantes Unicode tienen intercalación. Esta intercalación controla principalmente las comparaciones y la distinción entre mayúsculas y minúsculas. La intercalación predeterminada de la base de datos actual se asigna a las constantes de Unicode. Si se utiliza la cláusula COLLATE, la conversión a la intercalación predeterminada de la base de datos sigue teniendo lugar antes de la conversión a la intercalación especificada por la cláusula COLLATE. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).
  
Las constantes de cadena Unicode aceptan intercalaciones mejoradas.
  
> [!NOTE]  
> Las constantes Unicode con más de 8000 bytes se consideran como tipos de datos **nvarchar(max)**.  
  
## <a name="binary-constants"></a>Constantes binarias
Las constantes binarias tienen el prefijo `0x` y son cadenas de números hexadecimales. No se incluyen entre comillas.
  
Éstos son algunos ejemplos de cadenas binarias:
  
```
0xAE  
0x12Ef  
0x69048AEFDD010E  
0x  (empty binary string)  
```  
  
> [!NOTE]  
>  Las constantes binarias con más de 8000 bytes se consideran como tipos de datos **varbinary(max)**.  
  
## <a name="bit-constants"></a>Constantes de tipo bit
Las constantes de tipo **bit** se representan con los números 0 o 1, y no se incluyen entre comillas. Si se utiliza un número mayor que uno, se convierte en uno.
  
## <a name="datetime-constants"></a>Constantes de tipo datetime
Las constantes de tipo **datetime** se representan mediante valores de fecha en formatos específicos de cadenas de caracteres, y se incluyen entre comillas simples.
  
Estos son algunos ejemplos de constantes **datetime**:
  
```
'December 5, 1985'  
'5 December, 1985'  
'851205'  
'12/5/98'  
```  
  
Ejemplos de constantes datetime:
  
```
'14:30:24'  
'04:24 PM'  
```  
  
## <a name="integer-constants"></a>constantes de enteros
Las constantes de tipo **integer** se representan mediante una cadena de números sin comillas y sin separadores decimales. Las constantes de tipo **integer** deben ser números enteros y no pueden contener decimales.
  
Estos son algunos ejemplos de constantes **integer**:
  
```
1894  
2  
```  
  
## <a name="decimal-constants"></a>Constantes de tipo decimal
Las constantes de tipo **decimal** se representan mediante una cadena de números sin comillas y con separador decimal.
  
Estos son algunos ejemplos de constantes **decimal**:
  
```
1894.1204  
2.0  
```  
  
## <a name="float-and-real-constants"></a>Constantes de tipo float y real
Las constantes de tipo **float** y **real** se representan en notación científica.
  
Estos son algunos ejemplos de valores de **float** o **real**:
  
```
101.5E5  
0.5E-2  
```  
  
## <a name="money-constants"></a>Constantes de tipo money
Las constantes de tipo **money** se representan como una cadena de números con un separador decimal y un símbolo de moneda opcionales como prefijo. Las constantes de tipo **money** no se incluyen entre comillas.
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no impone ninguna clase de reglas de agrupamiento, como la inserción de una coma (,) cada tres dígitos, en las cadenas que representan dinero.
  
> [!NOTE]  
>  Las comas no se tienen en cuenta en ningún lugar del valor literal **money** especificado.  
  
Estos son algunos ejemplos de constantes **money**:
  
```
$12  
$542023.14  
```  
  
## <a name="uniqueidentifier-constants"></a>Constantes de tipo uniqueidentifier
Las constantes de tipo **uniqueidentifier** son una cadena que representa un GUID. Se pueden especificar en formato de cadena de caracteres o binario.
  
Estos dos ejemplos especifican el mismo GUID:
  
```
'6F9619FF-8B86-D011-B42D-00C04FC964FF'  
0xff19966f868b11d0b42d00c04fc964ff  
```  
  
## <a name="specifying-negative-and-positive-numbers"></a>Especificar números negativos y positivos  
Para indicar si un número es positivo o negativo, aplique los operadores unarios **+** o **-** a una constante numérica. Esto crea una expresión numérica que representa el valor numérico con signo. Las constantes numéricas usan positivos cuando no se aplican los operadores unarios **+** o **-** .
  
Expresiones **integer** con signo:  
  
```
+145345234
-2147483648
```
Expresiones **decimal** con signo:  
  
```
+145345234.2234
-2147483648.10
```
  
Expresiones **float** con signo:  
  
```
+123E-3
-12E5
```
  
Expresiones **money** con signo:  
  
```
-$45.56
+$423456.99
```
  
## <a name="enhanced-collations"></a>Intercalaciones mejoradas  
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] admite las constantes de cadena de caracteres y Unicode compatibles con las intercalaciones mejoradas. Para más información, vea la cláusula [COLLATE &#40;Transact-SQL&#41;](../../t-sql/statements/collations.md).
  
## <a name="see-also"></a>Consulte también
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)
[Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)  
[Prioridad de intercalación](../../t-sql/statements/collation-precedence-transact-sql.md)    
  
