---
title: Trabajador de escalabilidad horizontal de SQL Server Integration Services (SSIS) | Microsoft Docs
description: Obtenga información sobre el componente Servicio principal de Escalabilidad horizontal de Escalabilidad horizontal de SQL Server Integration Services (SSIS).
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 35131a210e54557d63cb3e1986bddfd9d75e35ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338377"
---
# <a name="integration-services-ssis-scale-out-worker"></a>Trabajador de escalado horizontal de Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



El trabajador de escalabilidad horizontal ejecuta el servicio de trabajador de escalabilidad horizontal para extraer las tareas de ejecución del patrón de escalabilidad horizontal. Después, ejecuta los paquetes localmente con `ISServerExec.exe`.

## <a name="configure-the-scale-out-worker-service"></a>Configuración del servicio de escalabilidad horizontal
Puede configurar el servicio de trabajador de escalabilidad horizontal mediante el archivo `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`. Deberá reiniciar el servicio después de actualizar el archivo de configuración.

|Configuración  |Descripción  |Valor predeterminado|
|---------|---------|---------|
|DisplayName|Nombre para mostrar del trabajador de escalado horizontal. **No está en uso en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017.**|Nombre de equipo|
|Descripción|Descripción del trabajador de escalado horizontal. **No está en uso en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017.**|Vacío|
|MasterEndpoint|Punto de conexión para conectarse al patrón de escalado horizontal.|Punto de conexión establecido durante la instalación del trabajador de escalado horizontal|
|MasterHttpsCertThumbprint|Huella digital del certificado TLS/SSL de cliente usado para autenticar el patrón de escalabilidad horizontal|Huella digital del certificado de cliente especificado durante la instalación del trabajador de escalado horizontal.|
|WorkerHttpsCertThumbprint|Huella digital del certificado del patrón de escalado horizontal usado para autenticar el trabajador de escalado horizontal.|Huella digital de un certificado creado e instalado automáticamente durante la instalación del trabajador de escalado horizontal|
|StoreLocation|Ubicación del almacén del certificado del trabajador.|LocalMachine|
|StoreName|Nombre del almacén en el que está el certificado de ese trabajador.|My|
|AgentHeartbeatInterval|Intervalo del latido del trabajador de escalado horizontal.|00:01:00|
|TaskHeartbeatInterval|Intervalo del estado de la tarea de generación de informes del trabajador de escalado horizontal.|00:00:10|
|HeartbeatErrorTolerance|Una vez transcurrido este período desde el último latido correcto, la tarea finaliza si se recibe una respuesta de error del latido.|00:10:00|
|TaskRequestMaxCPU|Límite superior de CPU del trabajador de escalado horizontal para solicitar tareas.|70.0|
|TaskRequestMinMemory|Límite inferior de memoria en MB del trabajador de escalado horizontal para solicitar tareas.|100.0|
|MaxTaskCount|Número máximo de tareas que puede contener el trabajador de escalado horizontal.|10|
|LeaseInterval|Intervalo de concesión de una tarea por parte del trabajador de escalado horizontal.|00:01:00|
|TasksRootFolder|Carpeta de registros de tareas. Si el valor está vacío, se usará la ruta de acceso de carpeta `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Tasks`. [cuenta] es la cuenta que ejecuta el servicio de trabajador de escalado horizontal. De forma predeterminada, la cuenta es SSISScaleOutWorker140.|Vacío|
|TaskLogLevel|Nivel de registro de tarea del trabajador de escalado horizontal. (Verbose 0x01, Information 0x02, Warning 0x04, Error 0x08, Progress 0x10, CriticalError 0x20, Audit 0x40)|126 (Information, Warning, Error, Progress, CriticalError, Audit)|
|TaskLogSegment|Intervalo de tiempo de un archivo de registro de tarea.|00:00:00|
|TaskLogEnabled|Especifica si el registro de tarea está habilitado.|true|
|ExecutionLogCacheFolder|Carpeta que se usa para almacenar en caché el registro de ejecución del paquete. Si el valor está vacío, se usará la ruta de acceso de carpeta `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Agent\ELogCache`. [cuenta] es la cuenta que ejecuta el servicio de trabajador de escalado horizontal. De forma predeterminada, la cuenta es SSISScaleOutWorker140.|Vacío|
|ExecutionLogMaxBufferLogCount|Número máximo de registros de ejecución en caché en un búfer de registro de ejecución en memoria.|10000|
|ExecutionLogMaxInMemoryBufferCount|Número máximo de búferes de registro de ejecución en memoria para los registros de ejecución.|10|
|ExecutionLogRetryCount|Número de reintentos si se produce un error en el registro de ejecución.|3|
|ExecutionLogRetryTimeout|Tiempo de expiración de reintentos si se produce un error en el registro de ejecución. i\ExecutionLogRetryCount se omite si se alcanza ExecutionLogRetryTimeout. |7.00:00:00 (7 días)|
|AgentId|Id. de agente de trabajador del trabajador de escalabilidad horizontal|Se genera automáticamente|
||||    

## <a name="view-the-scale-out-worker-log"></a>Ver el registro del trabajador de escalabilidad horizontal
El archivo de registro del servicio de escalabilidad horizontal está en la carpeta `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Agent`.

La ubicación del registro de cada tarea individual está configurada en el archivo `WorkerSettings.config` del `TasksRootFolder`. Si no se especifica ningún valor, el registro estará en la carpeta `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. 

El parámetro *[account]* es la cuenta que ejecuta el servicio de trabajador de escalabilidad horizontal. De forma predeterminada, la cuenta es `SSISScaleOutWorker140`.

## <a name="next-steps"></a>Pasos siguientes
[Servicio principal de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
