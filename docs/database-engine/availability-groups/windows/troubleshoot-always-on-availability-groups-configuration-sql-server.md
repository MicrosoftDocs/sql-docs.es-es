---
title: Problemas comunes y soluciones con grupos de disponibilidad
description: Solucione los problemas más habituales relacionados con la configuración de las instancias de servidor para grupos de disponibilidad Always On en SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], deploying
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], configuring
ms.assetid: 8c222f98-7392-4faf-b7ad-5fb60ffa237e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7b7c6a5df1bcef7e8438c01910233c2c3a38d070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337933"
---
# <a name="troubleshoot-always-on-availability-groups-configuration-sql-server"></a>Solucionar problemas de configuración de grupos de disponibilidad AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  En este tema se proporciona información para ayudarle a solucionar los problemas más habituales relacionados con la configuración de las instancias de servidor para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Entre los problemas de configuración más habituales se incluyen los siguientes: [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] está deshabilitado, las cuentas no están configuradas correctamente, el extremo de creación de reflejo de la base de datos no existe, el extremo no es accesible (error 1418 de SQL Server), el acceso de red no existe y un comando de unión genera el error 35250 de SQL Server.  
  
> [!NOTE]  
>  Asegúrese de que cumple los requisitos previos de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Para obtener más información, vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 **En este tema:**  
  
