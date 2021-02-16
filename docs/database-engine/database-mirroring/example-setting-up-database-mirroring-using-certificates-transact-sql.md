---
title: 'Ejemplo: Creación de reflejo de base de datos con certificados (T-SQL)'
description: Este artículo contiene un ejemplo de configuración de la creación de reflejo de la base de datos de SQL Server con autenticación basada en certificados mediante Transact-SQL en SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- certificates [SQL Server], database mirroring
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: df489ecd-deee-465c-a26a-6d1bef6d7b66
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 12459a76f612ba2e9acc915acf5189792512b2ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342347"
---
# <a name="example-setting-up-database-mirroring-using-certificates-transact-sql"></a>Ejemplo: configurar la creación de reflejo de la base de datos con certificados (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este ejemplo se muestran todos los pasos necesarios para crear una sesión de creación de reflejo de la base de datos mediante la autenticación basada en certificados. En los ejemplos descritos en este tema se utiliza [!INCLUDE[tsql](../../includes/tsql-md.md)]. A menos que garantice que su red es segura, se recomienda utilizar el cifrado para las conexiones de creación de reflejo de la base de datos.  
  
 Cuando copie un certificado en otro sistema, utilice un método de copia seguro. Tenga mucho cuidado de mantener todos sus certificados protegidos.  
  
##  <a name="example"></a><a name="ExampleH2"></a> Ejemplo  
 En el ejemplo siguiente se muestra lo que se debe hacer en un asociado que reside en el HOST_A. En este ejemplo, los dos asociados son las instancias de servidor predeterminadas en tres equipos. Las dos instancias de servidor se ejecutan en dominios de Windows que no son de confianza, por lo que se requiere la autenticación basada en certificados.  
  
 HOST_A adopta el rol principal inicial y HOST_B adopta el rol reflejado.  
  
 Configurar la creación de reflejo de la base de datos mediante certificados implica cuatro fases generales. En este ejemplo se muestran tres de ellas: 1, 2 y 4. Estas fases son las siguientes:  
  
