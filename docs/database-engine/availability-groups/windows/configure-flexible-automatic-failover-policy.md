---
title: Configuración de una directiva de conmutación automática por error flexible para un grupo de disponibilidad
description: Se describe cómo configurar una directiva de conmutación automática por error flexible para un grupo de disponibilidad Always On mediante Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.date: 11/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], flexible failover policy
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 1ed564b4-9835-4245-ae35-9ba67419a4ce
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.custom: seo-lt-2019
ms.openlocfilehash: 487b155561944e1303aac0fdb7453fc2d494802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342448"
---
# <a name="configure-a-flexible-automatic-failover-policy-for-an-always-on-availability-group"></a>Configuración de una directiva de conmutación automática por error flexible para un grupo de disponibilidad Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  En este tema se describe cómo configurar la directiva de conmutación por error flexible para un grupo de disponibilidad AlwaysOn mediante [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell en SQL Server. Una directiva de conmutación por error flexible proporciona mayor control sobre las condiciones que produce una [conmutación automática por error](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) para un grupo de disponibilidad. Al cambiar las condiciones de error que activan una conmutación automática por error y la frecuencia de comprobaciones de estado, se puede aumentar o reducir la probabilidad de una conmutación automática por error que sea compatible con su SLA por tener alta disponibilidad.  

   La directiva flexible de conmutación por error de un grupo de disponibilidad se define por su nivel de condición de error y el umbral de tiempo de espera de comprobación de estado. Al detectar que un grupo de disponibilidad ha superado su nivel de condición de error o su umbral de tiempo de espera de comprobación de estado, la DLL de recursos del grupo de disponibilidad responde a los clústeres de conmutación por error de Windows Server (WSFC). El clúster WSFC entonces inicia una conmutación automática por error a la réplica secundaria.  
 
  > [!NOTE]  
  > La directiva flexible de conmutación por error de un grupo de disponibilidad no se puede configurar utilizando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 
## <a name="limitations-on-automatic-failovers"></a><a name="Limitations"></a> Limitaciones de la conmutación automática por error  
  
-   Para que se produzca la conmutación automática por error, la réplica principal actual y una réplica secundaria deben estar configuradas para el modo de confirmación sincrónica con conmutación automática por error, y la réplica secundaria debe estar sincronizada con la réplica principal.  
  
-   Solo se admiten tres réplicas en la conmutación automática por error.  
  
-   Si un grupo de disponibilidad supera su umbral de error de WSFC, el clúster de WSFC no intentará una conmutación automática por error para dicho grupo. Además, el grupo de recursos de WSFC del grupo de disponibilidad permanece en un estado de error hasta que el administrador del clúster conecta manualmente el grupo de recursos con error o el administrador de la base de datos realiza una conmutación por error manual del grupo de disponibilidad. El *umbral de error de WSFC* se define como el número máximo de errores admitidos para el grupo de disponibilidad en un período de tiempo determinado. El período predeterminado es de seis horas, y el valor predeterminado del número máximo de errores durante este período es *n*-1, donde *n* es el número de nodos de WSFC. Para cambiar los valores del umbral de error de un grupo de disponibilidad determinado, use la consola del administrador de conmutación por error de WSFC.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal.  
   
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
  
|Tarea|Permisos|  
|----------|-----------------|  
|Para configurar la directiva flexible de conmutación por error para un nuevo grupo de disponibilidad|Se requiere la pertenencia al rol fijo de servidor **sysadmin** y el permiso de servidor CREATE AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
|Para modificar la directiva de un grupo de disponibilidad|Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  

##  <a name="health-check-timeout-threshold"></a><a name="HCtimeout"></a> Umbral de tiempo de espera de comprobación de estado  
 La DLL de recursos del clúster WSFC del grupo de disponibilidad realiza una *comprobación de estado* de la réplica principal, llamando al procedimiento almacenado [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) en la instancia de SQL Server que hospeda la réplica principal. **sp_server_diagnostics** devuelve resultados en un intervalo que iguala 1/3 del umbral de tiempo de espera de comprobación de estado del grupo de disponibilidad. El umbral de tiempo de espera de comprobación de estado predeterminado es 30 segundos, lo que hace que **sp_server_diagnostics** devuelva en un intervalo de 10 segundos. Si **sp_server_diagnostics** es lento o no devuelve información, la DLL de recursos esperará al intervalo completo del umbral de tiempo de espera de comprobación de estado antes de determinar que la réplica primaria no responde. Si la réplica primaria no responde, se inicia una conmutación automática por error, si se admite actualmente.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** no realiza comprobaciones de estado en el nivel de base de datos.  
  
