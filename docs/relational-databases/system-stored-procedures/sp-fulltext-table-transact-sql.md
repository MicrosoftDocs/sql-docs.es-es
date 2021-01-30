---
description: sp_fulltext_table (Transact-SQL)
title: sp_fulltext_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_table_TSQL
- sp_fulltext_table
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_table
ms.assetid: a765f311-07fc-4af3-b74c-e9a027fbecce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 18f807181005e20e94a15a8c4c95b43e82ecbb44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211326"
---
# <a name="sp_fulltext_table-transact-sql"></a>sp_fulltext_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Marca o quita la marca de una tabla para la indización de texto completo.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md)y [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_fulltext_table   
   [ @tabname= ] 'qualified_table_name'           
   , [ @action= ] 'action'   
   [   
   , [ @ftcat= ] 'fulltext_catalog_name'           
   , [ @keyname= ] 'unique_index_name'   
   ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @tabname = ] 'qualified_table_name'` Es un nombre de tabla de una o dos partes. La tabla debe existir en la base de datos actual. *qualified_table_name* es de tipo **nvarchar (** de-bajo) y no tiene ningún valor predeterminado.  
  
`[ @action = ] 'action'` Es la acción que se va a realizar. *Action* es de tipo **nvarchar (50)**, no tiene ningún valor predeterminado y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Creación**|Crea los metadatos de un índice de texto completo para la tabla a la que hace referencia *qualified_table_name* y especifica que los datos del índice de texto completo de esta tabla deben residir en *fulltext_catalog_name*. Esta acción también designa el uso de *unique_index_name* como columna de clave de texto completo. Este índice único ya debe estar presente y debe estar definido en una columna de la tabla.<br /><br /> No se puede realizar ninguna búsqueda de texto completo sobre esta tabla hasta que se rellene el catálogo de texto completo.|  
|**Omisiones**|Quita los metadatos del índice de texto completo para *qualified_table_name*. Si el índice de texto completo está activo, se desactiva automáticamente antes de quitarlo. No es necesario quitar columnas antes de quitar el índice de texto completo.|  
|**Activar**|Activa la capacidad de recopilar datos de índice de texto completo para *qualified_table_name*, después de que se haya desactivado. Debe haber al menos una columna que participe en el índice de texto completo antes de que se pueda activar.<br /><br /> Un índice de texto completo se convierte automáticamente en activo para su rellenado en el momento en que se agrega la primera columna para la indización. Si se quita la última columna del índice, éste se desactiva. Si está en proceso un seguimiento de cambios, al activar un índice inactivo se inicia otro rellenado.<br /><br /> Tenga en cuenta que esto no rellena realmente el índice de texto completo, sino que simplemente registra la tabla en el catálogo de texto completo en el sistema de archivos para que se puedan recuperar filas de *qualified_table_name* durante el siguiente rellenado del índice de texto completo.|  
|**Desactivación**|Desactiva el índice de texto completo para *qualified_table_name* de forma que los datos del índice de texto completo ya no se puedan recopilar para el *qualified_table_name*. Los metadatos del índice de texto completo permanecen y se puede volver a activar la tabla.<br /><br /> Si está activo el seguimiento de cambios, desactivar un índice activo inmoviliza el estado del índice: se detiene cualquier rellenado en curso y no se propagan más cambios al índice.|  
|**start_change_tracking**|Inicia un rellenado incremental del índice de texto completo. Si la tabla no incluye marca de tiempo, inicia un rellenado completo del índice de texto completo. Inicia un seguimiento de cambios en la tabla.<br /><br /> El seguimiento de cambios de texto completo no realiza un seguimiento de las operaciones WRITETEXT o UPDATETEXT realizadas en columnas indizadas de texto completo que son de tipo **Image**, **Text** o **ntext**.|  
|**stop_change_tracking**|Detiene el seguimiento de cambios en la tabla.|  
|**update_index**|Propaga el conjunto actual de cambios de los que se ha realizado el seguimiento al índice de texto completo.|  
|**start_background_updateindex**|Comienza a propagar los cambios de los que se ha realizado el seguimiento al índice de texto completo mientras se producen.|  
|**stop_background_updateindex**|Detiene la propagación de los cambios de los que se ha realizado el seguimiento al índice de texto completo mientras se producen.|  
|**start_full**|Inicia un rellenado completo del índice de texto completo de la tabla.|  
|**start_incremental**|Inicia un rellenado incremental del índice de texto completo de la tabla.|  
|**Detención**|Detiene un rellenado completo o incremental.|  
  
`[ @ftcat = ] 'fulltext_catalog_name'` Es un nombre de catálogo de texto completo válido y existente para una acción **Create** . Para todas las demás acciones, este parámetro debe ser NULL. *fulltext_catalog_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @keyname = ] 'unique_index_name'` Es un índice válido único de columna de clave única que no admite valores NULL en *qualified_table_name* para una acción **Create** . Para todas las demás acciones, este parámetro debe ser NULL. *unique_index_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Una vez desactivado un índice de texto completo para una tabla determinada, el índice de texto completo existente permanece en vigor hasta el siguiente rellenado completo; sin embargo, este índice no se utiliza porque [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bloquea las consultas en las tablas desactivadas.  
  
 Si se vuelve a activar la tabla y no se vuelve a llenar el índice, el índice antiguo sigue disponible para las consultas que se realizan sobre las columnas restantes habilitadas para texto completo, pero no sobre las nuevas. Las consultas que especifican una búsqueda en todas las columnas de texto completo encuentran datos de columnas eliminadas.  
  
 Una vez definida una tabla para la indización de texto completo, cambiar la columna de clave única de texto completo de un tipo de datos a otro, ya sea cambiando el tipo de datos de esa columna o cambiando la clave única de texto completo de una columna a otra, sin un rellenado completo puede producirse un error durante una consulta posterior y devolver el mensaje de error: "error de conversión *al* tipo data_type *para* el valor de clave de búsqueda de texto completo key_value Para evitarlo, quite la definición de texto completo de esta tabla mediante la acción **Drop** de **sp_fulltext_table** y vuelva a definirla con **sp_fulltext_table** y **sp_fulltext_column**.  
  
 La columna de clave de texto completo se debe definir para que tenga 900 bytes o menos. Se recomienda que el tamaño de la columna de clave sea lo más reducido posible por razones de rendimiento.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , **db_owner** y **db_ddladmin** roles fijos de base de datos, o un usuario con permisos de referencia en el catálogo de texto completo pueden ejecutar **sp_fulltext_table**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-enabling-a-table-for-full-text-indexing"></a>A. Habilitar una tabla para la indización de texto completo  
 En el ejemplo siguiente se crean los metadatos de índice de texto completo para la tabla `Document` de la base de datos `AdventureWorks`. `Cat_Desc` es un catálogo de texto completo. `PK_Document_DocumentID` es un índice único de una sola columna de `Document`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'create', 'Cat_Desc', 'PK_Document_DocumentID';  
--Add some columns  
EXEC sp_fulltext_column 'Production.Document','DocumentSummary','add';  
-- Activate the full-text index  
EXEC sp_fulltext_table 'Production.Document','activate';  
GO  
```  
  
### <a name="b-activating-and-propagating-track-changes"></a>B. Activar y propagar los cambios de los que se ha realizado un seguimiento  
 En el ejemplo siguiente se activan y comienzan a propagar los cambios de los que se ha hecho un seguimiento al índice de texto completo mientras se producen.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'Start_change_tracking';  
EXEC sp_fulltext_table 'Production.Document', 'Start_background_updateindex';  
GO  
```  
  
### <a name="c-removing-a-full-text-index"></a>C. Quitar un índice de texto completo.  
 En este ejemplo se quitan los metadatos de índice de texto completo de la tabla `Document` de la base de datos `AdventureWorks`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'drop';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_tables &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [sp_helpindex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Búsqueda de texto completo y procedimientos almacenados de búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
