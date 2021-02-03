---
description: char y varchar (Transact-SQL)
title: char y varchar (Transact-SQL)
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- varchar
- varchar_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fixed-length data types [SQL Server]
- character data type
- char data type
- varchar(max) data type
- variable-length data types [SQL Server]
- varchar data type
- utf8
ms.assetid: 282cd982-f4fb-4b22-b2df-9e8478f13f6a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a54d7e0abbbb15379f83a13fc0e604b6d087326d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236387"
---
# <a name="char-and-varchar-transact-sql"></a>char y varchar (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Los tipos de datos de caracteres son de tamaño fijo, **char**, o de tamaño variable, **varchar**. A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], cuando se usa una intercalación con UTF-8 habilitado, estos tipos de datos almacenan el intervalo completo de datos de caracteres [Unicode](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn) y usan la codificación de caracteres [UTF-8](https://www.wikipedia.org/wiki/UTF-8). Si se especifica una intercalación cuyo formato no es UTF-8, estos tipos de datos solo almacenan un subconjunto de caracteres admitidos por la página de códigos correspondiente de esa intercalación.

## <a name="arguments"></a>Argumentos

**char** [ ( *n* ) ] Datos de cadena de tamaño fijo. *n* define el tamaño de la cadena en bytes y debe ser un valor entre 1 y 8000. Para juegos de caracteres de codificación de byte único, como el *latino*, el tamaño de almacenamiento es *n* bytes y el número de caracteres que se pueden almacenar también es *n*. Para los juegos de caracteres de codificación multibyte, el tamaño de almacenamiento sigue siendo *n* bytes, pero el número de caracteres que se pueden almacenar puede ser menor que *n*. El sinónimo ISO para **char** es **character**. Para más información sobre los juegos de caracteres, vea [Juegos de caracteres de un solo byte y de varios bytes](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets).

**varchar** [ ( *n* | **max** ) ] Datos de cadena de tamaño variable. Utilice *n* para definir el tamaño de la cadena en bytes, que puede ser un valor comprendido entre 1 y 8000, o bien use **max** para indicar un tamaño de restricción de columna hasta un almacenamiento máximo de 2^31-1 bytes (2 GB). Para juegos de caracteres de codificación de byte único, como el *latino*, el tamaño de almacenamiento es *n* bytes + 2 bytes y el número de caracteres que se pueden almacenar también es *n*. Para los juegos de caracteres de codificación multibyte, el tamaño de almacenamiento sigue siendo *n* bytes + 2 bytes, pero el número de caracteres que se pueden almacenar puede ser menor que *n*. Los sinónimos ISO para **varchar** son **charvarying** o **charactervarying**. Para más información sobre los juegos de caracteres, vea [Juegos de caracteres de un solo byte y de varios bytes](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets).

## <a name="remarks"></a>Observaciones

Una idea equivocada habitual es pensar que en [CHAR(*n*) y VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md), la *n* define el número de caracteres. Pero en [CHAR(*n*) y VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) la *n* define la longitud de la cadena en **bytes** (0-8.000). *n* nunca define números de caracteres que se pueden almacenar. Esto es similar a la definición de [NCHAR(*n*) y NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md).
La idea equivocada se produce porque cuando se usa la codificación de un solo byte, el tamaño de almacenamiento de CHAR y VARCHAR es de *n* bytes y el número de caracteres también es *n*. Sin embargo, para la codificación de varios bytes como [UTF-8](https://www.wikipedia.org/wiki/UTF-8), los intervalos Unicode más altos (128-1.114.111) dan como resultado un carácter que usa dos o más bytes. Por ejemplo, en una columna definida como CHAR(10), [!INCLUDE[ssde_md](../../includes/ssde_md.md)] puede almacenar 10 caracteres que usan la codificación de un solo byte (intervalo de Unicode 0-127), pero menos de 10 caracteres cuando se usa la codificación de varios bytes (intervalo de Unicode 128-1.114.111). Para obtener más información sobre el almacenamiento Unicode y los intervalos de caracteres, vea [Diferencias de almacenamiento entre UTF-8 y UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).

Cuando no se especifica *n* en una instrucción de definición de datos o de declaración de variable, la longitud predeterminada es 1. Cuando no se especifica *n* al usar las funciones CAST y CONVERT, la longitud predeterminada es 30.

Los objetos que utilizan **char** o **varchar** se asignan a la intercalación predeterminada de la base de datos, a menos que se asigne una intercalación específica por medio de la cláusula COLLATE. La intercalación controla la página de códigos utilizada para almacenar los datos de caracteres.

Las codificaciones multibyte de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluyen:

- Juegos de caracteres de doble byte (DBCS) para algunos lenguajes asiáticos orientales que usan páginas de códigos 936 y 950 (chino), 932 (japonés) o 949 (coreano).
- UTF-8 con página de códigos 65001. **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]))

