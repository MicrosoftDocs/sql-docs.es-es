---
title: Búferes de anillo para la información de estado de los grupos de disponibilidad
description: Obtenga cierta información de diagnóstico sobre los grupos de disponibilidad Always On mediante los búferes en anillo de SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 47bb7a1a-c0a5-473c-a7db-d9f4bf3ee650
author: rothja
ms.author: jroth
ms.openlocfilehash: c16454e9589b8f14fda04af591355178fd45eb7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347484"
---
# <a name="use-ring-buffers-to-obtain-health-information-about-always-on-availability-groups"></a>Uso de búferes en anillo para obtener información de estado sobre los grupos de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En los búferes de anillo de SQL Server o en la vista de administración dinámica (DMV) sys.dm_os_ring_buffers puede obtener información de diagnóstico para los grupos de disponibilidad Always On. Los búferes de anillo se crean durante el inicio de SQL Server y registran las alertas del sistema de SQL Server para el diagnóstico interno. No se admiten, pero pueden servirle para extraer información valiosa cuando tenga que solucionar problemas. Estos búferes de anillo proporcionan otra fuente de diagnóstico cuando SQL Server se bloquea o ha dejado de funcionar.  
  
 La siguiente consulta de Transact-SQL (T-SQL) recupera todos los registros de eventos de los búferes de anillo de grupos de disponibilidad.  
  
```sql  
SELECT * FROM sys.dm_os_ring_buffers WHERE ring_buffer_type LIKE '%HADR%'  
```  
  
 Para que los datos sean más fáciles de administrar, filtre los datos por fecha y por tipo de búfer de anillo. La siguiente consulta recupera los registros del búfer de anillo especificado que han ocurrido hoy.  
  
