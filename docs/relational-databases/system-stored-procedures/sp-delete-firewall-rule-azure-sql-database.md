---
description: sp_delete_firewall_rule (Azure SQL Database)
title: sp_delete_firewall_rule
titleSuffix: Azure SQL Database
ms.date: 07/27/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_delete_firewall_rule_TSQL
- sp_delete_firewall_rule
- sys.sp_delete_firewall_rule
- sys.sp_delete_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_firewall_rule procedure
ms.assetid: cf93eed1-ba97-4850-9fcc-b9c5a9317908
author: VanMSFT
ms.author: vanto
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 0908e90e8f84c98a3b4b74cc5877cc11d41d6de2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195512"
---
# <a name="sp_delete_firewall_rule-azure-sql-database"></a>sp_delete_firewall_rule (Azure SQL Database)
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  Quita la configuración del firewall de nivel de servidor del servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Este procedimiento almacenado solo está disponible en la base de datos maestra para el inicio de sesión principal de nivel de servidor.  

  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_delete_firewall_rule [@name =] 'name' 
[ ; ] 
```  
  
## <a name="arguments"></a>Argumentos  
 El argumento del procedimiento almacenado es:  
  
 [ @name =] '*nombre*'  
 El nombre de la configuración del firewall de nivel de servidor que se quitará. *Name* es de tipo **nvarchar (128)** y no tiene ningún valor predeterminado.  
  
## <a name="remarks"></a>Observaciones  
 En [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los datos de inicio de sesión necesarios para autenticar una conexión y reglas de firewall de nivel de servidor se almacenan temporalmente en caché en cada base de datos. Esta caché se actualiza regularmente. Para forzar una actualización de la caché de autenticación y garantizar que una base de datos tenga la versión más reciente de la tabla de inicios de sesión, ejecute [DBCC FLUSHAUTHCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Solo el inicio de sesión principal de nivel de servidor creado por el proceso de aprovisionamiento puede eliminar reglas de firewall de nivel de servidor. El usuario debe estar conectado a la base de datos maestra para ejecutar sp_delete_firewall_rule.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se quita la configuración del firewall de nivel de servidor denominada 'Example setting 1'. Ejecute la instrucción en la base de datos maestra virtual.  
  
```   
EXEC sp_delete_firewall_rule N'Example setting 1';   
```  
  
## <a name="see-also"></a>Consulte también  
 [Firewall de Azure SQL Database](/azure/azure-sql/database/firewall-configure)   
 [Cómo configurar los valores del firewall (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)   
 [sp_set_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sys.firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)  
  
