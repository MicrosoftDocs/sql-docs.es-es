---
description: sys.sysservers (Transact-SQL)
title: Servidores de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysservers
- sysservers_TSQL
- sysservers
- sys.sysservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysservers system table
- sys.sysservers compatibility view
ms.assetid: d02f186f-c00f-44a6-b38d-dc78a3d2145b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7b5e908361cc0e0b7313877ab63faee2a61b2c4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99147767"
---
# <a name="syssysservers-transact-sql"></a>sys.sysservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada servidor al que una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene acceso como origen de datos OLE DB.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**srvid**|**smallint**|Id. (solo para uso local) del servidor remoto.|  
|**srvstatus**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|Nombre del servidor.|  
|**srvproduct**|**sysname**|Nombre de producto del servidor remoto.|  
|**ProviderName**|**nvarchar(128)**|Nombre de proveedor OLE DB para el acceso a este servidor.|  
|**DataSource**|**nvarchar(4000)**|Valor de origen de datos OLE DB.|  
|**ubicación**|**nvarchar(4000)**|Valor de ubicación OLE DB.|  
|**providerstring**|**nvarchar(4000)**|Valor de cadena de proveedor OLE DB.|  
|**schemadate**|**datetime**|Fecha en que se actualizó la fila por última vez.|  
|**topologyx**|**int**|No se usa.|  
|**topologyy**|**int**|No se usa.|  
|**catalog**|**sysname**|Catálogo utilizado al establecer una conexión con un proveedor OLE DB.|  
|**srvcollation**|**sysname**|La intercalación del servidor.|  
|**ConnectTimeout**|**int**|Configuración de tiempo de espera para la conexión del servidor.|  
|**QueryTimeout**|**int**|Configuración del tiempo de espera para consultas sobre un servidor.|  
|**srvnetname**|**Char (30)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**isremote**|**bit**|1 = El servidor es un servidor remoto.<br /><br /> 0 = El servidor es un servidor vinculado.|  
|**RPC**|**bit**|1 = **sp_serveroption \@ RPC** establecida en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ RPC** establecido en **false** u **OFF**.|  
|**Pub**|**bit**|1 = **sp_serveroption \@ pub** establecida en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ pub** establecida en **false** u **OFF**.|  
|**sub**|**bit**|1 = **sp_serveroption \@ Sub** establecida en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ Sub** establecido en **false** u **OFF**.|  
|**dist**|**bit**|1 = **sp_serveroption \@ Dist** establecido en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ Dist** establecido en **false** u **OFF**.|  
|**dpub**|**bit**|1 = **sp_serveroption \@ DPUB** establecido en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ DPUB** establecido en **false** u **OFF**.|  
|**rpcout**|**bit**|1 = **sp_serveroption \@ RPC out** establecida en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ RPC out** establecido en **false** u **OFF**.|  
|**DataAccess**|**bit**|1 = **sp_serveroption el \@ acceso a datos** se establece en **true** u **on**.<br /><br /> 0 = **sp_serveroption el \@ acceso a datos** se establece en **false** u **OFF**.|  
|**collationcompatible**|**bit**|1 = **sp_serveroption \@ compatible con la intercalación** establecida en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ compatible con la intercalación** establecida en **false** u **OFF**.|  
|**sistema**|**bit**|1 = **sp_serveroption \@ sistema** está establecido en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ sistema** está establecido en **false** u **OFF**.|  
|**useremotecollation**|**bit**|1 = **sp_serveroption \@ Intercalación remota** establecida en **true** u **on**.<br /><br /> 0 = **sp_serveroption \@ Intercalación remota** establecida en **false** u **OFF**.|  
|**lazyschemavalidation**|**bit**|1 = **sp_serveroption \@ validación diferida de esquema** establecida en **true** u **on**.<br /><br /> 0 = **sp_serveroption la \@ validación diferida de esquemas** establecida en **false** u **OFF**.|  
|**intercalación**|**sysname**|Intercalación de servidor establecida por **sp_serveroption \@ nombre de intercalación**.|  
|**nonsqlsub**|bit|0 = el servidor es una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> 1 = el servidor no es una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
