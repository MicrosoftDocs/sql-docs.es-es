---
title: Descarga de carga de trabajo a una réplica secundaria de un grupo de disponibilidad
description: Obtenga información sobre la descarga de consultas e informes de solo lectura a una réplica secundaria de un grupo de disponibilidad Always On en SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/06/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 78f3f81a-066a-4fff-b023-7725ff874fdf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 02e07cbb5b0fd6a917d6455c9d41a0fda3f03055
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343950"
---
# <a name="offload-read-only-workload-to-secondary-replica-of-an-always-on-availability-group"></a>Descarga de cargas de trabajo de solo lectura a la réplica secundaria de un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Las funcionalidades secundarias activas de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] incluyen compatibilidad con el acceso de solo lectura a una o varias réplicas secundarias (*réplicas secundarias legibles*). Una réplica secundaria legible puede estar en modo de disponibilidad de confirmación sincrónica o en el modo de disponibilidad de confirmación asincrónica. Una réplica secundaria legible permite el acceso de solo lectura a todas las bases de datos secundarias. Sin embargo, las bases de datos secundarias legibles no se establecen como de solo lectura. Son dinámicas. Una base de datos secundaria dada cambia a medida que se aplican los cambios en la base de datos principal correspondiente. En lo que respecta a las réplicas secundarias típicas, los datos, lo cual incluye las tablas con optimización para memoria durables, las bases de datos secundarias están en tiempo prácticamente real. Además, los índices de texto completo se sincronizan con las bases de datos secundarias. En muchas circunstancias, la latencia de datos entre una base de datos principal y la base de datos secundaria correspondiente suele ser de solo unos pocos segundos.  
  
 La configuración de seguridad de las bases de datos principales se mantiene en las secundarias. Esto incluye usuarios, roles de base de datos y roles de aplicación, junto con sus permisos correspondientes, y también incluye cifrado de datos transparentes (TDE) si está habilitado en la base de datos principal.  
  
> [!NOTE]  
>  Aunque no puede escribir datos en las bases de datos secundarias, puede escribir en bases de datos de lectura y escritura de la instancia del servidor que hospeda la réplica secundaria, incluidas las bases de datos de usuario y las bases de datos del sistema, como **tempdb**.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] también admite el reenrutamiento de las solicitudes de conexión con intención de lectura a una réplica secundaria legible (*enrutamiento de solo lectura*). Para obtener información sobre el enrutamiento de solo lectura, vea [Usar un agente de escucha para conectarse a una réplica secundaria de solo lectura (enrutamiento de solo lectura)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary).  
  
##  <a name="benefits"></a><a name="bkmk_Benefits"></a> Ventajas  
 La dirección de conexiones de solo lectura a las réplicas secundarias legibles proporciona las siguientes ventajas:  
  
-   Alivia las cargas de trabajo de solo lectura secundarias de la réplica primaria, que conserva los recursos para las cargas de trabajo esenciales de la misión. Si tiene una carga de trabajo de lectura de gran importancia o si la carga de trabajo no puede tolerar la latencia, debe ejecutarla en el servidor principal.  
  
-   Mejora la rentabilidad de la inversión para los sistemas que hospedan las réplicas secundarias legibles.  
  
 Además, las réplicas secundarias legibles proporcionan compatibilidad robusta con las operaciones de solo lectura, de la forma siguiente:  
  
