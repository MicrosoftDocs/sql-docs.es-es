---
description: sys.syslogins (Transact-SQL)
title: Inicios de sesión de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syslogins_TSQL
- syslogins
- sys.syslogins
- sys.syslogins_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syslogins compatibility view
- syslogins system table
ms.assetid: 4cb34f17-a4bb-469f-a218-71f074e6308f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f051ffe3a01c4d2dabccfcc43c806dc21e4b8785
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201369"
---
# <a name="syssyslogins-transact-sql"></a>sys.syslogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada cuenta de inicio de sesión.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Junction**|**varbinary(85)**|Identificador de seguridad.|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**CreateDate**|**datetime**|Fecha en que se agregó el inicio de sesión.|  
|**updatedate**|**datetime**|Fecha en que se actualizó el inicio de sesión.|  
|**accdate**|**datetime**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totcpu**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totio**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**spacelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**timelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**resultlimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**name**|**sysname**|Nombre de inicio de sesión del usuario.|  
|**nombrebd**|**sysname**|Nombre de la base de datos predeterminada del usuario al establecer una conexión.|  
|**password**|**nvarchar(128)**|Devuelve NULL.|  
|**language**|**sysname**|Idioma predeterminado del usuario.|  
|**denylogin**|**int**|1 = El inicio de sesión corresponde a un usuario o grupo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows al que se le ha denegado el acceso.|  
|**hasaccess**|**int**|1 = El inicio de sesión tiene acceso al servidor.|  
|**isntname**|**int**|1 = El inicio de sesión corresponde a un usuario o grupo de Windows.<br /><br /> 0 = Es un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**isntgroup**|**int**|1 = El inicio de sesión corresponde a un grupo de Windows.|  
|**isntuser**|**int**|1 = El inicio de sesión corresponde a un usuario de Windows.|  
|**sysadmin**|**int**|1 = el inicio de sesión es miembro del rol de servidor **sysadmin** .|  
|**securityadmin**|**int**|1 = el inicio de sesión es miembro del rol de servidor **securityadmin** .|  
|**serveradmin**|**int**|1 = el inicio de sesión es miembro del rol fijo de servidor **ServerAdmin** .|  
|**setupadmin**|**int**|1 = el inicio de sesión es miembro del rol fijo de servidor **setupadmin** .|  
|**processadmin**|**int**|1 = el inicio de sesión es miembro del rol fijo de servidor **processadmin** .|  
|**diskadmin**|**int**|1 = el inicio de sesión es miembro del rol fijo de servidor **diskadmin** .|  
|**dbcreator**|**int**|1 = el inicio de sesión es miembro del rol fijo de servidor **dbcreator** .|  
|**bulkadmin**|**int**|1 = el inicio de sesión es miembro del rol fijo de servidor **bulkadmin** .|  
|**LoginName**|**nvarchar(128)**|Nombre de inicio de sesión del usuario. Se proporciona para mantener la compatibilidad con versiones anteriores.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
