---
title: Cambios no visibles en la réplica del grupo de disponibilidad secundario
description: Obtenga información sobre cómo determinar por qué los cambios de una réplica principal no se reflejan en la secundaria de un grupo de disponibilidad Always On.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: c602fd39-db93-4717-8f3a-5a98b940f9cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 48b31c951cc74f1dc848cb5705b0fd3d756291db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337888"
---
# <a name="determine-why-changes-from-primary-replica-are-not-reflected-on-secondary-replica-for-an-always-on-availability-group"></a>Determinación de por qué los cambios de la réplica principal no se reflejan en una réplica secundaria de un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  La aplicación cliente finaliza una actualización en la réplica principal correctamente, pero una consulta a la réplica secundaria muestra que el cambio no se ha reflejado. En este caso, se presupone que el estado de sincronización de su disponibilidad es correcto. En la mayoría de casos, este comportamiento se resuelve pasados unos minutos.  
  
 Si los cambios aún no se reflejan en la réplica secundaria pasados unos minutos, puede haber un cuello de botella en el flujo de trabajo de sincronización. La ubicación del cuello de botella depende de si la réplica secundaria se establece en confirmación sincrónica o asincrónica.  
  
 **Confirmación sincrónica**  
  
 Cada actualización correcta de la réplica principal ya se ha sincronizado con la réplica secundaria o las entradas del registro ya se han vaciado para la protección en la réplica secundaria. Por lo tanto, el cuello de botella debe estar en el proceso de la fase de puesta al día que se produce después de que el registro se vacíe en la réplica secundaria.  
  
 Sin embargo, cuando se alcanza la fase de puesta al día, todas las cargas de trabajo de lectura de la réplica secundaria pasan por el aislamiento de instantáneas:  
  
  -   Transacciones de ejecución prolongada en la réplica principal  
  
  -   Fase de puesta al día en la réplica secundaria  


**Confirmación asincrónica**  
 
 Puesto que la confirmación asincrónica confirma una transacción en cuanto se vuelca en el disco local, el cuello de botella puede estar en cualquier lugar pasado ese punto:  
 
  -   Transacciones de ejecución prolongada en la réplica principal  
  
  -   Latencia o rendimiento de red  
  
  -   Protección del registro en la réplica secundaria  
  
  -   Fase de puesta al día en la réplica secundaria  


En las siguientes secciones se describen las causas comunes de los cambios en la réplica principal que no se reflejan en la réplica secundaria para las consultas de solo lectura.  


##  <a name="long-running-active-transactions"></a><a name="BKMK_OLDTRANS"></a> Transacciones activas de ejecución prolongada  
 Una transacción de ejecución prolongada en la réplica principal impide que las actualizaciones se lean en la réplica secundaria.  
  
### <a name="explanation"></a>Explicación  
 Todas las cargas de trabajo de lectura de la réplica secundaria son consultas de aislamiento de instantáneas. En el aislamiento de instantáneas, los clientes de solo lectura ven la base de datos de disponibilidad en la réplica secundaria en el punto de comienzo de la transacción activa más antigua en el registro de la acción de rehacer. Si una transacción no se confirma durante horas, la transacción abierta impide que todas las consultas de solo lectura vean las actualizaciones nuevas.  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 En la réplica principal, use [DBCC OPENTRAN &#40;Transact-SQL&#41; ](~/t-sql/database-console-commands/dbcc-opentran-transact-sql.md) para ver las transacciones activas más antiguas y vea si se pueden revertir. Cuando las transacciones activas más antiguas se han revertido y sincronizado en la réplica secundaria, las cargas de trabajo de lectura en la réplica secundaria pueden ver las actualizaciones en la base de datos de disponibilidad hasta el principio de la transacción activa que en aquel momento sea la más antigua.  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> La alta latencia de red o el bajo rendimiento de red producen una acumulación de registros en la réplica principal  
 La alta latencia de red o el bajo rendimiento pueden impedir que los registros se envíen a la réplica secundaria lo suficientemente rápido.  
  
### <a name="explanation"></a>Explicación  
 La réplica principal activa el control de flujo en el envío del registro cuando ha superado el número máximo permitido de mensajes no confirmados que se han enviado a través de la réplica secundaria. Hasta que no se acepten algunos de estos mensajes, no se podrán enviar más bloques de registro a la réplica secundaria. Esta situación puede tener un impacto más grave en la posible pérdida de datos, lo que podría poner en peligro su objetivo de punto de recuperación (RPO).  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 Un valor alto de DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) puede indicar que los registros están siendo retenidos en la réplica principal. Dividir este valor por [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) puede proporcionarle una estimación aproximada del momento en que se pueden alcanzar los datos en la réplica secundaria.  
  
 Además, resulta útil para comprobar los dos objetos de rendimiento: [SQL Server: Réplica de disponibilidad > Tiempo de control de flujo (ms/s)](~/relational-databases/performance-monitor/sql-server-availability-replica.md) y [SQL Server: Réplica de disponibilidad > Control de flujo/s](~/relational-databases/performance-monitor/sql-server-availability-replica.md). Al multiplicar estos dos valores, puede saber en el último segundo cuánto tiempo tardó en borrarse el control de flujo. Cuanto más tiempo de espera tarde el control de flujo, menor será la velocidad de envío.  
  
 A continuación encontrará métricas útiles para diagnosticar el rendimiento y la latencia de red. Puede usar otras herramientas de Windows, como **ping.exe**, para evaluar el uso de la red.  
  