|Sección|Descripción|  
|-------------|-----------------|  
|[Los grupos de disponibilidad AlwaysOn no están habilitados](#IsHadrEnabled)|Si una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no está habilitada para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], la instancia no admite la creación de grupos de disponibilidad y no puede hospedar réplicas de disponibilidad.|  
|[Cuentas](#Accounts)|Analiza los requisitos para configurar correctamente las cuentas en que se ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[Extremos](#Endpoints)|Analiza cómo diagnosticar problemas relativos al extremo de creación de reflejo de la base de datos de una instancia de servidor.|  
|[Nombre del sistema](#SystemName)|Resume las alternativas para especificar el nombre del sistema de una instancia de servidor en una dirección URL del extremo.|  
|[Acceso de red](#NetworkAccess)|Documenta el requisito de que cada instancia de servidor que hospeda una réplica de disponibilidad debe tener acceso al puerto de cada una de las demás instancias de servidor en TCP.|  
|[Acceso al extremo (error 1418 de SQL Server)](#Msg1418)|Contiene información sobre este mensaje de error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[Error de unión de la base de datos (error 35250 de SQL Server)](#JoinDbFails)|Analiza las posibles causas y la resolución de un error al unir las bases de datos secundarias a un grupo de disponibilidad porque la conexión a la réplica principal no está activa.|  
|[El enrutamiento de solo lectura no funciona correctamente](#ROR)||  
|[Tareas relacionadas](#RelatedTasks)|Contiene una lista de temas orientados a tareas de los Libros en pantalla de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] que son particularmente especialmente pertinentes para solucionar problemas de configuración de un grupo de disponibilidad.|  
|[Contenido relacionado](#RelatedContent)|Contiene una lista de recursos importantes externos a los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
  
##  <a name="always-on-availability-groups-is-not-enabled"></a><a name="IsHadrEnabled"></a> Los grupos de disponibilidad AlwaysOn no están habilitados  
 La característica [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] debe estar habilitada en cada instancia de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Para obtener más información, vea [Habilitar y deshabilitar grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
##  <a name="accounts"></a><a name="Accounts"></a> Cuentas  
 Las cuentas en las que se ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deben estar configuradas correctamente.  
  
1.  ¿Tienen las cuentas los permisos adecuados?  
  
    1.  Si los asociados se ejecutan como la misma cuenta de usuario de dominio, automáticamente existen los inicios de sesión de usuario correctos en ambas bases de datos **maestra** . Esto simplifica la configuración de seguridad de la base de datos y es recomendable su aplicación.  
  
    2.  Si dos instancias del servidor se ejecutan como cuentas diferentes, el inicio de sesión en cada cuenta debe crearse en la base de datos **maestra** en la instancia del servidor remoto, y se deben conceder a ese inicio de sesión permisos CONNECT para conectarse al extremo de creación de reflejo de la base de datos de esa instancia del servidor. Para obtener más información, vea [Configurar cuentas de inicio de sesión para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
2.  Si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se ejecuta como una cuenta integrada, (como sistema local, servicio local o servicio de red), o una cuenta que no es de dominio, debe utilizar certificados para la autenticación de extremos. Si las cuentas de servicio utilizan cuentas de dominio en el mismo dominio, puede elegir conceder acceso CONNECT para cada cuenta de servicio en todas las ubicaciones de réplica o puede utilizar certificados. Para obtener más información, vea [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 Los extremos deben estar configurados correctamente.  
  
1.  Asegúrese de que cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vaya a hospedar una réplica de disponibilidad (cada *ubicación de réplica*) tenga un punto de conexión de creación de reflejo de la base de datos. Para determinar si existe un punto de conexión de creación de reflejo de la base de datos en una instancia de servidor determinada, use la vista de catálogo [sys.database_mirroring_endpoints](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md). Para obtener más información, vea [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) o [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md).  
  
2.  Compruebe que los números de puerto son correctos.  
  
     Para identificar el puerto asociado actualmente al extremo de creación de reflejo de la base de datos de una instancia de servidor, utilice la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] :  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints;  
    GO  
    ```  
  
3.  En caso de problemas de configuración de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] difíciles de explicar, se recomienda que compruebe cada una de las instancias de servidor para determinar si escuchan en los puertos correctos.  
  
4.  Asegúrese de que se han iniciado los extremos (STATE=STARTED). En cada una de las instancias de servidor, utilice la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)]:  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Para obtener más información sobre la columna **state_desc**, vea [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Para iniciar un extremo, utilice la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)]:  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Para obtener más información, vea [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Asegúrese de que el inicio de sesión del otro servidor dispone de permiso CONNECT. Para determinar quién tiene permiso CONNECT para un extremo, utilice la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] en cada instancia de servidor:  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-name"></a><a name="SystemName"></a> System Name  
 Como nombre del sistema de una instancia de servidor en una dirección URL del extremo, se puede utilizar cualquier nombre que identifique el sistema de forma inequívoca. La dirección del sistema puede ser un nombre del sistema (si los sistemas se encuentran en el mismo dominio), un nombre de dominio completo o una dirección IP (de preferencia, una dirección IP estática). Se garantiza que el uso de un nombre de dominio completo funciona correctamente. Para obtener más información, vea [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Cada instancia de servidor que hospeda una réplica de disponibilidad debe tener acceso al puerto de cada una de las demás instancias de servidor en TCP. Esto es especialmente importante si las instancias de servidor están en distintos dominios que no confían unos en otros (dominios que no son de confianza).  
  
##  <a name="endpoint-access-sql-server-error-1418"></a><a name="Msg1418"></a> Acceso al extremo (error 1418 de SQL Server)  
 Este mensaje de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] indica que la dirección de red del servidor especificada en la dirección URL del extremo no se encuentra o no existe, y recomienda que se compruebe el nombre de dirección de red y se vuelva a emitir el comando.  
  
##  <a name="join-database-fails-sql-server-error-35250"></a><a name="JoinDbFails"></a> Error de unión de la base de datos (error 35250 de SQL Server)  
 En esta sección se analiza las posibles causas y la resolución de un error al unir las bases de datos secundarias al grupo de disponibilidad porque la conexión a la réplica principal no está activa.  
  
 **Resolución:**  
  
1.  Compruebe la configuración de firewall para ver si permite la comunicación de puerto del extremo entre las instancias de servidor que hospedan la réplica principal y la réplica secundaria (puerto 5022 de forma predeterminada).  
  
2.  Compruebe si la cuenta de servicio de red tiene permiso de conexión para el extremo.  
  
##  <a name="read-only-routing-is-not-working-correctly"></a><a name="ROR"></a> El enrutamiento de solo lectura no funciona correctamente  
 Compruebe los siguientes valores de configuración y corríjalos si es necesario.  
  
|En...|Acción|Comentarios|Vínculo|  
|---------|------------|--------------|----------|  
|Réplica principal actual|Asegúrese de que el agente de escucha del grupo de disponibilidad está en línea.|**Para comprobar si el agente de escucha está en línea:**<br /><br /> `SELECT * FROM sys.dm_tcp_listener_states;`<br /><br /> **Para reiniciar un agente de escucha sin conexión:**<br /><br /> `ALTER AVAILABILITY GROUP myAG RESTART LISTENER 'myAG_Listener';`|[sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Réplica principal actual|Asegúrese de que READ_ONLY_ROUTING_LIST contiene solo instancias de servidor que hospedan una réplica secundaria legible.|**Para identificar réplicas secundarias legibles:** sys.availability_replicas (columna **secondary_role_allow_connections_desc** )<br /><br /> **Para ver una lista de enrutamiento de solo lectura:** sys.availability_read_only_routing_lists<br /><br /> **Para cambiar una lista de enrutamiento de solo lectura:** ALTER AVAILABILITY GROUP|[sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Todas las réplicas de read_only_routing_list|Asegúrese de que el firewall de Windows no está bloqueando el puerto de READ_ONLY_ROUTING_URL.|-|[Configuración de Firewall de Windows para el acceso al motor de base de datos](../../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|Todas las réplicas de read_only_routing_list|En el Administrador de configuración [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , compruebe lo siguiente:<br /><br /> La conectividad remota de SQL Server está habilitada.<br /><br /> TCP/IP está habilitado.<br /><br /> Las direcciones IP están configuradas correctamente.|-|[Ver o cambiar las propiedades del servidor &#40;SQL Server&#41;](../../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)<br /><br /> [Configurar un servidor para que escuche en un puerto TCP específico &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)|  
|Todas las réplicas de read_only_routing_list|Asegúrese de que READ_ONLY_ROUTING_URL (TCP <strong>://</strong>*system-address*<strong>:</strong>*port*) contiene el nombre de dominio completo (FQDN) y el número de puerto correctos.|-|[Calcular Read_only_routing_url para AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)<br /><br /> [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|Sistema cliente|Compruebe que el controlador cliente admite el enrutamiento de solo lectura.|-|[Conectividad de cliente de AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Solucionar problemas relativos a una operación de agregar archivos con error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
-   [Administración de inicios de sesión y de trabajos para las bases de datos de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
-   [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [View Events and Logs for a Failover Cluster](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Cmdlet de clúster de conmutación por error Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
-   [Blog del equipo Always On de SQL Server: el blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Consulte también  
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Configuración de red de cliente](../../../database-engine/configure-windows/client-network-configuration.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
