---
title: Replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad
description: Obtenga información sobre la interoperabilidad de la replicación, el seguimiento de cambios y la captura de datos modificados al usarlos con grupos de disponibilidad AlwaysOn de SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server], AlwaysOn Availability Groups
- change data capture [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: e17a9ca9-dd96-4f84-a85d-60f590da96ad
author: cawrites
ms.author: chadam
ms.openlocfilehash: f82db97e9b818ecca6682cf6778850dab8a238c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344549"
---
# <a name="replication-change-tracking--change-data-capture---always-on-availability-groups"></a>Replicación, seguimiento de cambios y captura de datos modificados - Grupos de disponibilidad AlwaysOn
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] En [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]se admiten la replicación, la captura de datos modificados (CDC) y el seguimiento de cambios (CT). [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] ayuda a proporcionar alta disponibilidad y capacidades adicionales de recuperación de base de datos.  
  
##  <a name="overview-of-replication-with-availability-groups"></a><a name="Overview"></a> Información general de la replicación con grupos de disponibilidad  
  
###  <a name="publisher-redirection"></a><a name="PublisherRedirect"></a> Redirección del publicador  
 Cuando una base de datos publicada se basa en [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], el distribuidor que proporciona el acceso del agente a la base de datos de publicación se configura con entradas de publicadores redirigidos. Estas entradas redirigirán el par publicador/base de datos configurado originalmente, haciendo uso de un nombre de agente de escucha del grupo de disponibilidad para conectarse al publicador y a la base de datos de publicación. Las conexiones establecidas a través del nombre de agente de escucha del grupo de disponibilidad producirán errores en la conmutación por error. Cuando se reinicia el agente de replicación después de la conmutación por error, la conexión será redirigida automáticamente al nuevo elemento principal.  
  
 En un grupo de disponibilidad, una base de datos secundaria no puede ser un publicador. La acción de volver a publicar solo se admite cuando la replicación transaccional se combina con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 Si una base de datos publicada es miembro de un grupo de disponibilidad y el publicador está redirigido, se debe redirigir a un nombre de agente de escucha del grupo de disponibilidad asociado al grupo de disponibilidad. No se puede redirigir a un nodo explícito.  
  
