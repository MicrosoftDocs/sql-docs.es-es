---
description: sp_delete_backuphistory (Transact-SQL)
title: sp_delete_backuphistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_backuphistory
- sp_delete_backuphistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_backuphistory
ms.assetid: bdb56834-616e-47e4-b942-e895d2325e97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac5426940ed3fd2a94c055968a22ec1000e4d564
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195549"
---
# <a name="sp_delete_backuphistory-transact-sql"></a>sp_delete_backuphistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Reduce el tamaño de las tablas del historial de copias de seguridad y restauración eliminando las entradas de conjuntos de copias de seguridad anteriores a la fecha especificada. Se agregan filas adicionales a las tablas del historial de copias de seguridad y restauración después de realizar cada operación de copia de seguridad o restauración; por lo tanto, se recomienda ejecutar periódicamente **sp_delete_backuphistory**.  
  
> [!NOTE]  
>  Las tablas de historial de copias de seguridad y restauración residen en la base de datos **msdb** .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_delete_backuphistory [ @oldest_date = ] 'oldest_date'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @oldest_date = ] 'oldest\_date'` Es la fecha más antigua conservada en las tablas de historial de copias de seguridad y restauración. *oldest_date* es **DateTime** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_delete_backuphistory** se debe ejecutar desde la base de datos **msdb** y afecta a las tablas siguientes:  
  
-   [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
  
-   [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
  
-   [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
  
-   [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
  
-   [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
-   [restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)  
  
-   [restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)  
  
-   [restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)  
  
 Los archivos de copia de seguridad físicos se conservan, aunque se elimine todo el historial.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** , pero se pueden conceder permisos a otros usuarios.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se eliminan todas las entradas anteriores al 14 de enero de 2010, 12:00 a. m. de las tablas de historial de copias de seguridad y restauración.  
  
```  
USE msdb;  
GO  
EXEC sp_delete_backuphistory @oldest_date = '01/14/2010';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_delete_database_backuphistory &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-database-backuphistory-transact-sql.md)   
 [Historial de copias de seguridad e información de encabezados &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
