---
description: UPDATETEXT (Transact-SQL)
title: UPDATETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATETEXT
- UPDATETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- text updates [SQL Server]
- updating data [SQL Server]
- data updates [SQL Server], UPDATETEXT statement
- UPDATETEXT statement
ms.assetid: d73c28ee-3972-4afd-af8d-ebbbd9e50793
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5304394a75662d5cdc37196aab70444ebfbcbe41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186914"
---
# <a name="updatetext-transact-sql"></a>UPDATETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza un campo **text**, **ntext** o **image** existente. Use UPDATETEXT para cambiar solo una parte de una columna **text**, **ntext** o **image** existente. Use WRITETEXT para actualizar y reemplazar un campo **text**, **ntext** o **image** completo.  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use los tipos de datos de valores grandes y la cláusula **.** WRITE de la instrucción [UPDATE](../../t-sql/queries/update-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
UPDATETEXT [BULK] { table_name.dest_column_name dest_text_ptr }  
  { NULL | insert_offset }  
     { NULL | delete_length }  
     [ WITH LOG ]  
     [ inserted_data  
    | { table_name.src_column_name src_text_ptr } ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 BULK  
 Hace posible que las herramientas de carga carguen flujos de datos binarios. La herramienta debe proporcionar flujos en el nivel de protocolo TDS. Cuando el flujo de datos no esté presente el procesador de consultas omite la opción BULK.  
  
> [!IMPORTANT]  
>  Recomendamos que la opción BULK no se utilice en las aplicaciones basadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Es posible que esta opción se modifique o quite en versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *table_name* **.** *dest_column_name*  
 Es el nombre de la tabla y la columna **text**, **ntext** o **image** que se van a actualizar. Los nombres de las tablas y de las columnas deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). La especificación de los nombres de la base de datos y del propietario es opcional.  
  
 *dest_text_ptr*  
 Es el valor de un puntero de texto (devuelto por la función TEXTPTR) que señala a los datos **text**, **ntext** o **image** que se van a actualizar. *dest_text_ptr* debe ser **binary(** 16 **)**.  
  
 *insert_offset*  
 Es la posición de inicio de la actualización a partir de cero. En columnas **text** o **image**, *insert_offset* es el número de bytes que se van a omitir desde el principio de la columna existente antes de insertar nuevos datos. En columnas **ntext**, *insert_offset* es el número de caracteres (cada carácter **ntext** usa dos bytes). Los datos **text**, **ntext** o **image** existentes a partir de esta posición de inicio basada en cero se desplazan a la derecha para dejar espacio a los nuevos datos. Un valor 0 inserta los nuevos datos al principio de los datos existentes. Un valor NULL anexa los nuevos datos al valor de datos existente.  
  
 *delete_length*  
 Es la longitud de los datos que se van a eliminar de la columna **text**, **ntext** o **image** existente, a partir de la posición de *insert_offset*. El valor *delete_length* se expresa en bytes en las columnas **text** e **image** y en caracteres en las columnas **ntext**. Cada carácter **ntext** usa dos bytes. Con un valor 0 no se eliminan datos. Con un valor NULL, se eliminan todos los datos desde la posición de *insert_offset* hasta el final de la columna **text** o **image** existente.  
  
 WITH LOG  
 El registro está determinado por el modelo de recuperación vigente para la base de datos.  
  
 *inserted_data*  
 Son los datos que se van a insertar en la columna **text**, **ntext** o **image** existente en la posición de *insert_offset*. Se trata de un solo valor **char**, **nchar**, **varchar**, **nvarchar**, **binary**, **varbinary**, **text**, **ntext** o **image**. *inserted_data* puede ser un literal o una variable.  
  
 *table_name.src_column_name*  
 Es el nombre de la tabla y de la columna **text**, **ntext** o **image** que se van a usar como origen de los datos insertados. Los nombres de las tablas y de las columnas se deben ajustar a las reglas para los identificadores.  
  
 *src_text_ptr*  
 Es un valor de puntero de texto (devuelto por la función TEXTPTR) que señala a una columna **text**, **ntext** o **image** usada como origen de los datos insertados.  
  
> [!NOTE]  
>  El valor *scr_text_ptr* no debe ser igual que el valor *dest_text_ptr*.  
  
## <a name="remarks"></a>Comentarios  
 Los datos recién insertados pueden ser una única constante *inserted_data*, un nombre de tabla, un nombre de columna o un puntero de texto.  
  
|Acción de actualización|Parámetros de UPDATETEXT|  
|-------------------|---------------------------|  
|Para sustituir los datos existentes|Especifique un valor *insert_offset* que no sea NULL, un valor *delete_length* que no sea cero y los nuevos datos que se van a insertar.|  
|Para eliminar datos existentes|Especifique un valor *insert_offset* que no sea NULL y un valor *delete_length* que no sea cero. No especifique nuevos datos para la inserción.|  
|Para insertar nuevos datos|Especifique el valor *insert_offset*, un valor *delete_length* cero y los nuevos datos que se van a insertar.|  
  
 Para mejorar el rendimiento, se recomienda insertar o actualizar los datos **text**, **ntext** e **image** en fragmentos que sean múltiplos de 8.040 bytes.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], es posible que existan punteros de texto consecutivos a datos **text**, **ntext** o **image**, pero que no sean válidos. Para obtener más información sobre la opción "text in row", vea [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Para más información sobre cómo invalidar punteros de texto, vea [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
 Para inicializar columnas **text** en NULL, use WRITETEXT; UPDATETEXT inicializa columnas **text** en una cadena vacía.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso UPDATE en la base de datos especificada.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se coloca el puntero de texto en la variable local `@ptrval`; a continuación, se utiliza `UPDATETEXT` para actualizar un error de ortografía.  
  
> [!NOTE]  
>  Para ejecutar este ejemplo, debe instalar la base de datos pubs.  
  
```sql  
USE pubs;  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
DECLARE @ptrval BINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
   FROM pub_info pr, publishers p  
      WHERE p.pub_id = pr.pub_id   
      AND p.pub_name = 'New Moon Books'  
UPDATETEXT pub_info.pr_info @ptrval 88 1 'b';  
GO  
ALTER DATABASE pubs SET RECOVERY FULL;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
  
