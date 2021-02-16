---
description: catalog.set_object_parameter_value (base de datos de SSISDB)
title: catalog.set_object_parameter_value (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: fb887543-f92f-404d-9495-a1dd23a6716e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bdc1a5ff8944a29d7e2a2fcdeec297973c9849f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355304"
---
# <a name="catalogset_object_parameter_value-ssisdb-database"></a>catalog.set_object_parameter_value (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece el valor de un parámetro del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Asocia el valor a una variable de entorno o asigna un valor literal que se va a usar como valor predeterminado si no se asignan otros valores.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.set_object_parameter_value [ @object_type = ] object_type   
    , [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @parameter_name = ] parameter_name   
    , [ @parameter_value = ] parameter_value   
 [  , [ @object_name = ] object_name ]  
 [  , [ @value_type = ] value_type ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [@object_type =] *object_type*  
 Tipo de parámetro. Use el valor `20` para indicar un parámetro de proyecto o el valor `30` para indicar un parámetro de paquete. *object_type* es **smallInt**.  
  
 [@folder_name =] *folder_name*  
 Nombre de la carpeta que contiene el parámetro. *folder_name* es **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Nombre del proyecto que contiene el parámetro. *project_name* es **nvarchar(128)** .  
  
 [@parameter_name =] *parameter_name*  
 El nombre del parámetro. El parámetro *parameter_name* es de tipo **nvarchar(128)**.  
  
 [@parameter_value =] *parameter_value*  
 Valor del parámetro. El parámetro *parameter_value* es de tipo **sql_variant**.  
  
 [@object_name =] *object_name*  
 Nombre del paquete. Este argumento es necesario cuando el parámetro es un parámetro de paquete. El parámetro *object_name* es **nvarchar(260)**.  
  
 [@value_type =] *value_type*  
 Tipo de valor del parámetro. Use el carácter `V` para indicar que *parameter_value* es un valor literal que se usa de forma predeterminada cuando no hay otros valores asignados antes de la ejecución. Use el carácter `R` para indicar que *parameter_value* es un valor al que se hace referencia y que se ha establecido en el nombre de una variable de entorno. Este argumento es opcional; de forma predeterminada, se usa el carácter `V`. El parámetro *value_type* es **char(1)**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos READ y MODIFY en el proyecto  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la lista siguiente se describen algunas condiciones que pueden hacer que el procedimiento almacenado produzca un error:  
  
-   El tipo de parámetro no es válido  
  
-   El nombre de proyecto no es válido  
  
-   En los parámetros de paquete, el nombre del paquete no es válido  
  
-   El tipo de valor no es válido.  
  
-   El usuario no tiene los permisos adecuados.  
  
## <a name="remarks"></a>Observaciones  
  
-   Si no se especifica ningún parámetro *value_type*, se usa un valor literal de *parameter_value* de forma predeterminada. Cuando se usa un valor literal, el parámetro *value_set* de la vista [object_parameters](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md) se establece en `1`. No se permiten valores de parámetro NULL.  
  
-   Si *value_type* contiene el carácter `R` (el cual denota un valor de referencia), el parámetro *parameter_value* hace referencia al nombre de una variable de entorno.  
  
-   El valor `20` se puede usar para que *object_type* denote un parámetro de proyecto. En este caso, el valor de *object_name* no es necesario y cualquier valor especificado para *object_name* se omite. Este valor se usa cuando el usuario desea establecer un parámetro de proyecto.  
  
-   El valor `30` se puede usar para que *object_type* denote un parámetro de paquete. En este caso, se usa un valor de *object_name* para denotar el paquete correspondiente. Si no se especifica *object_name*, el procedimiento almacenado devuelve un error y finaliza.  
  
  
