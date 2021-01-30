---
description: managed_backup.fn_backup_instance_config managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_backup_instance_config (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_backup_instance_config
- smart_admin.fn_backup_instance_config_TSQL
- fn_backup_instance_config_TSQL
- smart_admin.fn_backup_instance_config
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_backup_instance_config
- fn_backup_instance_config
ms.assetid: 2382a547-c0c9-4e1d-87c9-d8526192eb5a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 534b1417536b233cafd0c7f3499e6ff801f0fa42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207422"
---
# <a name="managed_backupfn_backup_instance_config-transact-sql"></a>managed_backup.fn_backup_instance_config managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve una fila con la configuración predeterminada de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] para la instancia de SQL Server.  
  
 Utilice este procedimiento almacenado para revisar o para determinar la configuración predeterminada actual de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] para la instancia de SQL Server.  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
managed_backup.fn_backup_db_config ()  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 None  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|is_smart_backup_enabled|INT|Muestra 1 cuando [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] está habilitada y 0 cuando [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] está deshabilitada.|  
|credential_name|SYSNAME|La Credencial SQL predeterminada que se usa para autenticarse en el almacenamiento.|  
|retention_days|INT|Período de retención predeterminado establecido en el nivel de instancia.|  
|storage_url|NVARCHAR (1024)|La dirección URL de la cuenta de almacenamiento predeterminada establecida en el nivel de instancia.|  
|encryption_algorithm|SYSNAME|Nombre del algoritmo de cifrado. Se establece en NULL si no se especifica el cifrado.|  
|encryptor_type|NVARCHAR (32)|Tipo de sistema de cifrado usado: certificado o clave asimétrica. Se establece en NULL si no hay ningún sistema de cifrado especificado.|  
|encryptor_name|SYSNAME|Nombre del certificado o de la clave asimétrica. Se establece en NULL si no hay ningún nombre especificado|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol de base de datos **db_backupoperator** con permisos **ALTER any Credential** . No se debe denegar el permiso **View any Definition** a los usuarios.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente devuelve la configuración predeterminada de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] para la instancia en la que se ejecuta:  
  
```  
Use msdb;  
GO  
SELECT * FROM managed_backup.fn_backup_instance_config ();  
  
```  
  
  
