---
description: sys.database_recovery_status (Transact-SQL)
title: sys.database_recovery_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_recovery_status_TSQL
- database_recovery_status
- sys.database_recovery_status
- sys.database_recovery_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_recovery_status catalog view
ms.assetid: 46fab234-1542-49be-8edf-aa101e728acf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 015a82160b434afdbd3be5f6220e3dbf42070979
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209445"
---
# <a name="sysdatabase_recovery_status-transact-sql"></a>sys.database_recovery_status (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por base de datos. Si la base de datos no está abierta, el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] intenta iniciarla.  
  
 Para ver la fila de una base de datos distinta de **Master** o **tempdb**, debe aplicarse una de las siguientes opciones:  
  
-   Ser el propietario de la base de datos.  
  
-   Tener los permisos de servidor ALTER ANY DATABASE o VIEW ANY DATABASE.  
  
-   Tener el permiso CREATE DATABASE en la base de datos **maestra** .    
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Id. de la base de datos, único en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**database_guid**|**uniqueidentifier**|Se utiliza para relacionar entre sí todos los archivos de una base de datos. Para que la base de datos se inicie de la forma esperada, todos los archivos deben tener este GUID en la página de encabezado. Solo una base de datos puede tener este GUID, aunque se pueden crear duplicados al copiar y adjuntar bases de datos. RESTORE siempre genera un nuevo GUID cuando se restaura una base de datos que todavía no existe.<br /><br /> NULL= La base de datos está sin conexión o no se va a iniciar.|  
|**family_guid**|**uniqueidentifier**|Identificador de la familia de copias de seguridad de la base de datos para detectar estados de restauración coincidentes.<br /><br /> NULL = la base de datos está sin conexión o la base de datos no se iniciará.|  
|**last_log_backup_lsn**|**numeric(25,0)**|El número de secuencia de registro inicial de la siguiente copia de seguridad de registros.<br /><br /> Si es NULL, no se puede realizar una copia de seguridad del registro de transacciones porque la base de datos está en una recuperación SIMPLE o porque no hay ninguna copia de seguridad de base de datos actual.|  
|**recovery_fork_guid**|**uniqueidentifier**|Identifica la bifurcación de recuperación actual en la que está activa la base de datos.<br /><br /> NULL= La base de datos está sin conexión o no se va a iniciar.|  
|**first_recovery_fork_guid**|**uniqueidentifier**|Identificador de la bifurcación de recuperación inicial.<br /><br /> NULL= La base de datos está sin conexión o no se va a iniciar.|  
|**fork_point_lsn**|**numeric(25,0)**|Si **first_recovery_fork_guid** no es igual a **recovery_fork_guid**, **fork_point_lsn** es el número de secuencia de registro del punto de bifurcación actual. En caso contrario, el valor es NULL.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vistas de catálogo de archivos y bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