##  <a name="failure-condition-level"></a><a name="FClevel"></a> Nivel de condición de error  
 Si **sp_server_diagnostics** devuelve los datos de diagnóstico y la información de estado, se garantiza que una conmutación automática por error dependa del nivel de condición de error del grupo de disponibilidad. El *nivel de condición de error* especifica qué condiciones de error desencadenan una conmutación automática por error. Hay cinco niveles de condición de error que abarcan desde el nivel menos restrictivo (nivel uno) al más restrictivo (nivel cinco). Un nivel dado abarca los niveles menos restrictivos. Así pues, el nivel de condición más estricto, cinco, incluye las condiciones menos restrictivas, y así sucesivamente.  
  
> [!IMPORTANT]  
>  Ningún nivel de condición de error detecta las bases de datos dañadas ni las bases de datos sospechosas. Por tanto, una base de datos que está dañada o es sospechosa (debido a un error de hardware, a daños en los datos o a otro problema) nunca desencadena una conmutación automática por error.  
  
 En la tabla siguiente se describen las condiciones de error correspondientes a cada nivel.  
  
|Nivel|Condición de error|[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valor|Valor de PowerShell|  
|-----------|-----------------------|------------------------------|----------------------|  
|Uno|Por inactividad de servidor. Especifica que se inicia una conmutación automática por error en alguno de los casos siguientes:<br /><br /> El servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] está inactivo.<br /><br /> La concesión del grupo de disponibilidad para conectarse al clúster de WSFC expira porque no se ha recibido ninguna confirmación de la instancia del servidor. Para obtener más información, vea [Cómo funciona: tiempo de espera de concesión de AlwaysOn de SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).<br /><br /> <br /><br /> Este es el nivel menos restrictivo.|1|**OnServerDown**|  
|Dos|Al dejar de responder el servidor. Especifica que se inicia una conmutación automática por error en alguno de los casos siguientes:<br /><br /> La instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no se conecta al clúster y se ha superado el umbral de tiempo de espera de comprobación de estado del grupo de disponibilidad definido por el usuario.<br /><br /> La réplica de disponibilidad tiene un estado de error.|2|**OnServerUnresponsive**|  
|tres|En errores de servidor críticos. Especifica que se inicia una conmutación automática por error en caso de errores internos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] graves, como bloqueos por subproceso huérfanos, infracciones serias de acceso de escritura o volcado excesivo.<br /><br /> Es el nivel predeterminado.|3|**OnCriticalServerError**|  
|Cuatro|En errores de servidor moderados. Especifica que se inicia una conmutación automática por error en caso de errores internos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] moderados, tales como una condición persistente de falta de memoria en el grupo de recursos de servidor interno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|4|**OnModerateServerError**|  
|Cinco|En cualquier condición de error apta. Especifica que se inicia una conmutación automática por error en el caso de condiciones de error aptas, incluidas las siguientes:<br /><br /> Detección de un interbloqueo del programador.<br /><br /> Detección de un interbloqueo irresoluble.<br /><br /> <br /><br /> Es el nivel más restrictivo.|5|**OnAnyQualifiedFailureConditions**|  
  
> [!NOTE]  
>  La falta de respuesta de una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a solicitudes del cliente no es pertinente a los grupos de disponibilidad.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para configurar la directiva flexible de conmutación por error**  
  
1.  Conéctese a la instancia del servidor que hospeda la réplica principal.  
  
