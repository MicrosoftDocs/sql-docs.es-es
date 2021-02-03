---
title: Configuración del enrutamiento de solo lectura para un grupo de disponibilidad
description: Enrute de forma automática todo el tráfico de solo lectura a una réplica secundaria mediante enrutamiento de solo lectura para el grupo de disponibilidad Always On con Transact-SQL (T-SQL) o PowerShell.
ms.custom: seodec18
ms.date: 02/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 7bd89ddd-0403-4930-a5eb-3c78718533d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f31e0e3a961736bb514c31bfabf670893dc78e8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236321"
---
# <a name="configure-read-only-routing-for-an-always-on-availability-group"></a>Configuración del enrutamiento de solo lectura para un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Para configurar un grupo de disponibilidad AlwaysOn para admitir el enrutamiento de solo lectura en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], puede usar [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell. El *enrutamiento de solo lectura* hace referencia a la capacidad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de enrutar las solicitudes de conexión de solo lectura a una [réplica secundaria legible](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) de AlwaysOn disponible (es decir, una réplica configurada para permitir cargas de trabajo de solo lectura al ejecutarse en un rol secundario). Para admitir el enrutamiento de solo lectura, el grupo de disponibilidad debe poseer un [agente de escucha de grupo de disponibilidad](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md). Los clientes de solo lectura deben dirigir sus solicitudes de conexión a este agente de escucha y las cadenas de conexión del cliente deben especificar el intento de aplicación como de “solo lectura”. Es decir, deben ser *solicitudes de conexión de intento de lectura*.  

El enrutamiento de solo lectura está disponible en [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] y versiones posteriores.