-   DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   DMV [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   Contador de rendimiento `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Contador de rendimiento `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Resent Messages/sec`  
  
 Para solucionar este problema, intente actualizar el ancho de banda de red o eliminar o reducir el tráfico de red innecesario.  
  
##  <a name="another-reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a> Otra carga de trabajo de los informes impide que se ejecute el subproceso de la fase de puesta al día de ejecución  
 El subproceso de la fase de puesta al día de la réplica secundaria no puede hacer cambios en el lenguaje de definición de datos (DDL) a partir de una consulta de solo lectura de ejecución prolongada. El subproceso de la fase de puesta al día debe desbloquearse para poder poner más actualizaciones a disposición de la carga de trabajo de lectura.  
  
### <a name="explanation"></a>Explicación  
 En la réplica secundaria, las consultas de solo lectura adquieren bloqueos de estabilidad del esquema (`Sch-S`). Estos bloqueos `Sch-S` pueden impedir que el subproceso de la fase de puesta al día adquiera bloqueos de modificación del esquema (`Sch-M`) para hacer cambios en el lenguaje de definición de datos. El subproceso de la fase de puesta al día no puede aplicar las entradas del registro hasta que se desbloquee.  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 Si el subproceso de la fase de puesta al día se bloquea, se genera un evento extendido denominado `sqlserver.lock_redo_blocked`. Además, puede consultar la solicitud sys.dm_exec_request de DMV en la réplica secundaria para averiguar qué sesión está bloqueando el subproceso de la fase de puesta al día para después emprender una acción correctiva. La siguiente consulta devuelve el identificador de sesión de la carga de trabajo de informes que está bloqueando el subproceso de la fase de puesta al día.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 Puede esperar a que la carga de trabajo de los informes finalice, momento en que el subproceso de la fase de puesta al día se desbloqueará, o puede desbloquear inmediatamente el subproceso de la fase de puesta al día mediante la ejecución del comando [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) en el identificador de sesión de bloqueo.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_REDOBEHIND"></a> El subproceso de la fase de puesta al día se retrasa debido a la contención de recursos  
 Una gran carga de trabajo de informes en la réplica secundaria ha ralentizado el rendimiento de la réplica secundaria y el subproceso de la fase de puesta al día se ha retrasado.  
  
### <a name="explanation"></a>Explicación  
 Al aplicar las entradas de registro en la réplica secundaria, el subproceso de la fase de puesta al día lee las entradas del registro desde el disco de registro y accede a las páginas de datos de cada entrada del registro para aplicar la entrada del registro. El acceso a la página puede enlazarse a E/S (accediendo al disco físico) si la página no está lista en el grupo de búferes. Si hay una carga de trabajo de informes enlazada a E/S, esta carga de trabajo compite por los recursos de E/S con el subproceso de la fase de puesta al día y puede ralentizar dicho subproceso. Esta situación no solo repercute en la posibilidad de otras cargas de trabajo de informes de ver datos actualizados, sino que también repercute en el RTO.  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 Puede usar la siguiente consulta DMV para ver hasta qué punto el subproceso de la fase de puesta al día se ha retrasado, midiendo la diferencia entre `last_redone_lsn` y `last_received_lsn`.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Si realmente el subproceso de la fase de puesta al día se retrasa, debe investigar la causa principal de la degradación del rendimiento en la réplica secundaria. Si se produce una contención de E/S en la carga de trabajo de los informes, puede usar [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) para controlar, hasta cierto punto, los ciclos de CPU que usa la carga de trabajo de informes para controlar indirectamente los ciclos de E/S realizados. Por ejemplo, si la carga de trabajo de los informes está consumiendo un 10 % de la CPU, pero la carga de trabajo está enlazada a E/S, puede utilizar Resource Governor para limitar el uso de recursos de la CPU al 5 % y regular así la carga de trabajo de lectura. De esta forma, se reduce el impacto en la E/S.  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Troubleshooting performance problems in SQL Server 2008](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100)) (Solucionar problemas de rendimiento en SQL Server 2008)
  