2.  Para un nuevo grupo de disponibilidad, use la instrucción [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Si va a modificar un grupo de disponibilidad existente, use la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Para establecer el nivel de la condición de conmutación por error, use la opción FAILURE_CONDITION_LEVEL = *n* , donde *n* es un entero entre 1 y 5.  
  
         Por ejemplo, la siguiente instrucción de [!INCLUDE[tsql](../../../includes/tsql-md.md)] cambia el nivel de condición de error de un grupo de disponibilidad existente, `AG1`, al nivel uno:  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1);  
        ```  
  
         La relación de estos valores enteros con los niveles de condición de error es la siguiente:  
  
        |[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valor|Nivel|La conmutación por error iniciada es automática cuando se produce...|  
        |------------------------------|-----------|-------------------------------------------|  
        |1|Uno|Por inactividad de servidor. El servicio SQL Server se detiene debido a una conmutación por error o reinicio.|  
        |2|Dos|Al dejar de responder el servidor. Se cumple cualquier condición de un valor inferior, el servicio SQL Server se conecta al clúster y se supera el umbral del tiempo de espera de comprobación de estado, o la réplica principal actual está en un estado de error.|  
        |3|tres|En errores de servidor críticos. Se cumple cualquier condición de un valor inferior o se produce un error de servidor crítico interno.<br /><br /> Es el nivel predeterminado.|  
        |4|Cuatro|En errores de servidor moderados. Se cumple cualquier condición de un valor inferior o se produce un error de servidor moderado.|  
        |5|Cinco|En cualquier condición de error apta. Se cumple cualquier condición de un valor inferior o se produce una condición de error adecuada.|  
  
         Para obtener más información sobre los niveles de condición de conmutación por error, vea [Directiva de conmutación por error flexible para conmutación automática por error de un grupo de disponibilidad &#40;SQL Server&#41;]().  
  
    -   Para configurar el umbral del tiempo de espera de comprobación de estado, use la opción HEALTH_CHECK_TIMEOUT = *n* , donde *n* es un entero de 15 000 milisegundos (15 segundos) a 4 294 967 295 milisegundos. El valor predeterminado es 30000 milisegundos (30 segundos).  
  
         Por ejemplo, la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] cambia el umbral de tiempo de espera de comprobación de estado de un grupo de disponibilidad disponible, `AG1`, 60.000 milisegundos (un minuto).  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT = 60000);  
        ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para configurar la directiva flexible de conmutación por error**  
  
1.  Establezca el valor predeterminado (**cd**) en la instancia del servidor que hospeda la réplica principal.  
  
2.  Para agregar una réplica de disponibilidad a un grupo de disponibilidad, use el cmdlet **New-SqlAvailabilityGroup** . Para modificar una réplica de disponibilidad existente, use el cmdlet **Set-SqlAvailabilityGroup** .  
  
    -   Para establecer el nivel de condición de conmutación por error, utilice el parámetro **FailureConditionLevel**_level_ , donde *level* es uno de los siguientes valores:  
  
        |Value|Nivel|La conmutación por error iniciada es automática cuando se produce...|  
        |-----------|-----------|-------------------------------------------|  
        |**OnServerDown**|Uno|Por inactividad de servidor. El servicio SQL Server se detiene debido a una conmutación por error o reinicio.|  
        |**OnServerUnresponsive**|Dos|Al dejar de responder el servidor. Se cumple cualquier condición de un valor inferior, el servicio SQL Server se conecta al clúster y se supera el umbral del tiempo de espera de comprobación de estado, o la réplica principal actual está en un estado de error.|  
        |**OnCriticalServerError**|tres|En errores de servidor críticos. Se cumple cualquier condición de un valor inferior o se produce un error de servidor crítico interno.<br /><br /> Es el nivel predeterminado.|  
        |**OnModerateServerError**|Cuatro|En errores de servidor moderados. Se cumple cualquier condición de un valor inferior o se produce un error de servidor moderado.|  
        |**OnAnyQualifiedFailureConditions**|Cinco|En cualquier condición de error apta. Se cumple cualquier condición de un valor inferior o se produce una condición de error adecuada.|  
  
         Para obtener más información sobre los niveles de condición de conmutación por error, vea [Directiva de conmutación por error flexible para conmutación automática por error de un grupo de disponibilidad &#40;SQL Server&#41;]().  
  
         Por ejemplo, el siguiente comando cambia el nivel de condición de error de un grupo de disponibilidad existente, `AG1`al nivel uno.  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `   
        -FailureConditionLevel OnServerDown  
        ```  
  
    -   Para establecer el umbral de tiempo de espera de comprobación de estado, use el parámetro **HealthCheckTimeout**_n_ , donde *n* es un entero de 15 000 milisegundos (15 segundos) a 4 294 967 295 milisegundos. El valor predeterminado es 30000 milisegundos (30 segundos).  
  
         Por ejemplo, el siguiente comando cambia el umbral de tiempo de espera de comprobación de estado de un grupo de disponibilidad disponible, `AG1`, a 120.000 milisegundos (dos minutos).  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
        -HealthCheckTimeout 120000  
        ```  
  
> [!NOTE]  
>  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  

##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para configurar la conmutación automática por error**  
  
-   [Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md) (la conmutación automática por error necesita el modo de disponibilidad de confirmación sincrónica)  
  
-   [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Configurar la directiva de conmutación por error flexible para controlar las condiciones de la conmutación automática por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Cómo funciona: tiempo de espera de concesión de AlwaysOn de SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Directiva de conmutación por error para instancias de clústeres de conmutación por error](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)  
  