1.  [Configurar conexiones salientes](#ConfiguringOutboundConnections)  
  
     En este ejemplo muestran los pasos para:  
  
    1.  Configurar Host_A para conexiones salientes.  
  
    2.  Configurar Host_B para conexiones salientes.  
  
     Para obtener más información sobre esta fase de configuración de la creación de reflejo de la base de datos, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  [Configurar conexiones entrantes](#ConfigureInboundConnections)  
  
     En este ejemplo muestran los pasos para:  
  
    1.  Configurar Host_A para conexiones entrantes.  
  
    2.  Configurar Host_B para conexiones entrantes.  
  
     Para obtener más información sobre esta fase de configuración de la creación de reflejo de la base de datos, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
3.  Crear la base de datos reflejada  
  
     Para obtener más información sobre cómo crear una base de datos reflejada, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
4.  [Configurar los asociados de creación de reflejo](#ConfigureMirroringPartners)  
  
###  <a name="configuring-outbound-connections"></a><a name="ConfiguringOutboundConnections"></a> Configurar conexiones salientes  
 **Para configurar Host_A para conexiones salientes**  
  
1.  En la base de datos maestra, cree la clave maestra de la base de datos si es necesaria.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<1_Strong_Password!>';  
    GO  
    ```  
  
2.  Cree un certificado para esta instancia de servidor.  
  
    ```  
    USE master;  
    CREATE CERTIFICATE HOST_A_cert   
       WITH SUBJECT = 'HOST_A certificate';  
    GO  
    ```  
  
3.  Cree un extremo de creación de reflejo para la instancia de servidor mediante el certificado.  
  
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
  
4.  Realice una copia de seguridad del certificado HOST_A y cópielo en el otro sistema, HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_A_cert TO FILE = 'C:\HOST_A_cert.cer';  
    GO  
    ```  
  
5.  Utilizando cualquier método de copia seguro, copie C:\HOST_A_cert.cer en HOST_B.  
  
 **Para configurar Host_B para conexiones salientes**  
  
1.  En la base de datos maestra, cree la clave maestra de la base de datos si es necesaria.  
  
    ```  
    USE master;  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Strong_Password_#2>';  
    GO  
    ```  
  
2.  Cree un certificado en la instancia de servidor HOST_B.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert   
       WITH SUBJECT = 'HOST_B certificate for database mirroring';  
    GO  
    ```  
  
3.  Cree un extremo de creación de reflejo para la instancia de servidor en HOST_B.  
  
    ```  
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
    ```  
  
4.  Realice una copia de seguridad del certificado de HOST_B.  
  
    ```  
    BACKUP CERTIFICATE HOST_B_cert TO FILE = 'C:\HOST_B_cert.cer';  
    GO   
    ```  
  
5.  Utilizando cualquier método de copia seguro, copie C:\HOST_B_cert.cer en HOST_A.  
  
 Para obtener más información, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
 [&#91;Inicio del ejemplo&#93;](#ExampleH2)  
  
###  <a name="configuring-inbound-connections"></a><a name="ConfigureInboundConnections"></a> Configurar conexiones entrantes  
 **Para configurar Host_A para conexiones entrantes**  
  
1.  Cree un inicio de sesión en HOST_A para HOST_B.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_B_login WITH PASSWORD = '1Sample_Strong_Password!@#';  
    GO  
    ```  
  
2.  Cree un usuario para dicho inicio de sesión.  
  
    ```  
    CREATE USER HOST_B_user FOR LOGIN HOST_B_login;  
    GO  
    ```  
  
3.  Asocie el certificado al usuario.  
  
    ```  
    CREATE CERTIFICATE HOST_B_cert  
       AUTHORIZATION HOST_B_user  
       FROM FILE = 'C:\HOST_B_cert.cer'  
    GO  
    ```  
  
4.  Conceda el permiso CONNECT para el inicio de sesión al extremo de creación de reflejo remoto.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_B_login];  
    GO  
    ```  
  
 **Para configurar Host_B para conexiones entrantes**  
  
1.  Cree un inicio de sesión en HOST_B para HOST_A.  
  
    ```  
    USE master;  
    CREATE LOGIN HOST_A_login WITH PASSWORD = '=Sample#2_Strong_Password2';  
    GO  
    ```  
  
2.  Cree un usuario para dicho inicio de sesión.  
  
    ```  
    CREATE USER HOST_A_user FOR LOGIN HOST_A_login;  
    GO  
    ```  
  
3.  Asocie el certificado al usuario.  
  
    ```  
    CREATE CERTIFICATE HOST_A_cert  
       AUTHORIZATION HOST_A_user  
       FROM FILE = 'C:\HOST_A_cert.cer'  
    GO  
    ```  
  
4.  Conceda el permiso CONNECT para el inicio de sesión al extremo de creación de reflejo remoto.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [HOST_A_login];  
    GO  
    ```  
  
> [!IMPORTANT]  
>  Si tiene planeado que la ejecución se realice en modo de alta seguridad con conmutación automática por error, debe repetir los mismos pasos de configuración para configurar el testigo de las conexiones entrantes y salientes. La configuración de conexiones de entrantes cuando un testigo está implicado requiere configurar inicios de sesión y usuarios para los testigos de los asociados y de los asociados del testigo.  
  
 Para obtener más información, vea [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md).  
  
 [&#91;Inicio del ejemplo&#93;](#ExampleH2)  
  
### <a name="creating-the-mirror-database"></a>Crear la base de datos reflejada  
 Para obtener más información sobre cómo crear una base de datos reflejada, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
###  <a name="configuring-the-mirroring-partners"></a><a name="ConfigureMirroringPartners"></a> Configurar los asociados de creación de reflejo  
  
1.  En la instancia del servidor reflejado en HOST_B, establezca la instancia de servidor en HOST_A como asociado (para convertirla en la instancia inicial del servidor principal): Sustituya una dirección de red válida por `TCP://HOST_A.Mydomain.Corp.Adventure-Works``.com:7024`. Para obtener más información, vea [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
    ```  
    --At HOST_B, set server instance on HOST_A as partner (principal server):  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_A.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
2.  En la instancia del servidor principal en HOST_A, establezca la instancia de servidor en HOST_B como asociado (para convertirla en la instancia inicial del servidor reflejado): Sustituya una dirección de red válida por `TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024`.  
  
    ```  
    --At HOST_A, set server instance on HOST_B as partner (mirror server).  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://HOST_B.Mydomain.Corp.Adventure-Works.com:7024';  
    GO  
    ```  
  
3.  En este ejemplo se considera que la sesión se ejecutará en modo de alto rendimiento. Para configurar esta sesión para el modo de alto rendimiento, en la instancia del servidor principal (en HOST_A), establezca la seguridad de la transacción en OFF.  
  
    ```  
    --Change to high-performance mode by turning off transacton safety.  
    ALTER DATABASE AdventureWorks   
        SET PARTNER SAFETY OFF  
    GO  
    ```  
  
    > [!NOTE]  
    >  Si piensa realizar la ejecución en modo de alta seguridad con conmutación automática por error, deje la seguridad de la transacción en FULL (configuración predeterminada) y agregue el testigo en cuanto sea posible después de ejecutar la segunda instrucción **'** _partner_server_ **'** . Tenga en cuenta que primero se debe configurar el testigo para conexiones salientes y entrantes.  
  
 [&#91;Inicio del ejemplo&#93;](#ExampleH2)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Permitir que un punto de conexión de creación de reflejo de la base de datos use certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Administración de inicios de sesión y trabajos tras la conmutación de roles &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
-   [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
-   [Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Centro de seguridad para el Motor de base de datos de SQL Server y Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
