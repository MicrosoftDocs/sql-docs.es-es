---
title: Patrón de escalabilidad horizontal | Microsoft Docs
description: Obtenga información sobre el componente Servicio principal de Escalabilidad horizontal del Servicio principal de Escalabilidad horizontal de SQL Server Integration Services (SSIS).
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 8f3b205acd32ba48b86ac797fd99fe4cef4fccf5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347403"
---
# <a name="integration-services-ssis-scale-out-master"></a>Patrón de escalado horizontal de Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



El Servicio principal de escalabilidad horizontal administra el sistema de escalabilidad horizontal mediante el catálogo SSISDB y el propio servicio. 

El catálogo SSISDB almacena toda la información de los trabajos de escalabilidad horizontal, los paquetes y las ejecuciones. Proporciona la interfaz para habilitar un trabajador de escalado horizontal y ejecutar paquetes en el escalado horizontal. Para más información, consulte [Tutorial: Configurar la escalabilidad horizontal de Integration Services](walkthrough-set-up-integration-services-scale-out.md) y [Ejecutar paquetes en Integration Services](run-packages-in-integration-services-ssis-scale-out.md).

El Servicio principal de escalabilidad horizontal es un servicio de Windows que se encarga de la comunicación con los trabajos de escalabilidad horizontal. Devuelve el estado de las ejecuciones de paquetes con los trabajos de escalabilidad horizontal a través de HTTPS y opera en los datos en SSISDB. 

## <a name="scale-out-views-and-stored-procedures-in-ssisdb"></a>Vistas y procedimientos almacenados relativos a la escalabilidad horizontal en SSISDB

### <a name="views"></a>Vistas

- [[catalog].[master_properties]](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)
- [[catalog].[worker_agents]](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md)

### <a name="stored-procedures"></a>Procedimientos almacenados

- Para administrar los trabajos de escalabilidad horizontal:
    - [[catalog].[disable_worker_agent]](../../integration-services/system-stored-procedures/catalog-disable-worker-agent-ssisdb-database.md)
    - [[catalog].[enable_worker_agent]](../../integration-services/system-stored-procedures/catalog-enable-worker-agent-ssisdb-database.md)

- Para ejecutar paquetes en escalabilidad horizontal:
    - [[catalog].[create_execution]](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)
    - [[catalog].[add_execution_worker]](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)
    - [[catalog].[start_execution]](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)

## <a name="configure-the-scale-out-master-service"></a>Configuración del Servicio principal de escalabilidad horizontal

Configure el Servicio principal de escalabilidad horizontal con el archivo `<drive>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`. Deberá reiniciar el servicio después de actualizar el archivo de configuración.


|Configuración  |Descripción  |Valor predeterminado  |
|---------|---------|---------|
|PortNumber|Número de puerto de red usado para comunicarse con un trabajador de escalado horizontal.|8391|
|SSLCertThumbprint|Huella digital del certificado TLS/SSL usado para proteger la comunicación con un trabajo de escalabilidad horizontal.|Huella digital del certificado SSL especificado durante la instalación del patrón de escalabilidad horizontal|
|SQLServerName|Es el nombre de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] que contiene el catálogo SSISDB. Por ejemplo, NombreServidor\\NombreInstancia.|Es el nombre del servidor de SQL Server que se instala con el patrón de escalabilidad horizontal.|
|CleanupCompletedJobsIntervalInMs|Intervalo de limpieza de los trabajos de ejecución terminados en milisegundos.|43200000|
|DealWithExpiredTasksIntervalInMs|Intervalo para tratar con los trabajos de ejecución terminados en milisegundos.|300000|
|MasterHeartbeatIntervalInMs|Intervalo de latido del patrón de escalado horizontal en milisegundos. Esta propiedad especifica el intervalo según el que el Servicio principal de escalabilidad horizontal actualiza su estado de conexión en el catálogo SSISDB.|30000|
|SqlConnectionTimeoutInSecs|Es el tiempo de espera de la conexión de SQL (en segundos) al conectarse a SSISDB.|15|
||||    

## <a name="view-the-scale-out-master-service-log"></a>Ver el registro del Servicio principal de escalabilidad horizontal

El archivo de registro del Servicio principal de escalabilidad horizontal se encuentra en la carpeta `<drive>:\Users\[account]\AppData\Local\SSIS\ScaleOut\Master`. 

El parámetro *[cuenta]* indica la cuenta que ejecuta el Servicio principal de escalabilidad horizontal. De forma predeterminada, la cuenta es `SSISScaleOutMaster140`.

## <a name="next-steps"></a>Pasos siguientes

[Trabajo de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)
