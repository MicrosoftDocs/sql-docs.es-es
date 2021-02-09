---
description: catalog.execution_property_override_values
title: catalog.execution_property_override_values | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 83cbdd6f-ddde-47bf-abde-36bd24272621
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 36c222a0d08782e252e171ceb2f0004bf8f2ac8d
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835116"
---
# <a name="catalogexecution_property_override_values"></a>catalog.execution_property_override_values 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra los valores de invalidación de la propiedad que se establecieron durante la ejecución del paquete.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|property_id|**bigint**|Identificador único para el valor de invalidación de la propiedad.|  
|execution_id|**bigint**|Identificador único de la instancia de ejecución.|  
|property_path|**nvarchar(4000)**|Ruta de acceso a la propiedad en el paquete.|  
|property_value|**nvarchar(max)**|Valor invalidación de la propiedad.|  
|sensitive|**bit**|Cuando el valor es 1, la propiedad es confidencial y se cifra cuando se almacena. Cuando el valor es 0, la propiedad no es confidencial y el valor se almacena como texto simple.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra una fila para cada ejecución en la que los valores de propiedad se invalidan mediante la sección **Invalidaciones de propiedad** en la pestaña **Opciones avanzadas** del diálogo **Ejecutar paquete**. La ruta de acceso a la propiedad se deriva de la propiedad **Ruta de acceso del paquete** de la tarea del paquete.  
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Permiso READ en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
> [!NOTE]  
>  Cuando se dispone de permiso para realizar una operación en el servidor, también se dispone de permiso para ver información sobre la operación. Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
  
