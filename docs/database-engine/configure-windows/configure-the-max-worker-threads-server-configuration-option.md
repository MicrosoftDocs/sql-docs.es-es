---
title: Establecer la opción de configuración del servidor Máximo de subprocesos de trabajo | Microsoft Docs
description: Descubra cómo usar la opción "max worker threads" para configurar el número de subprocesos de trabajo disponibles en SQL Server a fin de procesar determinadas solicitudes.
ms.custom: contperf-fy20q4
ms.date: 04/14/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- worker threads [SQL Server]
- max worker threads option
ms.assetid: abeadfa4-a14d-469a-bacf-75812e48fac1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5291b502ec70be8b86c6806e14404664d0b0831
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100347005"
---
# <a name="configure-the-max-worker-threads-server-configuration-option"></a>Establecer la opción de configuración del servidor Máximo de subprocesos de trabajo
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo establecer la opción de configuración del servidor **máximo de subprocesos de trabajo** en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción **max worker threads** configura el número de subprocesos de trabajo disponibles en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para procesar solicitudes de consulta, inicio de sesión, cierre de sesión y solicitudes de aplicación similares.


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza los servicios de subprocesos nativos de los sistemas operativos para garantizar las condiciones siguientes:

- Uno o más subprocesos admiten simultáneamente cada red que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite.

- Un subproceso controla los puntos de control de base de datos.

- Un grupo de subprocesos controla todos los usuarios.

El valor predeterminado de **máximo de subprocesos de trabajo** es 0. Esto permite a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configurar automáticamente el número de subprocesos de trabajo en el inicio. El valor predeterminado es el más adecuado para la mayor parte de los sistemas. No obstante, dependiendo de la configuración del sistema, el uso de un valor concreto para **máximo de subprocesos de trabajo** en ocasiones puede mejorar el rendimiento.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El número real de solicitudes de consulta puede superar el número establecido en **max worker threads**, en cuyo caso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agrupa los subprocesos de trabajo de manera que el siguiente subproceso de trabajo disponible pueda controlar la solicitud. Un subproceso de trabajo se asigna solo a las solicitudes activas y se libera una vez que se ha atendido la solicitud. Esto sucede incluso si la sesión de usuario o la conexión en la que se ha realizado la solicitud permanece abierta. 

