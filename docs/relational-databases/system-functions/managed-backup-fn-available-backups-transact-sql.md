---
description: managed_backup.fn_available_backups managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_available_backups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_available_backups
- smart_admin.fn_available_backups_TSQL
- fn_available_backups_TSQL
- fn_available_backups
dev_langs:
- TSQL
helpviewer_keywords:
- fn_available_backups
- smart_admin.fn_available_backups
ms.assetid: 7aa84474-16e5-49bd-a703-c8d1408ef107
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e64cba484413a0d5ebc1a8365d0e843c49ba7692
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207436"
---
# <a name="managed_backupfn_available_backups-transact-sql"></a>managed_backup.fn_available_backups managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve una tabla de 0, una o más filas de los archivos de copia de seguridad disponibles para la base de datos especificada. Los archivos de copia de seguridad devueltos son copias de seguridad creadas por [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
managed_backup.fn_available_backups ([@database_name = ] 'database name')  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 @database_name  
 El nombre de la base de datos. Es de tipo @database_name nvarchar (512).  
  
## <a name="table-returned"></a>Tabla devuelta  
 La tabla tiene una restricción de clúster único en (database_guid, backup_start_date, first_lsn y backup_type).   
Si se quita y se vuelve a crear una base de datos, se devuelven los conjuntos de copia de seguridad de todas las bases de datos. La salida se ordena por el database_guid, que identifica de forma única cada base de datos.   
Si faltan datos en LSN que indican que hay una interrupción en la cadena de registros, la tabla contendrá una fila especial para cada segmento de LSN que falte.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|Backup_path|NVARCHAR(260) COLLATE Latin1_General_CI_AS_KS_WS|La dirección URL del archivo de copia de seguridad.|  
|backup_type|NVARCHAR (6)|' DB ' para la copia de seguridad de base de datos ' LOG ' para la copia de seguridad de registros|  
|expiration_date|DATETIME|Fecha en la que se espera que este archivo sea eliminado. Esto se determina según la capacidad de recuperar la base de datos a un momento dado durante el período de retención especificado.|  
|database_guid|UNIQUEIDENTIFIER|Valor GUID para la base de datos especificada.  El GUID identifica de forma única una base de datos.|  
|first_lsn|NUMERIC(25, 0)|Número de secuencia de registro de la primera entrada de registro del conjunto de copia de seguridad o de la más antigua. Puede ser NULL.|  
|last_lsn|NUMERIC(25, 0)|Número de secuencia de registro de la siguiente entrada del registro después del conjunto de copia de seguridad. Puede ser NULL.|  
|backup_start_date|DATETIME|Fecha y hora en que comenzó la operación de copia de seguridad.|  
|backup_finish_date|NVARCHAR(128)|Fecha y hora en que terminó la operación de copia de seguridad.|  
|machine_name|NVARCHAR(128)|Nombre del equipo donde se instala la instancia de SQL Server y se ejecuta [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|last_recovery_fork_id|UNIQUEIDENTIFIER|Número de identificación de la bifurcación de recuperación final.|  
|first_recovery_fork_id|UNIQUEIDENTIFIER|Id. de la bifurcación de recuperación inicial. Para las copias de seguridad de datos, first_recovery_fork_guid es igual a last_recovery_fork_guid.|  
|fork_point_lsn|NUMERIC(25, 0)|Si first_recovery_fork_id no es igual que last_recovery_fork_id, este es el número de secuencia de registro del punto de bifurcación. De lo contrario, este valor es NULL.|  
|availability_group_guid|UNIQUEIDENTIFIER|Si una base de datos es una base de datos Always On, este es el GUID del grupo de disponibilidad. De lo contrario, este valor es NULL.|  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto) o 1 (error).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere permisos **Select** en esta función.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se enumeran todas las copias de seguridad disponibles a través [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] de para la base de datos ' MyDB '  
  
```  
SELECT *   
FROM msdb.managed_backup.fn_available_backups ('MyDB')  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server copia de seguridad administrada en Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)   
 [Restaurar a partir de copias de seguridad archivadas en Microsoft Azure](../../relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure.md)  
  
  
