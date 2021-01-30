---
description: sp_delete_database_firewall_rule (Azure SQL Database)
title: sp_delete_database_firewall_rule (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 08/04/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_delete_database_firewall_rule
- sp_delete_database_firewall_rule_TSQL
- sys.sp_delete_database_firewall_rule
- sys.sp_delete_database_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_database_firewall_rule procedure
ms.assetid: ed295312-e586-4fc2-9e80-806b490ee7bd
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.openlocfilehash: 2206974640e6a2bb1d37c4b4853c8a74e37a1ceb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195494"
---
# <a name="sp_delete_database_firewall_rule-azure-sql-database"></a>sp_delete_database_firewall_rule (Azure SQL Database)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Quita la configuración de Firewall de nivel de base de datos de su [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] . Las reglas de Firewall de base de datos se pueden configurar y eliminar para la base de datos maestra y para las bases de datos de usuario en [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] .   
  
 
## <a name="syntax"></a>Sintaxis  
  
```    
sp_delete_database_firewall_rule [@name =] [N]'name'
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos  
 `[@name =] [N]'name'`  
 El nombre de la configuración del firewall de nivel de base de datos que se quitará. *Name* es de tipo **nvarchar (128)** y no tiene ningún valor predeterminado. El identificador Unicode `N` es opcional para [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] . 
  
## <a name="permissions"></a>Permisos  
 Solo el inicio de sesión principal de nivel de servidor creado por el proceso de aprovisionamiento o una entidad de seguridad Azure Active Directory asignada como administrador puede eliminar reglas de Firewall de nivel de base de datos.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se quita la configuración de Firewall de nivel de base de datos denominada `Example DB Setting 1` .
  
```  
-- Remove database-level firewall setting  
EXECUTE sp_delete_database_firewall_rule N'Example DB Setting 1';  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Firewall de Azure SQL Database](/azure/azure-sql/database/firewall-configure)   
 [Cómo configurar los valores del firewall (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)   
 [sp_set_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sp_set_database_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)   
 [sys.database_firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
  