> [!NOTE]  
>  Para obtener información sobre cómo configurar una réplica secundaria legible, vea [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   El grupo de disponibilidad debe poseer un agente de escucha de grupo de disponibilidad. Para obtener más información, vea [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
-   Se deben configurar una o varias réplicas de disponibilidad para aceptar el acceso de solo lectura para el rol secundario (es decir, para ser [replicas secundarias legibles](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)). Para obtener más información, vea [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal actual.  

-   Si usa un inicio de sesión de SQL, asegúrese de que la cuenta esté configurada correctamente. Para más información, consulte [Administración de inicios de sesión y de trabajos para las bases de datos de un grupo de disponibilidad (SQL Server)](logins-and-jobs-for-availability-group-databases.md).
  
##  <a name="what-replica-properties-do-you-need-to-configure-to-support-read-only-routing"></a><a name="RORReplicaProperties"></a> ¿Qué propiedades de réplica debe configurar para admitir el enrutamiento de solo lectura?  
  
-   Para cada réplica secundaria legible que vaya a admitir el enrutamiento de solo lectura, debe especificar una *dirección URL de enrutamiento de solo lectura*. Esta dirección URL tiene efecto cuando la réplica local se ejecuta en el rol secundario. La dirección URL de enrutamiento de solo lectura debe especificarse réplica a réplica, según sea necesario. Cada dirección URL de solo lectura se usa para enrutar las solicitudes de conexión de intento de lectura a una réplica secundaria legible específica. Normalmente, cada réplica secundaria legible se asigna a una dirección URL de enrutamiento de solo lectura.  
  
     Para obtener información sobre cómo calcular la dirección URL de enrutamiento de solo lectura para una réplica de disponibilidad, vea [Calcular Read_only_routing_url para AlwaysOn](https://web.archive.org/web/20170512023255/https://blogs.msdn.microsoft.com/mattn/2012/04/25/calculating-read_only_routing_url-for-alwayson/)
  
-   Para cada réplica de disponibilidad que quiera que admita el enrutamiento de solo lectura cuando sea la réplica principal, debe especificar una *lista de enrutamiento de solo lectura*. Una lista de enrutamiento de solo lectura dada solo tiene efecto cuando la réplica local se ejecuta en el rol principal. Esta lista se debe especificar réplica a réplica, según sea necesario. Normalmente, cada lista de enrutamiento de solo lectura contendría cada dirección URL de enrutamiento de solo lectura con la dirección URL de la réplica local al final de la lista.  
  
    > [!NOTE]  
    >  Las solicitudes de conexión de intento de lectura se enrutan a la primera entrada disponible en la lista de enrutamiento de solo lectura de la réplica principal actual. Sin embargo, se admite el equilibrio de carga entre réplicas de solo lectura. Para obtener más información, vea [Configuración del equilibrio de carga entre réplicas de solo lectura](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
> [!NOTE]  
>  Para obtener información sobre los agentes de escucha de grupo de disponibilidad y obtener más información sobre el enrutamiento de solo lectura, vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
  
|Tarea|Permisos|  
|----------|-----------------|  
|Para configurar réplicas al crear un grupo de disponibilidad|Se requiere la pertenencia al rol fijo de servidor **sysadmin** y el permiso de servidor CREATE AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
|Para modificar una réplica de disponibilidad|Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
### <a name="configure-a-read-only-routing-list"></a>Configuración de una lista de enrutamiento de solo lectura  
 Realice los pasos siguientes para configurar el enrutamiento de solo lectura mediante Transact-SQL. Para obtener un ejemplo de código, vea [Ejemplo (Transact-SQL)](#TsqlExample), más adelante en esta sección.  
  
1.  Conéctese a la instancia del servidor que hospeda la réplica principal.  
  
2.  Si va a especificar una réplica para un nuevo grupo de disponibilidad, use la instrucción [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Si va a agregar o modificar una réplica para un grupo de disponibilidad existente, use la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Para configurar el acceso de solo lectura para el rol secundario, en la cláusula ADD REPLICA o MODIFY REPLICA WITH, especifique la opción SECONDARY_ROLE, del siguiente modo:  
  
         SECONDARY_ROLE **(** READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **')**  
  
         Los parámetros de la dirección URL de enrutamiento de solo lectura son los siguientes:  
  
         *system-address*  
         Es una cadena, como un nombre de sistema, un nombre de dominio completo o una dirección IP, que identifica sin ambigüedad el equipo de destino.  
  
         *port*  
         Es un número de puerto que usa el motor de base de datos de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
         Por ejemplo:   `SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433')`  
  
         En una cláusula MODIFY REPLICA el ALLOW_CONNECTIONS es opcional si la réplica ya está configurada para permitir conexiones de solo lectura.  
  
         Para obtener más información, vea [Calcular read_only_routing_url para AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Para configurar el enrutamiento de solo lectura para el rol principal, en la cláusula ADD REPLICA o MODIFY REPLICA WITH, especifique la opción PRIMARY_ROLE, del siguiente modo:  
  
         PRIMARY_ROLE **(** READ_ONLY_ROUTING_LIST **=('** _server_ **'** [ **,** ...*n* ] **))**  
  
         donde *server* identifica una instancia del servidor que hospeda una réplica secundaria de solo lectura en el grupo de disponibilidad.  
  
         Por ejemplo:   `PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('Server1','Server2'))`  
  
        > [!NOTE]  
        >  Debe establecer la dirección URL de enrutamiento de solo lectura antes de configurar la lista de enrutamiento de solo lectura.  
  
###  <a name="configure-load-balancing-across-read-only-replicas"></a><a name="loadbalancing"></a> Configuración del equilibrio de carga entre réplicas de solo lectura  
 A partir de [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], puede configurar el equilibrio de carga entre un conjunto de réplicas de solo lectura. Anteriormente, el enrutamiento de solo lectura siempre dirigía el tráfico a la primera réplica disponible de la lista de enrutamiento. Para aprovechar esta característica, use un nivel de paréntesis anidados alrededor de las instancias de servidor **READ_ONLY_ROUTING_LIST** en los comandos **CREATE AVAILABILITY GROUP** o **ALTER AVAILABILITY GROUP** .  
  
 Por ejemplo, la siguiente carga de la lista de enrutamiento equilibra la solicitud de conexión con intención de lectura entre dos réplicas de solo lectura, `Server1` y `Server2`. Los paréntesis anidados que rodean estos servidores identifican el conjunto con equilibrio de carga. Si ninguna réplica está disponible en dicho conjunto, tratará de conectarse de forma secuencial a las demás réplicas, `Server3` y `Server4`, en la lista de enrutamiento de solo lectura.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), 'Server3', 'Server4')  
```  
  
 Tenga en cuenta que cada entrada de la lista de enrutamiento puede ser un conjunto de réplicas de solo lectura con equilibrio de carga. El ejemplo siguiente demuestra este patrón.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), ('Server3', 'Server4', 'Server5'), 'Server6')  
```  
  
 Se admite solo un nivel de paréntesis anidados.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En el ejemplo siguiente se modifica la disponibilidad de las dos réplicas de un grupo de disponibilidad existente, `AG1` para admitir el enrutamiento de solo lectura, si una de las réplicas posee actualmente el rol principal. Para identificar las instancias de servidor que hospedan la réplica de disponibilidad, este ejemplo especifica los nombres de instancia `COMPUTER01` y `COMPUTER02`.  
  
```  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
GO  
  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
  
### <a name="configure-a-read-only-routing-list"></a>Configuración de una lista de enrutamiento de solo lectura  
 Realice los pasos siguientes para configurar el enrutamiento de solo lectura mediante PowerShell. Para obtener un ejemplo de código, vea [Ejemplo (PowerShell)](#PSExample), más adelante en esta sección.  
  
1.  Establezca el valor predeterminado (**cd**) en la instancia del servidor que hospeda la réplica principal.  
  
2.  Para agregar una réplica de disponibilidad a un grupo de disponibilidad, use el cmdlet **New-SqlAvailabilityReplica** . Para modificar una réplica de disponibilidad existente, use el cmdlet **Set-SqlAvailabilityReplica** . Los parámetros pertinentes son los siguientes:  
  
    -   Para configurar el enrutamiento de solo lectura para el rol secundario, especifique el parámetro **ReadonlyRoutingConnectionUrl"** _url_ **"** .  
  
         donde, *url* es el nombre de dominio completo (FQDN) y puerto que se usa para el enrutamiento de la réplica para las conexiones de solo lectura. Por ejemplo: `-ReadonlyRoutingConnectionUrl "TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024"`  
  
         Para obtener más información, vea [Calcular read_only_routing_url para AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Para configurar el acceso de conexión para el rol principal, especifique **ReadonlyRoutingList"** _server_ **"** [ **,** ...*n* ], donde *server* identifica una instancia del servidor que hospeda una réplica secundaria de solo lectura en el grupo de disponibilidad. Por ejemplo: `-ReadOnlyRoutingList "SecondaryServer","PrimaryServer"`  
  
        > [!NOTE]  
        >  Debe establecer la dirección URL de enrutamiento de solo lectura de la réplica antes de configurar su lista de enrutamiento de solo lectura.  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
### <a name="set-up-and-use-the-sql-server-powershell-provider"></a>Configuración y uso del proveedor de SQL Server PowerShell  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Ejemplo (PowerShell)  
 En el ejemplo siguiente se configura la réplica principal y una réplica secundaria en un grupo de disponibilidad para el enrutamiento de solo lectura. Primero, en el el ejemplo se asigna una dirección URL de solo lectura de enrutamiento a cada réplica. Después, establece la lista de enrutamiento de solo lectura en la réplica principal. Las conexiones con la propiedad "ReadOnly" establecida en la cadena de conexión se redirigirán a la réplica secundaria. Si esta réplica secundaria no es legible (según determina la opción **ConnectionModeInSecondaryRole** ), la conexión se dirigirá a la réplica principal.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
$secondaryReplica = Get-Item "AvailabilityReplicas\SecondaryServer"  
  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://PrimaryServer.domain.com:1433" -InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://SecondaryServer.domain.com:1433" -InputObject $secondaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingList "SecondaryServer","PrimaryServer" -InputObject $primaryReplica  
```  
  
##  <a name="follow-up-after-configuring-read-only-routing"></a><a name="FollowUp"></a> Seguimiento: después de configurar el enrutamiento de solo lectura  
 Una vez que la réplica principal actual y las réplicas secundarias legibles están configuradas para admitir el enrutamiento de solo lectura en ambos roles, las réplicas secundarias legibles pueden recibir solicitudes de intentos de conexión de lectura de los clientes que se conectan mediante el agente de escucha de grupo de disponibilidad.  
  
> [!TIP]  
>  Cuando se usa la [utilidad bcp](../../../tools/bcp-utility.md) o la [utilidad sqlcmd](../../../tools/sqlcmd-utility.md), se puede especificar el acceso de solo lectura a cualquier réplica secundaria que esté habilitada para el acceso de solo lectura mediante el modificador **-K ReadOnly** .  
  
###  <a name="requirements-and-recommendations-for-client-connection-strings"></a><a name="ConnStringReqsRecs"></a> Requisitos y recomendaciones para las cadenas de conexión de cliente  
 Para que una aplicación cliente use el enrutamiento de solo lectura, la cadena de conexión debe cumplir los requisitos siguientes:  
  
-   Usar el protocolo TCP.  
  
-   Establecer el atributo o propiedad para el intento de aplicaciones de solo lectura.  
  
-   Hacer referencia al agente de escucha de un grupo de disponibilidad que se configura para admitir el enrutamiento de solo lectura.  
  
-   Hacer referencia a una base de datos en ese grupo de disponibilidad.  
  
 Además, se recomienda que las cadenas de conexión habiliten la conmutación por error de múltiples subredes, que admite un subproceso de cliente paralelo para cada réplica en cada subred. Esto reduce el tiempo de reconexión de cliente después de una conmutación por error.  
  
 La sintaxis para una cadena de conexión depende del proveedor de SQL Server que una aplicación está utilizando. El siguiente ejemplo de cadena de conexión para el proveedor de datos de .NET Framework 4.0.2 para SQL Server, muestra las partes de una cadena de conexión necesarias y que se recomiendan para que funcionen con el enrutamiento de solo lectura.  
  
```  
Server=tcp:MyAgListener,1433;Database=Db1;IntegratedSecurity=SSPI;ApplicationIntent=ReadOnly;MultiSubnetFailover=True  
```  
  
 Para obtener más información sobre el intento de aplicación de solo lectura y el enrutamiento de solo lectura, vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
### <a name="if-read-only-routing-is-not-working-correctly"></a>Si el enrutamiento de solo lectura no funciona correctamente  
 Para obtener información sobre la solución de problemas de una configuración de enrutamiento de solo lectura, vea [El enrutamiento de solo lectura no funciona correctamente](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md#ROR).  
  
##  <a name="next-steps"></a><a name="RelatedTasks"></a> Pasos siguientes 
**Para ver las configuraciones del enrutamiento de solo lectura**  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md) (columna **read_only_routing_url**)  
  
**Para configurar el acceso de la conexión de cliente**  
  
-   [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
**Para usar las cadenas de conexión en aplicaciones**  
  
-   [Compatibilidad de SQL Server Native Client para la alta disponibilidad con recuperación de desastres](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
  
-   [Usar palabras clave de cadena de conexión con SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
**Blogs:**  
  
-    [Calcular Read_only_routing_url para AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)  
  
-    [Blogs del equipo de Always On de SQL Server: el blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)  
  
-    [Blogs de los ingenieros de SQL Server de CSS](/archive/blogs/psssql/)  
  
**Notas del producto:**  
  
-    [Notas del producto de Microsoft para SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
-    [Notas del producto del equipo de asesoramiento al cliente de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  

**Contenido adicional**

- [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

- [Secundarias activas: réplicas secundarias legibles &#40;grupos de disponibilidad Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   

- [Acerca del acceso de conexión de cliente a réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 
- [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)