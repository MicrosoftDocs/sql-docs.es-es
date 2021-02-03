---
description: Conversión de tipos de datos (motor de base de datos)
title: Conversión de tipos de datos (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- converting data types [SQL Server]
- CONVERT function
- data types [SQL Server], converting
- implicit data type conversions
- explicit data type conversions [SQL Server]
- converting data types [SQL Server], about converting data types
ms.assetid: ffacf45e-a488-48d0-9bb0-dcc7fd365299
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d88c7b479f6a4d9cbd7bca4091690c79b3ba564
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194994"
---
# <a name="data-type-conversion-database-engine"></a>Conversión de tipos de datos (motor de base de datos)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Los tipos de datos se pueden convertir en los siguientes casos:
-   Cuando los datos de un objeto se comparan o se combinan con los datos de otro objeto, o bien se mueven a estos, puede que sea necesario convertir los datos desde el tipo de datos de un objeto al tipo de datos del otro.  
-   Cuando los datos de una columna de resultados, un código de retorno o un parámetro de salida de [!INCLUDE[tsql](../../includes/tsql-md.md)] se mueven a una variable de programa, se deben convertir del tipo de datos del sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al tipo de datos de la variable.  
  
Cuando se realiza una conversión entre una variable de aplicación y una columna de conjunto de resultados, código de retorno, parámetro o marcador de parámetro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la API de base de datos define cuáles son las conversiones de tipos de datos admitidas.
  
## <a name="implicit-and-explicit-conversion"></a>Conversiones implícitas y explícitas
Los tipos de datos se pueden convertir de forma implícita o explícita.
  
Las conversiones implícitas no son visibles para el usuario. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convierte automáticamente los datos de un tipo de datos al otro. Por ejemplo, cuando se comparan **smallint** con **int**, antes de realizar la comparación, **smallint** se convierte implícitamente al tipo **int**.
  
**GETDATE()** se convierte implícitamente al estilo de fecha 0. **SYSDATETIME()** se convierte implícitamente al estilo de fecha 21.
  
Las conversiones explícitas utilizan las funciones CAST o CONVERT.
  
Las funciones [CAST y CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) convierten un valor (una variable local, una columna u otra expresión) de un tipo de datos a otro. Por ejemplo, la siguiente función `CAST` convierte el valor numérico `$157.27` a una cadena de caracteres `'157.27'`:
  
```sql
CAST ( $157.27 AS VARCHAR(10) )  
```  
  
Utilice CAST en lugar de CONVERT si desea que el código de programa de [!INCLUDE[tsql](../../includes/tsql-md.md)] cumpla las normas ISO. Use CONVERT en lugar de CAST para aprovechar la funcionalidad de estilo de CONVERT.
  
En la ilustración siguiente se muestran todas las conversiones de tipos de datos explícitas e implícitas permitidas para los tipos de datos proporcionados por el sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Algunas de ellas son **xml**, **bigint** y **sql_variant**. No existe una conversión implícita en la asignación del tipo de datos **sql_variant**, pero sí hay una conversión implícita en **sql_variant**.
  
![Tabla de conversión de tipo de datos](../../t-sql/data-types/media/lrdatahd.png "Tabla de conversión de tipo de datos")

Si bien en el gráfico anterior se muestran todas las conversiones explícitas e implícitas permitidas en SQL Server, no se indica el tipo de datos resultante de la conversión. Cuando SQL Server realiza una conversión explícita, la instrucción misma determina el tipo de datos resultante. En las conversiones implícitas, las instrucciones de asignación, como establecer el valor de una variable o insertar un valor en una columna, general el tipo de datos definido por la declaración de la variable o la definición de la columna. En el caso de los operadores de comparación u otras expresiones, el tipo de datos resultante depende de las reglas de prioridad de los tipos de datos.

Como ejemplo, el siguiente script define una variable de tipo `varchar`, asigna un valor de tipo `int` a la variable y, luego, selecciona una concatenación de la variable con una cadena.

```sql
DECLARE @string VARCHAR(10);
SET @string = 1;
SELECT @string + ' is a string.'
```

El valor `int` de `1` se convierte en `varchar`, por lo que la instrucción `SELECT` devuelve el valor `1 is a string.`.

En el ejemplo siguiente, se muestra un script similar con una variable `int` en su lugar:

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + ' is not a string.'
```

En este caso, la instrucción `SELECT` produce el siguiente error:

`Msg 245, Level 16, State 1, Line 3`
`Conversion failed when converting the varchar value ' is not a string.' to data type int.`

Para evaluar la expresión `@notastring + ' is not a string.'`, SQL Server sigue las reglas de prioridad del tipo de datos para completar la conversión implícita antes de que se pueda calcular el resultado de la expresión. Dado que `int` tiene una prioridad más alta que `varchar`, SQL Server intenta convertir la cadena en un entero y produce un error porque esta cadena no se puede convertir en un entero. Si la expresión proporciona una cadena que se puede convertir, la instrucción se ejecuta correctamente, como en el ejemplo siguiente:

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + '1'
```

En este caso, la cadena `1` se puede convertir al valor entero `1`, por lo que esta instrucción `SELECT` devuelve el valor `2`. Tenga en cuenta que el operador `+` se agrega en lugar de concatenar cuando los tipos de datos proporcionados son enteros.

## <a name="data-type-conversion-behaviors"></a>Comportamientos de la conversión de tipos de datos

Algunas conversiones implícitas y explícitas de tipos de datos no se admiten cuando convierte el tipo de datos de un objeto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a otro. Por ejemplo, un valor **nchar** no se puede convertir a un valor **image**. Un valor **nchar** solo se puede convertir a **binary** con una conversión explícita; la conversión implícita a **binary** no se admite. Sin embargo, un valor **nchar** se puede convertir implícita o explícitamente a **nvarchar**.
  
En los temas siguientes se describen los comportamientos de conversión que presentan los tipos de datos correspondientes:
  
 - [binary y varbinary &#40;Transact-SQL&#41;](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)  
 - [datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)  
 - [money y smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)  
 - [bit &#40;Transact-SQL&#41;](../../t-sql/data-types/bit-transact-sql.md)  
 - [datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
 - [smalldatetime &#40;Transact-SQL&#41;](../../t-sql/data-types/smalldatetime-transact-sql.md)  
 - [char y varchar &#40;Transact-SQL&#41;](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
 - [decimal y numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)  
 - [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)  
 - [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md)  
 - [float y real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)  
 - [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md)  
 - [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)  
 - [int, bigint, smallint y tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
 - [uniqueidentifier &#40;Transact-SQL&#41;](../../t-sql/data-types/uniqueidentifier-transact-sql.md)  
  
###  <a name="converting-data-types-by-using-ole-automation-stored-procedures"></a>Convertir tipos de datos con procedimientos almacenados de OLE Automation  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza tipos de datos [!INCLUDE[tsql](../../includes/tsql-md.md)] y OLE Automation utiliza tipos de datos [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]; por tanto, los procedimientos almacenados de OLE Automation deben convertir los datos que se pasan entre ellos.
  
En la tabla siguiente se describen las conversiones de tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)].
  
|Tipos de datos de SQL Server|Tipo de datos en Visual Basic|  
|--------------------------|----------------------------|  
|**char**, **varchar**, **text**, **nvarchar**, **ntext**|**String**|  
|**decimal**, **numeric**|**String**|  
|**bit**|**Boolean**|  
|**binary**, **varbinary**, **image**|Matriz **Byte()** unidimensional|  
|**int**|**Long**|  
|**smallint**|**Entero**|  
|**tinyint**|**Byte**|  
|**float**|**Double**|  
|**real**|**Single**|  
|**money**, **smallmoney**|**Moneda**|  
|**datetime**, **smalldatetime**|**Fecha**|  
|Cualquiera establecido en NULL|**Variant** establecido en NULL|  
  
Los valores únicos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se convierten a un valor único de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], con la excepción de los valores **binary**, **varbinary** e **image**. Estos valores se convierten a una matriz **Byte()** unidimensional en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Esta matriz tiene un intervalo de **Byte(** 0 to _length_ 1 **)** donde *length* es el número de bytes en los valores [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **binary**, **varbinary** o **image**.
  
A continuación se indican las conversiones de tipos de datos de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] a tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
|Tipo de datos en Visual Basic|Tipos de datos de SQL Server|  
|----------------------------|--------------------------|  
|**Long**, **Integer**, **Byte**, **Boolean**, **Object**|**int**|  
|**Double**, **Single**|**float**|  
|**Moneda**|**money**|  
|**Fecha**|**datetime**|  
|**String** con 4000 caracteres o menos|**varchar**/**nvarchar**|  
|**String** con más de 4000 caracteres|**text**/**ntext**|  
|Matriz **Byte()** unidimensional con 8000 bytes o menos|**varbinary**|  
|Matriz **Byte()** unidimensional con más de 8000 bytes|**image**|  
  
## <a name="see-also"></a>Vea también
[Procedimientos almacenados de automatización OLE &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[COLLATE &#40;Transact-SQL&#41;](../statements/collations.md)
  
