---
title: Directiva de conmutación por error para instancias de clústeres de conmutación por error
description: Una descripción de las distintas directivas de conmutación por error disponibles para una instancia de clúster de conmutación por error de SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- flexible failover policy
ms.assetid: 39ceaac5-42fa-4b5d-bfb6-54403d7f0dc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4816bae2ba4317970dd801b07499c3653e774d4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340027"
---
# <a name="failover-policy-for-failover-cluster-instances"></a>Directiva de conmutación por error para instancias de clústeres de conmutación por error
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En una instancia de clúster de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , solo puede haber un nodo propietario del grupo de recursos de clúster de conmutación por error de Windows Server (WSFC) en cada momento. Las solicitudes del cliente se sirven a través de este nodo de la FCI. En caso de que se produzca un error o un reinicio incorrecto, la propiedad del grupo se traslada a otro nodo de WSFC de la FCI. Este proceso se denomina conmutación por error. [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] aumenta la confiabilidad de la detección de errores y proporciona una directiva de conmutación por error flexible.  
  
 Una FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] depende del servicio de WSFC subyacente para la detección de conmutaciones por error. Por tanto, son dos los mecanismos que determinan el comportamiento de conmutación por error de la FCI: el primero es la funcionalidad nativa de WSFC y el segundo es la funcionalidad agregada por la instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   El clúster de WSFC mantiene la configuración de quórum, lo que garantiza un destino único en caso de una conmutación automática por error. El servicio de WSFC determina si el clúster se encuentra en un estado de quórum óptimo en todo momento y conecta y desconecta el grupo de recursos según convenga.  
  
