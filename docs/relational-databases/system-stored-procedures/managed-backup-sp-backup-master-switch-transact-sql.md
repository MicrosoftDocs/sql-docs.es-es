---
description: managed_backup.sp_backup_master_switch managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_master_switch (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
- sp_ backup_master_switch_TSQL
- smart_admin.sp_backup_master_switch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
ms.assetid: 1ed2b2b2-c897-41cc-bed5-1c6bc47b9dd2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 97202d989a21002d7f24c886378fd3f6c6b35ebd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205306"
---
# <a name="managed_backupsp_backup_master_switch-transact-sql"></a>managed_backup.sp_backup_master_switch managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Pausa o reanuda la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Utilice este procedimiento almacenado para pausar temporalmente la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] y reanudarla después. Esto garantiza que toda la configuración permanece y se conserva cuando las operaciones se reanudan. Cuando se pausa la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], el período de retención no se aplica. Esto significa que no hay ninguna comprobación para determinar si los archivos deben eliminarse del almacenamiento o si hay archivos de copia de seguridad dañados o se ha interrumpido la cadena de registros.  
  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
EXEC managed_backup.sp_backup_master_switch   
                     [@new_state = ] { 0 | 1}  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 @state  
 Establezca el estado de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. El @state parámetro es **bit**. Cuando se establece en el valor 0, las operaciones se pausan y, cuando se establece en el valor 1, la operación se reanuda.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="security"></a>Seguridad  
 Describe problemas de seguridad relacionados con la instrucción. Permisos de inclusión como subsección (título H3). Considere incluir otras subsecciones para el encadenamiento de la propiedad y auditoría, si procede.  
  
### <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol de base de datos **db_backupoperator** , con permisos **ALTER any Credential** y permisos **Execute** en **sp_delete_backuphistory** procedimiento almacenado.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente se puede utilizar para pausar la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] en la instancia que se ejecuta en:  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=0;  
Go  
  
```  
  
 El ejemplo siguiente se puede utilizar para reanudar la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=1;  
Go  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Copia de seguridad administrada en Microsoft Azure para SQL Server](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
