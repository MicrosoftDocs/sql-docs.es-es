---
title: Análisis de interbloqueos
titleSuffix: SQL Server Profiler
description: Identifique la causa de un interbloqueo mediante la reproducción y visualización de eventos de interbloqueo para el análisis en SQL Server Profiler y la generación de gráficos de espera.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 72d6718f-501b-4ea6-b344-c0e653f19561
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/03/2017
ms.openlocfilehash: 373bcce8731dad1375efd9a4c2196ab184b38954
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349462"
---
# <a name="analyze-deadlocks-with-sql-server-profiler"></a>Analizar interbloqueos con SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Use el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para identificar la causa de un interbloqueo. Un interbloqueo se produce cuando hay una dependencia cíclica entre dos o más subprocesos o procesos para algún conjunto de recursos en SQL Server. El [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]le permite crear un seguimiento que registra, reproduce y muestra eventos de interbloqueo para su análisis.  
  
 Para realizar un seguimiento de los eventos de interbloqueo, agregue la clase de evento **Deadlock graph** a un seguimiento. Esta clase de evento rellena la columna de datos **TextData** del seguimiento con datos XML acerca de los procesos y objetos implicados en el interbloqueo. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pueden extraer el documento XML a un archivo XML de interbloqueo (.xdl) que puede ver después en SQL Server Management Studio. Puede configurar el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para extraer eventos **Deadlock graph** en un solo archivo que contenga todos los eventos **Deadlock graph** o en archivos independientes. Esta extracción se puede hacer de las siguientes formas:  
  
-   Al configurar el seguimiento, mediante la pestaña **Configuración de extracción de eventos** . Tenga en cuenta que esta pestaña no aparece hasta que haya seleccionado el evento **Deadlock graph** en la pestaña **Selección de eventos** .  
  
-   Mediante la opción **Extraer eventos de SQL Server** del menú **Archivo** .  
  
-   También puede extraer y guardar eventos individuales al hacer clic con el botón derecho en un evento concreto y elegir **Extraer datos de evento**.  
  
## <a name="deadlock-graphs"></a>Gráficos de interbloqueo  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] y [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usan un gráfico de espera de interbloqueo para describir un interbloqueo. El gráfico de espera de interbloqueo contiene nodos de proceso, nodos de recurso y bordes que representan las relaciones entre los procesos y los recursos. Los componentes de los gráficos de espera se definen en la siguiente tabla:  
  
 Nodo de proceso  
 Un subproceso que realiza una tarea; por ejemplo, INSERT, UPDATE o DELETE.  
  
 Nodo de recurso  
 Un objeto de la base de datos; por ejemplo, una tabla, un índice o una fila.  
  
 Edge  
 Una relación entre un proceso y un recurso. Un borde **request** se produce cuando un proceso espera un recurso. Un borde **owner** se produce cuando un recurso espera un proceso. El modo de bloqueo se incluye en la descripción del borde. Por ejemplo, **Modo: X**.  
  
## <a name="deadlock-process-node"></a>Nodo de proceso de interbloqueo  
 En un gráfico de espera, el nodo de proceso contiene información acerca del proceso. En la tabla siguiente se explican los componentes de un proceso.  
  
|Componente|Definición|  
|---------------|----------------|  
|Id. de proceso de servidor|Identificador de proceso de servidor (SPID), un identificador asignado a un servidor para el proceso que posee el bloqueo.|  
|Id. de lote de servidores|Identificador de lote de servidores (SBID).|  
|Id. de contexto de ejecución|Identificador de contexto de ejecución (ECID). Id. de contexto de ejecución de un subproceso dado asociado con un SPID específico.<br /><br /> ECID = {0,1,2,3, *...n*}, donde 0 siempre representa el subproceso principal o primario, y {1,2,3, *...n*} representan los subprocesos secundarios.|  
|Prioridad de interbloqueo|Prioridad de interbloqueo del proceso. Para obtener más información sobre los valores posibles, vea [SET DEADLOCK_PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/set-deadlock-priority-transact-sql.md).|  
|Registro utilizado|Cantidad de espacio del registro utilizado por el proceso.|  
|Id. de propietario|Id. de transacción de los procesos que están usando transacciones y que actualmente están esperando en un bloqueo.|  
|Descriptor de transacción|Puntero al descriptor de transacción que describe el estado de la transacción.|  
|Búfer de entrada|Búfer de entrada del proceso actual; define el tipo de evento y la instrucción que se está ejecutando. Los valores posibles son:<br /><br /> **Lenguaje**<br /><br /> **RPC**<br /><br /> **None**|  
|.|Tipo de instrucción. Los valores posibles son:<br /><br /> **NOP**<br /><br /> **SELECT**<br /><br /> **UPDATE**<br /><br /> **INSERT**<br /><br /> **DELETE**<br /><br /> **Unknown**|  
  
## <a name="deadlock-resource-node"></a>Nodo de recurso de interbloqueo  
 En un interbloqueo, dos procesos están esperando un recurso retenido por el otro recurso. En un gráfico de interbloqueo, los recursos se muestran como nodos de recursos.  
  
  
