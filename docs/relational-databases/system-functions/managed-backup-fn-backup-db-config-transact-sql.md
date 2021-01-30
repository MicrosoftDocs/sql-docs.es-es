---
description: managed_backup.fn_backup_db_config managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_backup_db_config (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_backup_db_config
- smart_admin.fn_backup_db_config_TSQL
- fn_backup_db_config
- fn_backup_db_config_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_backup_db_config
- fn_backup_db_config
ms.assetid: 7c755d8a-64dd-44b2-be5e-735d30758900
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 01d2ecc1fffc66d92508b71951ca05d33a224cff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207430"
---
# <a name="managed_backupfn_backup_db_config-transact-sql"></a>managed_backup.fn_backup_db_config managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve 0, 1 o más filas con la configuración de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Devuelve una fila para la base de datos especificada o la información de todas las bases de datos configuradas con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] de la instancia.  
  
 Utilice este procedimiento almacenado para revisar o para determinar la configuración actual de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] para una base de datos o para todas las bases de datos de una instancia de SQL Server.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
managed_backup.fn_backup_db_config ('database_name' | '' | NULL)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 @db_name  
 El nombre de la base de datos. El @db_name parámetro es de **tipo SYSNAME**. Si una cadena vacía o un valor NULL se pasan a este parámetro, se devuelve la información sobre todas las bases de datos de la instancia de SQL Server.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|db_name|SYSNAME|nombre de base de datos.|  
|db_guid|UNIQUEIDENTIFIER|Identificador que identifica la base de datos de forma única.|  
|is_availability_database|BIT|Si la base de datos participa en un grupo de disponibilidad. El valor 1 indica que la base de datos es una base de datos de disponibilidad y el valor 0, que no lo es.|  
|is_dropped|BIT|El valor 1 indica que se trata de una base de datos quitada.|  
|credential_name|SYSNAME|Nombre de la credencial SQL que se usa para autenticarse en la cuenta de almacenamiento. El valor NULL indica que no se ha establecido ninguna credencial de SQL.|  
|retention_days|INT|Período de retención actual, en días. El valor NULL indica que [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] nunca se configuró para esta base de datos.|  
|is_managed_backup_enabled|INT|Indica si [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] está habilitada actualmente para esta base de datos. El valor 1 indica que [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] está habilitada y el valor 0 indica que [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] está deshabilitada para esta base de datos.|  
|storage_url|NVARCHAR (1024)|Dirección URL de la cuenta de almacenamiento.|  
|Encryption_algorithm|NCHAR (20)|Devuelve el algoritmo de cifrado actual que usar cuando se cifra la copia de seguridad.|  
|Encryptor_type|NCHAR(15)|Devuelve el valor del sistema de cifrado: certificado o clave asimétrica.|  
|Encryptor_name|NCHAR(long_max_de_cert/nombre_clave_asim)|Nombre del certificado o de la clave asimétrica.|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol de base de datos **db_backupoperator** con permisos **ALTER any Credential** . No se debe denegar el permiso **View any Definition** a los usuarios.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] configuración para ' TestDB '  
  
 Para cada fragmento de código, seleccione 'tsql' en el campo de atributo language.  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config('TestDB')  
```  
  
 El ejemplo siguiente devuelve la configuración de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] para todas las bases de datos de la instancia de SQL Server en la que se ejecuta.  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config (NULL)  
```  
  
  