-   La instancia activa de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] notifica periódicamente al grupo de recursos de WSFC un conjunto de diagnósticos de componentes a través de una conexión dedicada. El grupo de recursos de WSFC mantiene la directiva de conmutación por error, que define las condiciones de error que desencadenarán los reinicios y las conmutaciones por error.  
  
 En este tema se describe el segundo mecanismo mencionado anteriormente. Para obtener más información sobre el comportamiento de WSFC en la configuración de cuórum y la detección del estado, vea [Configuración de los votos y modos de cuórum WSFC &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
> [!IMPORTANT]  
>  Las conmutaciones por error automáticas que tienen como origen o destino FCI no están permitidas en el grupo de disponibilidad AlwaysOn. Pero las conmutaciones por error manuales que tienen como origen y destino FCI están permitidas en el grupo de disponibilidad AlwaysOn.  
  
##  <a name="failover-policy-overview"></a><a name="Concepts"></a> Información general sobre la directiva de migración tras error  
 El proceso de conmutación por error se puede descomponer en los pasos siguientes:  
  
1.  [Supervisar el estado de mantenimiento](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#monitor)  
  
2.  [Determinar los errores](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#determine)  
  
3.  [Responder a los errores](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#respond)  
  
###  <a name="monitor-the-health-status"></a><a name="monitor"></a> Supervisar el estado de mantenimiento  
 Hay tres tipos de estados de mantenimiento que se supervisan en la FCI:  
  
-   [El estado del servicio SQL Server](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#service)  
  
-   [Capacidad de respuesta de la instancia de SQL Server](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#instance)  
  
-   [Diagnósticos de componentes de SQL Server](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#component)  
  
####  <a name="state-of-the-sql-server-service"></a><a name="service"></a> El estado del servicio SQL Server  
 El servicio de WSFC supervisa el estado de inicio del servicio SQL Server del nodo activo de la FCI para detectar cuándo se detiene.  
  
####  <a name="responsiveness-of-the-sql-server-instance"></a><a name="instance"></a> Capacidad de respuesta de la instancia de SQL Server  
 Durante la instalación de SQL Server, el servicio de WSFC usa la DLL de recursos del motor de base de datos de SQL Server para crear una nueva conexión en un subproceso independiente que se usa exclusivamente para supervisar el estado de mantenimiento. Esto garantiza que en la instancia de SQL tiene los recursos necesarios para notificar su estado de mantenimiento mientras está sujeta a carga. Con esta conexión dedicada, SQL Server ejecuta el procedimiento almacenado del sistema [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) en modo de repetición para notificar periódicamente el estado de mantenimiento de los componentes de SQL Server a la DLL de recursos.  
  
 La DLL de recursos determina la capacidad de respuesta de la instancia de SQL mediante un tiempo de espera de comprobación de estado. La propiedad HealthCheckTimeout define cuánto tiempo deber esperar la DLL de recursos al procedimiento almacenado de sp_server_diagnostics antes de notificar al servicio de WSFC que la instancia de SQL no responde. Esta propiedad se puede configurar utilizando T-SQL, así como el complemento Administrador de clústeres de conmutación por error. Para obtener más información, vea [Configure HealthCheckTimeout Property Settings](../../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md). Los siguientes elementos describen cómo afecta esta propiedad al tiempo de espera y a la configuración del intervalo de repetición:  
  
-   La DLL de recursos llama al procedimiento almacenado sp_server_diagnostics y establece el intervalo de repetición en un tercio del valor de HealthCheckTimeout.  
  
-   Si el procedimiento almacenado sp_server_diagnostics es lento o no está devolviendo información, la DLL de recursos esperará el intervalo especificado por HealthCheckTimeout antes de notificar al servicio WSFC que la instancia de SQL no responde.  
  
-   Si se pierde la conexión dedicada, la DLL de recursos intentará establecer conexión con la instancia de SQL durante el intervalo especificado por HealthCheckTimeout antes de notificar al servicio de WSFC que la instancia de SQL no responde.  
  
####  <a name="sql-server-component-diagnostics"></a><a name="component"></a> Diagnósticos de componentes de SQL Server  
 El procedimiento almacenado del sistema sp_server_diagnostics recopila periódicamente diagnósticos de componentes de la instancia de SQL. La información de diagnóstico que se recopila se exhibe como una fila para cada uno de los componentes siguientes y se pasa al subproceso que realiza la llamada.  
  
1.  sistema  
  
2.  resource  
  
3.  query process  
  
4.  io_subsystem  
  
5.  events  
  
 Los componentes system, resource y query process se utilizan para la detección de errores. Los componentes io_subsytem y events solo se utilizan con fines de diagnóstico.  
  
 Cada conjunto de filas de información también se escribe en el registro de diagnósticos de clúster del servidor SQL Server. Para obtener más información, vea [Ver y leer el registro de diagnósticos de la instancia de clúster de conmutación por error](../../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md).  
  
> [!TIP]  
>  Aunque el procedimiento almacenado sp_server_diagnostic se usa en la tecnología AlwaysOn de SQL Server, puede usarse en cualquier instancia de SQL Server para ayudar a detectar y solucionar problemas.  
  
####  <a name="determining-failures"></a><a name="determine"></a> Determinar los errores  
 La DLL de recursos del motor de base de datos de SQL Server determina si el estado de mantenimiento detectado es una condición de error mediante la propiedad FailureConditionLevel. La propiedad FailureConditionLevel define los estados de mantenimiento detectados que causan reinicios o conmutaciones por error. Hay varios niveles de opciones disponibles, que van desde el inicio o conmutación por error no automáticos hasta todas las posibles condiciones de error que generan un reinicio o conmutación por error automáticos. Para obtener más información acerca de cómo configurar esta propiedad, vea [Configure FailureConditionLevel Property Settings](../../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).  
  
 Las condiciones de error se establecen en una escala creciente. Para los niveles 1 a 5, cada nivel incluye todas las condiciones de los niveles anteriores además de sus propias condiciones. Esto significa que con cada nivel, hay mayor probabilidad de que se produzca una conmutación por error o un reinicio. Los niveles de la condición de error se describen en la siguiente tabla.  
  
 Consulte [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md), ya que este procedimiento almacenado del sistema juega un papel importante en los niveles de condiciones de error.  
  
|Nivel|Condición|Descripción|  
|-----------|---------------|-----------------|  
|0|No hay conmutación automática por error ni reinicio|Indica que no se activará la conmutación por error ni el reinicio automáticamente si se dan condiciones de error. Este nivel solo tiene como finalidad el mantenimiento del sistema.|  
|1|Conmutación por error o reinicio si el servidor tiene error|Indica que se reiniciará el servidor o se desencadenará una conmutación por error si se produce la siguiente condición:<br /><br /> El servicio SQL Server se apaga.|  
|2|Conmutación por error o  reinicio si el servidor no responde|Indica que se reiniciará el servidor o se desencadenará una conmutación por error si se produce alguna de las condiciones siguientes:<br /><br /> El servicio SQL Server se apaga.<br /><br /> La instancia de SQL Server no responde (la DLL de recursos no puede recibir datos de sp_server_diagnostics en el intervalo establecido por la configuración de HealthCheckTimeout).|  
|3*|Conmutación por error o reinicio si hay errores de servidor críticos|Indica que se reiniciará el servidor o se desencadenará una conmutación por error si se produce alguna de las condiciones siguientes:<br /><br /> El servicio SQL Server se apaga.<br /><br /> La instancia de SQL Server no responde (la DLL de recursos no puede recibir datos de sp_server_diagnostics en el intervalo establecido por la configuración de HealthCheckTimeout).<br /><br /> El procedimiento almacenado del sistema sp_server_diagnostics devuelve "error del sistema".|  
|4|Conmutación por error o reinicio si hay errores de servidor moderados|Indica que se reiniciará el servidor o se desencadenará una conmutación por error si se produce alguna de las condiciones siguientes:<br /><br /> El servicio SQL Server se apaga.<br /><br /> La instancia de SQL Server no responde (la DLL de recursos no puede recibir datos de sp_server_diagnostics en el intervalo establecido por la configuración de HealthCheckTimeout).<br /><br /> El procedimiento almacenado del sistema sp_server_diagnostics devuelve "error del sistema".<br /><br /> El procedimiento almacenado del sistema sp_server_diagnostics devuelve "error de recurso".|  
|5|Conmutación por error o reinicio si se produce cualquier condición de error calificada|Indica que se reiniciará el servidor o se desencadenará una conmutación por error si se produce alguna de las condiciones siguientes:<br /><br /> El servicio SQL Server se apaga.<br /><br /> La instancia de SQL Server no responde (la DLL de recursos no puede recibir datos de sp_server_diagnostics en el intervalo establecido por la configuración de HealthCheckTimeout).<br /><br /> El procedimiento almacenado del sistema sp_server_diagnostics devuelve "error del sistema".<br /><br /> El procedimiento almacenado del sistema sp_server_diagnostics devuelve "error de recurso".<br /><br /> El procedimiento almacenado del sistema sp_server_diagnostics devuelve "error de query_processing".|  
  
 *Valor predeterminado  
  
####  <a name="responding-to-failures"></a><a name="respond"></a> Responder a los errores  
 Una vez que se detectan una o varias condiciones de error, el modo en que responda el servicio de WSFC a los errores dependerá del estado de quórum de WSFC y de la configuración de reinicio y conmutación por error del grupo de recursos de la FCI. Si la FCI ha perdido el quórum de WSFC, toda la FCI se queda sin conexión y deja de tener una disponibilidad elevada. Si la FCI todavía mantiene el quórum de WSFC, el servicio de WSFC puede responder primero intentando reiniciar el nodo con errores y, si el intento de reinicio no tiene éxito, intentando la conmutación por error. La configuración de reinicio y conmutación por error se establece en el complemento Administrador de clústeres de conmutación por error. Para obtener más información sobre esta configuración, vea [Propiedades de \<Resource>: ficha Directivas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725685(v=ws.11)).  
  
 Para obtener más información sobre el mantenimiento del cuórum, vea [Configuración de los votos y modos de cuórum WSFC &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-configuration-transact-sql.md)  
  
