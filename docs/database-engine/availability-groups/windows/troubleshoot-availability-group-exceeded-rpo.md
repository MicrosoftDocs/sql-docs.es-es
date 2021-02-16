---
title: El grupo de disponibilidad superó el RPO
description: Problemas comunes y soluciones para cuando el grupo de disponibilidad de AlwaysOn supera el objetivo de punto de recuperación (RPO)
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 38de1841-9c99-435a-998d-df81c7ca0f1e
author: rothja
ms.author: jroth
ms.openlocfilehash: f70696592cae8f486e7ff7b52e417bb67edfa8d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337927"
---
# <a name="troubleshoot-availability-group-exceeded-rpo"></a>Solución de problemas: El grupo de disponibilidad superó el RPO
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Después de realizar una conmutación por error manual forzada de un grupo de disponibilidad a una réplica secundaria de confirmación asincrónica, es posible que la pérdida de datos supere su objetivo de punto de recuperación (RPO). También puede ser que al calcular la pérdida de datos potencial de una réplica secundaria de confirmación sincrónica usando el método que se describe en [Monitor performance for Always On Availability Groups](monitor-performance-for-always-on-availability-groups.md) (Supervisión del rendimiento de grupos de disponibilidad Always On), descubra que supera el RPO.  
  
 Una réplica secundaria de confirmación sincrónica garantiza que no se pierdan datos, pero la pérdida de datos potencial de una réplica secundaria de confirmación asincrónica depende de la cantidad de registro que todavía está esperando a ser protegido en la réplica secundaria.  
  
 En las siguientes secciones se describen las causas más probables de una gran pérdida de datos en una réplica secundaria de confirmación asincrónica, suponiendo que no tenga ningún problema de rendimiento sistémico en la instancia del servidor que no esté relacionado con los grupos de disponibilidad.  
  
