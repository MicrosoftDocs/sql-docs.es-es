---
description: nchar y nvarchar (Transact-SQL)
title: nchar y nvarchar (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- nvarchar data type
- nchar data type
ms.assetid: 81ee5637-ee31-4c4d-96d0-56c26a742354
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 038cb4e80af0423a5581ef2b7b51c1bbc54bb5cc
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237625"
---
# <a name="nchar-and-nvarchar-transact-sql"></a>nchar y nvarchar (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Los tipos de datos de caracteres son de tamaño fijo, **nchar**, o de tamaño variable, **nvarchar**. A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], cuando se usa una intercalación con [carácter complementario (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) habilitado, estos tipos de datos almacenan el intervalo completo de datos de caracteres [Unicode](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn) y usan la codificación de caracteres [UTF-16](https://www.wikipedia.org/wiki/UTF-16). Si se especifica una intercalación que no es de tipo SC, estos tipos de datos almacenan solo el subconjunto de datos de caracteres admitidos por la codificación de caracteres [UCS-2](https://www.wikipedia.org/wiki/Universal_Coded_Character_Set#Encoding_forms).

## <a name="arguments"></a>Argumentos
**nchar** [ ( n ) ]  
Datos de cadena de tamaño fijo. *n* define el tamaño de la cadena en pares de bytes y debe ser un valor entre 1 y 4000. El tamaño de almacenamiento es dos veces *n* bytes. Para la codificación [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF), el tamaño de almacenamiento es el doble de *n* bytes y el número de caracteres que se pueden almacenar también en *n*. Para la codificación UTF-16, el tamaño de almacenamiento sigue siendo el doble de *n* bytes, pero el número de caracteres que se pueden almacenar puede ser menor que *n* porque los caracteres complementarios usan dos pares de bytes (también denominados [par suplente](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)). Los sinónimos ISO de **nchar** son **national char** y **national character**.
  
**nvarchar** [ ( n | **max** ) ]  
Datos de cadena de tamaño variable. *n* define el tamaño de la cadena en pares de bytes y puede ser un valor entre 1 y 4000. **max** indica que el tamaño máximo de almacenamiento es de 2^30-1 caracteres (2 GB). El tamaño de almacenamiento es el doble de *n* bytes + 2 bytes. Para la codificación [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF), el tamaño de almacenamiento es el doble de *n* bytes + 2 bytes y el número de caracteres que se pueden almacenar también en *n*. Para la codificación UTF-16, el tamaño de almacenamiento sigue siendo el doble de *n* bytes + 2 bytes, pero el número de caracteres que se pueden almacenar puede ser menor que *n* porque los caracteres complementarios usan dos pares de bytes (también denominados [par suplente](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)). Los sinónimos ISO de **nvarchar** son **national char varying** y **national character varying**.
  
## <a name="remarks"></a>Comentarios  
Una idea equivocada habitual es pensar que en [NCHAR(*n*) y NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), la *n* define el número de caracteres. Pero en [NCHAR(*n*) y NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) la *n* define la longitud de la cadena en **pares de bytes** (0-4000). *n* nunca define números de caracteres que se pueden almacenar. Esto es similar a la definición de [CHAR(*n*) y VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md).   
La idea equivocada se produce porque cuando se usan caracteres definidos en el intervalo de Unicode 0-65.535, se puede almacenar un carácter por cada par de bytes. Sin embargo, en intervalos de Unicode más altos (65.536-1.114.111), un carácter puede usar dos pares de bytes. Por ejemplo, en una columna definida como NCHAR(10), [!INCLUDE[ssde_md](../../includes/ssde_md.md)] puede almacenar 10 caracteres que usan la codificación de un par de bytes (intervalo de Unicode 0-65,535), pero menos de 10 caracteres cuando se usan dos pares de bytes (intervalo de Unicode 65.536-1.114.111). Para obtener más información sobre el almacenamiento Unicode y los intervalos de caracteres, vea [Diferencias de almacenamiento entre UTF-8 y UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).     

Cuando no se especifica el argumento *n* en una instrucción de definición de datos o de declaración de variable, la longitud predeterminada es 1. Cuando no se especifica *n* con la función CAST, la longitud predeterminada es 30.

Si usa **nchar** o **nvarchar**, se recomienda:
- Utilice **nchar** cuando los tamaños de las entradas de datos de columna sean coherentes.  
- Use **nvarchar** cuando los tamaños de las entradas de datos de columna varíen considerablemente.  
- Utilice **nvarchar(max)** cuando los tamaños de las entradas de datos de columna varíen de forma considerable y la longitud de la cadena pueda superar los 4000 pares de bytes.  
  
**sysname** es un tipo de datos definido por el usuario y suministrado por el sistema que es funcionalmente equivalente a **nvarchar(128)**, excepto que no acepta valores NULL. **sysname** se usa para hacer referencia a nombres de objetos de base de datos.
  
Los objetos que usan **nchar** o **nvarchar** se asignan a la intercalación predeterminada de la base de datos, a menos que se asigne una intercalación específica por medio de la cláusula COLLATE.
  
SET ANSI_PADDING siempre es ON para **nchar** y **nvarchar**. SET ANSI_PADDING OFF no se aplica a los tipos de datos **nchar** ni **nvarchar**.
  
Prefijo de una constante de cadena de caracteres Unicode con la letra N para señalar la entrada UCS-2 o UTF-16, dependiendo de si se utiliza una intercalación SC o no. Sin el prefijo N, la cadena se convierte a la página de códigos predeterminada de la base de datos que puede no reconocer determinados caracteres. A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], cuando se utiliza una intercalación con UTF-8 habilitado, la página de códigos predeterminada es capaz de almacenar el juego de caracteres UNICODE UTF-8. 
 
> [!NOTE]  
> Cuando se antepone un prefijo con la letra N en una constante de cadena, la conversión implícita dará como resultado una cadena UCS-2 o UTF-16 si la constante que se convierte no supera la longitud máxima de un tipo de datos de cadena nvarchar (4000). En caso contrario, la conversión implícita generará un valor grande nvarchar(max).
  
> [!WARNING]  
> Cada columna **varchar(max)** o **nvarchar(max)** cuyo valor no sea NULL requiere 24 bytes de asignación fija adicional, que se descuentan del límite de 8060 bytes de las filas durante una operación de ordenación. Estos bytes adicionales pueden crear un límite implícito del número de columnas **varchar(max)** o **nvarchar(max)** cuyo valor no sea NULL en una tabla. No se produce ningún error especial cuando se crea la tabla (más allá de la advertencia habitual de que el tamaño máximo de la fila supera el máximo permitido de 8060 bytes) ni en el momento de la inserción de los datos. Este gran tamaño de fila puede provocar errores (como el error 512) que los usuarios no puedan prever durante algunas operaciones habituales.  Dos ejemplos de operaciones son las actualizaciones de la clave de índice agrupado o las ordenaciones del conjunto de columnas completo.
  
## <a name="converting-character-data"></a>Convertir datos de caracteres  
Para más información sobre cómo convertir datos de caracteres, vea [char y varchar &#40;Transact-SQL&#41;](../../t-sql/data-types/char-and-varchar-transact-sql.md).
  
## <a name="see-also"></a>Consulte también
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[COLLATE &#40;Transact-SQL&#41;](../statements/collations.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)    
[Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md)     
[Juegos de caracteres de un solo byte y de varios bytes](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  
