---
description: CREATE REMOTE SERVICE BINDING (Transact-SQL)
title: CREATE REMOTE SERVICE BINDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE REMOTE SERVICE BINDING
- SERVICE_BINDING_TSQL
- CREATE REMOTE SERVICE
- REMOTE_TSQL
- CREATE_REMOTE_SERVICE_BINDING_TSQL
- CREATE_REMOTE_SERVICE_TSQL
- BINDING
- SERVICE BINDING
- BINDING_TSQL
- CREATE_REMOTE_TSQL
- REMOTE_SERVICE_TSQL
- CREATE REMOTE
- REMOTE SERVICE
- REMOTE_SERVICE_BINDING_TSQL
- REMOTE
- REMOTE SERVICE BINDING
dev_langs:
- TSQL
helpviewer_keywords:
- binding remote service [Service Broker]
- dialog security [Service Broker]
- end-to-end security [Service Broker]
- security [Service Broker], remote service bindings
- CREATE REMOTE SERVICE BINDING statement
- conversation security [Service Broker]
- remote service bindings [Service Broker], creating
ms.assetid: 4165c404-4d50-4063-9a6e-6e267d309376
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13abf5a16b800bc856d482fbc525a833b9d7e872
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188521"
---
# <a name="create-remote-service-binding-transact-sql"></a>CREATE REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un enlace que define las credenciales de seguridad que se utilizarán para iniciar una conversación con un servicio remoto.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE REMOTE SERVICE BINDING binding_name   
   [ AUTHORIZATION owner_name ]   
   TO SERVICE 'service_name'   
   WITH  USER = user_name [ , ANONYMOUS = { ON | OFF } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *binding_name*  
 Es el nombre del enlace de servicio remoto que se va a crear. No se pueden especificar nombres de servidor, base de datos o esquema. *binding_name* debe ser un **sysname** válido.  
  
 AUTHORIZATION *owner_name*  
 Establece el propietario del enlace en el usuario o el rol de base de datos que se ha especificado. Cuando el usuario actual es **dbo** o **sa**, *owner_name* puede ser el nombre de cualquier usuario o rol válidos. En caso contrario, *owner_name* debe ser el nombre del usuario actual, el nombre de un usuario para el que el usuario actual tiene permisos IMPERSONATE o el nombre de un rol al que pertenece el usuario actual.  
  
 TO SERVICE '*service_name*'  
 Especifica el servicio remoto que se enlazará al usuario identificado en la cláusula WITH USER.  
  
 USER = *user_name*  
 Especifica la entidad de seguridad de base de datos que posee el certificado asociado con el servicio remoto identificado por la cláusula TO SERVICE. Este certificado se utiliza para el cifrado y la autenticación de los mensajes que se intercambian con el servicio remoto.  
  
 ANONYMOUS  
 Especifica si se va a utilizar la autenticación anónima en la comunicación con el servicio remoto. Si ANONYMOUS = ON, se usa la autenticación anónima y las operaciones en la base de datos remota se realizan como miembro del rol fijo de base de datos **public**. Si ANONYMOUS = OFF, las operaciones en la base de datos remota se realizan como un usuario específico de esa base de datos. Si no se especifica esta cláusula, el valor predeterminado es OFF.  
  
## <a name="remarks"></a>Observaciones  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] usa un enlace de servicio remoto para encontrar el certificado que se utilizará en una nueva conversación. La clave pública del certificado asociado a *user_name* se usa para autenticar mensajes enviados al servicio remoto y para cifrar una clave de sesión que se usará después para cifrar la conversación. El certificado de *user_name* debe corresponder al certificado de un usuario de la base de datos que hospede el servicio remoto.  
  
 Un enlace de servicio remoto solo es necesario para servicios iniciadores que se comunican con servicios de destino que están fuera de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Una base de datos que hospeda un servicio iniciador debe contener enlaces de servicio remoto para todos los servicios de destino que están fuera de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La base de datos que hospeda un servicio de destino no necesita contener enlaces de servicio remoto para los servicios iniciadores que se comunican con el servicio de destino. Cuando los servicios iniciador y de destino están en la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], no es necesario ningún enlace de servicio remoto. No obstante, si hay un enlace de servicio remoto cuyo *service_name* especificado para TO SERVICE coincide con el nombre del servicio local, [!INCLUDE[ssSB](../../includes/sssb-md.md)] usará el enlace.  
  
 Cuando ANONYMOUS = ON, el servicio iniciador se conecta al servicio de destino como miembro del rol fijo de base de datos **public**. De manera predeterminada, los miembros de este rol no tienen permiso para conectarse a una base de datos. Para que los mensajes se envíen correctamente, la base de datos de destino debe conceder al rol **public** el permiso CONNECT en la base de datos y el permiso SEND en el servicio de destino.  
  
 Cuando un usuario posee más de un certificado, [!INCLUDE[ssSB](../../includes/sssb-md.md)] selecciona, entre todos los certificados válidos y marcados como AVAILABLE FOR BEGIN_DIALOG, el que tenga la fecha de vencimiento más tardía.  
  
## <a name="permissions"></a>Permisos  
 Los permisos para crear un enlace de servicio remoto pertenecen de forma predeterminada al usuario mencionado en la cláusula USER, a los miembros del rol fijo de base de datos **db_owner**, a los miembros del rol fijo de base de datos **db_ddladmin** y a los miembros del rol fijo de servidor **sysadmin**.  
  
 El usuario que ejecuta la instrucción CREATE REMOTE SERVICE BINDING debe tener permiso de suplantación para la entidad de seguridad especificada en la instrucción.  
  
 Un enlace de servicio remoto no puede ser un objeto temporal. Se permiten los nombres de enlace de servicio remoto que empiecen por **#** , aunque se trata de objetos permanentes.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-remote-service-binding"></a>A. Crear un enlace de servicio remoto  
 En el ejemplo siguiente se crea un enlace para el servicio `//Adventure-Works.com/services/AccountsPayable`. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utiliza el certificado de la entidad de seguridad de base de datos `APUser` para autenticarse en el servicio remoto e intercambiar la clave de cifrado de sesión con el servicio remoto.  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser ;  
```  
  
### <a name="b-creating-a-remote-service-binding-using-anonymous-authentication"></a>B. Crear un enlace de servicio remoto con autenticación anónima  
 En el ejemplo siguiente se crea un enlace para el servicio `//Adventure-Works.com/services/AccountsPayable`. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utiliza el certificado de la entidad de seguridad de base de datos `APUser` para intercambiar la clave de cifrado de sesión con el servicio remoto. Service Broker no se autentica en el servicio remoto. En la base de datos que hospeda el servicio remoto, los mensajes se entregan como usuario **guest** (invitado).  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser, ANONYMOUS=ON ;  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
