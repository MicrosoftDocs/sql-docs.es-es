---
description: sp_kill_filestream_non_transacted_handles (Transact-SQL)
title: sp_kill_filestream_non_transacted_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_kill_filestream_non_transacted_handles_TSQL
- sp_kill_filestream_non_transacted_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sp_kill_filestream_non_transacted_handles
ms.assetid: 7188353e-ab29-49a0-8f25-7fb8ab122589
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dd3ba3d653b8f1626f89c7bb979580c666dd830a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165233"
---
# <a name="sp_kill_filestream_non_transacted_handles-transact-sql"></a>sp_kill_filestream_non_transacted_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cierra los identificadores de archivos no transaccionales para datos de FileTable.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
sp_kill_filestream_non_transacted_handles [[ @table_name = ] 'table_name', [[ @handle_id = ] @handle_id]]  
```  
  
## <a name="arguments"></a>Argumentos  
 *table_name*  
 El nombre de la tabla en la que se van a cerrar los identificadores no transaccionales.  
  
 Puede pasar *TABLE_NAME* sin *handle_id* para cerrar todos los identificadores no transaccionales abiertos para la filetable.  
  
 Puede pasar NULL para el valor de *TABLE_NAME* para cerrar todos los identificadores no transaccionales abiertos para todos los FileTables de la base de datos actual. NULL es el valor predeterminado.  
  
 *handle_id*  
 El Id. opcional del identificador individual que se cerrará. Puede obtener la *handle_id* de la sys.dm_filestream_non_transacted_handles &#40;la vista de administración dinámica de [Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md) . Cada Id. es único en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si especifica *handle_id*, también tiene que proporcionar un valor para *TABLE_NAME*.  
  
 Puede pasar NULL para el valor de *handle_id* para cerrar todos los identificadores no transaccionales abiertos para la filetable especificada por *TABLE_NAME*. NULL es el valor predeterminado.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-set"></a>Tipo de cursor  
 Ninguno.  
  
## <a name="general-remarks"></a>Notas generales  
 Los *handle_id* requeridos por **sp_kill_filestream_non_transacted_handles** no están relacionados con el session_id o la unidad de trabajo que se usa en otros comandos **Kill** .  
  
 Para obtener más información, vea [Administrar FileTables](../../relational-databases/blob/manage-filetables.md).  
  
## <a name="metadata"></a>Metadatos  
 Para obtener información acerca de los identificadores de archivos no transaccionales abiertos, consulte la vista de administración dinámica [sys.dm_filestream_non_transacted_handles &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Debe tener el permiso **View Database State** para obtener los identificadores de archivo de la vista de administración dinámica **Sys.dm_FILESTREAM_non_transacted_handles** y para ejecutar **sp_kill_filestream_non_transacted_handles**.  
  
## <a name="examples"></a>Ejemplos  
 En los siguientes ejemplos se muestra cómo llamar a **sp_kill_filestream_non_transacted_handles** para cerrar identificadores de archivos no transaccionales para datos de filetable.  
  
```sql  
-- Close all open handles in the current database.  
sp_kill_filestream_non_transacted_handles  
  
-- Close all open handles in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable'  
  
-- Close a specific handle in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable', @handle_id = 0xFFFAAADD  
```  
  
 En el ejemplo siguiente se muestra cómo usar un script para obtener una *handle_id* y cerrarla.  
  
```sql  
DECLARE @handle_id varbinary(16);  
DECLARE @table_name sysname;  
  
SELECT TOP 1 @handle_id = handle_id, @table_name = Object_name(table_id)  
FROM sys.dm_FILESTREAM_non_transacted_handles;  
  
EXEC sp_kill_filestream_non_transacted_handles @dbname, @table_name, @handle_id;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Administrar FileTables](../../relational-databases/blob/manage-filetables.md)  
 [Vistas de administración dinámica de FileStream y FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
 <br>[Vistas de catálogo de FileStream y FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
 <br>[sp_filestream_force_garbage_collection (Transact-SQL)](filestream-and-filetable-sp-filestream-force-garbage-collection.md)
  
