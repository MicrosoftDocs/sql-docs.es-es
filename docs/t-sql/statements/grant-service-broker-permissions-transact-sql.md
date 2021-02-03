---
description: GRANT (permisos de Service Broker de Transact-SQL)
title: GRANT (permisos de Service Broker de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], Service Broker
- routes [Service Broker], permissions
- Service Broker, permissions
- GRANT statement, Service Broker
- remote service bindings [Service Broker], permissions
- message types [Service Broker], permissions
- contracts [Service Broker], permissions
ms.assetid: c5579976-97c4-4123-be0c-d0b98a9e38fb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a6518ccca07c6b8014d830381d4c045b67625c8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209562"
---
# <a name="grant-service-broker-permissions-transact-sql"></a>GRANT (permisos de Service Broker de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Con esta opción se conceden permisos sobre un tipo de mensaje, enlace remoto, ruta, servicio o contrato de Service Broker.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
GRANT permission  [ ,...n ] ON  
    {    
              [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
    }  
    TO database_principal [ ,...n ]   
    [ WITH GRANT OPTION ]  
        [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede conceder para un elemento protegible de Service Broker.  Se muestra a continuación.  
  
 CONTRACT **::**_contract_name_  
 Especifica el contrato para el que se concede el permiso. Es preciso usar el calificador de ámbito "::".  
  
 MESSAGE TYPE **::**_message_type_name_  
 Especifica el tipo de mensaje para el que se concede el permiso. Es preciso utilizar el calificador de ámbito "::".  
  
 REMOTE SERVICE BINDING **::**_remote_binding_name_  
 Especifica el enlace de servicio remoto para el que se concede el permiso. Es preciso utilizar el calificador de ámbito "::".  
  
 ROUTE **::**_route_name_  
 Especifica la ruta para la que se concede el permiso. Es preciso utilizar el calificador de ámbito "::".  
  
 SERVICE **::**_nombre_del_servicio_  
 Especifica el servicio para el que se concede el permiso. Es preciso utilizar el calificador de ámbito "::".  
  
 *database_principal*  
 Especifica la entidad de seguridad para la que se concede el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
  
-   rol de base de datos  
  
-   rol de aplicación  
  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
  
-   usuario de base de datos asignado a un grupo de Windows  
  
-   usuario de base de datos asignado a un certificado  
  
-   usuario de base de datos asignado a una clave asimétrica  
  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
 GRANT OPTION  
 Indica que la entidad de seguridad también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
 *granting_principal*  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de conceder el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
  
-   rol de base de datos  
  
-   rol de aplicación  
  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
  
-   usuario de base de datos asignado a un grupo de Windows  
  
-   usuario de base de datos asignado a un certificado  
  
-   usuario de base de datos asignado a una clave asimétrica  
  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="service-broker-contracts"></a>Contratos de Service Broker  
 Un contrato de Service Broker es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. A continuación, se enumeran los permisos más específicos y limitados que se pueden conceder para un contrato de Service Broker, además de los permisos más generales que los incluyen implícitamente.  
  
|Permiso de contrato de Service Broker|Implícito en el permiso de contrato de Service Broker|Implícito en el permiso de base de datos|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Tipos de mensaje de Service Broker  
 Un tipo de mensaje de Service Broker es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. A continuación, se enumeran los permisos más específicos y limitados que se pueden conceder para un tipo de mensaje de Service Broker, además de los permisos más generales que los incluyen implícitamente.  
  
|Permiso de tipo de mensaje de Service Broker|Implícito en el permiso de tipo de mensaje de Service Broker|Implícito en el permiso de base de datos|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Enlaces de servicio remoto de Service Broker  
 Un enlace de servicio remoto de Service Broker es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. A continuación se enumeran los permisos más específicos y limitados que se pueden conceder para un enlace de servicio remoto de Service Broker, además de los permisos más generales que los incluyen implícitamente.  
  
|Permiso de enlace de servicio remoto de Service Broker|Implícito en el permiso de enlace de servicio remoto de Service Broker|Implícito en el permiso de base de datos|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Rutas de Service Broker  
 Una ruta de Service Broker es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. A continuación, se enumeran los permisos más específicos y limitados que se pueden conceder para una ruta de Service Broker, además de los permisos más generales que los incluyen implícitamente.  
  
|Permiso de ruta de Service Broker|Implícito en el permiso de ruta de Service Broker|Implícito en el permiso de base de datos|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Servicios de Service Broker  
 Un servicio de Service Broker es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. A continuación, se enumeran los permisos más específicos y limitados que se pueden conceder para un servicio de Service Broker, además de los permisos más generales que los incluyen implícitamente.  
  
|Permiso de servicio de Service Broker|Implícito en el permiso de servicio de Service Broker|Implícito en el permiso de base de datos|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ENVIAR|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 El otorgante del permiso (o la entidad de seguridad especificada con la opción AS) debe tener el permiso con GRANT OPTION, o un permiso superior que implique el permiso que se va a conceder.  
  
 Si se utiliza la opción AS, debe tener en cuenta los requisitos adicionales siguientes.  
  
|AS *granting_principal*|Permiso adicional necesario|  
|------------------------------|------------------------------------|  
|Usuario de la base de datos|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un inicio de sesión de Windows|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un grupo de Windows|Pertenencia al grupo de Windows, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un certificado|Pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a una clave asimétrica|Pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos no asignado a una entidad de seguridad del servidor|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Rol de base de datos|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Rol de aplicación|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
  
 Los propietarios de objetos pueden conceder permisos para los objetos que poseen. Las entidades de seguridad con permiso CONTROL sobre un elemento protegible pueden conceder permisos para ese elemento.  
  
 Los beneficiarios del permiso CONTROL SERVER, como por ejemplo, los miembros del rol fijo de servidor **sysadmin**, pueden conceder cualquier permiso para cualquier elemento protegible en el servidor. Los beneficiarios del permiso CONTROL para una base de datos, como por ejemplo, los miembros del rol fijo de base de datos **db_owner**, pueden conceder cualquier permiso para cualquier elemento protegible en la base de datos. Los receptores del permiso CONTROL en un esquema pueden conceder los permisos en cualquier objeto del esquema.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
