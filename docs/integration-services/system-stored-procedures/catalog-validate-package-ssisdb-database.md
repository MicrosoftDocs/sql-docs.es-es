---
description: catalog.validate_package (base de datos de SSISDB)
title: catalog.validate_package (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- validate_package stored procedure [Integration Services]
- catalog.validate_package stored procedure [Integration Services]
ms.assetid: 0dc03df1-b793-408f-af4c-c11188729abf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2aca1e5c0b1ace9e1020b58464459415a8939ce1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350258"
---
# <a name="catalogvalidate_package-ssisdb-database"></a>catalog.validate_package (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Valida de forma asincrónica un paquete del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql
catalog.validate_package [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @package_name = ] package_name  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @folder_name = ] *folder_name*  
 El nombre de la carpeta que contiene el paquete. *folder_name* es **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 El nombre del proyecto que contiene el paquete. *project_name* es **nvarchar(128)** .  
  
 [ @package_name = ] *package_name*  
 Nombre del paquete. El parámetro *package_name* es de tipo **nvarchar(260)**.  
  
 [ @validation_id = ] *validation_id*  
 Devuelve el identificador único (ID) de la validación. El parámetro *validation_id* es **bigint**.  
  
 [ @use32bitruntime = ] *use32bitruntime*  
 Indica si el motor en tiempo de ejecución de 32 bits se debe usar para ejecutar el paquete en un sistema operativo de 64 bits. Use el valor de `1` para ejecutar el paquete con el entorno de ejecución de 32 bits cuando se ejecute en un sistema operativo de 64 bits. Use el valor `0` para ejecutar el paquete con el motor en tiempo de ejecución de 64 bits cuando se ejecute en un sistema operativo de 64 bits. Este parámetro es opcional. El parámetro *use32bitruntime* es **bit**.  
  
 [ @environment_scope = ] *environment_scope*  
 Indica las referencias de entorno que la validación tiene en cuenta. Cuando el valor es `A`, todas las referencias de entorno asociadas con el proyecto se incluyen en la validación. Cuando el valor es `S`, solo se incluye una sola referencia de entorno. Cuando el valor es `D`, no se incluyen referencias de entorno y todos los parámetros deben tener un valor literal predeterminado para pasar la validación. Este parámetro es opcional. El carácter `D` se usa de forma predeterminada. El parámetro *environment_scope* es **char(1)** .  
  
 [ @reference_id = ] *reference_id*  
 El identificador único de la referencia de entorno. Este parámetro es obligatorio solo cuando se incluye una sola referencia de entorno en la validación, cuando el parámetro *environment_scope* es `S`. *reference_id* es **bigint**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos de lectura en el objeto y, si es aplicable, permisos de lectura en los entornos a los que se hace referencia  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El nombre del proyecto o el nombre del paquete no son válidos  
  
-   El usuario no tiene los permisos adecuados.  
  
-   Uno o varios de los entornos a los que se hace referencia incluidos en la validación no contienen variables a las que se haga referencia  
  
-   Se produce un error de validación del paquete  
  
-   El entorno al que se hace referencia no existe  
  
-   Las variables a las que se hace referencia no se pueden encontrar en los entornos a los que se hace referencia incluidos en la validación  
  
-   Se hace referencia a las variables de los parámetros del paquete, pero no se han incluido entornos a los que se haga referencia en la validación  
  
## <a name="remarks"></a>Observaciones  
 La validación ayuda a identificar problemas que pueden impedir que el paquete se ejecute correctamente. Use las vistas [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md) o [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) para supervisar el estado de validación.  
  
  
