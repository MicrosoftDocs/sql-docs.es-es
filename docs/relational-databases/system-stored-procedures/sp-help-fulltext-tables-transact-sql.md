---
description: sp_help_fulltext_tables (Transact-SQL)
title: sp_help_fulltext_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_tables
- sp_help_fulltext_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_tables
ms.assetid: 86e24a5f-a869-43f6-b83e-c52b7b01b5ff
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac2e0711ba1c18fef41d18a41a4a8155160d2c02
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200177"
---
# <a name="sp_help_fulltext_tables-transact-sql"></a>sp_help_fulltext_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una lista de tablas registradas para la indización de texto completo.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo **Sys.fulltext_indexes** . Para obtener más información, vea [sys.fulltext_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_fulltext_tables [ [ @fulltext_catalog_name = ] 'fulltext_catalog_name' ]   
     [ , [ @table_name = ] 'table_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` Es el nombre del catálogo de texto completo. *fulltext_catalog_name* es de **tipo sysname y su** valor predeterminado es NULL. Si *fulltext_catalog_name* se omite o es null, se devuelven todas las tablas indizadas de texto completo asociadas a la base de datos. Si se especifica *fulltext_catalog_name* , pero *TABLE_NAME* se omite o es null, se recupera la información del índice de texto completo para cada tabla indizada de texto completo asociada a este catálogo. Si se especifican *fulltext_catalog_name* y *TABLE_NAME* , se devuelve una fila si *TABLE_NAME* está asociado a *fulltext_catalog_name*; de lo contrario, se produce un error.  
  
`[ @table_name = ] 'table_name'` Es el nombre de tabla de una o dos partes para la que se solicitan los metadatos de texto completo. *TABLE_NAME* es de tipo **nvarchar (r)** y su valor predeterminado es NULL. Si solo se especifica *TABLE_NAME* , solo se devuelve la fila pertinente a *TABLE_NAME* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Propietario de la tabla. Es el nombre del usuario de la base de datos que ha creado la tabla.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla.|  
|**FULLTEXT_KEY_INDEX_NAME**|**sysname**|Índice que impone la restricción UNIQUE a la columna designada como columna de clave única.|  
|**FULLTEXT_KEY_COLID**|**int**|Id. de columna del índice único que identifica FULLTEXT_KEY_NAME.|  
|**FULLTEXT_INDEX_ACTIVE**|**int**|Especifica si las columnas de esta tabla marcadas para indización de texto completo pueden utilizarse en consultas:<br /><br /> 0 = Inactivo <br /><br /> 1 = Activo|  
|**FULLTEXT_CATALOG_NAME**|**sysname**|Catálogo de texto completo en el que residen los datos de índice de texto completo.|  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los permisos de ejecución corresponden a los miembros del rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se obtienen los nombres de las tablas con índice de texto completo asociadas al catálogo de texto completo `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help_fulltext_tables 'Cat_Desc';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_fulltext_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