> [!NOTE]  
>  Después de la conmutación por error a una réplica secundaria, el Monitor de replicación no puede ajustar el nombre de la instancia de publicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y seguirá mostrando información de replicación bajo el nombre de la instancia principal original de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Después de la conmutación por error, el Monitor de replicación no puede especificar un token de seguimiento, aunque muestra un token de seguimiento especificado en el nuevo publicador mediante [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
###  <a name="general-changes-to-replication-agents-to-support-availability-groups"></a><a name="Changes"></a> Cambios generales en los agentes de replicación para admitir grupos de disponibilidad  
 Tres agentes de replicación se han modificado para admitir [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Los agentes de registro del LOG, de instantáneas y de mezcla se han modificado para consultar la base de datos de distribución del publicador redirigido y utilizar el nombre de agente de escucha del grupo de disponibilidad devuelto, si se ha declarado un publicador redirigido, para conectarse al publicador de la base de datos.  
  
 De forma predeterminada, cuando los agentes consultan el distribuidor para determinar si se ha redirigido el publicador original, se comprobará la idoneidad de la redirección o el destino actual antes de devolver el host redirigido al agente. Este es el comportamiento recomendado. Sin embargo, si el inicio del agente se produce con mucha frecuencia, la sobrecarga asociada al procedimiento almacenado de validación puede ser demasiado costosa. Se ha agregado un nuevo modificador de la línea de comandos, *BypassPublisherValidation*, a los agentes de registro del LOG, de instantáneas y de mezcla. Cuando se utiliza el modificador, el publicador redirigido se devuelve inmediatamente al agente y se omite la ejecución del procedimiento almacenado de validación.  
  
 Los errores devueltos por el procedimiento almacenado de validación se registran en los registros de historial del agente. Los errores con una gravedad superior o igual a 16 provocarán la terminación de los agentes. Algunas capacidades de reintento se han integrado en los agentes para controlar la desconexión esperada de una base de datos publicada cuando se conmuta por error a una principal.  
  
#### <a name="log-reader-agent-modifications"></a>Modificaciones del Agente de registro del LOG  
 El Agente de registro del LOG ha sido objeto de los siguientes cambios.  
  
-   **Coherencia de la base de datos replicada**  
  
     Cuando una base de datos publicada es miembro de un grupo de disponibilidad, el registro del LOG no procesará de forma predeterminada las entradas de registro que no se hayan protegido todavía en todas las réplicas secundarias del grupo de disponibilidad. Esto garantiza que, en la conmutación por error, todas las filas replicadas a un suscriptor también estarán presentes en el nuevo elemento principal.  
  
     Cuando el publicador solo tiene dos réplicas de disponibilidad (una principal y otra secundaria) y se produce una conmutación por error, la réplica principal original sigue inactiva porque el lector del registro no avanza hasta que todas las bases de datos secundarias se hayan puesto en conexión o hasta que las réplicas secundarias con error se quiten del grupo de disponibilidad. El lector del registro, que ahora se ejecuta con la base de datos secundaria, no continuará, ya que AlwaysOn no puede proteger ningún cambio en ninguna base de datos secundaria. Para que el lector del registro pueda continuar y seguir disfrutando de la capacidad de recuperación ante desastres, quite la réplica principal original del grupo de disponibilidad usando ALTER AVAILABITY GROUP <nombre_de_grupo> REMOVE REPLICA. Después, agregue una nueva réplica secundaria al grupo de disponibilidad.  
  
-   **Marca de seguimiento 1448**  
  
     La marca de seguimiento 1448 permite que el lector del registro de replicación avance aunque las réplicas secundarias asincrónicas no hayan confirmado la recepción de un cambio. Incluso con esta marca de seguimiento habilitada, el lector del registro espera siempre las réplicas secundarias sincrónicas. El lector del registro no irá más allá de la confirmación mínima de las réplicas secundarias sincrónicas. Esta marca de seguimiento se aplica a la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], no solo a un grupo disponibilidad, una base de datos de disponibilidad o una instancia del lector de registros. Esta marca de seguimiento surte efecto inmediatamente sin necesidad de reiniciar. Se puede activar de antemano o cuando se produce un error en una réplica secundaria asincrónica.  
  
###  <a name="stored-procedures-supporting-availability-groups"></a><a name="StoredProcs"></a> Procedimientos almacenados que admiten grupos de disponibilidad  
  
-   **sp_redirect_publisher**  
  
     El procedimiento almacenado **sp_redirect_publisher** sirve para especificar un publicador redirigido para un par publicador/base de datos existente. Si la base de datos del publicador pertenece a un grupo de disponibilidad, el publicador redirigido es el nombre de agente de escucha del grupo de disponibilidad.  
  
-   **sp_get_redirected_publisher**  
  
     Los agentes de replicación usan el procedimiento almacenado **sp_get_redirected_publisher** para consultar un distribuidor y saber si un par publicador/base de datos tiene un publicador redirigido definido. Este procedimiento almacenado cumple dos fines. Primero, permite al agente determinar si se ha redirigido el publicador original. En segundo lugar, también puede iniciar la ejecución de un procedimiento almacenado de validación en el distribuidor (**sp_validate_redirected_publisher**) que comprueba la idoneidad del nodo de destino de la redirección para actuar como publicador de la base de datos con nombre.  
  
     Para ejecutar este procedimiento almacenado, el autor de la llamada debe ser miembro del rol de servidor **sysadmin** , del rol de base de datos **db_owner** para la base de datos de distribución o de una **lista de acceso a la publicación** para una publicación definida asociada a la base de datos del publicador.  
  
-   **sp_validate_redirected_publisher**  
  
     Este procedimiento almacenado intenta validar que el publicador actual es capaz de hospedar la base de datos publicada. Se le puede llamar en cualquier momento para comprobar que el host actual de la base de datos publicada es capaz de admitir la replicación.  
  
-   **sp_validate_replicate_hosts_as_publishers**  
  
     Aunque es útil que los agentes se aseguren de que la réplica principal actual puede actuar como publicador de replicación para una base de datos del publicador, se necesita una capacidad de validación más general para establecer la validez de una topología de replicación total en una base de datos de disponibilidad AlwaysOn. El procedimiento almacenado **sp_validate_replica_hosts_as_publishers** está diseñado para satisfacer esta necesidad.  
  
     Este procedimiento almacenado se ejecuta siempre manualmente. El autor de la llamada debe tener el rol **sysadmin** en el distribuidor, el rol **dbowner** de la base de datos de distribución o ser miembro de la **lista de acceso a la publicación** de una publicación de la base de datos del publicador. Además, el inicio de sesión del autor de la llamada debe ser un inicio de sesión válido para todos los hosts de réplicas de disponibilidad y tener determinados privilegios en la base de datos de disponibilidad asociada a la base de datos del publicador.  
  
###  <a name="change-data-capture"></a><a name="CDC"></a> Captura de datos modificados  
 Las bases de datos habilitadas para la captura de datos modificados (CDC) pueden aprovechar las ventajas de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] para asegurarse de que no solo la base de datos sigue estando disponible en caso de error, sino que los cambios en las tablas de la base de datos se siguen supervisando y depositando en las tablas de cambios de CDC. El orden en que se configuran CDC y [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] no es importante. Las bases de datos habilitadas para CDC se pueden agregar a [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]y las bases de datos que son miembros de un grupo de disponibilidad AlwaysOn se pueden habilitar para CDC. Sin embargo, en ambos casos, la configuración de CDC se realiza siempre en la réplica principal prevista o actual. CDC usa el agente de registro del LOG y tiene las mismas limitaciones descritas en la sección **Modificaciones del Agente de registro del LOG** anteriormente en este tema.  
  
