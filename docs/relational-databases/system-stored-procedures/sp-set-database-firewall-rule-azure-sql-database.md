---
description: sp_set_database_firewall_rule (Azure SQL Database)
title: sp_set_database_firewall_rule
titleSuffix: Azure SQL Database
ms.date: 08/04/2017
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_set_database_firewall_rule
- sp_set_database_firewall_rule_TSQL
- sys.sp_set_database_firewall_rule
- sys.sp_set_database_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_database_firewall_rule
- firewall_rules, setting database rules
ms.assetid: 8f0506b6-a4ac-4e4d-91db-8077c40cb17a
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 5aca09db0b26d15826547c3fb9847614ad5aa722
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184748"
---
# <a name="sp_set_database_firewall_rule-azure-sql-database"></a>sp_set_database_firewall_rule (Azure SQL Database)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Crea o actualiza las reglas de Firewall de nivel de base de datos para [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] . Las reglas de Firewall de base de datos se pueden configurar para la base de datos **maestra** y para las bases de datos de usuario en [!INCLUDE[ssSDS](../../includes/sssds-md.md)] . Las reglas de Firewall de base de datos son especialmente útiles cuando se usan usuarios de bases de datos independientes. Para obtener más información, vea [Usuarios de base de datos independiente: hacer que la base de datos sea portátil](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_set_database_firewall_rule [@name = ] [N]'name'  
, [@start_ip_address =] 'start_ip_address'  
, [@end_ip_address =] 'end_ip_address'
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] [N]'name'` El nombre que se usa para describir y distinguir la configuración de Firewall de nivel de base de datos. *Name* es de tipo **nvarchar (128)** y no tiene ningún valor predeterminado. El identificador Unicode `N` es opcional para [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] . 
  
`[ @start_ip_address = ] 'start_ip_address'` La dirección IP más baja en el intervalo de la configuración de Firewall de nivel de base de datos. Las direcciones IP iguales o superiores a esta pueden intentar conectarse con la instancia de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. La dirección IP más baja posible es `0.0.0.0`. *start_ip_address* es de tipo **VARCHAR (50)** y no tiene ningún valor predeterminado.  
  
`[ @end_ip_address = ] 'end_ip_address'` La dirección IP más alta en el intervalo de la configuración de Firewall de nivel de base de datos. Las direcciones IP iguales o inferiores a esta pueden intentar conectarse con la instancia de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. La dirección IP más alta posible es `255.255.255.255`. *end_ip_address* es de tipo **VARCHAR (50)** y no tiene ningún valor predeterminado.  
  
 En la tabla siguiente se muestran los argumentos y las opciones admitidos en [!INCLUDE[ssSDS](../../includes/sssds-md.md)] .  
  
> [!NOTE]  
>  Se permiten los intentos de conexión de Azure cuando este campo y el campo *start_ip_address* es igual a `0.0.0.0` .  
  
## <a name="remarks"></a>Observaciones  
 Los nombres de la configuración del firewall de nivel de base de datos para una base de datos deben ser únicos. Si el nombre de la configuración del firewall de nivel de base de datos proporcionado para el procedimiento almacenado ya existe en la tabla de configuración del firewall de nivel de base de datos, las direcciones IP inicial y final se actualizarán. De lo contrario, se creará una nueva configuración del firewall de nivel de base de datos.  
  
 Cuando se agrega una configuración de Firewall de nivel de base de datos donde las direcciones IP inicial y final son iguales a `0.0.0.0` , se habilita el acceso a la base de datos en el [!INCLUDE[ssSDS](../../includes/sssds-md.md)] servidor desde cualquier recurso de Azure. Proporcione un valor al parámetro *Name* que le ayude a recordar para qué sirve la configuración de Firewall.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso **CONTROL** en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el código siguiente se crea una configuración de firewall de nivel de base de datos denominada `Allow Azure` que habilita el acceso a la base de datos desde Azure.  
  
```  
-- Enable Azure connections.  
EXECUTE sp_set_database_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';  
  
```  
  
 El código siguiente crea una configuración del firewall de nivel de base de datos denominada `Example DB Setting 1` solo para la dirección IP `0.0.0.4`. A continuación, `sp_set_database firewall_rule` se llama de nuevo al procedimiento almacenado para actualizar la dirección IP final a `0.0.0.6` , en esa configuración de Firewall. Esto crea un intervalo que permite que las direcciones IP `0.0.0.4` , `0.0.0.5` y `0.0.0.6` tengan acceso a la base de datos.
  
```  
-- Create database-level firewall setting for only IP 0.0.0.4  
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.4';  
  
-- Update database-level firewall setting to create a range of allowed IP addresses
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.6';  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Firewall de Azure SQL Database](/azure/azure-sql/database/firewall-configure)   
 [Cómo configurar los valores del firewall (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)   
 [sp_set_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sp_delete_database_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)   
 [sys.database_firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
  
