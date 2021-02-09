---
description: catalog.execution_component_phases
title: catalog.execution_component_phases | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 07a9a163-4787-40f7-b371-ac5c6cb4b095
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d449dae68ce74abcf8509eb9f6b2e22ef46c51bb
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835316"
---
# <a name="catalogexecution_component_phases"></a>catalog.execution_component_phases 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra el tiempo dedicado por un componente de flujo de datos a cada fase de ejecución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|phase_stats_id|**bigint**|Identificador (id.) único de la fase.|  
|execution_id|**bigint**|Identificador único de la instancia de ejecución.|  
|package_name|**nvarchar(260)**|Nombre del primer paquete que se inició durante la ejecución.|  
|task_name|**nvarchar(4000)**|Nombre de la tarea Flujo de datos.|  
|subcomponent_name|**nvarchar(4000)**|Nombre del componente de flujo de datos.|  
|phase|**nvarchar(128)**|Nombre de la fase de ejecución.|  
|start_time|**datetimeoffset(7)**|Hora a la que se inició la fase.|  
|end_time|**datetimeoffset(7)**|Hora a la que finalizó la fase.|  
|execution_path|**nvarchar(max)**|Ruta de ejecución de la tarea Flujo de datos.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra una fila para cada fase de ejecución de un componente de flujo de datos, como Validate, Pre-Execute, Post-Execute, PrimeOutput y ProcessInput. Cada fila muestra la hora de inicio y de finalización de una fase de ejecución concreta.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se usa la vista catalog.execution_component_phases para averiguar la cantidad total de tiempo que invierte un paquete concreto para su ejecución en todas las fases (**active_time**) y el tiempo transcurrido total para el paquete (**total_time**).  
  
> [!WARNING]  
>  La vista catalog.execution_component_phases proporciona esta información cuando el nivel de registro de la ejecución del paquete se establece en Performance (Rendimiento) o Verbose (Detallado). Para más información, vea [Habilitar el registro para la ejecución de paquetes en el servidor SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
  
```sql
use SSISDB  
select package_name, task_name, subcomponent_name, execution_path,  
    SUM(DATEDIFF(ms,start_time,end_time)) as active_time,  
    DATEDIFF(ms,min(start_time), max(end_time)) as total_time  
from catalog.execution_component_phases  
where execution_id = 1841  
group by package_name, task_name, subcomponent_name, execution_path  
order by package_name, task_name, subcomponent_name, execution_path  
```  
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Permiso READ en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
> [!NOTE]  
>  Cuando se dispone de permiso para realizar una operación en el servidor, también se dispone de permiso para ver información sobre la operación. Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
  