-   **Recolección de cambios para la captura de datos modificados sin replicación**  
  
     Si CDC está habilitada para una base de datos pero la replicación no lo está, el proceso de captura se utiliza para recopilar cambios del registro y depositarlos en las ejecuciones de las tablas de cambios de CDC en el host de CDC como su propio trabajo del Agente SQL.  
  
     Para reanudar la recolección de cambios después de la conmutación por error, el procedimiento almacenado **sp_cdc_add_job** se debe ejecutar en el nuevo elemento principal para crear el trabajo de captura local.  
  
     En el ejemplo siguiente se crea el trabajo de captura.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'capture';  
    ```  
  
-   **Recolección de cambios para la captura de datos modificados con replicación**  
  
     Si CDC y la replicación están habilitadas para una base de datos, el agente de registro del LOG controla el rellenado de las tablas de cambios de CDC. En este caso, las técnicas empleadas por la replicación para aprovechar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] asegurarán que los cambios continúan recopilándose del registro y depositándose en las tablas de cambios de CDC después de la conmutación por error. No es necesario realizar ninguna acción más para CDC en esta configuración para garantizar que las tablas de cambios se van a rellenar.  
  
-   **Limpieza de la captura de datos modificados**  
  
     Para garantizar que se produce la limpieza apropiada en la nueva base de datos principal, siempre debe crearse un trabajo de limpieza local. En el ejemplo siguiente se crea el trabajo de limpieza.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'cleanup';  
    ```  
  
    > [!NOTE]  
    >  Se deben crear los trabajos en todos los destinos posibles de conmutación por error antes de la conmutación por error y marcarlos como deshabilitados hasta que la réplica de disponibilidad de un host se convierta en la nueva réplica principal. Los trabajos de CDC que se ejecutan en la base de datos principal anterior también deben estar deshabilitados cuando la base de datos local se convierte en una base de datos secundaria. Para deshabilitar y habilitar trabajos, use la opción *\@enabled* de [sp_update_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md). Para obtener más información sobre cómo crear trabajos de CDC, vea [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)se admiten la replicación, la captura de datos modificados (CDC) y el seguimiento de cambios (CT).  
  
-   **Agregar roles de CDC a una réplica de la base de datos principal de AlwaysOn**  
  
     Cuando se habilita una tabla para CDC, se puede asociar un rol de base de datos con la instancia de captura. Si se especifica un rol, el usuario que desea utilizar funciones con valores de tabla CDC para tener acceso a los cambios de la tabla no solo debe tener acceso de selección a las columnas de la tabla sometida a seguimiento, sino que también debe ser miembro del rol con nombre. Si no existe todavía el rol especificado, se creará. Cuando los roles de base de datos se agregan automáticamente a una base de datos principal AlwaysOn, los roles también se propagan a las bases de datos secundarias del grupo de disponibilidad.  
  
