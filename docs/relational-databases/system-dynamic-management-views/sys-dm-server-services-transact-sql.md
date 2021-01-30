---
description: sys.dm_server_services (Transact-SQL)
title: sys.dm_server_services (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/07/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_server_services
- sys.dm_server_services
- sys.dm_server_services_TSQL
- dm_server_services_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_services dynamic management view
ms.assetid: 3f0defd0-478d-4e7f-96be-8795c9de4e3f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fd00b91eb2ba5018a7ae9865f323ac2f3faeb39d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99134589"
---
# <a name="sysdm_server_services-transact-sql"></a>sys.dm_server_services (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los servicios de SQL Server, texto completo SQL Server Launchpad (SQL Server 2017 +) y Agente SQL Server en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Use esta vista de administración dinámica para notificar información de estado sobre estos servicios.  
  
 
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|servicename|**nvarchar(256)**|Nombre del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] servicio de, de texto completo o de Agente SQL Server. No puede ser NULL.|  
|startup_type|**int**|Indica el modo de inicio del servicio. A continuación se muestran los valores posibles y sus descripciones correspondientes.<br /><br /> 0: otros<br />1: otros<br />2: automático<br />3: manual<br />4: deshabilitado<br /><br /> Acepta valores NULL.|  
|startup_type_desc|**nvarchar(256)**|Describe el modo de inicio del servicio. A continuación se muestran los valores posibles y sus descripciones correspondientes.<br /><br /> Otros: otros (inicio de arranque)<br />Otros: otros (inicio del sistema)<br />Automático: Inicio automático<br />Manual: Inicio de la demanda<br />Deshabilitado: deshabilitado<br /><br /> No puede ser NULL.|  
|status|**int**|Indica el estado actual del servicio. A continuación se muestran los valores posibles y sus descripciones correspondientes.<br /><br /> 1: detenido<br />2: otros (Inicio pendiente)<br />3: otros (detención pendiente)<br />4: ejecución<br />5: otros (continuación pendiente)<br />6: otros (pausa pendiente)<br />7: en pausa<br /><br /> Acepta valores NULL.|  
|status_desc|**nvarchar(256)**|Describe el estado actual del servicio. A continuación se muestran los valores posibles y sus descripciones correspondientes.<br /><br /> Detenido: el servicio se ha detenido.<br />Otro (operación de inicio pendiente): el servicio está en proceso de inicio.<br />Otro (operación de detención pendiente): el servicio está en proceso de detención.<br />En ejecución: el servicio se está ejecutando.<br />Otros (continuar con las operaciones pendientes): el servicio está en un estado pendiente.<br />Otro (pausa pendiente): el servicio está en proceso de pausa.<br />En pausa: el servicio está en pausa.<br /><br /> No puede ser NULL.|  
|process_id|**int**|Identificador de proceso del servicio. No puede ser NULL.|  
|last_startup_time|**datetimeoffset(7)**|Fecha y hora en que el servicio se inició por última vez. Acepta valores NULL.|  
|service_account|**nvarchar(256)**|Cuenta autorizada para controlar el servicio. Esta cuenta puede iniciar o detener el servicio, o puede modificar las propiedades del servicio. No puede ser NULL.|  
|nombreDeArchivo|**nvarchar(256)**|Ruta de acceso y nombre del archivo ejecutable del servicio. No puede ser NULL.|  
|is_clustered|**nvarchar (1)**|Indica si el servicio está instalado como un recurso de un servidor en clúster. No puede ser NULL.|  
|cluster_nodename|**nvarchar(256)**|Nombre del nodo de clúster en el que está instalado el servicio. Acepta valores NULL.|
|instant_file_initialization_enabled|**nvarchar (1)**|Especifica si la inicialización instantánea de archivos está habilitada para el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] servicio.<br /><br />Y = la inicialización instantánea de archivos está habilitada para el servicio.<br /><br />N = la inicialización instantánea de archivos está deshabilitada para el servicio.<br /><br /> Acepta valores NULL.<br /><br /> **Nota:** No se aplica a otros servicios como el Agente SQL Server.<br /><br /> **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (A partir de [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4 y [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 y versiones posteriores).|  

## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE` en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_server_registry &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-server-registry-transact-sql.md)  
  
