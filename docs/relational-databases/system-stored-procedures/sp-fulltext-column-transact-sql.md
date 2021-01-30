---
description: sp_fulltext_column (Transact-SQL)
title: sp_fulltext_column (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_column_TSQL
- sp_fulltext_column
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_column
ms.assetid: a84cc45d-1b50-44af-85df-2ea033b8a6a9
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4203f4ce8d603bd8715047b8a3f0ffb93e6b1e8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185400"
---
# <a name="sp_fulltext_column-transact-sql"></a>sp_fulltext_column (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Especifica si una columna concreta de una tabla participa en la indización de texto completo.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilice [ALTER fulltext index](../../t-sql/statements/alter-fulltext-index-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_fulltext_column [ @tabname= ] 'qualified_table_name' ,   
     [ @colname= ] 'column_name' ,   
     [ @action= ] 'action'   
     [ , [ @language= ] 'language_term' ]   
     [ , [ @type_colname= ] 'type_column_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @tabname = ] 'qualified_table_name'` Es un nombre de tabla de una o dos partes. La tabla debe existir en la base de datos actual. La tabla debe tener un índice de texto completo. *qualified_table_name* es de tipo **nvarchar (** de-bajo) y no tiene ningún valor predeterminado.  
  
`[ @colname = ] 'column_name'` Es el nombre de una columna de *qualified_table_name*. La columna debe ser una columna de tipo carácter, **varbinary (Max)** o **Image** y no puede ser una columna calculada. *column_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede crear índices de texto completo de los datos de texto almacenados en columnas que son de tipo de datos **varbinary (Max)** o **Image** . Las imágenes no se indizan.  
  
`[ @action = ] 'action'` Es la acción que se va a realizar. *Action* es de tipo **VARCHAR (20)**, no tiene ningún valor predeterminado y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**add**|Agrega *column_name* de *qualified_table_name* al índice de texto completo inactivo de la tabla. Esta acción habilita el indizado de texto completo de la columna.|  
|**omisiones**|Quita *column_name* de *qualified_table_name* del índice de texto completo inactivo de la tabla.|  
  
`[ @language = ] 'language_term'` Es el idioma de los datos almacenados en la columna. Para obtener una lista de los idiomas incluidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [sys.fulltext_languages &#40;&#41;de TRANSACT-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).  
  
> [!NOTE]  
>  Use 'Neutral' cuando una columna contiene datos en varios idiomas o en un idioma no admitido. El valor predeterminado se especifica en la opción de configuración 'default full-text language'.  
  
`[ @type_colname = ] 'type_column_name'` Es el nombre de una columna de *qualified_table_name* que contiene el tipo de documento de *column_name*. Esta columna debe ser **Char**, **nchar**, **VARCHAR** o **nvarchar**. Solo se usa cuando el tipo de datos de *column_name* es de tipo **varbinary (Max)** o **Image**. *type_column_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Si el índice de texto completo está activo, se detiene cualquier llenado en proceso. Además, si una tabla con un índice de texto completo activo tiene habilitado el seguimiento de cambios, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] garantiza que el índice está actualizado. Por ejemplo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detiene cualquier llenado actual de la tabla, quita el índice existente e inicia un nuevo llenado.  
  
 Si está activado el seguimiento de cambios y es necesario agregar o quitar columnas del índice de texto completo sin eliminar éste, la tabla se tiene que desactivar y las columnas apropiadas se tienen que agregar o quitar. Estas acciones bloquean el índice. Se puede activar la tabla más adelante, cuando sea más práctico iniciar un llenado.  
  
## <a name="permissions"></a>Permisos  
 El usuario debe ser miembro del rol fijo de base de datos **db_ddladmin** , o un miembro del rol fijo de base de datos **db_owner** o del propietario de la tabla.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se agrega la columna `DocumentSummary` de la tabla `Document` al índice de texto completo de la tabla.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_column 'Production.Document', DocumentSummary, 'add';  
GO  
```  
  
 El ejemplo presupone que ha creado un índice de texto completo en una tabla denominada `spanishTbl`. Para agregar la columna `spanishCol` al índice de texto completo, ejecute el siguiente procedimiento almacenado:   
  
```  
EXEC sp_fulltext_column 'spanishTbl', 'spanishCol', 'add', 0xC0A;  
GO  
```  
  
 Al ejecutar esta consulta:  
  
```  
SELECT *   
FROM spanishTbl   
WHERE CONTAINS(spanishCol, 'formsof(inflectional, trabajar)')  
```  
  
 El conjunto de resultados incluiría filas con diferentes formas de `trabajar`, como `trabajo`, `trabajamos` y `trabajan`.  
  
> [!NOTE]  
>  Todas las columnas mostradas en una única cláusula de función para consulta de texto completo deben usar el mismo idioma.  
  
## <a name="see-also"></a>Consulte también  
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)   
 [sp_help_fulltext_columns_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)   
 [sp_help_fulltext_tables &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Búsqueda de texto completo y procedimientos almacenados de búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
