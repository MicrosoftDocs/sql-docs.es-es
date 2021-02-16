---
description: catalog.validate_project (base de datos de SSISDB)
title: catalog.validate_project (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5270689a-46d4-4847-b41f-3bed1899e955
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f57af2d5c29af5d3fd904c04637c0ba585b107a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355292"
---
# <a name="catalogvalidate_project-ssisdb-database"></a>catalog.validate_project (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Valida de forma asincrónica un proyecto del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql
catalog.validate_project [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @validate_type = ] validate_type  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @folder_name = ] *folder_name*  
 Nombre de una carpeta que contiene el proyecto. *folder_name* es **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Nombre del proyecto. *project_name* es **nvarchar(128)** .  
  
 [ @validate_type = ] *validate_type*  
 Indica el tipo de validación que se llevará a cabo. Utilice el carácter `F` para realizar una validación completa. Este parámetro es opcional, el carácter `F` se usará de forma predeterminada. El parámetro *validate_type* es **char(1)**.  
  
 [ @validation_id = ] *validation_id*  
 Devuelve el identificador único (ID) de la validación. El parámetro *validation_id* es **bigint**.  
  
 [ @use32bitruntime = ] *use32bitruntime*  
 Indica si el motor en tiempo de ejecución de 32 bits se debe usar para ejecutar el paquete en un sistema operativo de 64 bits. Use el valor de `1` para ejecutar el paquete con el entorno de ejecución de 32 bits cuando se ejecute en un sistema operativo de 64 bits. Use el valor `0` para ejecutar el paquete con el motor en tiempo de ejecución de 64 bits cuando se ejecute en un sistema operativo de 64 bits. Este parámetro es opcional. El parámetro *use32bitruntime* es **bit**.  
  
 [ @environment_scope = ] *environment_scope*  
 Indica las referencias de entorno que la validación tiene en cuenta. Cuando el valor es `A`, todas las referencias de entorno asociadas con el proyecto se incluyen en la validación. Cuando el valor es `S`, solo se incluye una sola referencia de entorno. Cuando el valor es `D`, no se incluyen referencias de entorno y todos los parámetros deben tener un valor literal predeterminado para pasar la validación. Este parámetro es opcional, el carácter `D` se usará de forma predeterminada. El parámetro *environment_scope* es **char(1)** .  
  
 [ @reference_id = ] *reference_id*  
 El identificador único de la referencia de entorno. Este parámetro es obligatorio solo cuando se incluye una sola referencia de entorno en la validación, cuando el parámetro *environment_scope* es `S`. *reference_id* es **bigint**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 La salida de los pasos de la validación se devuelve como secciones diferentes del conjunto de resultados.  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos de lectura en el objeto y, si es aplicable, permisos de lectura en los entornos a los que se hace referencia  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se proporcionan algunas condiciones que pueden producir un error o una advertencia:  
  
-   La validación no puede realizarse para uno o más paquetes del proyecto  
  
-   La validación no puede realizarse si uno o varios de los entornos a los que se hace referencia y que están incluidos en la validación no contienen variables a las que se haga referencia  
  
-   El tipo de validación especificado no es válido  
  
-   El nombre del proyecto o el identificador de la referencia del entorno es no válido  
  
-   El usuario no tiene los permisos adecuados.  
  
## <a name="remarks"></a>Observaciones  
 La validación ayuda a identificar problemas que impedirán que los paquetes del proyecto se ejecuten correctamente. Use las vistas [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md) o [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) para supervisar el estado de validación.  
  
 Solo los entornos que son accesibles por el usuario se pueden utilizar en la validación. El resultado de la validación se envía al cliente como un conjunto de resultados.  
  
 En esta versión, la validación del proyecto no admite la validación de dependencias.  
  
 La validación completa confirma que todas las variables de entorno a las que se hace referencia se encuentran dentro de los entornos a los que se hace referencia y que se incluyeron en la validación. Los resultados de la validación completa enumeran las referencias de entorno que no son válidas y las variables de entorno a las que se hace referencia que podrían no encontrarse en ninguno de los entornos a los que se hace referencia y que se incluyeron en la validación.  
  
  