-   La opción de configuración del servidor **max worker threads** no limita todos los subprocesos que pueden generarse en el motor. Los subprocesos del sistema necesarios para tareas como Escritura diferida, Punto de control, Escritura de registro, Service Broker, Administrador de bloqueos u otras se generan fuera de este límite. Los grupos de disponibilidad usan algunos de los subprocesos de trabajo en el **límite máximo de subprocesos de trabajo**, pero también utilizan subprocesos del sistema (vea [Uso de subprocesos por parte de los grupos de disponibilidad](../availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md#ThreadUsage)). Si se supera el número de subprocesos configurados, la siguiente consulta proporcionará información sobre las tareas del sistema que han generado los subprocesos adicionales.  
  
 ```sql  
 SELECT  s.session_id, r.command, r.status,  
    r.wait_type, r.scheduler_id, w.worker_address,  
    w.is_preemptive, w.state, t.task_state,  
    t.session_id, t.exec_context_id, t.request_id  
 FROM sys.dm_exec_sessions AS s  
 INNER JOIN sys.dm_exec_requests AS r  
    ON s.session_id = r.session_id  
 INNER JOIN sys.dm_os_tasks AS t  
    ON r.task_address = t.task_address  
 INNER JOIN sys.dm_os_workers AS w  
    ON t.worker_address = w.worker_address  
 WHERE s.is_user_process = 0;  
 ```  
  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si sospecha que hay un problema de rendimiento, probablemente no se deba a la disponibilidad de subprocesos de trabajo. Es más probable que la causa esté relacionada con las actividades que ocupan los subprocesos de trabajo y no los liberan. Entre los ejemplos se incluyen consultas de ejecución prolongada o cuellos de botella en el sistema (E/S, bloqueo, tiempos de espera de bloqueo temporal y esperas de red), que provocan consultas de espera prolongada. Lo mejor es buscar la causa principal de un problema de rendimiento antes de cambiar el valor de configuración de máximo de subprocesos de trabajo. Para obtener más información sobre cómo evaluar el rendimiento, vea [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md).
  
-   La agrupación de subprocesos permite optimizar el rendimiento cuando un gran número de clientes se conecta al servidor. Normalmente, se crea un subproceso del sistema operativo independiente para cada solicitud de la consulta. Sin embargo, cuando hay cientos de conexiones al servidor, el uso de un subproceso por solicitud de consulta puede consumir grandes cantidades de recursos del sistema. La opción de **máximo de subprocesos de trabajo** permite que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cree un grupo de subprocesos de trabajo para atender un gran número de solicitudes de consulta, lo que mejora el rendimiento.  
  
-   En la tabla siguiente se muestra el número configurado automáticamente de máximo de subprocesos de trabajo (cuando el valor se establece en 0) basado en diferentes combinaciones de CPU, arquitectura de equipo y versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mediante la fórmula: **_Trabajos máximos predeterminados_ + ((* CPU lógicas* - 4) * *Trabajos por CPU*)**.  
  
    |Número de CPU|Equipo de 32 bits (hasta [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])|Equipo de 64 bits (hasta [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1)|Equipo de 64 bits (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])|   
    |------------|------------|------------|------------|  
    |\<= 4|256|512|512|   
    |8|288|576|576|   
    |16|352|704|704|   
    |32|480|960|960|   
    |64|736|1472|1472|   
    |128|1248|2496|4480|   
    |256|2272|4544|8576|   
    
    Hasta [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, los *trabajos por CPU* solo dependen de la arquitectura (32 bits o 64 bits):
    
    |Número de CPU|Equipo de 32 bits <sup>1</sup>|Equipo de 64 bits|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4|256 + ((CPU lógicas - 4) * 8)|512 <sup>2</sup> + [(CPU lógicas - 4) * 16]|   
    
    A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], los *Trabajos por CPU* dependen de la arquitectura y el número de procesadores (entre 4 y 64, o superior a 64):
    
    |Número de CPU|Equipo de 32 bits <sup>1</sup>|Equipo de 64 bits|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4 y \<= 64|256 + ((CPU lógicas - 4) * 8)|512 <sup>2</sup> + [(CPU lógicas - 4) * 16]|   
    |\> 64|256 + [(CPU lógicas - 4) * 32]|512 <sup>2</sup> + [(CPU lógicas - 4) * 32]|   
  
    <sup>1</sup> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ya no se puede instalar en un sistema operativo de 32 bits. Se muestran los valores de equipo de 32 bits como ayuda para los clientes que ejecutan [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones anteriores. Se recomienda 1024 como número máximo de subprocesos de trabajo para una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en un equipo de 32 bits.
    
    <sup>2</sup> A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], el valor de *trabajos máximos predeterminados* se divide por 2 con menos de 2 GB de memoria.
  
    > [!TIP]  
    > Para obtener recomendaciones sobre el uso de más de 64 CPU, vea [Prácticas recomendadas para ejecutar SQL Server en equipos que tienen más de 64 CPU](../../relational-databases/thread-and-task-architecture-guide.md#best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus).  
  
-   Si todos los subprocesos de trabajo están activos con consultas de ejecución prolongada, puede parecer que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no responde hasta que finaliza un subproceso de trabajo y vuelve a estar disponible. Aunque no se trata de un defecto, puede que a veces este comportamiento no sea deseable. Si un proceso parece no responder y no se pueden procesar nuevas consultas, conéctese a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante la conexión de administrador dedicada (DAC) y finalice el proceso. Para impedir este comportamiento, aumente el número máximo de subprocesos de trabajo.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción `RECONFIGURE`, un usuario debe tener el permiso `ALTER SETTINGS` en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso `ALTER SETTINGS` de forma implícita.  
  
##  <a name="using-ssmanstudiofull"></a><a name="SSMSProcedure"></a> Usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Para configurar la opción de máximo de subprocesos de trabajo  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Procesadores** .  
  
3.  En el cuadro **Máximo de subprocesos de trabajo**, escriba o seleccione un valor entre 128 y 32 767.  
  
> [!TIP]
> Utilice la opción **max worker threads** para configurar el número de subprocesos de trabajo disponibles para procesos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El valor predeterminado de la opción **max worker threads** es el óptimo para la mayor parte de los sistemas. No obstante, dependiendo de la configuración del sistema, el uso de un valor inferior para el **máximo de subprocesos de trabajo** puede mejorar el rendimiento a veces.
> Vea las [Recomendaciones](#Recommendations) en esta página para obtener más información.
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Para configurar la opción de máximo de subprocesos de trabajo  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para configurar la opción `max worker threads` en `900`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'max worker threads', 900 ;  
GO  
RECONFIGURE;  
GO  
```  
  
##  <a name="follow-up-after-you-configure-the-max-worker-threads-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de máximo de subprocesos de trabajo  
 El cambio se aplicará inmediatamente después de ejecutar [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md), sin necesidad de reiniciar [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Conexión de diagnóstico para administradores de bases de datos](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)
