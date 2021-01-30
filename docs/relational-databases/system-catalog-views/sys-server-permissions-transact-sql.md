---
description: sys.server_permissions (Transact-SQL)
title: sys.server_permissions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/20/2019
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.server_permissions_TSQL
- sys.server_permissions
- server_permissions
- server_permissions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_permissions catalog view
ms.assetid: 7d78bf17-6c64-4166-bd0b-9e9e20992136
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b30e7adeb6ad09417b9ab02cb01610774d09a148
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159044"
---
# <a name="sysserver_permissions-transact-sql"></a>sys.server_permissions (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Devuelve una fila por cada permiso de nivel de servidor.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Identifica la clase de elemento sobre el que existe el permiso.<br /><br /> 100 = Servidor<br /><br /> 101 = Entidad de seguridad de servidor<br /><br /> 105 = Extremo|  
|**class_desc**|**nvarchar(60)**|Descripción de la clase en la que existe el permiso. Uno de los siguientes valores:<br /><br /> **SERVIDOR**<br /><br /> **SERVER_PRINCIPAL**<br /><br /> **FINALES**|  
|**major_id**|**int**|Id. del elemento protegible sobre el que existe el permiso, interpretado según la clase. Para la mayoría, solo es el tipo de Id. que se aplica a lo que representa la clase. La interpretación de lo que no es estándar es la siguiente:<br /><br /> 100 = siempre 0|  
|**minor_id**|**int**|Id. secundaria del elemento sobre el que existe el permiso, interpretado según la clase.|  
|**grantee_principal_id**|**int**|Id. de la entidad de seguridad de servidor a la que se conceden los permisos.|  
|**grantor_principal_id**|**int**|Id. de la entidad de seguridad de servidor del que concede esos permisos.|  
|**type**|**Char (4)**|Tipo de permiso de servidor. Para obtener una lista de los tipos de permisos, vea la tabla siguiente.|  
|**permission_name**|**nvarchar(128)**|Nombre del permiso.|  
|**state**|**char(1)**|Estado del permiso:<br /><br /> D = Denegar<br /><br /> R = Revocar<br /><br /> G = Conceder<br /><br /> W = Conceder con la opción conceder|  
|**state_desc**|**nvarchar(60)**|Descripción del estado del permiso:<br /><br /> DENEGAR<br /><br /> REVOKE<br /><br /> GRANT<br /><br /> GRANT_WITH_GRANT_OPTION|  
  
|Tipo de permiso|Nombre de permiso|Se aplica a un elemento protegible|  
|---------------------|---------------------|--------------------------|  
|AAES|ALTER ANY EVENT SESSION|SERVER|
|ADBO|ADMINISTER BULK OPERATIONS|SERVER|  
|AL|ALTER|ENDPOINT, LOGIN|  
|ALAA|ALTER ANY SERVER AUDIT|SERVER|
|ALAG|ALTER ANY AVAILABILITY GROUP|SERVER|
|ALCD|ALTER ANY CREDENTIAL|SERVER|  
|ALCO|ALTER ANY CONNECTION|SERVER|  
|ALDB|ALTER ANY DATABASE|SERVER|  
|ALES|ALTER ANY EVENT NOTIFICATION|SERVER|  
|ALHE|ALTER ANY ENDPOINT|SERVER|  
|ALLG|ALTER ANY LOGIN|SERVER|  
|ALLS|ALTER ANY LINKED SERVER|SERVER|  
|ALRS|ALTER RESOURCES|SERVER|
|ALSR|ALTER ANY SERVER ROLE|SERVER|  
|ALSS|ALTER SERVER STATE|SERVER|  
|ALST|ALTER SETTINGS|SERVER|  
|ALTR|ALTER TRACE|SERVER|  
|AUTH|AUTHENTICATE SERVER|SERVER|
|CADB|CONNECT ANY DATABASE|SERVER|  
|CL|CONTROL|ENDPOINT, LOGIN|  
|CL|CONTROL SERVER|SERVER|  
|CO|CONNECT|ENDPOINT|  
|COSQ|CONNECT SQL|SERVER|
|CRAC|Crear grupo de disponibilidad|SERVER|  
|CRDB|CREATE ANY DATABASE|SERVER|  
|CRDE|CREATE DDL EVENT NOTIFICATION|SERVER|  
|CRHE|CREATE ENDPOINT|SERVER|
|CRSR|CREATE SERVER ROLE|SERVER|  
|CRTE|CREATE TRACE EVENT NOTIFICATION|SERVER|
|IAL|IMPERSONATE ANY LOGIN|SERVER|  
|IM|IMPERSONATE|LOGIN|  
|SHDN|SHUTDOWN|SERVER|
|SUS|SELECT ALL USER SECURABLES|SERVER|
|TO|TAKE OWNERSHIP|ENDPOINT|  
|VW|VIEW DEFINITION|ENDPOINT, LOGIN|  
|VWAD|VIEW ANY DEFINITION|SERVER|  
|VWDB|VIEW ANY DATABASE|SERVER|  
|VWSS|VIEW SERVER STATE|SERVER|  
|XA|EXTERNAL ACCESS|SERVER|
|XU|UNSAFE ASSEMBLY|SERVER|
  
## <a name="permissions"></a>Permisos  
 Cualquier usuario puede ver sus propios permisos. Para ver los permisos de otros inicios de sesión, se requiere VIEW DEFINITION, ALTER ANY LOGIN o cualquier permiso en un inicio de sesión. Para ver los roles de servidor definidos por el usuario, se requiere ALTER ANY SERVER ROLE o la pertenencia al rol.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 La consulta siguiente enumera los permisos que se otorgan o deniegan específicamente a las entidades de seguridad de servidor.  
  
> [!IMPORTANT]  
> Los permisos de roles fijos de servidor no aparecen en sys.server_permissions. Por tanto, es posible que las entidades de seguridad de servidor tengan permisos adicionales que no aparezcan aquí.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pe.state_desc, pe.permission_name   
FROM sys.server_principals AS pr   
JOIN sys.server_permissions AS pe   
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
