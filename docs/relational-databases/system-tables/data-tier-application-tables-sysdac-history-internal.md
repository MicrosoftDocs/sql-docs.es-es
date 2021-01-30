---
description: 'Tablas de aplicación de capa de datos: sysdac_history_internal'
title: sysdac_history_internal (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdac_history_internal
- sysdac_history_internal_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysdac_history_internal
ms.assetid: 774a1678-0b27-42be-8adc-a6d7a4a56510
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2c1770701b0373e56bac0a6c7c5bb2e9eac44ad3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187936"
---
# <a name="data-tier-application-tables---sysdac_history_internal"></a>Tablas de aplicación de capa de datos: sysdac_history_internal
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información sobre las acciones realizadas para administrar las aplicaciones de capa de datos (DAC). Esta tabla se almacena en el esquema **DBO** de la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**action_id**|**int**|Identificador de la acción|  
|**sequence_id**|**int**|Identifica un paso dentro de una acción.|  
|**instance_id**|**uniqueidentifier**|Identificador de la instancia de DAC. Esta columna se puede combinar en la columna **instance_id** en [dbo.sysdac_instances &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md).|  
|**action_type**|**tinyint**|Identificador del tipo de acción:<br /><br /> **0** = implementar<br /><br /> **1** = crear<br /><br /> **2** = cambiar nombre<br /><br /> **3** = desasociar<br /><br /> **4** = eliminar|  
|**action_type_name**|**VARCHAR (19)**|Nombre del tipo de acción:<br /><br /> **deploy**<br /><br /> **create**<br /><br /> **rename**<br /><br /> **conecta**<br /><br /> **delete**|  
|**dac_object_type**|**tinyint**|Identificador del tipo de objeto afectado por la acción:<br /><br /> **0** = dacpac<br /><br /> **1** = inicio de sesión<br /><br /> **2** = base de datos|  
|**dac_object_type_name**|**VARCHAR (8)**|Nombre del tipo de objeto afectado por la acción:<br /><br /> **dacpac** = instancia de DAC<br /><br /> **login**<br /><br /> **database**|  
|**action_status**|**tinyint**|Código que identifica el estado actual de la acción:<br /><br /> **0** = pendiente<br /><br /> **1** = correcto<br /><br /> **2** = error|  
|**action_status_name**|**VARCHAR (11)**|Estado actual de la acción:<br /><br /> **pending**<br /><br /> **realizado**<br /><br /> **puedan**|  
|**Obligatorio**|**bit**|Lo utiliza el [!INCLUDE[ssDE](../../includes/ssde-md.md)] al revertir una operación DAC.|  
|**dac_object_name_pretran**|**sysname**|Nombre del objeto antes de que se confirme la transacción que contiene la acción. Solo se utiliza para las bases de datos e inicios de sesión.|  
|**dac_object_name_posttran**|**sysname**|Nombre del objeto después de que se confirme la transacción que contiene la acción. Solo se utiliza para las bases de datos e inicios de sesión.|  
|**sqlscript**|**nvarchar(max)**|Script [!INCLUDE[tsql](../../includes/tsql-md.md)] que implementa una acción en una base de datos o inicio de sesión.|  
|**útiles**|**varbinary(max)**|Definición de paquete DAC guardada en una cadena con codificación binaria.|  
|**Comentarios**|**ntext**|Registra el inicio de sesión de un usuario que ha aceptado una pérdida de datos potencial en una actualización de DAC.|  
|**error_string**|**nvarchar(max)**|Mensaje de error generado si la acción encuentra un error.|  
|**created_by**|**sysname**|Inicio de sesión que inició la acción que creó esta entrada.|  
|**date_created**|**datetime**|Fecha y hora en que se creó esta entrada.|  
|**date_modified**|**datetime**|Fecha y hora en la que se modificó por última vez la entrada.|  
  
## <a name="remarks"></a>Observaciones  
 Las acciones de administración de una DAC, por ejemplo su implementación o eliminación, generan varios pasos. A cada acción se le asigna un identificador de acción. A cada paso se le asigna un número de secuencia y una fila en **sysdac_history_internal**, donde se registra el estado del paso. Cada fila se crea cuando el paso de la acción comienza y se actualiza según convenga para reflejar el estado de la operación. Por ejemplo, se puede asignar una acción de implementación de DAC **action_id** 12 y obtener cuatro filas en **sysdac_history_internal**:  
  
| action_id | sequence_id | action_type_name | dac_object_type_name |
| --------- | ----------- | ---------------- | -------------------- |
|12|0|create|dacpac|  
|12|1|create|login|  
|12|2|create|database|  
|12|3|rename|database|  
  
 Las operaciones de DAC, como eliminar, no quitan filas de **sysdac_history_internal**. Puede usar la siguiente consulta para eliminar manualmente las filas de las DAC que ya no se implementen en una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]:  
  
```sql  
DELETE FROM msdb.dbo.sysdac_history_internal  
WHERE instance_id NOT IN  
   (SELECT instance_id  
    FROM msdb.dbo.sysdac_instances_internal);  
```  
  
 Si se eliminan filas de DAC activas, no influyen en las operaciones DAC; lo único que ocurre es que no podrá informar del historial completo de la DAC.  
  
> [!NOTE]  
>  Actualmente, no hay ningún mecanismo para eliminar **sysdac_history_internal** filas en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor sysadmin. El acceso de solo lectura a esta vista está disponible para todos los usuarios con permisos para conectarse a la base de datos maestra.  
  
## <a name="see-also"></a>Consulte también  
 [Aplicaciones de capa de datos](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [dbo.sysdac_instances &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md)   
 [sysdac_instances_internal &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/data-tier-application-tables-sysdac-instances-internal.md)  
  
  
