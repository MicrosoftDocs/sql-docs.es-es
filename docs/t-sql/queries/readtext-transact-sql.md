---
description: READTEXT (Transact-SQL)
title: READTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- READTEXT_TSQL
- READTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- column reading [SQL Server]
- READTEXT statement
- reading columns
ms.assetid: 91b69853-1381-4306-8343-afdb73105738
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e312bff216746345fc028b1ff581bdee56a12eca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207615"
---
# <a name="readtext-transact-sql"></a>READTEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Lee los valores **text**, **ntext** o **image** de una columna **text**, **ntext** o **image**. Comienza a leer desde un desplazamiento especificado el número de bytes definido.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use la función [SUBSTRING](../../t-sql/functions/substring-transact-sql.md) en su lugar.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
READTEXT { table.column text_ptr offset size } [ HOLDLOCK ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
_table_ **.** _column_  
Es el nombre de la tabla y de la columna donde se va a leer. Los nombres de tablas y columnas deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). Es necesario especificar los nombres de la tabla y de la columna; sin embargo, es opcional especificar el nombre de la base de datos y del propietario.  
  
_text\_ptr_  
Es un puntero de texto válido. _text\_ptr_ debe ser **binary(16)** .  
  
_offset_  
Es el número de bytes cuando se usan los tipos de datos **text** o **image**. Puede ser también el número de bytes de los caracteres cuando el tipo de datos **ntext** se usa para omitir los datos de **text**, **image** o **ntext** antes de comenzar a leerlos.  
  
_size_ es el número de bytes cuando se usan los tipos de datos **text** o **image**. También puede ser el número de bytes de los caracteres cuando se usa el tipo de datos **ntext** para leer los datos. Si _size_ es 0, se leen 4 KB de datos.  
  
HOLDLOCK  
Hace que se bloquee el valor de texto para lectura hasta el final de la transacción. Otros usuarios pueden leer el valor, pero no pueden modificarlo.  
  
## <a name="remarks"></a>Observaciones  
Use la función [TEXTPTR](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md) para obtener un valor _text\_ptr_ válido. TEXTPTR devuelve un puntero a la columna **text**, **ntext** o **image** de la fila especificada. TEXTPRT también puede devolver un puntero a la columna **text**, **ntext** o **image** de la última fila que la consulta devuelve si esta devuelve más de una fila. Debido a que TEXTPTR devuelve una cadena binaria de 16 bytes, se recomienda declarar una variable local para que contenga el puntero de texto y, a continuación, utilizar la variable con READTEXT. Para más información sobre la declaración de una variable local, vea [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).  
  
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pueden existir punteros de texto consecutivos, aunque quizás no sean válidos. Para más información sobre la opción **text in row**, vea [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Para más información sobre cómo invalidar punteros de texto, vea [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
El valor de la función @@TEXTSIZE reemplaza el tamaño especificado para READTEXT si es menor que este. La función @@TEXTSIZE especifica el límite que establece la instrucción SET TEXTSIZE sobre el número de bytes de datos que se va a devolver. Para más información sobre cómo establecer la configuración de sesión para TEXTSIZE, vea [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
Los permisos READTEXT se conceden de manera predeterminada a los usuarios con permisos SELECT para la tabla especificada. Los permisos se pueden transferir cuando se transfieren los permisos SELECT.  
  
## <a name="examples"></a>Ejemplos  
En el siguiente ejemplo se lee desde el segundo carácter hasta el 26 de la columna `pr_info` de la tabla `pub_info`.  
  
> [!NOTE]  
>  Para ejecutar este ejemplo, debe instalar la base de datos de ejemplo [**pubs**](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases).  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
   FROM pub_info pr INNER JOIN publishers p  
      ON pr.pub_id = p.pub_id   
      AND p.pub_name = 'New Moon Books'  
READTEXT pub_info.pr_info @ptrval 1 25;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
[@@TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/functions/textsize-transact-sql.md)   
[UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)   
[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
  
