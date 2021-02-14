---
description: sys.availability_group_listener_ip_addresses (Transact-SQL)
title: sys.availability_group_listener_ip_addresses (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_group_listener_ip_addresses
- sys.availability_group_listener_ip_addresses
- availability_group_listener_ip_addresses_TSQL
- sys.availability_group_listener_ip_addresses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], listeners
- sys.availability_group_listener_ip_addresses catalog view
ms.assetid: e515fa6b-1354-4110-9b70-ab2e6164c992
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6bc6887ad63b56330eab09d20f0b6fa86de4d3d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341383"
---
# <a name="sysavailability_group_listener_ip_addresses-transact-sql"></a>sys.availability_group_listener_ip_addresses (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila para cada dirección IP asociada a cualquier escucha de grupo de disponibilidad AlwaysOn del clúster de clústeres de conmutación por error de Windows Server (WSFC).  
  
 Clave principal: **listener_id**  +  **ip_address**  +  **ip_sub_mask**  
  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**listener_id**|**nvarchar (36)**|GUID de recurso del clúster de conmutación por error de Windows Server (WSFC).|  
|**ip_address**|**nvarchar (48)**|Dirección IP virtual configurada del agente de escucha del grupo de disponibilidad. Devuelve una sola dirección IPv4 o IPv6.|  
|**ip_subnet_mask**|**nvarchar(15**|Máscara de subred de IP configurada para la dirección IPv4, si existe, que se configura para el agente de escucha del grupo de disponibilidad.<br /><br /> NULL = subred IPv6|  
|**is_dhcp**|**bit**|Si la dirección IP está configurada por DHCP, puede ser:<br /><br /> 0 = La dirección IP no está configurada por DHCP.<br /><br /> 1 = La dirección IP está configurada por DHCP|  
|**network_subnet_ip**|**nvarchar (48)**|Dirección IP de subred de red que especifica la subred a la que pertenece la dirección IP.|  
|**network_subnet_prefix_length**|**int**|Longitud de prefijo de subred de red de la subred a la que pertenece la dirección IP.|  
|**network_subnet_ipv4_mask**|**nvarchar (45)**|Máscara de subred de red de la subred a la que pertenece la dirección IP. **network_subnet_ipv4_mask** para especificar el <DHCP network_subnet_option opciones de> en una cláusula with DHCP de la instrucción [Create Availability Group](../../t-sql/statements/create-availability-group-transact-sql.md) o [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .<br /><br /> NULL = subred IPv6|  
|**state**|**tinyint**|Estado ONLINE/OFFLINE de recursos IP del clúster de WSFC; puede ser:<br /><br /> 1 = En línea. El recurso de IP está en línea.<br /><br /> 0 = Sin conexión. El recurso de IP está sin conexión.<br /><br /> 2 = Pendiente en línea. El recurso de IP está sin conexión pero se está poniendo en línea.<br /><br /> 3 = error. El recurso de IP se estaba poniendo en línea pero sufrió un error.|  
|**state_desc**|**nvarchar(60)**|Descripción del **Estado**, uno de los siguientes:<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> ONLINE_PENDING<br /><br /> FAILED|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
