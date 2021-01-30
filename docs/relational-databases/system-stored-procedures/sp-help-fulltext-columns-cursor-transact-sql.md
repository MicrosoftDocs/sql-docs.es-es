---
description: sp_help_fulltext_columns_cursor (Transact-SQL)
title: sp_help_fulltext_columns_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_columns_cursor
- sp_help_fulltext_columns_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_columns_cursor
ms.assetid: 26054e76-53b7-4004-8d48-92ba3435e9d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2a27c719888b21d9762d77e1f14f3590e93fe452
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198318"
---
# <a name="sp_help_fulltext_columns_cursor-transact-sql"></a>sp_help_fulltext_columns_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utiliza un cursor para devolver las columnas designadas para la indización de texto completo.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo [Sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_fulltext_columns_cursor [ @cursor_return = ] @cursor_variable OUTPUT   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @cursor_return = ] @cursor_variable OUTPUT` Es la variable de salida de tipo **cursor**. El cursor resultante es desplazable, dinámico y de solo lectura.  
  
`[ @table_name = ] 'table_name'` Es el nombre de tabla de una o dos partes para la que se solicita información de índice de texto completo. *TABLE_NAME* es de tipo **nvarchar (r)** y su valor predeterminado es NULL. Si se omite *TABLE_NAME* , se recupera la información de la columna de índice de texto completo de cada tabla indizada de texto completo.  
  
`[ @column_name = ] 'column_name'` Es el nombre de la columna para la que se desea obtener los metadatos del índice de texto completo. *column_name* es de **tipo sysname y su** valor predeterminado es NULL. Si *column_name* se omite o es null, se devuelve información de la columna de texto completo para cada columna indizada de texto completo de *TABLE_NAME*. Si también se omite *TABLE_NAME* o es null, se devuelve información de la columna de índice de texto completo para cada columna indizada de texto completo de todas las tablas de la base de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Propietario de la tabla. Es el nombre del usuario de la base de datos que ha creado la tabla.|  
|**TABLE_ID**|**int**|Id. de la tabla.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla.|  
|**FULLTEXT_COLUMN_NAME**|**sysname**|Columna de una tabla con índice de texto completo que está designada para su indización.|  
|**FULLTEXT_COLID**|**int**|Id. de columna de la columna de índice de texto completo.|  
|**FULLTEXT_BLOBTP_COLNAME**|**sysname**|Columna de una tabla con índice de texto completo que especifica el tipo de documento de la columna de índice de texto completo. Este valor solo es aplicable cuando la columna indizada de texto completo es una columna **varbinary (Max)** o **Image** .|  
|**FULLTEXT_BLOBTP_COLID**|**int**|Id. de la columna de tipo de documento. Este valor solo es aplicable cuando la columna indizada de texto completo es una columna **varbinary (Max)** o **Image** .|  
|**FULLTEXT_LANGUAGE**|**sysname**|Lenguaje utilizado para la búsqueda de texto completo de la columna.|  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los permisos de ejecución corresponden a los miembros del rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información acerca de las columnas que se han designado para la indización de texto completo en todas las tablas de la base de datos.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @mycursor CURSOR;  
EXEC sp_help_fulltext_columns_cursor @mycursor OUTPUT  
FETCH NEXT FROM @mycursor;  
WHILE (@@FETCH_STATUS <> -1)  
   BEGIN  
      FETCH NEXT FROM @mycursor;  
   END;  
CLOSE @mycursor;  
DEALLOCATE @mycursor;  
GO   
```  
  
## <a name="see-also"></a>Consulte también  
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [sp_fulltext_column &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-column-transact-sql.md)   
 [sp_help_fulltext_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
