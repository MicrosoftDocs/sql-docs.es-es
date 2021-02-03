---
description: CREATE ENDPOINT (Transact-SQL)
title: CREATE ENDPOINT (Transact-SQL)
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENDPOINT
- CREATE ENDPOINT
- ENDPOINT_TSQL
- CREATE_ENDPOINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HTTP SOAP support [SQL Server]
- CREATE ENDPOINT statement
- Availability Groups [SQL Server], configuring
- endpoints [SQL Server], creating
- SOAP [SQL Server built-in support], endpoints
- SOAP [SQL Server built-in support], sqlbatch
- DATABASE_MIRRORING option
- HTTP protocol option [SQL Server]
- SOAP [SQL Server built-in support], ad hoc
- TCP protocol option [SQL Server]
- SERVICE_BROKER option
- Availability Groups [SQL Server], endpoint
ms.assetid: 6405e7ec-0b5b-4afd-9792-1bfa5a2491f6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6f92bac093c72ccf93f4db8f8251fe3b9b8027d1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234276"
---
# <a name="create-endpoint-transact-sql"></a>CREATE ENDPOINT (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea extremos y define sus propiedades, incluidos los métodos disponibles para las aplicaciones cliente. Para obtener más información sobre permisos relacionados, vea [GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
 La sintaxis de CREATE ENDPOINT puede dividirse lógicamente en dos partes:  
  
-   La primera parte comienza con AS y termina antes de la cláusula FOR.  
  
     En esta parte, proporcione la información específica para el protocolo de transporte (TCP) y establezca un número de puerto de escucha para el extremo, así como el método de autenticación de extremo y/o una lista de direcciones IP (si hay alguna) a las que desee limitar el acceso al extremo.  
  
-   La segunda parte comienza con la cláusula FOR.  
  
     En esta parte, defina la carga que admite el extremo. La carga puede ser uno de los tipos admitidos: [!INCLUDE[tsql](../../includes/tsql-md.md)], Service Broker y creación de reflejo de la base de datos. En esta parte, también puede incluir información específica del lenguaje.  
  
> **NOTA:** Los servicios web XML nativos (puntos de conexión HTTP/SOAP) se quitaron en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE ENDPOINT endPointName [ AUTHORIZATION login ]  
[ STATE = { STARTED | STOPPED | DISABLED } ]  
AS { TCP } (  
   <protocol_specific_arguments>  
        )  
FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING } (  
   <language_specific_arguments>  
        )  
  
<AS TCP_protocol_specific_arguments> ::=  
AS TCP (  
  LISTENER_PORT = listenerPort  
  [ [ , ] LISTENER_IP = ALL | ( xx.xx.xx.xx IPv4 address ) | ( '__:__1' IPv6 address ) ]  
  
)  
  
<FOR SERVICE_BROKER_language_specific_arguments> ::=  
FOR SERVICE_BROKER (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ [ , ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
   ]  
   [ [ , ] MESSAGE_FORWARDING = { ENABLED | DISABLED } ]  
   [ [ , ] MESSAGE_FORWARD_SIZE = forward_size ]  
)  
  

