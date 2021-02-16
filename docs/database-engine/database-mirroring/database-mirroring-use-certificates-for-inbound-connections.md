---
title: Uso de certificados para la conexión entrante
description: Describe los pasos necesarios para configurar instancias del servidor que utilicen certificados para autenticar conexiones entrantes para la creación de reflejo de la base de datos.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- inbound connections
- database mirroring [SQL Server], security
ms.assetid: 5d48bb98-61f0-4b99-8f1a-b53f831d63d0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c1ff8414ef765459bea59af5d158a0d4bdcd6cc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349220"
---
# <a name="database-mirroring---use-certificates-for-inbound-connections"></a>Creación de reflejo de base de datos: usar certificados para las conexiones entrantes
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describen los pasos necesarios para configurar instancias del servidor que utilicen certificados para autenticar conexiones entrantes para la creación de reflejo de la base de datos. Antes de poder configurar las conexiones entrantes, deberá configurar las conexiones salientes en cada una de las instancias del servidor. Para obtener más información, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 El proceso de configuración de conexiones entrantes implica los siguientes pasos generales:  
  
1.  Crear un inicio de sesión para otro sistema.  
  
2.  Cree un usuario para dicho inicio de sesión.  
  
3.  Obtenga el certificado para el extremo de reflejo de la otra instancia de servidor.  
  
4.  Asociar el certificado al usuario creado en el paso 2.  
  
5.  Conceda el permiso CONNECT para el inicio de sesión para el extremo de reflejo.  
  
 Si existe un testigo, también debe configurar conexiones de entrada para él. Esto requiere la configuración de inicios de sesión, usuarios y certificados para el testigo en los dos asociados y viceversa.  
  
 En el siguiente procedimiento se describen estos pasos detalladamente. Para cada paso, el procedimiento proporciona un ejemplo para configurar una instancia del servidor en un sistema denominado HOST_A. En la sección Ejemplo anexa se muestran los mismos pasos para otra instancia del servidor en un sistema denominado HOST_B.  
  
### <a name="to-configure-server-instances-for-inbound-mirroring-connections-on-host_a"></a>Para configurar instancias del servidor para conexiones entrantes de creación de reflejo (en HOST_A)  
  
1.  Cree un inicio de sesión para el otro sistema.  
  
     En el siguiente ejemplo se crea un inicio de sesión para el sistema HOST_B en la base de datos **master** de la instancia del servidor de HOST_A; en este ejemplo, el inicio de sesión se denomina `HOST_B_login`. Sustituya la contraseña de ejemplo por su propia contraseña.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login   
       WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
     Para obtener más información, vea [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
     Para ver los inicios de sesión en esta instancia del servidor, puede utilizar la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT * FROM sys.server_principals  
    ```  
  
     Para obtener más información, vea [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
2.  Cree un usuario para dicho inicio de sesión.  
  
     En el siguiente ejemplo se crea un usuario, `HOST_B_user`, para el inicio de sesión creado en el paso anterior.  
  
    ```  
    USE master;  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
     Para obtener más información, vea [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md).  
  
     Para ver los usuarios en esta instancia del servidor, puede utilizar la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT * FROM sys.sysusers;  
    ```  
  
     Para obtener más información, vea [sys.sysusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md).  
  
3.  Obtenga el certificado para el extremo de reflejo de la otra instancia de servidor.  
  
     Si todavía no lo ha hecho al configurar las conexiones entrantes, obtenga una copia del certificado para el extremo de creación de reflejo de la instancia de servidor. Para ello, realice una copia de seguridad del certificado en esa instancia del servidor como se describe en [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md). Cuando copie un certificado en otro sistema, utilice un método de copia seguro. Tenga mucho cuidado de mantener todos sus certificados protegidos.  
  
     Para obtener más información, vea [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
4.  Asociar el certificado al usuario creado en el paso 2.  
  
     En el siguiente ejemplo se asocia el certificado de HOST_B al usuario de HOST_A.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
     Para obtener más información, vea [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Para ver los certificados de esta instancia del servidor, utilice la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    SELECT * FROM sys.certificates  
    ```  
  
     Para obtener más información, vea [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
5.  Conceda el permiso CONNECT para el inicio de sesión al extremo de creación de reflejo remoto.  
  
     Por ejemplo, para conceder permiso para HOST_A a la instancia del servidor remoto de HOST_B para que se conecte a su inicio de sesión local (es decir, para que se conecte a `HOST_B_login`), use las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes:  
  
    ```  
    USE master;  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
     Para obtener más información, vea [GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
 Este último paso completa el proceso de configuración de la autenticación del certificado para que HOST_B pueda iniciar sesión en HOST_A.  
  
 Ahora tiene que seguir los pasos de salida de entrada equivalentes para HOST_A en HOST_B. Estos pasos se muestran en la parte entrante del ejemplo en la sección Ejemplo a continuación.  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se muestra la forma de configurar HOST_B para las conexiones entrantes.  
  
> [!NOTE]  
>  Este ejemplo usa un archivo de certificado que contiene el certificado HOST_A que se ha creado por un fragmento de código en [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
```  
USE master;  
--On HOST_B, create a login for HOST_A.  
CREATE LOGIN HOST_A_login WITH PASSWORD = 'AStrongPassword!@#';  
GO  
--Create a user, HOST_A_user, for that login.  
CREATE USER HOST_A_user FOR LOGIN HOST_A_login  
GO  
--Obtain HOST_A certificate. (See the note   
--   preceding this example.)  
--Asscociate this certificate with the user, HOST_A_user.  
CREATE CERTIFICATE HOST_A_cert  
   AUTHORIZATION HOST_A_user  
   FROM FILE = 'C:\HOST_A_cert.cer';  
GO  
--Grant CONNECT permission for the server instance on HOST_A.  
GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO HOST_A_login  
GO  
```  
  
 Si tiene planeado que la ejecución se realice en modo de seguridad alta con conmutación automática por error, debe repetir los mismos pasos de configuración para configurar el testigo de las conexiones entrantes y salientes.  
  
 Para obtener información sobre la creación de una base de datos reflejada, incluido un ejemplo de Transact-SQL, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Para ver un ejemplo de Transact-SQL del establecimiento de una sesión de modo de alto rendimiento, vea [Ejemplo: configurar la creación de reflejo de la base de datos con certificados &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>Seguridad de .NET Framework  
 Cuando copie un certificado en otro sistema, utilice un método de copia seguro. Tenga mucho cuidado de mantener todos sus certificados protegidos.  
  
## <a name="see-also"></a>Consulte también  
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [Establecer una base de datos reflejada cifrada](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