Si tiene sitios que admiten varios idiomas:

- A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], considere el uso de una intercalación con UTF-8 habilitado para admitir Unicode y minimizar los problemas de conversión de caracteres.
- Si usa una versión anterior de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], considere la posibilidad de usar tipos de datos Unicode **nchar** o **nvarchar** para minimizar los problemas de conversión de caracteres.

Si usa **char** o **varchar**, se recomienda:

- Utilice **char** cuando los tamaños de las entradas de datos de columna sean coherentes.
- Use **varchar** cuando los tamaños de las entradas de datos de columna varíen considerablemente.
- Utilice **varchar(max)** cuando los tamaños de las entradas de datos de columna varíen de forma considerable y la longitud de la cadena pueda superar los 8000 bytes.

Si SET ANSI_PADDING es OFF cuando se ejecuta CREATE TABLE o ALTER TABLE, una columna de tipo **char** definida como NULL se trata como si fuera de tipo **varchar**.

> [!WARNING]
> Cada columna varchar(max) o nvarchar(max) cuyo valor no sea NULL requiere 24 bytes de asignación fija adicional que se descuentan del límite de 8060 bytes de las filas durante una operación de ordenación. Esto puede crear un límite implícito del número de columnas varchar(max) o varchar(max) cuyo valor no sea NULL que es posible crear en una tabla.
No se produce ningún error especial cuando se crea la tabla (más allá de la advertencia habitual de que el tamaño máximo de la fila supera el máximo permitido de 8060 bytes) ni en el momento de la inserción de los datos. Este tamaño grande de fila puede provocar errores (como el error 512) durante algunas operaciones normales, como la actualización de claves de un índice agrupado o las ordenaciones del conjunto completo de columnas, que solo se pueden producir cuando se realiza una operación.

## <a name="converting-character-data"></a><a name="_character"></a> Convertir datos de caracteres

Cuando se convierten expresiones de caracteres a un tipo de datos de caracteres de un tamaño distinto, se truncan los valores que son demasiado grandes para el nuevo tipo de datos. El tipo **uniqueidentifier** se considera un tipo de carácter para la conversión de una expresión de caracteres y, por tanto, está sujeto a las reglas de truncamiento de la conversión a un tipo de carácter. Vea la sección Ejemplos que aparece más adelante.

Cuando una expresión de caracteres se convierte a una expresión de caracteres de un tipo de datos o tamaño distinto (como de **char(5)** a **varchar(5)** o de **char(20)** a **char(15)** ), se asigna la intercalación del valor de entrada al valor convertido. Si una expresión que no es de carácter se convierte a un tipo de datos de carácter, se asigna al valor convertido la intercalación predeterminada de la base de datos actual. En cualquiera de los casos, puede asignar una intercalación específica mediante la cláusula [COLLATE](../../t-sql/statements/collations.md).

> [!NOTE]
> Las traducciones de páginas de códigos se admiten para los tipos de datos **char** y **varchar**, pero no para el tipo de datos **text**. Al igual que en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], las pérdidas de datos durante las conversiones de páginas de códigos no se notifican.

Las expresiones de caracteres que se van a convertir a un tipo de datos **numeric** aproximado pueden incluir una notación exponencial opcional. Esta notación es una e minúscula o una E mayúscula seguida de un signo opcional más (+) o menos (-) y un número.

Las expresiones de carácter que se convierten a un tipo de datos **numeric** exacto se componen de dígitos, un separador decimal y un signo más (+) o menos (-) opcional. Los espacios en blanco iniciales se omiten. En la cadena no se permiten los separadores de coma (como el separador de miles en algunas representaciones de 123,456.00).

Las expresiones de caracteres que se convierten a los tipos de datos **money** o **smallmoney** pueden incluir también un separador decimal opcional y un símbolo de dólar ($). Los separadores de coma (por ejemplo, $123,456.00) están permitidos.

Cuando una cadena vacía se convierte en un valor **int**, su valor se convierte en ```0```. Cuando una cadena vacía se convierte en una fecha, su valor se convierte en el [ valor predeterminado para la fecha](date-transact-sql.md), que es ```1900-01-01```.

## <a name="examples"></a>Ejemplos

### <a name="a-showing-the-default-value-of-n-when-used-in-variable-declaration"></a>A. Representación del valor predeterminado de n cuando se usa en una declaración de variable

En el ejemplo siguiente se muestra que el valor predeterminado de *n* es 1 para los tipos de datos `char` y `varchar` cuando se usan en una declaración de variable.

