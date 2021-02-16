---
title: Configuración de la creación de reflejo de la base de datos mediante la autenticación de Windows (Transact-SQL)
description: Este artículo contiene un ejemplo de cómo crear una sesión de creación de reflejos de una base de datos con un testigo mediante la autenticación de Windows con Transact-SQL en SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: 35800769-aede-4aac-b077-0e0e487e302f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 50b27862389c0977abcfe5072534043458bd5b3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342310"
---
# <a name="example-configure-database-mirroring-using-windows-authentication-transact-sql"></a>Ejemplo: Configuración de la creación de reflejo de la base de datos mediante la autenticación de Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este ejemplo se muestran todos los pasos necesarios para crear una sesión de creación de reflejo de la base de datos con un testigo mediante la autenticación de Windows. En los ejemplos descritos en este tema se utiliza [!INCLUDE[tsql](../../includes/tsql-md.md)]. Tenga en cuenta que como alternativa a los pasos que utilizan [!INCLUDE[tsql](../../includes/tsql-md.md)], puede utilizar el Asistente para la configuración de seguridad de la creación de reflejo de bases de datos para configurar la creación de reflejo de la base de datos. Para obtener más información, vea [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
## <a name="prerequisite"></a>Requisito previo  
 Este ejemplo utiliza la base de datos de ejemplo **AdventureWorks** , que usa de forma predeterminada un modelo de recuperación simple. Para utilizar la creación de reflejo de la base de datos con esta base de datos, modifíquela para que utilice el modelo de recuperación completa. Para llevarlo a cabo en [!INCLUDE[tsql](../../includes/tsql-md.md)], use la instrucción ALTER DATABASE del siguiente modo:  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks   
SET RECOVERY FULL;  
GO  
```  
  
 Para obtener más información sobre el cambio del modelo de recuperación de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vea [Ver o cambiar el modelo de recuperación de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
### <a name="permissions"></a>Permisos  
 Necesita el permiso ALTER en la base de datos y el permiso CREATE ENDPOINT, o la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="example"></a>Ejemplo  
 En este ejemplo, dos asociados y el testigo son las instancias de servidor predeterminadas en tres equipos. Las tres instancias de servidor se ejecutan en el mismo dominio de Windows, pero la cuenta de usuario (usada como cuenta de servicio de inicio) es diferente para la instancia del servidor testigo del ejemplo.  
  
 En la siguiente tabla se muestran de forma resumida los valores utilizados en este ejemplo.  
  
|Rol de creación de reflejo inicial|Sistema host|Cuenta de usuario de dominio|  
|----------------------------|-----------------|-------------------------|  
|Principal|PARTNERHOST1|*\<Mydomain>\\<dbousername\>*|  
|Reflejo|PARTNERHOST5|*\<Mydomain>\\<dbousername\>*|  
|Testigo|WITNESSHOST4|*\<Somedomain>\\<witnessuser\>*|  
  
1.  Cree un extremo en la instancia del servidor principal (instancia predeterminada en PARTNERHOST1).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=PARTNER)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    -- Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
2.  Cree un extremo en la instancia del servidor reflejado (instancia predeterminada en PARTNERHOST5).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
3.  Cree un extremo en la instancia del servidor testigo (instancia predeterminada en WITNESSHOST4).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    --Create a login for the partner server instances,  
    --which are both running as Mydomain\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [Mydomain\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
4.  Cree la base de datos reflejada. Para obtener más información, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
5.  En la instancia del servidor reflejado en PARTNERHOST5, establezca la instancia de servidor en PARTNERHOST1 como asociado (para convertirla en la instancia del servidor principal inicial).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1.COM:7022'  
    GO  
    ```  
  
6.  En la instancia del servidor principal en PARTNERHOST1, establezca la instancia de servidor en PARTNERHOST5 como asociado (para convertirla en la instancia del servidor reflejado inicial).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5.COM:7022'  
    GO  
    ```  
  
7.  En el servidor principal, establezca el testigo (que se encuentra en WITNESSHOST4).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4.COM:7022'  
    GO  
    ```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Configurar una base de datos reflejada para usar la propiedad Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
-   [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Permitir que un punto de conexión de creación de reflejo de la base de datos use certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Ejemplo: Configurar la creación de reflejo de la base de datos con certificados &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Centro de seguridad para el Motor de base de datos de SQL Server y Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
