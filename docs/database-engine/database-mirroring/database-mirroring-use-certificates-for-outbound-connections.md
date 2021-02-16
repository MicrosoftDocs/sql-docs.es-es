---
title: 'Creación de reflejo de la base de datos: usar certificados en las conexiones salientes | Microsoft Docs'
description: Obtenga información sobre cómo configurar instancias del servidor para que usen certificados a fin de autenticar conexiones entrantes para la creación de reflejo de la base de datos, después de configurar conexiones de salida.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- certificates [SQL Server], database mirroring
- outbound connections [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 464c9096-10d6-4c5e-8bb1-19acba27ad9e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a53b3c3c871b62ac8824d170f71035d4359fa09f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349193"
---
# <a name="database-mirroring---use-certificates-for-outbound-connections"></a>Creación de reflejo de la base de datos: usar certificados en las conexiones salientes
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describen los pasos necesarios para configurar instancias de servidor que utilicen certificados para autenticar conexiones salientes en la creación de un reflejo de base de datos. La configuración de conexiones salientes se debe realizar antes de que se puedan configurar conexiones entrantes.  
  
> [!NOTE]  
>  Todas las conexiones de creación de reflejo en una instancia de servidor utilizan un único extremo de reflejo de la base de datos; debe especificar el método de autenticación de la instancia de servidor cuando cree el extremo.  
  
 El proceso de configuración de conexiones salientes implica los siguientes pasos generales:  
  
1.  En la base de datos **master** , cree una clave maestra de la base de datos.  
  
2.  En la base de datos **maestra** , cree un certificado cifrado en la instancia de servidor.  
  
3.  Cree un extremo para la instancia de servidor utilizando su certificado.  
  
4.  Realice una copia de seguridad del certificado en un archivo. A continuación, cópiela de forma segura a los demás sistemas.  
  
 Debe completar estos pasos para cada asociado y el testigo, si existe.  
  
 En el siguiente procedimiento se describen estos pasos detalladamente. Para cada paso, el procedimiento proporciona un ejemplo para configurar una instancia del servidor en un sistema denominado HOST_A. En la sección Ejemplo anexa se muestran los mismos pasos para otra instancia del servidor en un sistema denominado HOST_B.  
  
## <a name="procedure"></a>Procedimiento  
  
#### <a name="to-configure-server-instances-for-outbound-mirroring-connections-on-host_a"></a>Para configurar instancias del servidor para conexiones salientes de creación de reflejo (en HOST_A)  
  
1.  En la base de datos **master** , cree la clave maestra de la base de datos si no existe ninguna. Para ver las claves existentes en una base de datos, use la vista de catálogo [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md) .  
  
     Para crear la clave maestra de la base de datos, utilice el siguiente comando de [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
     Utilice una contraseña segura única y regístrela en un lugar seguro.  
  
     Para obtener más información, vea [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) y [Crear la clave maestra de una base de datos](../../relational-databases/security/encryption/create-a-database-master-key.md).  
  
2.  En la base de datos **maestra** , cree un certificado cifrado en la instancia de servidor con el fin de utilizarlo en las conexiones salientes para la creación de reflejo de la base de datos.  
  
     Por ejemplo, para crear un certificado para el sistema HOST_A.  
  
    > [!IMPORTANT]  
    >  Si piensa usar el certificado durante más de un año, especifique la fecha de vencimiento en hora UTC utilizando la opción EXPIRY_DATE en la instrucción CREATE CERTIFICATE. Además, se recomienda utilizar SQL Server Management Studio para crear una regla de administración basada en directivas que le notifique la fecha de expiración de los certificados. Mediante el cuadro de diálogo **Crear nueva condición** de Administración de directivas, cree esta regla en el campo **\@ExpirationDate** de la faceta **Certificado**. Para obtener más información, vea [Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) y [Proteger SQL Server](../../relational-databases/security/securing-sql-server.md).  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate for database mirroring',   
       EXPIRY_DATE = '11/30/2013';  
    GO  
    ```  
  
     Para obtener más información, vea [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md).  
  
     Para ver los certificados de la base de datos **maestra** , puede usar las siguientes instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    USE master;  
    SELECT * FROM sys.certificates;  
    ```  
  
     Para obtener más información, vea [sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).  
  
3.  Asegúrese de que el extremo de creación de reflejo de la base de datos existe en cada una de las instancias de servidor.  
  
     Si ya existe un extremo de creación de reflejo de la base de datos para la instancia de servidor, debe volver a utilizar dicho extremo para las demás sesiones que establezca en la instancia de servidor. Para determinar si existe un extremo de creación de reflejo de la base de datos en una instancia de servidor y ver su configuración, utilice la siguiente instrucción:  
  
    ```  
    SELECT name, role_desc, state_desc, connection_auth_desc, encryption_algorithm_desc   
       FROM sys.database_mirroring_endpoints;  
    ```  
  
     Si no existe ningún extremo, cree uno que utilice este certificado en las conexiones salientes y las credenciales del certificado en la comprobación del otro sistema. Se trata de un extremo para todo el servidor que utilizan todas las sesiones de creación de reflejo en las que participa la instancia de servidor.  
  
     Por ejemplo, para crear un extremo de creación de reflejo para la instancia del servidor de ejemplo en HOST_A.  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
       STATE = STARTED  
       AS TCP (  
          LISTENER_PORT=7024  
          , LISTENER_IP = ALL  
       )   
       FOR DATABASE_MIRRORING (   
          AUTHENTICATION = CERTIFICATE HOST_A_cert  
          , ENCRYPTION = REQUIRED ALGORITHM AES  
          , ROLE = ALL  
       );  
    GO  
    ```  
  
     Para obtener más información, vea [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
4.  Realice una copia de seguridad del certificado y cópiela en el otro sistema (o en los otros sistemas). Esto es necesario para configurar conexiones entrantes en el otro sistema.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
     Para obtener más información, vea [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
     Copie este certificado mediante el método seguro que elija. Tenga mucho cuidado de mantener todos sus certificados protegidos.  
  
 El código de ejemplo de los pasos anteriores configura conexiones salientes en HOST_A.  
  
 Ahora tiene que seguir los pasos de salida equivalentes para HOST_B. Estos pasos se ilustran en la siguiente sección Ejemplo.  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se muestra la configuración de HOST_B para conexiones salientes.  
  
```  
USE master;  
--Create the database Master Key, if needed.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
GO  
-- Make a certifcate on HOST_B server instance.  
CREATE CERTIFICATE HOST_B_cert   
   WITH SUBJECT = 'HOST_B certificate for database mirroring',   
   EXPIRY_DATE = '11/30/2013';  
GO  
--Create a mirroring endpoint for the server instance on HOST_B.  
CREATE ENDPOINT Endpoint_Mirroring  
   STATE = STARTED  
   AS TCP (  
      LISTENER_PORT=7024  
      , LISTENER_IP = ALL  
   )   
   FOR DATABASE_MIRRORING (   
      AUTHENTICATION = CERTIFICATE HOST_B_cert  
      , ENCRYPTION = REQUIRED ALGORITHM AES  
      , ROLE = ALL  
   );  
GO  
--Backup HOST_B certificate.  
BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
GO   
--Using any secure copy method, copy C:\HOST_B_cert.cer to HOST_A.  
```  
  
 Copie el certificado en el otro sistema mediante el método seguro que elija. Tenga mucho cuidado de mantener todos sus certificados protegidos.  
  
> [!IMPORTANT]  
>  Después de configurar conexiones salientes, debe configurar conexiones entrantes en cada instancia de servidor para la otra instancia (o instancias) de servidor. Para obtener más información, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 Para obtener información sobre la creación de una base de datos reflejada, incluido un ejemplo de Transact-SQL, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 Para obtener un ejemplo de Transact-SQL sobre cómo establecer una sesión en modo de alto rendimiento, vea [Ejemplo: configurar la creación de reflejo de la base de datos mediante certificados &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
## <a name="net-framework-security"></a>Seguridad de .NET Framework  
 A menos que garantice que su red es segura, se recomienda utilizar el cifrado para las conexiones de creación de reflejo de la base de datos.  
  
 Cuando copie un certificado en otro sistema, utilice un método de copia seguro.  
  
## <a name="see-also"></a>Consulte también  
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Ejemplo: configurar la creación de reflejo de la base de datos mediante certificados &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Establecer una base de datos reflejada cifrada](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
  
