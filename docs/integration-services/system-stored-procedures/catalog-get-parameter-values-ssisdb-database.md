---
description: catalog.get_parameter_values (base de datos de SSISDB)
title: catalog.get_parameter_values (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5b1aeaf7-c938-4aef-bafc-e4d7a82eb578
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 176fde267c490d41a41003e58cf1e201862a110b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342245"
---
# <a name="catalogget_parameter_values-ssisdb-database"></a>catalog.get_parameter_values (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Resuelve y recupera los valores de los parámetros predeterminados de un proyecto y los paquetes correspondientes en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.get_parameter_values [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @package_name = ] package_name  
  [  , [ @reference_id = ] reference_id  ]  
  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @folder_name = ] *folder_name*  
 Nombre de la carpeta que contiene el proyecto. *folder_name* es **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Nombre del proyecto donde los parámetros residen. *project_name* es **nvarchar(128)** .  
  
 [ @package_name = ] *package_name*  
 Nombre del paquete. Especifique el nombre del paquete para recuperar todos los parámetros de proyecto y los parámetros de un paquete concreto. El parámetro *package_name* es de tipo **nvarchar(260)**.  
  
 [ @reference_id = ] *reference_id*  
 Identificador único de una referencia de entorno. Este parámetro es opcional. *reference_id* es **bigint**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve una tabla que tiene el siguiente formato:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_type|**smallint**|Tipo de parámetro. El valor es `20` para un parámetro de proyecto y `30` para un parámetro de paquete.|  
|parameter_data_type|**nvarchar(128)**|El tipo de datos del parámetro.|  
|parameter_name|**sysname**|El nombre del parámetro.|  
|parameter_value|**sql_variant**|Valor del parámetro.|  
|sensitive|**bit**|Cuando el valor es `1`, el valor del parámetro es confidencial. Cuando el valor es `0`, el valor del parámetro no es confidencial.|  
|requerido|**bit**|Cuando el valor es `1`, se requiere el valor del parámetro para iniciar la ejecución. Cuando el valor es `0`, no se requiere el valor del parámetro para iniciar la ejecución.|  
|value_set|**bit**|Cuando el valor es `1`, el valor del parámetro se ha asignado. Cuando el valor es `0`, el valor del parámetro no se ha asignado.|  
  
> [!NOTE]  
>  Los valores literales se muestran en texto simple. **NULL** se muestra en lugar de los valores confidenciales.  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos de lectura en el objeto y, si es aplicable, el permiso de lectura en el entorno al que se hace referencia  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El paquete no se puede encontrar en la carpeta o proyecto especificados  
  
-   El usuario no tiene los permisos adecuados.  
  
-   La referencia del entorno especificado no existe  
  
  