-   **Aplicaciones cliente que tienen acceso a los datos modificados de CDC y AlwaysOn**  
  
     Las aplicaciones cliente que usan funciones con valores de tabla (TVF) o servidores vinculados para tener acceso a datos de las tablas de cambios también necesitan poder encontrar un host de CDC apropiado después de la conmutación por error. El nombre de agente de escucha del grupo de disponibilidad es el mecanismo proporcionado por [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] para permitir de forma transparente la redirección de una conexión a un host diferente. Una vez que un nombre de agente de escucha del grupo de disponibilidad está asociado a un grupo de disponibilidad, se puede utilizar en las cadenas de conexión de TCP. Se admiten dos escenarios de conexión diferentes mediante el nombre de agente de escucha del grupo de disponibilidad.  
  
    -   En uno se garantiza que las solicitudes de conexión se dirigen siempre a la réplica principal actual.  
  
    -   En el otro se garantiza que las solicitudes de conexión se dirigen a una réplica secundaria de solo lectura.  
  
     Si se usa para buscar una réplica secundaria de solo lectura, también se debe definir una lista de enrutamiento de solo lectura para el grupo de disponibilidad. Para obtener más información, vea [Para configurar réplicas de disponibilidad para el enrutamiento de solo lectura](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
    > [!NOTE]  
    >  Hay un determinado retardo de propagación asociado a la creación de un nombre de agente de escucha del grupo de disponibilidad y su uso por aplicaciones cliente para tener acceso a una réplica de base de datos del grupo de disponibilidad.  
  
     Utilice la consulta siguiente para determinar si se ha definido un nombre de agente de escucha del grupo de disponibilidad para el grupo de disponibilidad que hospeda una base de datos CDC. La consulta devolverá el nombre de agente de escucha del grupo de disponibilidad si se ha creado uno.  
  
    ```sql  
    SELECT dns_name   
    FROM sys.availability_group_listeners AS l  
    INNER JOIN sys.availability_databases_cluster AS d  
        ON l.group_id = d.group_id  
    WHERE d.database_name = N'MyCDCDB';  
    ```  
  
-   **Redirigir la carga de consulta a una réplica secundaria legible**  
  
     Aunque en muchos casos una aplicación cliente siempre deseará conectarse a la réplica principal actual, esa no es la única forma de aprovechar las ventajas de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Si se configura un grupo de disponibilidad para que admita réplicas secundarias legibles, también se pueden recopilar los datos modificados de nodos secundarios.  
  
     Cuando se configura un grupo de disponibilidad, el atributo ALLOW_CONNECTIONS asociado a SECONDARY_ROLE se usa para especificar el tipo de acceso secundario admitido. Si se configura como ALL, se permitirán todas las conexiones con la base de datos secundaria, pero solo se realizarán correctamente las que necesiten acceso de solo lectura. Si se configura como READ_ONLY, es necesario especificar la intención de solo lectura al realizar la conexión con la base de datos secundaria para que la conexión se realice correctamente. Para obtener más información, vea [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  
  
     Se puede usar la siguiente consulta para determinar si se necesita la intención de solo lectura para conectarse a una réplica secundaria legible.  
  
    ```sql  
    SELECT g.name AS AG, replica_server_name, secondary_role_allow_connections_desc  
    FROM sys.availability_replicas AS r  
    JOIN sys.availability_groups AS g  
        ON r.group_id = g.group_id  
    WHERE g.name = N'MY_AG_NAME';  
    ```  
  
     Se puede usar el nombre de agente de escucha del grupo de disponibilidad o el nombre de nodo explícito para buscar la réplica secundaria. Si se emplea el nombre de agente de escucha del grupo de disponibilidad, el acceso se dirigirá a cualquier réplica secundaria adecuada.  
  
     Cuando se utiliza **sp_addlinkedserver** para crear un servidor vinculado con el fin de acceder al elemento secundario, el parámetro *\@datasrc* se utiliza para el nombre de agente de escucha del grupo de disponibilidad o el nombre de servidor explícito. Por su parte, el parámetro *\@provstr* se utiliza para especificar la intención de solo lectura.  
  
    ```sql  
    EXEC sp_addlinkedserver   
    @server = N'linked_svr',   
    @srvproduct=N'SqlServer',  
    @provider=N'SQLNCLI11',   
    @datasrc=N'AG_Listener_Name',   
    @provstr=N'ApplicationIntent=ReadOnly',   
    @catalog=N'MY_DB_NAME';  
    ```  
  
-   **Acceso de cliente a los datos modificados de CDC y los inicios de sesión de dominio**  
  
     En general, se deben usar inicios de sesión de dominio para el acceso de cliente a los datos modificados de bases de datos que son miembros de grupos de disponibilidad AlwaysOn. Para garantizar el acceso continuado a los datos modificados después de la conmutación por error, el usuario de dominio necesitará tener privilegios de acceso en todos los hosts que admiten réplicas del grupo de disponibilidad. Si se agrega un usuario de base de datos a una base de datos en una réplica principal y el usuario está asociado a un inicio de sesión de dominio, el usuario de base de datos se propaga a las bases de datos secundarias y continúa asociado al inicio de sesión de dominio especificado. Si el nuevo usuario de base de datos está asociado a un inicio de sesión de autenticación de SQL Server, el usuario de las bases de datos secundarias se propagará sin un inicio de sesión. Mientras que el inicio de sesión de autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] asociado se puede utilizar para acceder a los datos modificados del elemento principal donde se definió originalmente el usuario de base de datos, ese nodo es el único donde sería posible el acceso. El inicio de sesión de autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no podría acceder a los datos de una base de datos secundaria ni a los de una nueva base de datos principal diferente de la base de datos original donde estaba definido el usuario de base de datos.  
     
-   **Deshabilitar la captura de datos modificados**  
Si la captura de datos modificados debe deshabilitarse en una base de datos que forma parte de un grupo de disponibilidad AlwaysOn, será necesario realizar más pasos para procurar que el truncamiento del registro no se vea afectado. Deberá implementar uno de los siguientes pasos para evitar que, cuando la deshabilite, la captura de datos modificados bloquee el truncamiento del registro:
    - Reiniciar el servicio SQL Server en cada instancia de réplica secundaria
    - Quitar la base de datos de todas las instancias de réplica secundaria del grupo de disponibilidad y agregarla a las instancias de réplica del grupo de disponibilidad, usando para ello la propagación automática o manual
  
###  <a name="change-tracking"></a><a name="CT"></a> Seguimiento de los cambios  
 Una base de datos habilitada para el seguimiento de cambios (CT) puede formar parte de un grupo de disponibilidad AlwaysOn. No se necesita ninguna configuración adicional. Las aplicaciones cliente de seguimiento de cambios que usan las funciones con valores de tabla (TVF) de CDC para tener acceso a los datos modificados también necesitarán poder encontrar la réplica principal después de la conmutación por error. Si la aplicación cliente se conecta mediante el nombre de agente de escucha del grupo de disponibilidad, las solicitudes de conexión siempre se dirigirán correctamente a la réplica principal actual.  
  
> [!NOTE]  
>  Los datos del seguimiento de cambios deben obtenerse siempre de la réplica principal. Si se intenta acceder a los datos modificados desde una réplica secundaria, se producirá el error siguiente:  
>   
>  Mensaje 22117, Nivel 16, Estado 1, Línea 1  
>   
>  Para las bases de datos que son miembros de una réplica secundaria (es decir, para las bases de datos secundarias), no se admite el seguimiento de cambios. Ejecute las consultas de seguimiento de cambios en las bases de datos de la réplica principal.  
  
##  <a name="prerequisites-restrictions-and-considerations-for-using-replication"></a><a name="Prereqs"></a> Requisitos previos, restricciones y consideraciones para el uso de la replicación  
 En esta sección se describe las consideraciones para implementar la replicación con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], incluidos los requisitos previos, las restricciones y las recomendaciones.  
  
