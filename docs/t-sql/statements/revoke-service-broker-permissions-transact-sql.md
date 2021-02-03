---
description: REVOKE (permisos de Service Broker de Transact-SQL)
title: REVOKE (permisos de Service Broker de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- routes [Service Broker], permissions
- Service Broker, permissions
- remote service bindings [Service Broker], permissions
- permissions [Service Broker]
- message types [Service Broker], permissions
- contracts [Service Broker], permissions
- services [Service Broker], permissions
- REVOKE statement, Service Broker
ms.assetid: 70f1d938-97e2-48a4-9bc0-8be9f2f2c36d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d28fe5ba46671e38089727735701107a9427ac4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194965"
---
# <a name="revoke-service-broker-permissions-transact-sql"></a>REVOKE (permisos de Service Broker de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Revoca los permisos en un contrato, tipo de mensaje, enlace de servicio remoto, ruta o servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] ON  
    {   
       [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
        }  
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 GRANT OPTION FOR  
 Indica que se revocará el derecho a conceder el derecho especificado a otras entidades de seguridad. No se revocará el permiso.  
  
> [!IMPORTANT]  
>  Si la entidad de seguridad dispone del permiso especificado sin la opción GRANT, se revocará el permiso.  
  
 *permission*  
 Especifica un permiso que se puede revocar para un elemento protegible de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Para obtener una lista de estos permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 CONTRACT **::**_contract_name_  
 Especifica el contrato en el que se va a revocar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 MESSAGE TYPE **::**_message_type_name_  
 Especifica el tipo de mensaje en el que se va a revocar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 REMOTE SERVICE BINDING **::**_remote_binding_name_  
 Especifica el enlace de servicio remoto en el que se va a revocar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 ROUTE **::**_route_name_  
 Especifica la ruta en la que se va a revocar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 SERVICE **::**_message_type_name_  
 Especifica el servicio en el que se va a revocar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 *database_principal*  
 Especifica la entidad de seguridad desde la que se revoca el permiso. *database_principal* puede ser uno de los valores siguientes:  
  
-   Usuario de la base de datos  
  
-   Rol de base de datos  
  
-   Rol de aplicación  
  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
  
-   Usuario de la base de datos asignado a un grupo de Windows  
  
-   Usuario de la base de datos asignado a un certificado  
  
-   Usuario de la base de datos asignado a una clave asimétrica  
  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor  
  
 CASCADE  
 Indica que el permiso que se va a revocar también se revocará de otras entidades de seguridad a las que esta entidad de seguridad ha concedido o denegado permisos.  
  
> [!CAUTION]  
>  Una revocación en cascada de un permiso concedido WITH GRANT OPTION revocará tanto GRANT como DENY de dicho permiso.  
  
 AS *revoking_principal*  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de revocar el permiso. *revoking_principal* puede ser uno de los valores siguientes:  
  
-   Usuario de la base de datos  
  
-   Rol de base de datos  
  
-   Rol de aplicación  
  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
  
-   Usuario de la base de datos asignado a un grupo de Windows  
  
-   Usuario de la base de datos asignado a un certificado  
  
-   Usuario de la base de datos asignado a una clave asimétrica  
  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor  
  
## <a name="remarks"></a>Comentarios  
  
## <a name="service-broker-contracts"></a>Contratos de Service Broker  
 Un contrato de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más específicos y limitados que se pueden revocar en un contrato de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de contrato de Service Broker|Implícito en el permiso de contrato de Service Broker|Implícito en el permiso de base de datos|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Tipos de mensaje de Service Broker  
 Un tipo de mensaje de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más específicos y limitados que se pueden revocar en un tipo de mensaje de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de tipo de mensaje de Service Broker|Implícito en el permiso de tipo de mensaje de Service Broker|Implícito en el permiso de base de datos|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Enlaces de servicio remoto de Service Broker  
 Un enlace de servicio remoto de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más específicos y limitados que se pueden revocar en un enlace de servicio remoto de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de enlace de servicio remoto de Service Broker|Implícito en el permiso de enlace de servicio remoto de Service Broker|Implícito en el permiso de base de datos|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Rutas de Service Broker  
 Una ruta de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más específicos y limitados que se pueden revocar en una ruta de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de ruta de Service Broker|Implícito en el permiso de ruta de Service Broker|Implícito en el permiso de base de datos|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Servicios de Service Broker  
 Un servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más específicos y limitados que se pueden revocar en un servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de servicio de Service Broker|Implícito en el permiso de servicio de Service Broker|Implícito en el permiso de base de datos|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ENVIAR|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL en el contrato, tipo de mensaje, enlace de servicio remoto, ruta o servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="see-also"></a>Vea también  
 [GRANT &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)   
 [DENY &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
