---
description: catalog.execution_parameter_values (base de datos de SSISDB)
title: catalog.execution_parameter_values (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ec93e67b-04ce-4aae-ab96-3ad20e9793ad
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae198e909c64c84b2aa02f074b903dd4587c6c41
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835182"
---
# <a name="catalogexecution_parameter_values-ssisdb-database"></a>catalog.execution_parameter_values (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra los valores de parámetro reales utilizados por los paquetes [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] durante una instancia de ejecución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|execution_parameter_id|**bigint**|Identificador único (ID) del parámetro de ejecución.|  
|execution_id|**bigint**|Identificador único de la instancia de ejecución.|  
|object_type|**smallint**|Cuando el valor es `20`, el valor del parámetro es un parámetro de proyecto. Cuando el valor es `30`, el valor del parámetro es un parámetro de paquete. Cuando el valor es `50`, el parámetro es uno de los siguientes:<br /><br /> **LOGGING_LEVEL**<br /><br /> **DUMP_ON_ERROR**<br /><br /> **DUMP_ON_EVENT**<br /><br /> **DUMP_EVENT_CODE**<br /><br /> **CALLER_INFO**<br /><br /> **SYNCHRONIZED**|  
|parameter_data_type|**nvarchar(128)**|El tipo de datos del parámetro.|  
|parameter_name|**sysname**|El nombre del parámetro.|  
|parameter_value|**sql_variant**|Valor del parámetro. Cuando el parámetro "sensitive" es `0`, se muestra el valor de texto no cifrado. Cuando el parámetro "sensitive" es `1`, se muestra el valor **NULL**.|  
|sensitive|**bit**|Cuando el valor es `1`, el valor del parámetro es confidencial. Cuando el valor es `0`, el valor del parámetro no es confidencial.|  
|requerido|**bit**|Cuando el valor es `1`, se requiere el valor del parámetro para iniciar la ejecución. Cuando el valor es `0`, no se requiere el valor del parámetro para iniciar la ejecución.|  
|value_set|**bit**|Cuando el valor es `1`, el valor del parámetro se ha asignado. Cuando el valor es `0`, el valor del parámetro no se ha asignado.|  
|runtime_override|**bit**|Si el valor es `1`, el valor original del parámetro se modificó antes de que se iniciara la ejecución. Si el valor es `0`, el valor del parámetro es el valor original que se estableció.|  
  
## <a name="remarks"></a>Observaciones  
 En esta vista se muestra una fila para cada parámetro de ejecución del catálogo. Un valor de parámetro de ejecución es el valor asignado a un parámetro de proyecto o un parámetro del paquete durante una única instancia de ejecución.  
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Permiso READ en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
> [!NOTE]  
>  Cuando se dispone de permiso para realizar una operación en el servidor, también se dispone de permiso para ver información sobre la operación. Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
  