### <a name="prerequisites"></a>Requisitos previos  
  
-   Cuando se utiliza la replicación transaccional y la base de datos de publicación está en un grupo de disponibilidad, tanto el publicador como el distribuidor deben ejecutar al menos [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. El suscriptor puede utilizar un nivel inferior de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Cuando se utiliza la replicación de mezcla y la base de datos de publicación está en un grupo de disponibilidad:  
  
    -   Suscripción de inserción: tanto el publicador como el distribuidor deben ejecutar como mínimo [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
    -   Suscripción de extracción: las bases de datos del publicador, el distribuidor y el suscriptor deben ser al menos de [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Esto se debe a que el agente de mezcla el suscriptor debe entender cómo un grupo de disponibilidad puede realizar la conmutación por error a su réplica secundaria.  
  
-   Las instancias del publicador cumplen todos los requisitos previos necesarios para participar en un grupo de disponibilidad AlwaysOn. Para obtener más información, vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)se admiten la replicación, la captura de datos modificados (CDC) y el seguimiento de cambios (CT).  
  
### <a name="restrictions"></a>Restricciones  
 Combinaciones admitidas de replicación en [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
|Replicación|Publicador|Distribuidor.|Suscriptor|  
|-|-|-|-|  
|**Transaccional**|Sí<br /><br /> Nota: No incluye compatibilidad con la replicación transaccional bidireccional y recíproca.|Sí|Sí| 
|**P2P**|No|No|No|  
|**Combinar**|Sí|No|No|  
|**Instantánea**|Sí|No|Sí|
  
 **La base de datos de distribución no se puede usar con la creación de reflejo de la base de datos.  
  
### <a name="considerations"></a>Consideraciones  
  
-   La base de datos de distribución no se admite para su uso con la creación de reflejo de la base de datos, pero sí con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sujetos a ciertas limitaciones; vea [Configuración del grupo de disponibilidad de distribución](../../../relational-databases/replication/configure-distribution-availability-group.md#limitations-or-exclusions). La configuración de replicación se acopla a la instancia de SQL Server donde se ha configurado el distribuidor; por lo tanto, la base de datos de distribución no se puede reflejar ni replicar. También se puede proporcionar alta disponibilidad para el distribuidor mediante un clúster de conmutación por error de SQL Server. Para obtener más información, vea [Instancias de clúster de conmutación por error de AlwaysOn &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
-   La conmutación por error del suscriptor a una base de datos de secundaria, aunque se admite, es un procedimiento para suscriptores de replicación de mezcla. El procedimiento es básicamente idéntico al método utilizado para la conmutación por error de una base de datos de suscriptor reflejada. Los suscriptores de replicación transaccional no necesitan un tratamiento especial mientras participan en [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Los suscriptores deben ejecutar [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] o una versión posterior para participar en un grupo de disponibilidad.  Para obtener más información, consulte [Suscriptores de replicación y grupos de disponibilidad AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)
  
-   Los metadatos y los objetos que existen fuera de la base de datos no se propagan a las réplicas secundarias, incluidos los inicios de sesión, los trabajos y los servidores vinculados. Si se necesitan los metadatos y los objetos en la nueva base de datos principal después de la conmutación por error, se deben copiar manualmente. Para obtener más información, vea [Administración de inicios de sesión y de trabajos para las bases de datos de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md).  

### <a name="distributed-availability-groups"></a>Grupos de disponibilidad distribuidos

El publicador o la base de datos de distribución de un grupo de disponibilidad no se pueden configurar como parte de un grupo de disponibilidad distribuido. La base de datos del publicador de un grupo de disponibilidad y la base de datos de distribución de un grupo de disponibilidad requieren un punto de conexión de escucha para la configuración y el uso adecuados. Pero no se puede configurar un punto de conexión de escucha para un grupo de disponibilidad distribuido.
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Replicación**  
  
-   [Configurar la replicación para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Mantener una base de datos de publicación AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)  
  
-   [Preguntas más frecuentes para administradores de replicación](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
 **Captura de datos modificados**  
  
-   [Habilitar y deshabilitar la captura de datos modificados &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)  
  
-   [Administrar y supervisar la captura de datos modificados &#40;SQL Server&#41;](../../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
-   [Trabajar con datos modificados &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
 **Seguimiento de cambios**  
  
-   [Habilitar y deshabilitar el seguimiento de cambios &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)  
  
-   [Administrar el seguimiento de cambios &#40;SQL Server&#41;](../../../relational-databases/track-changes/manage-change-tracking-sql-server.md)  
  
-   [Trabajar con el seguimiento de cambios &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-tracking-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Suscriptores de replicación y Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Grupos de disponibilidad Always On: interoperabilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Instancias de clúster de conmutación por error de AlwaysOn &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)   
 [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Acerca del seguimiento de cambios &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [Replicación de SQL Server](../../../relational-databases/replication/sql-server-replication.md)   
 [Seguimiento de cambios de datos &#40;SQL Server&#41;](../../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  