-   Las estadísticas temporales automáticas en las bases de datos secundarias legibles optimizan las consultas de solo lectura en tablas basadas en disco. Para las tablas con optimización para memoria, se crean automáticamente las estadísticas que faltan. Sin embargo, no hay actualizaciones automáticas de estadísticas en desuso. Deberá actualizar manualmente las estadísticas en la réplica primaria. Para obtener más información, vea [Estadísticas de las bases de datos de acceso de solo lectura](#Read-OnlyStats), más adelante en este tema.  
  
-   Las cargas de trabajo de solo lectura para tablas basadas en disco usan las versiones de fila para quitar la contención de bloqueo en las bases de datos secundarias. Todas las consultas que se ejecutan en las bases de datos secundarias se asignan automáticamente al nivel de transacción de aislamiento de instantánea, incluso cuando se establecen otros niveles de aislamiento de transacción de forma explícita. Asimismo, se pasan por alto todas las sugerencias de bloqueo. Esto elimina la contención de lectura y escritura.  
  
-   Las cargas de trabajo de solo lectura para tablas duraderas optimizadas para memoria tienen acceso a los datos exactamente de la misma forma que en la base de datos principal, con el uso de procedimientos almacenados nativos o interoperabilidad de SQL con las mismas limitaciones del nivel de aislamiento de transacción (vea [Niveles de aislamiento del motor de base de datos](/previous-versions/sql/sql-server-2008-r2/ms189122(v=sql.105))). La carga de trabajo de informes o las consultas de solo lectura que se ejecutan en la réplica principal se pueden ejecutar en la réplica secundaria sin necesidad de hacer ningún cambio. De forma similar, las cargas de trabajo de informes o las consultas de solo lectura que se ejecutan en una réplica secundaria se pueden ejecutar en la réplica principal sin necesidad de hacer ningún cambio.  Al igual que ocurre con las tablas basadas en disco, todas las consultas que se ejecutan en las bases de datos secundarias se asignan automáticamente al nivel de transacción de aislamiento de instantánea, incluso cuando se establecen otros niveles de aislamiento de transacción de forma explícita.  
  
-   Las operaciones DML se permiten en variables de tabla tanto para los tipos de tabla basadas en disco como para los tipos de tabla optimizada para memoria en la réplica secundaria.  
  
##  <a name="prerequisites-for-the-availability-group"></a><a name="bkmk_Prerequisites"></a> Requisitos previos del grupo de disponibilidad  
  
-   **Réplicas secundarias legibles (requeridas)**  
  
     El administrador de la base de datos debe configurar una o varias réplicas de modo que, cuando se ejecutan en el rol secundario, permiten todas las conexiones (solo para el acceso de solo lectura) o solo conexiones con intención de lectura.  
  
    > [!NOTE]  
    >  Opcionalmente, el administrador de bases de datos puede configurar cualquiera de las réplicas de disponibilidad para excluir las conexiones de solo lectura al ejecutarse en el rol principal.
  
     Para obtener más información, vea [Acerca del acceso de conexión de cliente a réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
    >[!WARNING]
    >  Solo se podrán leer las réplicas que se encuentren en la misma compilación principal de SQL Server. Consulte [Conceptos básicos de la actualización gradual](upgrading-always-on-availability-group-replica-instances.md#rolling-upgrade-basics-for-always-on-ags) para obtener más información.
  
  
-   **Agente de escucha de grupo de disponibilidad**  
  
     Para admitir el enrutamiento de solo lectura, un grupo de disponibilidad debe poseer un [agente de escucha de grupo de disponibilidad](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md). El cliente de solo lectura debe dirigir sus solicitudes de conexión a dicho agente y la cadena de conexión del cliente debe especificar la intención de la aplicación como de "solo lectura". Es decir, deben ser *solicitudes de conexión de intento de lectura*.  
  
-   **Enrutamiento de solo lectura**  
  
     El *enrutamiento de solo lectura* hace referencia a la capacidad de SQL Server para enrutar las solicitudes de conexión con intención de lectura entrantes, que se dirigen a un agente de escucha de grupo de disponibilidad, a una réplica secundaria legible disponible. Los requisitos previos para el enrutamiento de solo lectura son los siguientes:  
  
    -   Para admitir el enrutamiento de solo lectura, una réplica secundaria legible requiere una dirección URL de enrutamiento de solo lectura. Esta dirección URL tiene efecto cuando la réplica local se ejecuta en el rol secundario. La dirección URL de enrutamiento de solo lectura debe especificarse réplica a réplica, según sea necesario. Cada dirección URL de solo lectura se usa para enrutar las solicitudes de conexión de intento de lectura a una réplica secundaria legible específica. Normalmente, cada réplica secundaria legible se asigna a una dirección URL de enrutamiento de solo lectura.  
  
    -   Cada réplica de disponibilidad que vaya a admitir el enrutamiento de solo lectura cuando es la réplica principal requiere una lista de enrutamiento de solo lectura. Una lista de enrutamiento de solo lectura dada solo tiene efecto cuando la réplica local se ejecuta en el rol principal. Esta lista se debe especificar réplica a réplica, según sea necesario. Normalmente, cada lista de enrutamiento de solo lectura contendría cada dirección URL de enrutamiento de solo lectura con la dirección URL de la réplica local al final de la lista.  
  
        > [!NOTE]  
        >  Las solicitudes de conexión con intención de lectura pueden tener equilibrio de carga entre réplicas. Para obtener más información, vea [Configuración del equilibrio de carga entre réplicas de solo lectura](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
     Para obtener más información, vea [Configurar el enrutamiento de solo lectura para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Para obtener información sobre los agentes de escucha de grupo de disponibilidad y obtener más información sobre el enrutamiento de solo lectura, vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="limitations-and-restrictions"></a><a name="bkmk_LimitationsRestrictions"></a> Limitaciones y restricciones  
 Algunas operaciones no se admiten por completo, como se indica a continuación:  
  
-   Tan pronto como se habilita una réplica legible para lectura, puede comenzar a aceptar conexiones a sus bases de datos secundarias. Sin embargo, si hay transacciones activas en una base de datos principal, las versiones de fila no estarán del todo disponibles en la base de datos secundaria correspondiente. Las transacciones activas que existían en la réplica principal cuando se configuró la réplica secundaria deben confirmarse o revertirse. Hasta que el proceso finalice, la asignación del nivel de aislamiento de transacción en la base de datos secundaria estará incompleta y las consultas se bloquearán temporalmente.  
  
    > [!WARNING]  
    >  La ejecución de transacciones largas afecta al número de filas con control de versiones que se conservan, tanto en tablas basadas en disco como en tablas optimizadas para memoria.  
  
-   En las bases de datos secundarias con tablas con optimización para memoria, pese a que siempre se generan versiones de filas para las tablas con optimización para memoria, las consultas se bloquean hasta que se completan todas las transacciones activas que había en la réplica principal cuando se habilitó la réplica secundaria para lectura. De esta forma se garantiza que las tablas basadas en disco y las tablas optimizadas para memoria estén disponibles para la carga de trabajo de informes y para las consultas de solo lectura al mismo tiempo.  
  
-   El seguimiento de cambios y la captura de datos modificados no se admiten en las bases de datos secundarias que pertenecen a una réplica secundaria legible:  
  
    -   El seguimiento de cambios está deshabilitado de forma explícita en las bases de datos secundarias.  
  
    -   La captura de datos modificados no se puede habilitar solo en una base de datos de réplica secundaria. La captura de datos modificados puede habilitarse en la base de datos de réplica principal y los cambios pueden leerse desde las tablas de CDC mediante las funciones de la base de datos de réplica secundaria.  
  
-   Dado que las operaciones de lectura se asignan al nivel de transacción de aislamiento de instantánea, la limpieza de registros fantasma en la réplica principal puede bloquearse por las transacciones en una o varias réplicas secundarias. La tarea de limpieza de registros fantasma limpiará automáticamente los registros fantasma para las tablas basadas en disco en la réplica principal cuando las réplicas secundarias ya no los necesiten.  Esto es similar a lo que se realiza cuando se ejecutan transacciones en la réplica principal. En el caso extremo de la base de datos secundaria, deberá eliminar una consulta de lectura de ejecución prolongada que esté bloqueando la limpieza de registros fantasma. Tenga en cuenta que la limpieza de registros fantasma se puede bloquear si la réplica secundaria se desconecta o cuando se suspende el movimiento de datos en la base de datos secundaria. Este estado también evita el truncamiento del registro, por lo que si el estado persiste, se recomienda quitar esta base de datos secundaria del grupo de disponibilidad. No existen problemas de limpieza de registros fantasma con las tablas con optimización para memoria porque las versiones de filas se conservan en memoria y son independientes de las versiones de fila de la réplica principal.  
  
-   Se puede producir un error en la operación DBCC SHRINKFILE en los archivos que contienen tablas basadas en disco en la réplica principal si el archivo contiene registros fantasma que siguen siendo necesarios en una réplica secundaria.  
  
-   A partir de [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)], las réplicas secundarias legibles pueden permanecer en línea incluso cuando la réplica principal esté sin conexión debido a una acción del usuario o a un error (por ejemplo, que una sincronización se suspenda debido a un error o a un comando del usuario) o que una réplica esté resolviendo su estado porque el WSFC está sin conexión. Sin embargo, el enrutamiento de solo lectura no funciona en esta situación porque el agente de escucha del grupo de disponibilidad está desconectado también. Los clientes deben conectarse directamente a las réplicas secundarias de solo lectura para las cargas de trabajo de solo lectura.  
  
> [!NOTE]  
>  Si consulta la vista de administración dinámica [sys.dm_db_index_physical_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) en una instancia del servidor que está hospedando una réplica secundaria legible, puede producirse un problema de bloqueo de REDO. Esto se debe a que esta vista de administración dinámica adquiere un bloqueo IS en la tabla de usuario especificada o la vista que puede bloquear las solicitudes de un subproceso de REDO durante un bloqueo X en esa tabla o vista de usuario.  
  
##  <a name="performance-considerations"></a><a name="bkmk_Performance"></a> Consideraciones de rendimiento  
 En esta sección se describen las consideraciones de rendimiento para las bases de datos secundarias legibles  
  
 **En esta sección:**  
  
-   [Latencia de datos](#DataLatency)  
  
-   [Repercusión de la carga de trabajo de solo lectura](#ReadOnlyWorkloadImpact)  
  
-   [Indexación](#bkmk_Indexing)  
  
-   [Estadísticas de las bases de datos de acceso de solo lectura](#Read-OnlyStats)  
  
###  <a name="data-latency"></a><a name="DataLatency"></a> Latencia de datos  
 La implementación del acceso de solo lectura en las réplicas secundarias resulta útil si las cargas de trabajo de solo lectura pueden tolerar cierta latencia de datos. En las situaciones en las que la latencia de datos no es aceptable, considere la posibilidad de ejecutar cargas de trabajo de solo lectura en la réplica principal.  
  
 La réplica principal envía las entradas de registro de los cambios en la base de datos principal a las réplicas secundarias. En cada base de datos secundaria, un subproceso de rehacer dedicado aplica las entradas de registro. En una base de datos secundaria de acceso de lectura, un cambio determinado de datos no aparece en los resultados de la consulta hasta que la entrada del registro que contiene el cambio se haya aplicado a la base de datos secundaria y la transacción se haya confirmado en la base de datos principal.  
  
 Esto significa que hay latencia, normalmente solo se trata de unos segundos, entre las réplicas principales y secundarias. No obstante, en casos excepcionales, por ejemplo, si los problemas de red reducen el rendimiento, la latencia puede ser importante. La latencia aumenta cuando se producen cuellos de botella de E/S y cuando se suspende el movimiento de los datos. Para supervisar el movimiento de datos suspendido, puede usar el [panel AlwaysOn](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md) o la vista de administración dinámica [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) .  
  
####  <a name="data-latency-on-databases-with-memory-optimized-tables"></a><a name="bkmk_LatencyWithInMemOLTP"></a> Latencia de datos en bases de datos con tablas optimizadas para memoria  
 En [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] existían consideraciones especiales en torno a la latencia de datos en las secundarias activas: vea [[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Secundarias activas: réplicas secundarias legibles](https://technet.microsoft.com/library/ff878253(v=sql.120).aspx). A partir de [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] , no existe ninguna consideración especial en torno a la latencia de datos para tablas optimizadas para memoria. La latencia de datos esperada para tablas optimizadas para memoria es comparable a la latencia para tablas basadas en disco.  
  
###  <a name="read-only-workload-impact"></a><a name="ReadOnlyWorkloadImpact"></a> Repercusión de la carga de trabajo de solo lectura  
 Al configurar una réplica secundaria para el acceso de solo lectura, las cargas de trabajo de solo lectura en las bases de datos secundarias utilizan los recursos del sistema, como la CPU y E/S (para tablas basadas en disco) de los subprocesos REDO, especialmente si las cargas de trabajo de solo lectura en tablas basadas en disco realizan un uso intensivo de E/S. No hay ningún impacto en la E/S cuando se tiene acceso a tablas con optimización para memoria porque todas las filas residen en memoria.  
  
 Además, las cargas de trabajo de solo lectura en las réplicas secundarias pueden bloquear los cambios de lenguaje de definición de datos (DDL) que se aplican a través de las entradas de registro.  
  
-   Aunque las operaciones de lectura no tienen bloqueos compartidos debido a las versiones de fila, estas operaciones tienen bloqueos de estabilidad de esquema (Sch-S), que pueden bloquear las operaciones de puesta al día que aplican cambios DDL. Las operaciones DDL incluyen tablas de instrucciones ALTER/DROP y vistas, pero no incluyen instrucciones DROP o ALTER de procedimientos almacenados. Por ejemplo, cuando se quita una tabla basada en disco o con optimización para memoria en la réplica principal. Cuando el subproceso REDO procesa el registro para quitar la tabla, debe adquirir un bloqueo de SCH_M en la tabla y puede bloquearse mediante una consulta en ejecución con acceso a tablas.  Este comportamiento es el mismo que en la réplica primaria, salvo que la acción de quitar la tabla forma parte de una sesión de usuario y no de un subproceso REDO.  
  
-   Las tablas con optimización para memoria tienen un bloqueo adicional. Si se quita un procedimiento almacenado, podría hacer que el proceso REDO provoque bloqueos si existen ejecuciones simultáneas del procedimiento almacenado nativo en la réplica secundaria. Este comportamiento es el mismo en la réplica primaria, salvo que la acción de quitar el procedimiento almacenado forma parte de una sesión de usuario y no de un subproceso REDO.  
  
 Debe tener en cuenta los procedimientos recomendados acerca de la creación de consultas y aplicarlos a las bases de datos secundarias. Por ejemplo, programe las consultas de ejecución prolongada tales como agregaciones de datos durante las horas de menos actividad.  
  
> [!NOTE]  
>  Cuando las consultas en la réplica secundaria bloquean un subproceso de puesta al día, se genera el evento XEvent **sqlserver.lock_redo_blocked** .  
  
###  <a name="indexing"></a><a name="bkmk_Indexing"></a> Indización  
 Para optimizar las cargas de trabajo de solo lectura en réplicas secundarias legibles, tal vez desee crear índices en las tablas de las bases de datos secundarias. Debido a que no se pueden realizar cambios de esquema o de datos en las bases de datos secundarias, cree los índices en las bases de datos principales y permita que los cambios se transfieran a la base de datos secundaria mediante el proceso de puesta al día.  
  
 Para supervisar la actividad de uso de índices en una réplica secundaria, consulte las columnas **user_seeks**, **user_scans** y **user_lookups** de la vista de administración dinámica [sys.dm_db_index_usage_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md) .  
  
###  <a name="statistics-for-read-only-access-databases"></a><a name="Read-OnlyStats"></a> Estadísticas de las bases de datos de acceso de solo lectura  
 Las estadísticas de las columnas de tablas y vistas indizadas se usan para optimizar los planes de consulta. Para los grupos de disponibilidad, las estadísticas que se crean y se mantienen en las bases de datos principales se conservan automáticamente en las bases de datos secundarias como parte de la aplicación de los registros de transacciones. No obstante, la carga de trabajo de solo lectura en las bases de datos secundarias puede necesitar estadísticas distintas de las que se crean en las bases de datos principales. Sin embargo, debido a que las bases de datos secundarias están restringidas al acceso de solo lectura, las estadísticas no se pueden crear en las bases de datos secundarias.  
  
 Para resolver este problema, la réplica secundaria crea y mantiene las estadísticas temporales para las bases de datos secundarias en **tempdb**. El sufijo _readonly_database_statistic se anexa al nombre de las estadísticas temporales para diferenciarlas de las estadísticas permanentes que se mantienen de la base de datos principal.  
  
 Solo [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] puede crear y actualizar las estadísticas temporales. No obstante, puede eliminar las estadísticas temporales y supervisar sus propiedades mediante las mismas herramientas que se usan para las estadísticas permanentes:  
  
-   Elimine las estadísticas temporales con la instrucción [DROP STATISTICS](../../../t-sql/statements/drop-statistics-transact-sql.md) de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Supervise las estadísticas con las vistas de catálogo **sys.stats** y **sys.stats_columns** . **sys_stats** incluye una columna, **is_temporary**, para indicar las estadísticas que son permanentes y las que son temporales.  
  
 No se permite la actualización de estadísticas automáticas para tablas con optimización de memoria en la réplica principal o secundaria. Debe supervisar el rendimiento de las consultas y planes en la réplica secundaria y actualizar manualmente las estadísticas de la réplica principal cuando sea necesario. Sin embargo, las estadísticas que faltan se crean automáticamente tanto en la réplica principal como en la secundaria.  
  
 Para obtener más información sobre las estadísticas de SQL Server, vea [Estadísticas](../../../relational-databases/statistics/statistics.md).  
  
 **En esta sección:**  
  
-   [Estadísticas permanentes obsoletas en bases de datos secundarias](#StalePermStats)  
  
-   [Limitaciones y restricciones](#StatsLimitationsRestrictions)  
  
####  <a name="stale-permanent-statistics-on-secondary-databases"></a><a name="StalePermStats"></a> Estadísticas permanentes obsoletas en bases de datos secundarias  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] detecta cuándo están obsoletas las estadísticas permanentes de una base de datos secundaria. Pero no se pueden realizar cambios en las estadísticas permanentes, excepto a través de los cambios en la base de datos principal. Para la optimización de consultas, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crea estadísticas temporales para tablas basadas en disco en la base de datos secundaria y usa estas estadísticas en lugar de las estadísticas en desuso permanentes.  
  
 Cuando las estadísticas permanentes se actualizan en la base de datos principal, se guardan automáticamente en la base de datos secundaria. A continuación [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa las estadísticas actualizadas permanentes, más actuales que las estadísticas temporales.  
  
 Si el grupo de disponibilidad conmuta por error, las estadísticas temporales se eliminan en todas las réplicas secundarias.  
  
####  <a name="limitations-and-restrictions"></a><a name="StatsLimitationsRestrictions"></a> Limitaciones y restricciones  
  
-   Debido a que las estadísticas temporales se almacenan en **tempdb**, el reinicio del servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provoca que desaparezcan todas las estadísticas temporales.  
  
-   El sufijo _readonly_database_statistic está reservado para las estadísticas que genera [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Este sufijo no se puede usar al crear estadísticas en una base de datos principal. Para más información, consulte [Estadísticas](../../../relational-databases/statistics/statistics.md).  
  
##  <a name="accessing-memory-optimized-tables-on-a-secondary-replica"></a><a name="bkmk_AccessInMemTables"></a> Obtener acceso a tablas optimizadas para memoria en una réplica secundaria  
 Los niveles de aislamiento de transacción que se pueden usar con tablas optimizadas para memoria en una réplica secundaria son los mismos que en la réplica principal. Se recomienda establecer el nivel de aislamiento de nivel de sesión en READ COMMITTED y establecer la opción de nivel de base de datos MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT en ON. Por ejemplo:  
  
```sql  
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
GO  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED  
GO  
SELECT SUM(UnitPrice*OrderQty)   
FROM Sales.SalesOrderDetail_inmem  
GO  
  
```  
  
##  <a name="capacity-planning-considerations"></a><a name="bkmk_CapacityPlanning"></a> Consideraciones de planeamiento de capacidad  
  
-   En el caso de las tablas basadas en disco, las réplicas secundarias legibles pueden requerir espacio en **tempdb** por dos motivos:  
  
    -   El nivel de aislamiento de instantánea copia las versiones de fila en **tempdb**.  
  
    -   Se crean estadísticas temporales para las bases de datos secundarias y se mantienen en **tempdb**. Las estadísticas temporales pueden causar un ligero aumento del tamaño de **tempdb**. Para obtener más información, vea [Estadísticas de las bases de datos de acceso de solo lectura](#Read-OnlyStats), más adelante en esta sección.  
  
-   Al configurar el acceso de lectura en una o varias réplicas secundarias, las bases de datos principales agregan 14 bytes de sobrecarga en las filas de datos eliminadas, modificadas o insertadas para almacenar punteros a versiones de fila en las bases de datos secundarias para tablas basadas en disco. Esta sobrecarga de 14 bytes se aplica a las bases de datos secundarias. A medida que se agrega la sobrecarga de 14 bytes a las filas de datos, se pueden producir divisiones de página.  
  
     Las bases de datos principales no generan los datos de las versiones de fila. En su lugar, las bases de datos secundarias generan las versiones de fila. Sin embargo, el control de versiones de fila aumenta el almacenamiento de datos tanto en las bases de datos principales como en las secundarias.  
  
     La adición de los datos de las versiones de fila depende del valor de nivel de aislamiento de instantánea o de aislamiento de instantánea de lectura confirmada (RCSI) en la base de datos principal. En la tabla siguiente se describe el comportamiento del control de versiones en una base de datos secundaria legible con configuraciones diferentes para las tablas basadas en disco.  
  
    |¿Réplica secundaria legible?|¿Nivel de aislamiento de instantánea o de RCSI habilitado?|Base de datos principal|Base de datos secundaria|  
    |---------------------------------|-----------------------------------------------|----------------------|------------------------|  
    |No|No|Sin versiones de fila ni sobrecarga de 14 bytes|Sin versiones de fila ni sobrecarga de 14 bytes|  
    |No|Sí|Con versiones de fila y sobrecarga de 14 bytes|Sin versiones de fila pero con sobrecarga de 14 bytes|  
    |Sí|No|Sin versiones de fila pero con sobrecarga de 14 bytes|Con versiones de fila y sobrecarga de 14 bytes|  
    |Sí|Sí|Con versiones de fila y sobrecarga de 14 bytes|Con versiones de fila y sobrecarga de 14 bytes|  
  
##  <a name="related-tasks"></a><a name="bkmk_RelatedTasks"></a> Tareas relacionadas  
  
-   [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurar el enrutamiento de solo lectura para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
-   [Ver las propiedades de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Usar el cuadro de diálogo Nuevo grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Blog del equipo Always On de SQL Server: el blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Acerca del acceso de conexión de cliente a réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [estadísticas](../../../relational-databases/statistics/statistics.md)  
  
