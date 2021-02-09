---
description: catalog.catalog_properties (base de datos SSISDB)
title: catalog.catalog_properties (base de datos SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 12/11/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e604a382-95c8-4764-b268-742eb5c6d4cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4028247051220e1e45d054e400da050601254fb2
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835883"
---
# <a name="catalogcatalog_properties-ssisdb-database"></a>catalog.catalog_properties (base de datos SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra las propiedades del catálogo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|property_name|**nvarchar(256)**|Nombre de la propiedad de catálogo.|  
|property_value|**nvarchar(256)**|Valor de la propiedad de catálogo.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra una fila para cada propiedad de catálogo.
  
|Nombre de propiedad|Descripción|  
|-------------------|-----------------|  
|**DEFAULT_EXECUTION_MODE**|Modo de ejecución de paquetes predeterminado de todo el servidor: `Server` (0) o `Scale Out` (1). |
|**ENCRYPTION_ALGORITHM**|El tipo de algoritmo de cifrado que se utiliza para cifrar los datos confidenciales. Entre los valores compatibles se incluyen: `DES`, `TRIPLE_DES`, `TRIPLE_DES_3KEY`, `DESX`, `AES_128`, `AES_192` y `AES_256`. Nota: para modificar esta propiedad la base de datos de catálogo debe estar en modo de usuario único.|
|**IS_SCALEOUT_ENABLED**|Cuando el valor es `True`, se habilita la característica de escalabilidad horizontal de SSIS. Si no habilitó la escalabilidad horizontal, es posible que esta propiedad no aparezca en la vista.|
|**MAX_PROJECT_VERSIONS**|Es el número de nuevas versiones de proyecto que se retendrán en un proyecto único. Si está habilitada la limpieza de versiones, se eliminarán las versiones anteriores que superen este número.|  
|**OPERATION_CLEANUP_ENABLED**|Si el valor es `TRUE`, se eliminarán del catálogo los detalles y los mensajes de la operación anteriores a **RETENTION_WINDOW** (días). Si el valor es `FALSE`, se almacenarán todos los detalles y los mensajes de la operación en el catálogo. Nota: un trabajo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la limpieza de operaciones.|  
|**RETENTION_WINDOW**|Número de días que los detalles y los mensajes de la operación estarán almacenados en el catálogo. Si el valor es `-1`, la ventana de retención es infinita. Nota: Si no quiere realizar ninguna limpieza, establezca **OPERATION_CLEANUP_ENABLED** en **FALSE**.|
|**SCHEMA_BUILD**|Es el número de compilación del esquema de base de datos del catálogo SSISDB. Este número cambia cada vez que se crea o actualiza el catálogo de SSISDB.|
|**SCHEMA_VERSION**|Es el número de versión principal del esquema de base de datos del catálogo SSISDB. Este número cambia cada vez que se crea o actualiza la versión principal del catálogo de SSISDB.|
|**VALIDATION_TIMEOUT**|Las validaciones se detienen si no se completan dentro del número de segundos que especificó la propiedad.|  
|**SERVER_CUSTOMIZED_LOGGING_LEVEL**|Es el nivel de registro personalizado y predeterminado del servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si no creó los niveles de registro personalizados, es posible que esta propiedad no aparezca en la vista.|
|**SERVER_LOGGING_LEVEL**|Nivel de registro predeterminado del servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].|
|**SERVER_OPERATION_ENCRYPTION_LEVEL**|Cuando el valor es 1 (`PER_EXECUTION`), el certificado y la clave simétrica que se usan para proteger los parámetros de ejecución confidenciales y los registros de ejecución se crean para cada *ejecución*. Cuando el valor es 2 (`PER_PROJECT`), el certificado y la clave simétrica se crean una vez por cada *proyecto*. Para obtener más información acerca de esta propiedad, consulte la sección Comentarios del procedimiento almacenado [catalog.cleanup_server_log](../system-stored-procedures/catalog-cleanup-server-log.md#remarks) de SSIS.|
|**VERSION_CLEANUP_ENABLED**|Si el valor es `TRUE`, solo el número **MAX_PROJECT_VERSIONS** de versiones del proyecto se almacena en el catálogo y se eliminan todas las demás versiones del proyecto. Si el valor es **FALSE**, se almacenan todas las versiones del proyecto en el catálogo. Nota: un trabajo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la limpieza de operaciones.|
|||
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
  