```sql  
DECLARE @runtime datetime  
SET @runtime = GETDATE()  
SELECT CONVERT (varchar(30), @runtime, 121) as data_collection_runtime,   
DATEADD (ms, -1 * (inf.ms_ticks - ring.[timestamp]), GETDATE()) AS ring_buffer_record_time,   
ring.[timestamp] AS record_timestamp, inf.ms_ticks AS cur_timestamp, ring.*   
FROM sys.dm_os_ring_buffers ring  
CROSS JOIN sys.dm_os_sys_info inf where ring_buffer_type='<RING_BUFFER_TYPE>'  
```  
  
 La columna Registro de cada registro contiene datos de diagnóstico en formato XML. Los datos XML difieren entre los tipos de búfer de anillo. Para obtener más información sobre cada tipo de búfer de anillo, consulte [Tipos de búfer de anillo en los grupos de disponibilidad](#BKMK_RingBufferTypes). Para que los datos XML sean más legibles, debe personalizar su consulta de T-SQL para extraer los elementos XML deseados. Por ejemplo, la consulta siguiente recupera todos los eventos del búfer de anillo RING_BUFFER_HADRDBMGR_API y da formato a los datos XML en columnas independientes de la tabla.  
  
```sql  
WITH hadr(ts, type, record) AS  
(  
  SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
  FROM sys.dm_os_ring_buffers WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API'  
)  
SELECT   
  ts,  
  type,  
  record.value('(./Record/@id)[1]','bigint') AS [Record ID],  
  record.value('(./Record/@time)[1]','bigint') AS [Time],  
  record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [DBID],  
  record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
  record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
  record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
  record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
##  <a name="availability-groups-ring-buffer-types"></a><a name="BKMK_RingBufferTypes"></a> Tipos de búfer de anillo en los grupos de disponibilidad  
 Hay cuatro búferes de anillo de grupos de disponibilidad en sys.dm_os_ring_buffers. En la siguiente tabla se describen los tipos de búfer de anillo y se ofrece un ejemplo del contenido de la columna Registro de cada tipo de búfer de anillo.  
  
 **RING_BUFFER_HADRDBMGR_API**  
  
 Registra las transiciones de estado que han tenido o están teniendo lugar. Al consultar estas transiciones, preste especial atención a los valores objectType.  
  
```xml  
<Record id="11" type="RING_BUFFER_HADRDBMGR_STATE" time="860243">  
  <HadrDbMgrState>  
    <objectType>HadrUsers</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>1</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_STATE**  
  
 Registra el método interno o las llamadas de función realizadas por la actividad de Always On. Puede mostrar información como suspender, reanudar o cambiar el rol, incluidos los puntos de entrada y salida.  
  
```xml  
<Record id="45" type="RING_BUFFER_HADRDBMGR_STATE" time="1723487912">  
  <HadrDbMgrState>  
    <dbId>5</dbId>  
    <objectType>HadrDbMgr</objectType>  
    <currentState>HDbMState_Starting</currentState>  
    <proposedState>HDbMState_Started</proposedState>  
    <targetState>HDbMState_Started</targetState>  
    <legalTransition>Y</legalTransition>  
    <role>2</role>  
  </HadrDbMgrState>  
</Record>  
```  
  
 **RING_BUFFER_HADRDBMGR_COMMIT**  
  
```xml  
<Record id="0" type="RING_BUFFER_HADRDBMGR_COMMIT" time="1723475368">  
  <HadrDbMgrCommitPolicy>  
    <dbId>5</dbId>  
    <replicaId>883a18f5-97d5-450f-8f8f-9983a4fa5299</replicaId>  
    <dbHardenPolicy>KillAll</dbHardenPolicy>  
    <dbSyncConfig>0x0</dbSyncConfig>  
    <syncPartnerCount>0</syncPartnerCount>  
    <minSyncPartnerConfig>0</minSyncPartnerConfig>  
    <partnerHardenPolicy>KillAll</partnerHardenPolicy>  
    <partnerSyncConfig>0x0</partnerSyncConfig>  
    <logBlock>0x0000000000000000</logBlock>  
    <leaseExpired>Y</leaseExpired>  
    <partnerChange>N</partnerChange>  
    <role>2</role>  
  </HadrDbMgrCommitPolicy>  
</Record>  
```  
  
 **RING_BUFFER_HADR_TRANSPORT_STATE**  
  
```xml  
<Record id="3" type="RING_BUFFER_HADR_TRANSPORT_STATE" time="1723485399">  
  <HadrTransportState>  
    <agId>08264B79-D10B-412F-B38D-CA07B08E9BD8</agId>  
    <localArId>883A18F5-97D5-450F-8F8F-9983A4FA5299</localArId>  
    <targetArId>628D6349-72DD-4D18-A6E1-1272645660BA</targetArId>  
    <currentState>HadrSession_Configuring</currentState>  
    <targetState>HadrSession_Connected</targetState>  
    <legalTransition>Y</legalTransition>  
  </HadrTransportState>  
</Record>  
```  
  
## <a name="parse-xml-data-from-a-ring-buffer"></a>Analizar datos XML de un búfer de anillo  
 Puede analizar el campo Registro del búfer de anillo que está inspeccionando utilizando [value() (método del tipo de datos xml)](~/t-sql/xml/value-method-xml-data-type.md) en la consulta. Para usar este método, primero debe [CONVERTIR](~/t-sql/functions/cast-and-convert-transact-sql.md) la columna del registro del búfer de anillo a XML. Por ejemplo, la consulta siguiente muestra cómo analizar RING_BUFFER_HADRDBMGR_API en un formato legible utilizando este método.  
  
```sql 
WITH hadr(ts, type, record) AS  
   (SELECT timestamp AS ts, ring_buffer_type AS type, CAST(record AS XML) AS record   
FROM sys.dm_os_ring_buffers   
WHERE ring_buffer_type = 'RING_BUFFER_HADRDBMGR_API')  
SELECT ts,  
type,  
record.value('(./Record/@id)[1]','bigint') AS [Record id],  
record.value('(./Record/@time)[1]','bigint') AS [Time],  
record.value('(./Record/HadrDbMgrAPI/dbId)[1]', 'bigint') AS [dbid],  
record.value('(/Record/HadrDbMgrAPI/API)[1]', 'varchar(50)') AS [API],  
record.value('(/Record/HadrDbMgrAPI/Action)[1]', 'varchar(50)') AS [Action],  
record.value('(/Record/HadrDbMgrAPI/role)[1]', 'int') AS [Role],  
record.value('(/Record/Stack)[1]', 'varchar(100)') AS [Call Stack]  
FROM hadr  
ORDER BY record.value('(./Record/@time)[1]','bigint') DESC  
GO  
```  
  
  
