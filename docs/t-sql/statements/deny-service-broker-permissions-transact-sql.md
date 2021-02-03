---
description: DENY (permisos de Service Broker de Transact-SQL)
title: DENY (permisos de Service Broker de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [Service Broker]
- routes [Service Broker], permissions
- Service Broker, permissions
- DENY statement, Service Broker
- remote service bindings [Service Broker], permissions
- permissions [Service Broker]
- message types [Service Broker], permissions
- denying permissions [SQL Server], Service Broker
- contracts [Service Broker], permissions
- services [Service Broker], permissions
ms.assetid: 7c6de71b-865c-41db-9413-ad9b3562e579
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1a39e74300d5a522e1ebdb9f72c0b83e21c7a9df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200384"
---
# <a name="deny-service-broker-permissions-transact-sql"></a>DENY (permisos de Service Broker de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Deniega permisos en un contrato, tipo de mensaje, enlace de servicio remoto, ruta o servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DENY permission  [ ,...n ] ON  
    {    
       [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
        }  
    TO database_principal [ ,...n ]   
    [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede denegar para un elemento protegible de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 CONTRACT **::**_contract_name_  
 Especifica el contrato en el que se va a denegar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 MESSAGE TYPE **::**_message_type_name_  
 Especifica el tipo de mensaje en el que se va a denegar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 REMOTE SERVICE BINDING **::**_remote_binding_name_  
 Especifica el enlace de servicio remoto en el que se va a denegar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 ROUTE **::**_route_name_  
 Especifica la ruta en la que se va a denegar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 SERVICE **::**_message_type_name_  
 Especifica el servicio en el que se va a denegar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 *database_principal*  
 Especifica la entidad de seguridad a la que se deniega el permiso. Uno de los siguientes:  
  
-   Usuario de la base de datos  
-   Rol de base de datos  
-   Rol de aplicación  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
-   Usuario de la base de datos asignado a un grupo de Windows  
-   Usuario de la base de datos asignado a un certificado  
-   Usuario de la base de datos asignado a una clave asimétrica  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor  
  
CASCADE  
 Indica que el permiso que se va a denegar también se denegará a otras entidades de seguridad a las que esta entidad de seguridad ha concedido permisos.  
  
*denying_principal*  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso. Uno de los siguientes:  
  
-   Usuario de la base de datos  
-   Rol de base de datos  
-   Rol de aplicación  
-   Usuario de la base de datos asignado a un inicio de sesión de Windows  
-   Usuario de la base de datos asignado a un grupo de Windows  
-   Usuario de la base de datos asignado a un certificado  
-   Usuario de la base de datos asignado a una clave asimétrica  
-   Usuario de la base de datos no asignado a una entidad de seguridad de servidor  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="service-broker-contracts"></a>Contratos de Service Broker  
 Un contrato de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. En la siguiente tabla se muestran los permisos más limitados y específicos que se pueden denegar en un contrato de [!INCLUDE[ssSB](../../includes/sssb-md.md)], junto con los permisos más generales que los incluyen por implicación.  
  
|Permiso de contrato de Service Broker|Implícito en el permiso de contrato de Service Broker|Implícito en el permiso de base de datos|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Tipos de mensaje de Service Broker  
 Un tipo de mensaje de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden denegar en un tipo de mensaje de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de tipo de mensaje de Service Broker|Implícito en el permiso de tipo de mensaje de Service Broker|Implícito en el permiso de base de datos|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Enlaces de servicio remoto de Service Broker  
 Un enlace de servicio remoto de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden denegar en un enlace de servicio remoto de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de enlace de servicio remoto de Service Broker|Implícito en el permiso de enlace de servicio remoto de Service Broker|Implícito en el permiso de base de datos|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Rutas de Service Broker  
 Una ruta de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. En la siguiente tabla se muestran los permisos más limitados y específicos que se pueden denegar en una ruta de [!INCLUDE[ssSB](../../includes/sssb-md.md)], junto con los permisos más generales que los incluyen por implicación.  
  
|Permiso de ruta de Service Broker|Implícito en el permiso de ruta de Service Broker|Implícito en el permiso de base de datos|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Servicios de Service Broker  
 Un servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)] es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. En la siguiente tabla se muestran los permisos más limitados y específicos que se pueden denegar en un servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)], junto con los permisos más generales que los incluyen por implicación.  
  
|Permiso de servicio de Service Broker|Implícito en el permiso de servicio de Service Broker|Implícito en el permiso de base de datos|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ENVIAR|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL en el contrato, tipo de mensaje, enlace de servicio remoto, ruta o servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Si se usa la cláusula AS, la entidad de seguridad especificada debe ser propietaria del elemento protegible cuyos permisos que se van a denegar.  
  
## <a name="see-also"></a>Consulte también  
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVOKE &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)  
  
  
