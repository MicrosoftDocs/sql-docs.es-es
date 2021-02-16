---
title: Supervisar la creación de reflejo de la base de datos (SQL Server) | Microsoft Docs
description: Obtenga información sobre el Monitor de creación de reflejo de la base de datos, los procedimientos almacenados del sistema y cómo funciona la supervisión de la creación de reflejo, incluido el trabajo Monitor de creación de reflejo de la base de datos.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring [SQL Server], database mirroring
- database mirroring [SQL Server], monitoring
ms.assetid: a7b1b9b0-7c19-4acc-9de3-3a7c5e70694d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5dc7d451b3e6b26f06bb5273d5f043d9aff0d463
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343299"
---
# <a name="monitoring-database-mirroring-sql-server"></a>Supervisar la creación de reflejo de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En esta sección se presentan el Monitor de creación de reflejo de la base de datos y los procedimientos almacenados del sistema **sp_dbmmonitor** , se explica el funcionamiento de la supervisión de la creación de reflejo de la base de datos (incluido el **Trabajo del Monitor de creación de reflejo de la base de datos**) y se resume la información que se puede supervisar sobre las sesiones de creación de reflejo de la base de datos. Además, en esta sección se describe cómo definir umbrales de advertencia para un conjunto de eventos de creación de reflejo de la base de datos predefinidos y cómo configurar alertas sobre cualquier evento de creación de reflejo de la base de datos.  
  
 Se puede supervisar una base de datos reflejada durante una sesión de creación de reflejo para comprobar si hay flujo de datos y si éste es correcto. Para configurar y administrar la supervisión de una o varias bases de datos reflejadas en una instancia de servidor, se puede usar el Monitor de creación de reflejo de la base de datos o los procedimientos almacenados del sistema **sp_dbmmonitor** .  
  
 El trabajo de supervisión, **Trabajo del Monitor de creación de reflejo de la base de datos**, funciona en segundo plano, de forma independiente del Monitor de creación de reflejo de la base de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] llama al **Trabajo del Monitor de creación de reflejo de la base de datos** a intervalos regulares (el valor predeterminado es una vez por minuto) y el trabajo llama a un procedimiento almacenado que actualiza el estado de la creación de reflejo. Si usa [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para iniciar una sesión de creación de reflejo, el **Trabajo del Monitor de creación de reflejo de la base de datos** se crea automáticamente. Pero si solo usa ALTER DATABASE *<database_name>* SET PARTNER para iniciar la creación de reflejo, debe crear el trabajo ejecutando un procedimiento almacenado.  
  
 **En este tema:**  
  
-   [Supervisar el estado de la creación de reflejo](#MonitoringStatus)  
  
-   [Fuentes adicionales de información acerca de una base de datos reflejada](#AdditionalSources)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="monitoring-mirroring-status"></a><a name="MonitoringStatus"></a> Supervisar el estado de la creación de reflejo  
 Para configurar y administrar la supervisión de una o varias bases de datos reflejadas en una instancia de servidor, se puede usar el Monitor de creación de reflejo de la base de datos o los procedimientos almacenados del sistema **dbmmonitor** . Se puede supervisar una base de datos reflejada durante una sesión de creación de reflejo para comprobar si hay flujo de datos y si éste es correcto.  
  
 En concreto, la supervisión de una base de datos reflejada permite:  
  
-   Comprobar si la creación de reflejo funciona.  
  
     El estado básico incluye saber si las dos instancias de servidor están activas, los servidores conectados y el registro se ha movido desde el servidor principal al reflejado.  
  
-   Determinar si la base de datos reflejada se mantiene al nivel de la base de datos principal.  
  
     En el modo de alto rendimiento, un servidor principal puede desarrollar un registro de entradas pendientes del registro no enviadas que se van a enviar desde el servidor principal al reflejado. Además, en cualquier modo de operación, el servidor reflejado puede desarrollar un registro de entradas pendientes del registro que se han escrito en el archivo de registro, pero se deben restaurar en la base de datos reflejada.  
  
-   Determinar la pérdida de datos cuando la instancia de servidor principal deja de estar disponible en el modo de alto rendimiento.  
  
     Se puede determinar la pérdida de datos examinando la cantidad del registro de transacciones no enviadas (si existe) y el intervalo de tiempo en el que el servidor principal confirmó las transacciones perdidas.  
  
-   Comparar el rendimiento actual con el anterior.  
  
     Cuando se producen problemas, el administrador de la base de datos puede ver un historial del rendimiento de la creación de reflejo para obtener un mayor conocimiento del estado actual. El examen del historial permite al usuario detectar tendencias en el rendimiento e identificar patrones de problemas de rendimiento (como, por ejemplo, las horas del día en que la red se ralentiza o si el número de comandos que se escriben en el registro es demasiado grande).  
  
-   Solucionar el flujo de datos reducido entre los asociados de la creación de reflejo.  
  
-   Establecer umbrales de advertencia en las estadísticas de rendimiento clave.  
  
     Si una nueva fila de estado contiene un valor que excede un umbral, se envía un evento informativo al registro de eventos de Windows. A continuación, un administrador del sistema puede configurar manualmente las alertas basadas en estos eventos. Para obtener más información, vea [Usar alertas y umbrales de advertencia de las métricas de rendimiento de la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
###  <a name="tools-for-monitoring-database-mirroring-status"></a><a name="tools_for_monitoring_dbm_status"></a> Herramientas para la supervisión del estado de la creación de reflejo de la base de datos  
 Se puede supervisar el estado de la creación de reflejo mediante el Monitor de creación de reflejo de la base de datos o el procedimiento almacenado del sistema **sp_dbmmonitorresults** . Los administradores del sistema, es decir, los miembros del rol fijo de servidor **sysadmin** , y los usuarios que un administrador del sistema ha agregado al rol fijo de base de datos **dbm_monitor** de la base de datos **msdb** , pueden usar estas herramientas para supervisar la creación de reflejo de la base de datos en cualquier base de datos reflejada de la instancia de servidor local. Cuando se utilizan, un administrador del sistema puede actualizar manualmente el estado de la creación de reflejo.  
  
> [!NOTE]  
>  Los administradores del sistema también pueden configurar y ver los umbrales de advertencia para examinar las estadísticas de rendimiento clave. Para obtener más información, vea [Usar alertas y umbrales de advertencia de las métricas de rendimiento de la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
-   Monitor de creación de reflejo de la base de datos  
  
     El Monitor de creación de reflejo de la base de datos es una herramienta de la interfaz gráfica de usuario que permite a los administradores del sistema ver y actualizar el estado, y configurar los umbrales de advertencia en varias estadísticas de rendimiento clave. Los miembros del rol fijo de base de datos **dbm_monitor** también pueden usar el Monitor de creación de reflejo de la base de datos para ver la fila más reciente de la tabla de estado de la creación de reflejo, aunque no puedan actualizarla.  
  
     En la página con pestañas **Estado** , el monitor muestra el estado de una base de datos seleccionada, incluidas las estadísticas de rendimiento. El contenido de esta página procede de las instancias de servidor principal y reflejado. La página se actualiza de forma asincrónica, a medida que se recopila la información del estado a través de conexiones independientes en las instancias de servidor principal y reflejado. El Monitor intenta actualizar la tabla de estado a intervalos de 30 segundos. La actualización solo se realiza correctamente si la tabla no se ha actualizado en un intervalo de 15 segundos y el usuario es miembro del rol fijo de servidor **sysadmin** . Para obtener un resumen de la información de la página **Estado** , vea [Estado que muestra el Monitor de creación de reflejo de la base de datos](#perf_metrics_of_dbm_monitor), más adelante en este tema.  
  
     Para obtener una introducción a la interfaz del Monitor de creación de reflejo de la base de datos, vea [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md). Para obtener más información sobre cómo iniciar el Monitor de creación de reflejo de base de datos, vea [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md).  
  
-   Procedimientos almacenados del sistema  
  
     También puede recuperar o actualizar el estado actual si ejecuta el procedimiento almacenado del sistema **sp_dbmmonitorresults** . Otros procedimientos almacenados dbmmonitor permiten configurar la supervisión, cambiar los parámetros de supervisión, ver el período de actualización actual o eliminar la supervisión en la instancia de servidor.  
  
     En la siguiente tabla se presentan los procedimientos almacenados que se utilizan para administrar y usar la supervisión de la creación de reflejo de la base de datos de manera independiente del Monitor.  
  
    |Procedimiento|Descripción|  
    |---------------|-----------------|  
    |[sp_dbmmonitoraddmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)|Crea un trabajo que actualiza periódicamente la información de estado para todas las bases de datos reflejadas en la instancia de servidor.|  
    |[sp_dbmmonitorchangemonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)|Cambia el valor de un parámetro de supervisión de la creación de reflejo de la base de datos.|  
    |[sp_dbmmonitorhelpmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)|Devuelve el período de actualización actual.|  
    |[sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)|Devuelve las filas de estado de una base de datos supervisada y permite elegir si el procedimiento obtiene de antemano el último estado.|  
    |[sp_dbmmonitordropmonitoring](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)|Detiene y elimina el trabajo de supervisión de la creación de reflejo para todas las bases de datos de la instancia de servidor.|  
  
     Se pueden utilizar los procedimientos almacenados del sistema **dbmmonitor** como un complemento del Monitor de creación de reflejo de la base de datos. Por ejemplo, incluso si la supervisión se ha configurado mediante **sp_dbmmonitoraddmonitoring**, se puede usar el Monitor de creación de reflejo de la base de datos para ver el estado.  
  
### <a name="how-monitoring-works"></a>Cómo funciona la supervisión  
 En esta sección se presenta la tabla de estado de la creación de reflejo de la base de datos, el monitor y el trabajo del Monitor de creación de reflejo de la base de datos, cómo los usuarios pueden supervisar el estado de la creación de reflejo y cómo se puede quitar el trabajo de supervisión.  
  
#### <a name="database-mirroring-status-table"></a>Tabla de estado de la creación de reflejo de la base de datos  
 El estado de la creación de reflejo de la base de datos se almacena en una tabla de estado interna y no documentada de la base de datos **msdb** . La tabla de estado se crea de forma automática la primera vez que se actualiza el estado de la creación de reflejo en la instancia de servidor.  
  
 La tabla de estado puede actualizarse de forma automática o manual por un administrador del sistema, con un intervalo de actualización mínimo de 15 segundos. Este mínimo evita que las instancias de servidor se sobrecarguen con solicitudes de estado.  
  
 El Monitor de creación de reflejo de la base de datos o el trabajo del Monitor, si se está ejecutando, actualizan de forma automática la tabla de estado. El **Trabajo del Monitor de creación de reflejo de la base de datos** actualiza la tabla de forma predeterminada una vez por minuto (un administrador del sistema puede especificar un período de actualización de 1 a 120 minutos). Por su parte, el Monitor de creación de reflejo de la base de datos actualiza la tabla de forma automática cada 30 segundos. Para llevar a cabo estas actualizaciones, el **Trabajo del Monitor de creación de reflejo de la base de datos** y el Monitor de creación de reflejo de la base de datos llaman al procedimiento **sp_dbmmonitorupdate**.  
  
 La primera vez que se ejecuta **sp_dbmmonitorupdate** , crea la tabla de **estado de la creación de reflejo de la base de datos** y el rol fijo de base de datos **dbm_monitor** en la base de datos **msdb** . **sp_dbmmonitorupdate** actualiza el estado de la creación de reflejo de la base de datos mediante la inserción de una nueva fila en cada una de las bases de datos reflejadas de la instancia de servidor; para obtener más información, vea "Tabla de estado de la creación de reflejo de la base de datos", más adelante en este tema. Este procedimiento también evalúa las estadísticas de rendimiento de las nuevas filas y trunca las filas anteriores al período de retención actual (el valor predeterminado es 7 días). Para obtener más información, vea [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md).  
  
> [!NOTE]  
>  A menos que un miembro del rol fijo de servidor **sysadmin** use el Monitor de creación de reflejo de la base de datos, la tabla de estado solo se actualiza automáticamente si el **Trabajo del Monitor de creación de reflejo de la base de datos** existe y un Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está en ejecución.  
  
#### <a name="database-mirroring-monitor-job"></a>Trabajo del Monitor de creación de reflejo de la base de datos  
 El trabajo de supervisión, **Trabajo del Monitor de creación de reflejo de la base de datos**, funciona de forma independiente dl Monitor de creación de reflejo de la base de datos. El **Trabajo del Monitor de creación de reflejo de la base de datos** solo se crea automáticamente si se usa [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para iniciar una sesión de creación de reflejo. Si los comandos ALTER DATABASE *database_name* SET PARTNER se usan siempre para iniciar la sesión de creación de reflejo de la base de datos, el trabajo solo existirá si el administrador del sistema ejecuta el procedimiento almacenado **sp_dbmmonitoraddmonitoring** .  
  
 Suponiendo que el Agente **esté en ejecución, después de crear el** Trabajo del Monitor de creación de reflejo de la base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se llama al trabajo una vez por minuto de forma predeterminada. Después, el trabajo llama al procedimiento almacenado del sistema **sp_dbmmonitorupdate** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente llama al **Trabajo del Monitor de creación de reflejo de la base de datos** una vez por minuto de forma predeterminada, y el trabajo llama a **sp_dbmmonitorupdate** para actualizar la tabla de estado. Los administradores del sistema pueden cambiar el período de actualización con el procedimiento almacenado del sistema **sp_dbmmonitorchangemonitoring** y ver el período de actualización actual con el procedimiento almacenado del sistema **sp_dbmmonitorchangemonitoring** . Para obtener más información, vea [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md) y [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-system-administrators"></a>Supervisar el estado de la creación de reflejo de la base de datos (por los administradores del sistema)  
 Los miembros del rol fijo de servidor **sysadmin** pueden ver y actualizar la tabla de estado.  
  
-   Usar el Monitor de creación de reflejo de la base de datos  
  
     Cuando se utiliza el Monitor de creación de reflejo de la base de datos, un administrador del sistema puede actualizar manualmente la página **Estado** , el árbol de navegación o la página **Historial** . De este modo, se actualiza también la tabla de estado, a menos que se haya actualizado en los 15 segundos anteriores.  
  
     Para ver el historial del estado de la creación de reflejo de una determinada instancia de servidor, el administrador del sistema también puede hacer clic sobre el botón **Historial** de una instancia de servidor (en la página **Estado** ). El historial se muestra en el cuadro de diálogo **Historial de creación de reflejo de la base de datos** . Después, el administrador del sistema puede ver algunas o todas las filas de la tabla de estado de la instancia de servidor.  
  
     Para obtener información acerca de las estadísticas de la página **Estado** , vea Estadísticas de rendimiento que muestra el "Monitor de creación de reflejo de la base de datos", más adelante en este tema.  
  
-   Usar **sp_dbmmonitorresults**  
  
     Los administradores del sistema pueden usar el procedimiento almacenado del sistema **sp_dbmmonitorresults** para ver y, opcionalmente, actualizar la tabla de estado, si esta no se ha actualizado en los 15 segundos anteriores. Este procedimiento llama al procedimiento **sp_dbmmonitorupdate** y devuelve una o más filas del historial, en función de la cantidad solicitada en la llamada del procedimiento. Para obtener más información sobre el estado en su conjunto de resultados, vea [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md).  
  
#### <a name="monitoring-database-mirroring-status-by-dbm_monitor-members"></a>Supervisar el estado de la creación de reflejo de la base de datos (por los miembros dbm_monitor)  
 Como se ha mencionado, la primera vez que se ejecuta **sp_dbmmonitorupdate** , se crea el rol fijo de base de datos **dbm_monitor** en la base de datos **msdb** . Los miembros del rol fijo de base de datos **dbm_monitor** pueden ver el estado actual de la creación de reflejo mediante el Monitor de creación de reflejo de la base de datos o el procedimiento almacenado **sp_dbmmonitorresults** . No obstante, estos usuarios no pueden actualizar la tabla de estado. Para conocer la antigüedad del estado que se muestra, un usuario puede examinar las horas en las etiquetas **Registro del servidor principal (** _\<time>_ **)** y **Registro del servidor reflejado (** _\<time>_ **)** de la página **Estado**.  
  
 Los miembros del rol fijo de base de datos **dbm_monitor** dependen del **Trabajo del Monitor de creación de reflejo de la base de datos** para actualizar la tabla de estado a intervalos periódicos. Si el trabajo no existe o el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está inactivo, el estado pasa a estar cada vez más desusado y es posible que no refleje la configuración de la sesión de creación de reflejo. Por ejemplo, después de una conmutación por error, es posible que parezca que los asociados comparten el mismo rol (de servidor principal o reflejado) o que el servidor principal actual se muestre como reflejado, a la vez que el servidor reflejado actual se muestra como principal.  
  
#### <a name="dropping-the-database-mirroring-monitor-job"></a>Quitar el Trabajo del Monitor de creación de reflejo de la base de datos  
 El **Trabajo del Monitor de creación de reflejo de la base de datos** se mantendrá hasta su eliminación. El administrador del sistema debe administrar el trabajo de supervisión. Para quitar el **Trabajo del Monitor de creación de reflejo de la base de datos**, use **sp_dbmmonitordropmonitoring**. Para obtener más información, vea [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md).  
  
###  <a name="status-displayed-by-the-database-mirroring-monitor"></a><a name="perf_metrics_of_dbm_monitor"></a> Estado que muestra el Monitor de creación de reflejo de la base de datos  
 En la página **Estado** del Monitor de creación de reflejo de la base de datos se describen los asociados y el estado de la sesión de creación de reflejo. El estado incluye las estadísticas de rendimiento como, por ejemplo, el estado del registro de transacciones y otra información que se utiliza para realizar una estimación del tiempo necesario para completar una conmutación por error y la potencial pérdida de datos, si no se sincroniza la sesión. Además, en la página **Estado** se muestra el estado y la información sobre la sesión de creación de reflejo en general.  
  
> [!NOTE]  
>  Para obtener una introducción sobre el Monitor de creación de reflejo de la base de datos y de la página **Estado** , vea [Herramientas para la supervisión del estado de la creación de reflejo de la base de datos](#tools_for_monitoring_dbm_status), anteriormente en este tema.  
  
 La información que se proporciona de cada una de ellas se resume en las secciones siguientes.  
  
#### <a name="partners"></a>Asociados  
 En la página **Estado** se muestra la siguiente información para cada uno de los asociados:  
  
-   Instancia del servidor  
  
     Nombre de la instancia de servidor cuyo estado se muestra en la fila **Estado** .  
  
-   Rol actual  
  
     Rol actual de la instancia de servidor. Los estados posibles son:  
  
    -   Principal  
  
    -   Reflejo  
  
-   Estado de creación de reflejo  
  
     Los estados posibles son:  
  
    -   Desconocido  
  
    -   Sincronizando  
  
    -   Sincronizado  
  
    -   Suspended  
  
    -   Escenario desconectado  
  
-   Conexión del testigo  
  
     Estado de conexión del testigo. Los estados posibles son:  
  
    -   Desconocido  
  
    -   Conectado  
  
    -   Desconectado  
  
#### <a name="log-on-the-principal-server"></a>Registro en el servidor principal  
 En la página **Estado** se muestra la siguiente información acerca del estado del registro en el servidor principal, desde la hora indicada:  
  
-   Registro sin enviar  
  
     La cantidad de registro que espera en la cola de envío en kilobytes (KB).  
  
-   Transacción no enviada más antigua  
  
     Antigüedad de la transacción sin enviar más antigua de la cola de envío. La antigüedad de esta transacción indica la cantidad de minutos de transacciones que no se han enviado aún a la instancia del servidor reflejado. Este valor ayuda a medir la posibilidad de pérdida de datos con respecto a la hora.  
  
-   Hora de envío de registro (estimada)  
  
     Número estimado de minutos que requiere la instancia del servidor principal para enviar el registro que se encuentra actualmente en la cola de envío a la instancia del servidor reflejado basándose en la tasa de envío actual. La hora real de envío del registro se verá afectada por la tasa de transacciones entrantes, la cual puede variar de forma notable. Pero el valor **Hora de envío de registro (estimada)** puede ser útil para una estimación aproximada del tiempo necesario para una conmutación por error manual.  
  
-   Tasa actual de envío  
  
     Tasa a la que se envían las transacciones a la instancia del servidor reflejado en KB por segundo.  
  
-   Tasa actual de nuevas transacciones  
  
     Tasa a la que se escriben las transacciones entrantes en el registro del servidor principal en KB por segundo. Para determinar si la creación de reflejo se retrasa, se mantiene o mejora, compare este valor con **Hora de envío de registro (estimada)** .  
  
#### <a name="log-on-the-mirror-server"></a>Registro en el servidor reflejado  
 En la página **Estado** se muestra la siguiente información acerca del estado del registro en el servidor reflejado, desde la hora indicada:  
  
-   Registro sin restaurar  
  
     La cantidad de registro que espera en la cola de puesta al día en KB.  
  
-   Tiempo para restaurar registro (estimado)  
  
     Número aproximado de minutos necesarios para que el registro que se encuentra actualmente en la cola de puesta al día se aplique a la base de datos reflejada.  
  
-   Tasa actual de restauración  
  
     Tasa a la que se restauran las transacciones en la base de datos reflejada (en KB por segundo).  
  
#### <a name="mirroring-session"></a>Sesión de creación de reflejo  
 Además, en la página **Estado** se muestra la siguiente información sobre la sesión de creación de reflejo:  
  
-   Sobrecarga de confirmación del servidor reflejado  
  
     Retardo medio por transacción en milisegundos (solo relevante en el modo de alta seguridad). Este retardo es la cantidad de sobrecarga en la que se incurre mientras la instancia del servidor principal espera a la instancia del servidor reflejado para escribir la entrada de registro de la transacción en la cola de puesta al día.  
  
-   Hora de envío y restauración de todos los registros actuales (estimada)  
  
     Tiempo estimado necesario para enviar todo el registro sin enviar que se ha confirmado en el servidor principal y para restaurar todo el registro que se encuentra en la cola de puesta al día. Esta estimación puede ser menor que la suma de los valores de los campos **Hora de envío de registro (estimada)** y **Tiempo para restaurar registro (estimado)** , ya que el envío y la restauración pueden funcionar en paralelo.  
  
-   Dirección del testigo  
  
     Dirección de red de la instancia de servidor testigo. Para obtener más información sobre el formato de esta dirección, vea [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
-   Modo de funcionamiento  
  
     El modo de funcionamiento de la sesión de creación de reflejo de la base de datos:  
  
    -   Rendimiento alto (asincrónico)  
  
    -   Seguridad alta sin conmutación automática por error (sincrónico)  
  
    -   Seguridad alta con conmutación automática por error (sincrónico)  
  
##  <a name="additional-sources-of-information-about-a-mirrored-database"></a><a name="AdditionalSources"></a> Fuentes adicionales de información acerca de una base de datos reflejada  
 Además de usar el Monitor de creación de reflejo de la base de datos y los procedimientos almacenados dbmmonitor para supervisar una base de datos reflejada y configurar alertas de las variables de rendimiento supervisadas, [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] proporciona vistas de catálogo, contadores de rendimiento y notificaciones de eventos para la creación de reflejo de la base de datos.  
  
 **En esta sección:**  
  
-   [Metadatos de creación de reflejo de la base de datos](#DbmMetadata)  
  
-   [Contadores de rendimiento de creación de reflejo de la base de datos](#DbmPerfCounters)  
  
-   [Notificaciones de eventos de la creación de reflejo de la base de datos](#DbmEventNotif)  
  
###  <a name="database-mirroring-metadata"></a><a name="DbmMetadata"></a> Metadatos de creación de reflejo de la base de datos  
 Cada sesión de creación de reflejo de la base de datos se describe en metadatos que se exponen mediante las siguientes vistas de catálogo o de administración dinámica:  
  
-   **sys.database_mirroring**  
  
     En esta vista se muestran los metadatos de la creación de reflejo para cada base de datos reflejada en una instancia de servidor. Para obtener más información, vea [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
-   **sys.database_mirroring_endpoints**  
  
     En la vista de catálogo **sys.database_mirroring_endpoints** se muestra información acerca del punto de conexión de la creación de reflejo de la base de datos de la instancia de servidor. Para obtener más información, vea [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
-   **sys.database_mirroring_witnesses**  
  
     En esta vista de catálogo se muestran los metadatos de la creación de reflejo de la base de datos para cada sesión en la que una instancia de servidor es el testigo. Para obtener más información, vea [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md).  
  
-   **sys.dm_db_mirroring_connections**  
  
     Esta vista de administración dinámica devuelve una fila para cada conexión de red de la creación de reflejo de la base de datos.  
  
     Para obtener más información, vea [sys.dm_db_mirroring_connections &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md).  
  
###  <a name="database-mirroring-performance-counters"></a><a name="DbmPerfCounters"></a> Contadores de rendimiento de creación de reflejo de la base de datos  
 Los contadores de rendimiento le permiten supervisar el rendimiento de la creación de reflejo de la base de datos. Por ejemplo, puede examinar el contador **Retraso de transacción** para ver si la creación de reflejo de la base de datos está afectando al rendimiento del servidor principal; puede examinar los contadores **Cola rehecha** y **Envío de registro en cola** para ver el comportamiento de la base de datos reflejada con respecto a la base de datos principal. Puede examinar el contador **Bytes de registro enviados/s** para supervisar la parte del registro enviada por segundo.  
  
 En el Monitor de rendimiento de cada asociado, los contadores de rendimiento están disponibles en el objeto de rendimiento de la creación de reflejo de la base de datos (**SQLServer:Database Mirroring**). Para más información, consulte [SQL Server, Database Mirroring Object](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md).  
  
 **Para iniciar el Monitor de rendimiento**  
  
-   [Iniciar el Monitor de sistema &#40;Windows&#41;](../../relational-databases/performance/start-system-monitor-windows.md)  
  
###  <a name="database-mirroring-event-notifications"></a><a name="DbmEventNotif"></a> Notificaciones de eventos de la creación de reflejo de la base de datos  
 Las notificaciones de eventos son una clase especial de objetos de base de datos. Las notificaciones de eventos se ejecutan en respuesta a una variedad de instrucciones del lenguaje de definición de datos (DDL) de Transact-SQL y eventos de Seguimiento de SQL, y envían información acerca de los eventos de servidor y base de datos a un servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
 Los siguientes eventos están disponibles en la creación de reflejo de la base de datos:  
  
-   Clase de evento **Database Mirroring State Change**  
  
     Este evento indica cuándo cambia el estado de creación de reflejo de una base de datos reflejada. Para obtener más información, consulte [Database Mirroring State Change Event Class](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md).  
  
-   Clase de evento **Audit Database Mirroring Login**  
  
     Este evento le permite emitir mensajes de auditoría relacionados con la seguridad de transporte de la creación de reflejo de la base de datos. Para obtener más información, consulte [Audit Database Mirroring Login Event Class](../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Usar alertas y umbrales de advertencia de las métricas de rendimiento de la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md)  
  
-   [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [Ver el estado de una base de datos reflejada &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/view-the-state-of-a-mirrored-database-sql-server-management-studio.md)  
  
 **procedimientos almacenados**  
  
-   [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
-   [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)  
  
-   [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
-   [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)  
  
-   [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)  
  
-   [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
-   [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Conceptos del proveedor WMI para eventos de servidor](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)  
  
  