1.  [La alta latencia de red o el bajo rendimiento de red producen una acumulación de registros en la réplica principal](#BKMK_LATENCY)  
  
2.  [Un cuello de botella de E/S de disco ralentiza la protección de la réplica secundaria](#BKMK_IO_BOTTLENECK)  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> La alta latencia de red o el bajo rendimiento de red producen una acumulación de registros en la réplica principal  
 La causa que provoca con más frecuencia que las bases de datos superen su RPO es que no se pueden enviar lo suficientemente rápido a la réplica secundaria.  
  
### <a name="explanation"></a>Explicación  
 La réplica principal activa el control de flujo en el envío del registro cuando ha superado el número máximo permitido de mensajes no confirmados que se han enviado a través de la réplica secundaria. Hasta que no se acepten algunos de estos mensajes, no se podrán enviar más bloques de registro a la réplica secundaria. Puesto que la pérdida de datos solo se puede evitar cuando se protegen en la réplica secundaria, la acumulación de mensajes de registro sin enviar aumenta la posible pérdida de datos.  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 El reenvío de un gran número de mensajes a la réplica secundaria puede indicar una alta latencia y ruido en la red. También puede comparar el valor **log_send_rate** de DMV con el objeto de rendimiento de los bytes de registro vaciados por segundo. Si los registros se vacían en el disco más rápido de lo que se envían, la posible pérdida de datos puede aumentar indefinidamente.  
  
 Además, resulta útil para comprobar los dos objetos de rendimiento: `SQL Server:Availability Replica > Flow Control Time (ms/sec)` y `SQL Server:Availability Replica > Flow Control/sec`. Al multiplicar estos dos valores, puede saber en el último segundo cuánto tiempo tardó en borrarse el control de flujo. Cuanto más tiempo de espera tarde el control de flujo, menor será la velocidad de envío.  
  
 Las siguientes métricas son útiles para diagnosticar el rendimiento y la latencia de red. Puede usar otras herramientas de Windows, como **ping.exe** y [Monitor de red](https://www.microsoft.com/p/network-monitor-pro-free-edition/9n8gdvj32gp7) para evaluar el uso de la latencia y la red.  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_queue_size`  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_rate`  
  
-   Contador de rendimiento `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Contador de rendimiento `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Contador de rendimiento `SQL Server:Availability Replica > Resent Messages/sec`  

Para solucionar este problema, intente actualizar el ancho de banda de red o eliminar o reducir el tráfico de red innecesario.  


##  <a name="disk-io-bottleneck-slows-down-log-hardening-on-the-secondary-replica"></a><a name="BKMK_IO_BOTTLENECK"></a> Un cuello de botella de E/S de disco ralentiza la protección de la réplica secundaria  
 En función de la implementación del archivo de base de datos, la protección del registro puede ralentizarse debido a la contención de E/S con una carga de trabajo de informes.  
  
### <a name="explanation"></a>Explicación  
 La pérdida de datos se evita en cuanto el bloque de registro se protege en el archivo de registro. Por lo tanto, es fundamental aislar el archivo de registro del archivo de datos. Si el archivo de registro y el archivo de datos se asignan al mismo disco duro, la carga de trabajo de los informes con lecturas intensivas del archivo de datos consumirá los mismos recursos de E/S necesarios para la operación de protección del registro. Una protección de registro lenta puede traducirse en una aceptación lenta de la réplica principal, lo que puede producir una activación excesiva y largos tiempos de espera del control de flujo.  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 Si ha comprobado que la red no presenta una alta latencia o un rendimiento bajo, verifique si existen contenciones de E/S en la réplica secundaria. Las consultas de [SQL Server: minimizar la E/S de disco](/previous-versions/technet-magazine/jj643251(v=msdn.10)) son útiles para identificar las contenciones. A continuación encontrará los ejemplos de ese artículo.  
  
 En el siguiente script verá el número de lecturas y escrituras de cada archivo de datos y de registro para cada base de datos de disponibilidad que se ejecute en una instancia de SQL Server. Está ordenado por el tiempo de pausa de E/S promedio, en milisegundos. Tenga en cuenta que los números se acumulan desde la última vez que se inició la instancia del servidor. Por lo tanto, debe tomar la diferencia entre dos mediciones después de que haya transcurrido cierto tiempo.  
  
```sql  
SELECT DB_NAME(database_id) AS   
   [Database Name] ,   
   file_id ,   
   io_stall_read_ms ,   
   num_of_reads ,   
   CAST(io_stall_read_ms / ( 1.0 + num_of_reads ) AS NUMERIC(10, 1)) AS [avg_read_stall_ms] ,   
   io_stall_write_ms ,   
   num_of_writes ,  
   CAST(io_stall_write_ms / ( 1.0 + num_of_writes ) AS NUMERIC(10, 1)) AS [avg_write_stall_ms] ,   
   io_stall_read_ms + io_stall_write_ms AS [io_stalls] ,   
   num_of_reads + num_of_writes AS [total_io] ,   
   CAST(( io_stall_read_ms + io_stall_write_ms ) / ( 1.0 + num_of_reads  
+ num_of_writes) AS NUMERIC(10,1)) AS [avg_io_stall_ms]  
FROM sys.dm_io_virtual_file_stats(NULL, NULL)  
WHERE DB_NAME(database_id) IN (SELECT DISTINCT database_name FROM sys.dm_hadr_database_replica_cluster_states)  
ORDER BY avg_io_stall_ms DESC;  
```  
  
 La siguiente consulta proporciona una instantánea (no acumulativa) en un momento en que hay solicitudes de E/S pendientes en el sistema.  
  
```sql  
SELECT DB_NAME(mf.database_id) AS [Database] ,   
   mf.physical_name ,  
   r.io_pending ,   
   r.io_pending_ms_ticks ,   
   r.io_type ,   
   fs.num_of_reads ,   
   fs.num_of_writes  
FROM sys.dm_io_pending_io_requests AS r   
INNER JOIN sys.dm_io_virtual_file_stats(NULL, NULL) AS fs ON r.io_handle = fs.file_handle   
INNER JOIN sys.master_files AS mf ON fs.database_id = mf.database_id  
AND fs.file_id = mf.file_id  
ORDER BY r.io_pending , r.io_pending_ms_ticks DESC;  
```  
  
 Puede comparar cómo coinciden entre ellas la E/S de lectura y la E/S de escritura para identificar la contención de E/S.  
  
 Estos son otros contadores de rendimiento que pueden ayudarle a diagnosticar cuellos de botella de E/S:  
  
-   **Disco físico: todos los contadores**  
  
-   **Disco físico: Prom. de segundos de disco/transferencia**  
  
-   **SQL Server: bases de datos > tiempo de espera de vaciado de registro**  
  
-   **SQL Server: bases de datos > tiempo de espera de vaciado de registro/segundo**  
  
-   **SQL Server: bases de datos > lecturas de disco del grupo de registros/segundo**  
  
 Si detecta un cuello de botella de E/S y ha colocado el archivo de registro y el archivo de datos en el mismo disco duro, lo primero que debe hacer es colocar dichos archivos en discos separados. Este procedimiento recomendado impide que la carga de trabajo de informes interfiera con la ruta de transferencia de registros de la réplica principal al búfer de registro y con su capacidad de proteger la transacción en la réplica secundaria.  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Solucionar problemas de rendimiento en SQL Server (se aplica a SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  
