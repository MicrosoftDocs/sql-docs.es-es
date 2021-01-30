---
description: sys.dm_external_script_requests
title: sys.dm_external_script_requests | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- sys.dm_external_script_requests
- sys.dm_external_script_requests_TSQL
- dm_external_script_requests
- dm_external_script_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_requests dynamic management view
ms.assetid: e7e7c50f-b8b2-403c-b8c8-1955da5636c3
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: ca4cfdd348d04ab0332653fa6851107ffa5f16d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207120"
---
# <a name="sysdm_external_script_requests"></a>sys.dm_external_script_requests
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Devuelve una fila para cada cuenta de trabajo activa que ejecuta un script externo.
  
> [!NOTE]
> Esta vista de administración dinámica (DMV) solo está disponible si ha instalado y habilitado la característica que admite la ejecución de scripts externos. Para obtener más información, consulte [Machine Learning Services (r, Python) en SQL Server 2017 y versiones posteriores](../../machine-learning/sql-server-machine-learning-services.md), [R Services en SQL Server 2016](../../machine-learning/r/sql-server-r-services.md)y [Machine Learning Services en Azure SQL instancia administrada](/azure/azure-sql/managed-instance/machine-learning-services-overview).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|external_script_request_id|**identificador único**|Identificador del proceso que envió la solicitud de script externo. Esto corresponde al identificador de proceso como recibió la instancia de SQL.|  
|language|**nvarchar**|Palabra clave que representa un lenguaje de script compatible. |  
|degree_of_parallelism|**int**|Número que indica el número de procesos paralelos que se crearon. Este valor podría ser diferente del número de procesos paralelos que se solicitaron.|  
|external_user_name|**nvarchar**|La cuenta de trabajo de Windows bajo la que se ejecutó el script.|  
  
## <a name="permissions"></a>Permisos

 Requiere el `VIEW SERVER STATE` permiso en el servidor.  
  
> [!NOTE]
> Los usuarios que ejecutan scripts externos deben tener el permiso adicional `EXECUTE ANY EXTERNAL SCRIPT` ; sin embargo, los administradores pueden usar esta DMV sin este permiso. 
  
## <a name="remarks"></a>Observaciones  

Esta vista se puede filtrar usando el identificador del lenguaje de script.

La vista también devuelve la cuenta de trabajo en la que se ejecuta el script. Para obtener información sobre las cuentas de trabajo que usan los scripts externos, consulte la sección identidades usadas en el procesamiento (SQLRUserGroup) en [información general sobre seguridad para el marco de extensibilidad en SQL Server Machine Learning Services](../../machine-learning/concepts/security.md#sqlrusergroup).

El GUID que se devuelve en el campo **external_script_request_id** también representa el nombre de archivo del directorio seguro donde se almacenan los archivos temporales. Cada cuenta de trabajo, como MSSQLSERVER01, representa un único inicio de sesión de SQL o usuario de Windows y se puede usar para ejecutar varias solicitudes de script. De forma predeterminada, estos archivos temporales se limpian tras la finalización del script.

Esta DMV supervisa solo los procesos activos y no puede informar sobre scripts que ya han completado. Si necesita realizar un seguimiento de la duración de scripts, es recomendable agregar información de tiempo en el script y capturar eso como parte de la ejecución del script.

## <a name="examples"></a>Ejemplos  
  
### <a name="viewing-the-currently-active-scripts-for-a-particular-process"></a>Ver los scripts activos actualmente para un proceso determinado

 En el ejemplo siguiente se muestra el número de ejecuciones de scripts externos que tienen lugar en la instancia actual.  
  
```sql
SELECT external_script_request_id
  , [language]
  , degree_of_parallelism
  , external_user_name
FROM sys.dm_external_script_requests;
```  

Resultados  

external_script_request_id  |language  |degree_of_parallelism  |external_user_name  
---------|---------|---------|---------
183EE6FC-7399-4318-AA2E-7A6C68E435A8     |     R    |      1   |  MSSQLSERVER01

## <a name="see-also"></a>Vea también

+ [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
+ [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  
