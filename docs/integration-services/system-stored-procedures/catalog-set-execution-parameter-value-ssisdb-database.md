---
description: catalog.set_execution_parameter_value (base de datos de SSISDB)
title: catalog.set_execution_parameter_value (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 055d86c9-befd-4e63-acb1-6dfe833549d2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c8c294aa9aefa326ad9b8d84ddb599f99f181cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273176"
---
# <a name="catalogset_execution_parameter_value-ssisdb-database"></a>catalog.set_execution_parameter_value (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece el valor de un parámetro para una instancia de ejecución en el catálogo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 No se puede cambiar un valor de parámetro después de que se haya iniciado una instancia de ejecución.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.set_execution_parameter_value [ @execution_id = execution_id  
    , [ @object_type = ] object_type  
    , [ @parameter_name = ] parameter_name  
    , [ @parameter_value = ] parameter_value  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @execution_id = ] *execution_id*  
 Identificador único de la instancia de ejecución. El parámetro *execution_id* es de tipo **bigint**.  
  
 [ @object_type = ] *object_type*  
 Tipo de parámetro.  
  
 Para los parámetros siguientes, establezca *object_type* en 50.  
  
-   LOGGING_LEVEL  
  
-   CUSTOMIZED_LOGGING_LEVEL  
  
-   DUMP_ON_ERROR  
  
-   DUMP_ON_EVENT  
  
-   DUMP_EVENT_CODE  
  
-   CALLER_INFO  
  
-   SYNCHRONIZED  
  
 Use el valor `20` para indicar un parámetro de proyecto o el valor `30` para indicar un parámetro de paquete.  
  
 El parámetro *object_type* es de tipo **smallInt**.  
  
 [ @parameter_name = ] *parameter_name*  
 El nombre del parámetro. El parámetro *parameter_name* es de tipo **nvarchar(128)**.  
  
 [ @parameter_value = ] *parameter_value*  
 Valor del parámetro. El parámetro *parameter_value* es de tipo **sql_variant**.  
  
## <a name="remarks"></a>Observaciones  
 Para determinar los valores de parámetros usados para una ejecución determinada, consulte la vista catalog.execution_parameter_values.  
  
 Para especificar el ámbito de la información que se registra durante una ejecución de paquetes, establezca *parameter_name* en LOGGING_LEVEL y *parameter_value* en uno de los valores siguientes.  
  
 Establezca el parámetro *object_type* en 50.  
  
|Value|Descripción|  
|-----------|-----------------|  
|0|None<br /><br /> El registro está desactivado. Solo se registra el estado de ejecución del paquete.|  
|1|Básico<br /><br /> Se registran todos los eventos, excepto los eventos personalizados y de diagnóstico. Este es el valor predeterminado.|  
|2|Rendimiento<br /><br /> Solo se registran las estadísticas de rendimiento, y los eventos OnError y OnWarning.|  
|3|Verbose<br /><br /> Se registran todos los eventos, incluidos los eventos personalizados y de diagnóstico. <br />Los eventos personalizados incluyen los eventos registrados por las tareas de Integration Services. Para más información, vea [Mensajes personalizados para registro](../../integration-services/performance/integration-services-ssis-logging.md#custom_messages).|  
|4|Linaje en tiempo de ejecución<br /><br /> Recopila los datos necesarios para realizar un seguimiento del linaje en el flujo de datos.|  
|100|Nivel de registro personalizado<br /><br /> Especifique la configuración en el parámetro CUSTOMIZED_LOGGING_LEVEL. Para obtener más información acerca de los valores que se pueden especificar, vea [catalog.create_customized_logging_level](../../integration-services/system-stored-procedures/catalog-create-customized-logging-level.md).<br /><br /> Para obtener más información acerca de los niveles de registro personalizados, vea [Habilitar el registro para la ejecución de paquetes en el servidor SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).|  
  
 Para especificar que el servidor de Integration Services genere archivos de volcado cuando se produzca algún error durante la ejecución del paquete, establezca los siguientes valores de parámetro para una instancia de ejecución que no se haya ejecutado.  
  
|Parámetro|Value|  
|---------------|-----------|  
|*execution_id*|Identificador único de la instancia de ejecución.|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_ERROR|  
|*parameter_value*|1|  
  
 Para especificar que el servidor de Integration Services genere archivos de volcado cuando se produzcan eventos durante la ejecución del paquete, establezca los siguientes valores de parámetro para una instancia de ejecución que no se haya ejecutado.  
  
|Parámetro|Value|  
|---------------|-----------|  
|*execution_id*|Identificador único de la instancia de ejecución.|  
|*object_type*|50|  
|*parameter_name*|'DUMP_ON_EVENT|  
|*parameter_value*|1|  
  
 Para especificar los eventos durante la ejecución del paquete que causan que el servidor de Integration Services genere archivos de volcado, establezca los siguientes valores de parámetro para una instancia de ejecución que no se haya ejecutado. Separe los diversos códigos de evento mediante un punto y coma.  
  
|Parámetro|Value|  
|---------------|-----------|  
|*execution_id*|Identificador único de la instancia de ejecución.|  
|*object_type*|50|  
|*parameter_name*|DUMP_EVENT_CODE|  
|*parameter_value*|Uno o varios códigos de evento.|  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-generate-dump-files-for-errors"></a>A. Generar archivos de volcado en el caso de errores

 En el ejemplo siguiente se especifica que el servidor de Integration Services genera archivos de volcado cuando se produce un error durante la ejecución del paquete.  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_ERROR',1  
```  
  
### <a name="b-generate-dump-files-for-events"></a>B. Generar archivos de volcado en el caso de eventos

 En el ejemplo siguiente se especifica que el servidor de Integration Services genera archivos de volcado cuando se producen eventos durante la ejecución del paquete, y se especifica el evento que hace que el servidor genere los archivos.  
  
```sql
exec catalog.create_execution  'TR2','Recurring ETL', 'Dim_DCVendor.dtsx',NULL, 0,@execution_id out  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_ON_EVENT',1  
  
declare @event_code nvarchar(50)  
set @event_code = '0xC020801C'  
exec catalog.set_execution_parameter_value  @execution_id, 50, 'DUMP_EVENT_CODE', @event_code  
```  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos READ y MODIFY en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El usuario no tiene los permisos adecuados.  
  
-   El identificador de ejecución no es válido  
  
-   El nombre de parámetro no es válido  
  
-   El tipo de datos del valor de parámetro no coincide con el tipo de datos del parámetro.  
  
## <a name="see-also"></a>Consulte también  
 [catalog.execution_parameter_values &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)   
 [Generar archivos de volcado para la ejecución de paquetes](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
  
