---
description: DENY (permisos de esquema de Transact-SQL)
title: DENY (permisos de esquema de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [SQL Server], schemas
- schemas [SQL Server], permissions
- permissions [SQL Server], schemas
- DENY statement, schemas
ms.assetid: 300a67c4-d226-4653-9e9f-7ae4d53fcf33
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f27ddae862eb4cfc3b84db6d945818ca706e1034
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177720"
---
# <a name="deny-schema-permissions-transact-sql"></a>DENY (permisos de esquema de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Deniega permisos en un esquema.  
  

![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DENY permission  [ ,...n ] } ON SCHEMA :: schema_name  
    TO database_principal [ ,...n ]   
    [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*permission*  
Especifica un permiso que se puede denegar en un esquema. Para obtener una lista de estos permisos, vea la sección Comentarios posteriormente en este artículo.  
  
ON SCHEMA **::** schema *_name*  
Especifica el esquema en el que se deniega el permiso. El calificador de ámbito **::** es obligatorio.  
  
*database_principal*  
Especifica la entidad de seguridad de la que se va a denegar el permiso. *database_principal* puede ser una de estas entidades de seguridad:  
  
-   Usuario de la base de datos  
-   Rol de base de datos  
-   Rol de aplicación  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
-   Usuario de la base de datos asignado a un grupo de Windows  
-   Usuario de la base de datos asignado a un certificado  
-   Usuario de la base de datos asignado a una clave asimétrica  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor  
  
CASCADE  
Deniega el permiso a otras entidades de seguridad a las que se ha concedido permiso mediante *database_principal*.
  
*denying_principal*  
Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso. *denying_principal* puede ser una de estas entidades de seguridad:  
  
-   Usuario de la base de datos  
-   Rol de base de datos  
-   Rol de aplicación  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
-   Usuario de la base de datos asignado a un grupo de Windows  
-   Usuario de la base de datos asignado a un certificado  
-   Usuario de la base de datos asignado a una clave asimétrica  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor  
  
## <a name="remarks"></a>Observaciones  
Un esquema es un elemento protegible de nivel de base de datos. Se incluye en la base de datos que es su entidad primaria en la jerarquía de permisos. En la tabla siguiente se muestran los permisos más específicos y limitados que se pueden denegar en un esquema. En la tabla se muestran los permisos más generales que los incluyen por implicación.  
  
|Permiso del esquema|Implícito en el permiso del esquema|Implícito en el permiso de base de datos|  
|-----------------------|----------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SCHEMA|  
|CONTROL|CONTROL|CONTROL|  
|CREATE SEQUENCE|ALTER|ALTER ANY SCHEMA|  
|Delete|CONTROL|Delete|  
|Ejecute|CONTROL|Ejecute|  
|INSERT|CONTROL|INSERT|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|CONTROL|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
Requiere el permiso CONTROL en el esquema. Si usa la opción AS, la entidad de seguridad especificada debe ser propietaria del esquema.  
  
## <a name="see-also"></a>Consulte también  
[CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md)   
[DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
[Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
[Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
[sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
[HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  
