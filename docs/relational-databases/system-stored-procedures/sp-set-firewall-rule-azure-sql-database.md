---
description: sp_set_firewall_rule (Azure SQL Database)
title: sp_set_firewall_rule (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_set_firewall_rule
- sp_set_firewall_rule_TSQL
- sys.sp_set_firewall_rule
- sys.sp_set_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_firewall_rule
- firewall_rules, setting server rules
ms.assetid: a974a561-5382-4039-8499-3a56767bcefe
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: aa47f5578403fa6cfd2f945ebcaa5bff767d967b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184738"
---
# <a name="sp_set_firewall_rule-azure-sql-database"></a>sp_set_firewall_rule (Azure SQL Database)
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  Crea o actualiza la configuración del firewall de nivel de servidor para el servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Este procedimiento almacenado solo está disponible en la base de datos maestra para el inicio de sesión principal de nivel de servidor o asignado Azure Active Directory entidad de seguridad.  
  
  
## <a name="syntax"></a>Sintaxis  
  
```
sp_set_firewall_rule [@name =] 'name', 
    [@start_ip_address =] 'start_ip_address', 
    [@end_ip_address =] 'end_ip_address'
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos  
 En la tabla siguiente se muestran los argumentos y las opciones admitidos en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
|Nombre|Datatype|Descripción|  
|----------|--------------|-----------------|  
|[ @name =] ' nombre '|**NVARCHAR (128)**|El nombre utilizado para describir y distinguir la configuración del firewall de nivel de servidor.|  
|[ @start_ip_address =] ' start_ip_address '|**VARCHAR (50)**|La dirección IP más baja en el intervalo de la configuración del firewall de nivel de servidor. Las direcciones IP iguales o superiores a esta pueden intentar conectarse con el servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. La dirección IP más baja posible es `0.0.0.0`.|  
|[ @end_ip_address =] ' end_ip_address '|**VARCHAR (50)**|La dirección IP más alta en el intervalo de la configuración del firewall de nivel de servidor. Las direcciones IP iguales o inferiores a esta pueden intentar conectarse con el servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. La dirección IP más alta posible es `255.255.255.255`.<br /><br /> Nota: los intentos de conexión de Azure se permiten cuando este campo y el campo *start_ip_address* es igual a `0.0.0.0` .|  
  
## <a name="remarks"></a>Observaciones  
 Los nombres de la configuración del firewall de nivel de servidor deben ser únicos. Si el nombre del valor proporcionado para el procedimiento almacenado ya existe en la tabla de configuración del firewall, las direcciones IP inicial y final se actualizarán. De lo contrario, se creará una nueva configuración del firewall de nivel de servidor.  
  
 Al agregar una configuración de firewall de nivel de servidor donde las direcciones IP inicial y final son iguales a `0.0.0.0`, se permite al acceso a su servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)] desde Azure. Proporcione un valor al parámetro *Name* que le ayude a recordar para qué sirve la configuración de Firewall de nivel de servidor.  
  
 En [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los datos de inicio de sesión necesarios para autenticar una conexión y reglas de firewall de nivel de servidor se almacenan temporalmente en caché en cada base de datos. Esta caché se actualiza regularmente. Para forzar una actualización de la caché de autenticación y garantizar que una base de datos tenga la versión más reciente de la tabla de inicios de sesión, ejecute [DBCC FLUSHAUTHCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Solo el inicio de sesión principal de nivel de servidor creado por el proceso de aprovisionamiento o una entidad de seguridad Azure Active Directory asignada como administrador puede crear o modificar reglas de Firewall de nivel de servidor. El usuario debe estar conectado a la base de datos maestra para ejecutar sp_set_firewall_rule.  
  
## <a name="examples"></a>Ejemplos  
 En el código siguiente se crea una configuración de firewall de nivel de servidor denominada `Allow Azure` que habilita el acceso desde Azure. Ejecute lo siguiente en la base de datos maestra virtual.  
  
```  
-- Enable Azure connections.  
exec sp_set_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';  
  
```  
  
 El código siguiente crea una configuración del firewall de nivel de servidor denominada `Example setting 1` solo para la dirección IP `0.0.0.2`. A continuación, `sp_set_firewall_rule` se llama de nuevo al procedimiento almacenado para actualizar la dirección IP final a `0.0.0.4` , en esa configuración de Firewall. Esto crea un intervalo que permite que las direcciones IP `0.0.0.2` , `0.0.0.3` y `0.0.0.4` tengan acceso al servidor.  
  
```  
-- Create server-level firewall setting for only IP 0.0.0.2  
exec sp_set_firewall_rule N'Example setting 1', '0.0.0.2', '0.0.0.2';  
  
-- Update server-level firewall setting to create a range of allowed IP addresses
exec sp_set_firewall_rule N'Example setting 1', '0.0.0.2', '0.0.0.4';  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Firewall de Azure SQL Database](/azure/azure-sql/database/firewall-configure)   
 [Cómo configurar los valores del firewall (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)   
 [sys.firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)