<FOR DATABASE_MIRRORING_language_specific_arguments> ::=  
FOR DATABASE_MIRRORING (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
   [ [ [ , ] ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
  
    ]   
   [ , ] ROLE = { WITNESS | PARTNER | ALL }  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *endPointName*  
 Es el nombre asignado para el extremo que está creando. Se utiliza al actualizar o eliminar el extremo.  
  
 AUTHORIZATION *login*  
 Especifica un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Windows válido al que se le asigna la propiedad del objeto de extremo de nueva creación. Si no se especifica AUTHORIZATION, el autor de la llamada se convierte en el propietario del objeto de nueva creación de forma predeterminada.  
  
 Para asignar la propiedad al especificar AUTHORIZATION, el autor de la llamada debe tener el permiso IMPERSONATE en el parámetro *login* especificado.  
  
 Para volver a asignar la propiedad, vea [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 STATE **=** { STARTED | **STOPPED** | DISABLED }  
 Es el estado del extremo cuando se crea. Si el estado no se especifica cuando se crea el extremo, el valor predeterminado es STOPPED.  
  
 STARTED  
 El extremo se inicia y está activo a la escucha de conexiones.  
  
 DISABLED  
 El extremo está deshabilitado. En este estado, el servidor escucha las solicitudes del puerto pero devuelve errores a los clientes.  
  
 **STOPPED**  
 El extremo está detenido. En este estado, el servidor no escucha el puerto del extremo ni responde a ninguna solicitud que se haya intentado para usar el extremo.  
  
 Para cambiar el estado, use [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
 AS { TCP }  
 Especifica el protocolo de transporte que se va a usar.  
  
 FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING }  
 Especifica el tipo de carga.  
  
 Actualmente no hay ningún argumento específico del lenguaje [!INCLUDE[tsql](../../includes/tsql-md.md)] que se pueda pasar en el parámetro `<language_specific_arguments>`.  
  
 **Opción de protocolo TCP**  
  
 Los argumentos que aparecen a continuación solo se aplican a la opción de protocolo TCP.  
  
 LISTENER_PORT **=** _puertoDelAgenteDeEscucha_  
 Especifica el número de puerto que escucha el protocolo TCP/IP de Service Broker para las conexiones. Se usa 4022 por convención, pero cualquier número entre 1024 y 32767 es válido.  
  
 LISTENER_IP **=** ALL | **(** _4-part-ip_ **)**  |  **(** "*ip_address_v6*" **)**  
 Especifica la dirección IP en la que escuchará el extremo. El valor predeterminado es ALL. Esto significa que la escucha aceptará una conexión en cualquier dirección IP válida.  
  
 Si configura la creación de reflejo de la base de datos con una dirección IP en lugar de con un nombre de dominio completo (`ALTER DATABASE SET PARTNER = partner_IP_address` o `ALTER DATABASE SET WITNESS = witness_IP_address`), tiene que especificar `LISTENER_IP =IP_address` en lugar de `LISTENER_IP=ALL` al crear los extremos de los reflejos.  
  
 **Opciones SERVICE_BROKER y DATABASE_MIRRORING**  
  
 Los argumentos AUTHENTICATION y ENCRYPTION siguientes son comunes a las opciones SERVICE_BROKER y DATABASE_MIRRORING.  
  
> [!NOTE]  
>  Para obtener información sobre las opciones específicas de SERVICE_BROKER, vea "Opciones de SERVICE_BROKER", más adelante en esta sección. Para obtener información sobre las opciones específicas de DATABASE_MIRRORING, vea "Opciones de DATABASE_MIRRORING", más adelante en esta sección.  
  
 AUTHENTICATION **=** \<authentication_options> Especifica los requisitos de autenticación de TCP/IP para las conexiones referentes a punto de conexión. El valor predeterminado es WINDOWS.  
  
 Entre los métodos de autenticación admitidos se incluyen NTLM, Kerberos o ambos.  
  
> [!IMPORTANT]  
>  Todas las conexiones de creación de reflejos de una instancia de servidor utilizan un único extremo de creación de reflejos de base de datos. Los intentos de crear un extremo de creación de reflejos de base de datos adicional generarán un error.  
  
 **\<authentication_options> ::=**  
  
 **WINDOWS** [ { NTLM \| KERBEROS \| **NEGOTIATE** } ]  
 Especifica que el extremo se conecta mediante el protocolo de autenticación de Windows para autenticar los extremos. Este es el valor predeterminado.  
  
 Si especifica un método de autenticación (NTLM o KERBEROS), dicho método se utilizará siempre como el protocolo de autenticación. El valor predeterminado, NEGOTIATE, hace que el extremo utilice el protocolo de negociación de Windows para elegir NTLM o Kerberos.  
  
 CERTIFICATE *certificate_name*  
 Especifica que el punto de conexión va a autenticar la conexión mediante el certificado especificado por *certificate_name* para establecer la identidad para la autorización. El extremo alejado debe tener un certificado con la clave pública que coincida con la clave privada del certificado especificado.  
  
 WINDOWS [ { NTLM \| KERBEROS \| **NEGOTIATE** } ] CERTIFICATE *certificate_name*  
 Especifica que el extremo va a intentar conectarse mediante la autenticación de Windows y, si el intento da error, intentará utilizar el certificado especificado.  
  
 CERTIFICATE *certificate_name* WINDOWS [ { NTLM \| KERBEROS \| **NEGOTIATE** } ]  
 Especifica que el extremo va a intentar conectarse mediante el certificado especificado y, si el intento da error, intentará utilizar la autenticación de Windows.  
  
 ENCRYPTION = { DISABLED \| SUPPORTED \| **REQUIRED** } [ALGORITHM { **AES** \| RC4 \| AES RC4 \| RC4 AES } ]  
 Especifica si el proceso utiliza cifrado. El valor predeterminado es REQUIRED.  
  
 DISABLED  
 Especifica que los datos enviados a través de una conexión no están cifrados.  
  
 SUPPORTED  
 Especifica que los datos están cifrados solo si el extremo opuesto especifica SUPPORTED o REQUIRED.  
  
 REQUIRED  
 Especifica que las conexiones con este extremo deben utilizar el cifrado. Así, para conectarse a este extremo, otro extremo debe haber establecido ENCRYPTION en SUPPORTED o REQUIRED.  
  
 Si lo desea, puede utilizar el argumento ALGORITHM para especificar la forma de cifrado que utiliza el extremo, de la manera siguiente:  
  
 **AES**  
 Especifica que el extremo debe usar el algoritmo AES. Este es el valor predeterminado en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.  
  
 RC4  
 Especifica que el extremo debe usar el algoritmo RC4. Este es el valor predeterminado en [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
> [!NOTE]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
  
 AES RC4  
 Especifica que los dos extremos negociarán un algoritmo de cifrado con este extremo, dando preferencia al algoritmo AES.  
  
 RC4 AES  
 Especifica que los dos extremos negociarán un algoritmo de cifrado con este extremo, dando preferencia al algoritmo RC4.  
  
> [!NOTE]  
>  El algoritmo RC4 está obsoleto. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Se recomienda utilizar AES.  
  
 Si ambos extremos especifican estos dos algoritmos en distintas órdenes, el extremo que acepte la conexión gana.  
  
 **Opciones de SERVICE_BROKER**  
  
 Los argumentos que aparecen a continuación son específicos de la opción SERVICE_BROKER.  
  
 MESSAGE_FORWARDING **=** { ENABLED | **DISABLED** }  
 Determina si se reenviarán los mensajes recibidos por este extremo que sean para los servicios que se encuentran en otro lugar.  
  
 ENABLED  
 Reenvía mensajes si dispone de una dirección de reenvío.  
  
 DISABLED  
 Descarta los mensajes para los servicios que están ubicados en otro lugar. Este es el valor predeterminado.  
  
 MESSAGE_FORWARD_SIZE **=** _tamaño_del_reenvío_  
 Especifica la cantidad máxima de almacenamiento en megabytes que se va a asignar para que el extremo la utilice cuando almacene mensajes que se van a reenviar.  
  
 **Opciones de DATABASE_MIRRORING**  
  
 Los argumentos que aparecen a continuación son específicos de la opción DATABASE_MIRRORING.  
  
 ROLE **=** { WITNESS | PARTNER | ALL }  
 Especifica el rol o los roles de creación de reflejo de la base de datos que admite el extremo.  
  
 WITNESS  
 Permite al extremo realizar el rol de un testigo en el proceso de creación del reflejo.  
  
> [!NOTE]  
>  Para [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)], WITNESS es la única opción disponible.  
  
 PARTNER  
 Permite al extremo realizar el rol de un asociado en el proceso de creación del reflejo.  
  
 ALL  
 Permite al extremo desempeñar el rol de un testigo y un asociado en el proceso de creación del reflejo.  
  
 Para obtener más información sobre estos roles, vea [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!NOTE]  
>  No existe ningún puerto predeterminado para DATABASE_MIRRORING.  
  
## <a name="remarks"></a>Observaciones  
 Las instrucciones ENDPOINT DDL no pueden ejecutarse en una transacción de usuario. Las instrucciones ENDPOINT DDL no generan errores aunque una transacción activa de nivel de aislamiento de instantáneas utilice el extremo que se modifica.  
  
 Pueden ejecutar las solicitudes en un elemento ENDPOINT:  
  
-   Los miembros del rol fijo de servidor **sysadmin**  
  
-   El propietario del extremo  
  
-   Los usuarios o grupos con permiso CONNECT en el extremo  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso CREATE ENDPOINT o pertenecer al rol fijo de servidor **sysadmin** . Para obtener más información, vea [GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
## <a name="example"></a>Ejemplo  
  
### <a name="creating-a-database-mirroring-endpoint"></a>Crear un extremo de creación de reflejo de la base de datos  
 En el ejemplo siguiente se crea un extremo de creación de reflejo de la base de datos. El extremo utiliza el número de puerto `7022`, aunque cualquier número de puerto disponible sirve. El extremo se configura para utilizar la autenticación de Windows solo con Kerberos. La opción `ENCRYPTION` se configura con el valor que no es predeterminado de `SUPPORTED` para admitir datos cifrados o no cifrados. El extremo se configura para admitir los roles de asociado y testigo.  
  
```sql  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (  
       AUTHENTICATION = WINDOWS KERBEROS,  
       ENCRYPTION = SUPPORTED,  
       ROLE=ALL);  
GO  
```  

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv4-address-and-port"></a>Creación de un punto de conexión que apunta a una dirección IPv4 específica y un puerto

```sql
CREATE ENDPOINT ipv4_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = (10.0.75.1)
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public; -- Keep existing public permission on default endpoint for demo purpose
GRANT CONNECT ON ENDPOINT::ipv4_endpoint_special
TO login_name;
```

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv6-address-and-port"></a>Creación de un punto de conexión que apunta a una dirección IPv6 específica y un puerto

```sql
CREATE ENDPOINT ipv6_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = ('::1')
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public;
GRANT CONNECT ON ENDPOINT::ipv6_endpoint_special

```
  
## <a name="see-also"></a>Consulte también  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