```sql
DECLARE @myVariable AS VARCHAR = 'abc';
DECLARE @myNextVariable AS CHAR = 'abc';
--The following returns 1
SELECT DATALENGTH(@myVariable), DATALENGTH(@myNextVariable);
GO
```

### <a name="b-showing-the-default-value-of-n-when-varchar-is-used-with-cast-and-convert"></a>B. Representación del valor predeterminado de n cuando se usa varchar con CAST y CONVERT

En el ejemplo siguiente se muestra que el valor predeterminado de *n* es 30 cuando se usa el tipo de datos `char` o `varchar` con las funciones `CAST` y `CONVERT`.

```sql
DECLARE @myVariable AS VARCHAR(40);
SET @myVariable = 'This string is longer than thirty characters';
SELECT CAST(@myVariable AS VARCHAR);
SELECT DATALENGTH(CAST(@myVariable AS VARCHAR)) AS 'VarcharDefaultLength';
SELECT CONVERT(CHAR, @myVariable);
SELECT DATALENGTH(CONVERT(CHAR, @myVariable)) AS 'VarcharDefaultLength';
```

### <a name="c-converting-data-for-display-purposes"></a>C. Convertir datos para mostrarlos

En el ejemplo siguiente se convierten dos columnas a tipos de caracteres y se aplica un estilo que aplica un formato concreto a los datos mostrados. Un tipo **money** se convierte en datos de caracteres y se aplica el estilo 1, que muestra los valores con comas cada tres dígitos a la izquierda del separador decimal y dos dígitos a la derecha del separador decimal. Un tipo **datetime** se convierte en datos de caracteres y se aplica el estilo 3, que muestra los datos en el formato dd/mm/aa. En la cláusula WHERE, un tipo **money** se convierte en un tipo de caracteres para realizar una operación de comparación de cadenas.

```sql
USE AdventureWorks2012;
GO
SELECT BusinessEntityID,
   SalesYTD,
   CONVERT (VARCHAR(12),SalesYTD,1) AS MoneyDisplayStyle1,
   GETDATE() AS CurrentDate,
   CONVERT(VARCHAR(12), GETDATE(), 3) AS DateDisplayStyle3
FROM Sales.SalesPerson
WHERE CAST(SalesYTD AS VARCHAR(20) ) LIKE '1%';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
BusinessEntityID SalesYTD              DisplayFormat CurrentDate             DisplayDateFormat
---------------- --------------------- ------------- ----------------------- -----------------
278              1453719.4653          1,453,719.47  2011-05-07 14:29:01.193 07/05/11
280              1352577.1325          1,352,577.13  2011-05-07 14:29:01.193 07/05/11
283              1573012.9383          1,573,012.94  2011-05-07 14:29:01.193 07/05/11
284              1576562.1966          1,576,562.20  2011-05-07 14:29:01.193 07/05/11
285              172524.4512           172,524.45    2011-05-07 14:29:01.193 07/05/11
286              1421810.9242          1,421,810.92  2011-05-07 14:29:01.193 07/05/11
288              1827066.7118          1,827,066.71  2011-05-07 14:29:01.193 07/05/11
```

### <a name="d-converting-uniqueidentifer-data"></a>D. Convertir datos Uniqueidentifier

En el ejemplo siguiente se convierte un valor `uniqueidentifier` a un tipo de datos `char`.

```sql
DECLARE @myid uniqueidentifier = NEWID();
SELECT CONVERT(CHAR(255), @myid) AS 'char';
```

En el ejemplo siguiente se muestra el truncamiento de los datos cuando el valor es demasiado largo para el tipo de datos al que se va a convertir. Como el tipo **uniqueidentifier** tiene un límite de 36 caracteres, se truncan los caracteres que superan esa longitud.

```sql
DECLARE @ID NVARCHAR(max) = N'0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong';
SELECT @ID, CONVERT(uniqueidentifier, @ID) AS TruncatedValue;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
String                                       TruncatedValue
-------------------------------------------- ------------------------------------
0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong    0E984725-C51C-4BF4-9960-E1C80E27ABA0

(1 row(s) affected)
```

## <a name="see-also"></a>Consulte también

[nchar y nvarchar &#40;Transact-SQL&#41;](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)

[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)

[COLLATE &#40;Transact-SQL&#41;](../../t-sql/statements/collations.md)

[Conversiones de tipos de datos &#40;motor de base de datos&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)

[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)

[Estimar el tamaño de una base de datos](../../relational-databases/databases/estimate-the-size-of-a-database.md)

[Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)

[Juegos de caracteres de un solo byte y de varios bytes](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)
