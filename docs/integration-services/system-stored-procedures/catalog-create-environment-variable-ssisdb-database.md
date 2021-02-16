---
description: catalog.create_environment_variable (base de datos de SSISDB)
title: catalog.create_environment_variable (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91ed017b-6567-4bf2-b9f1-e2b5c70a5343
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7a3b2b55c5a0efbc2acf8152b89a292adf4cc298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346811"
---
# <a name="catalogcreate_environment_variable-ssisdb-database"></a>catalog.create_environment_variable (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cree una variable de entorno en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.create_environment_variable [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @data_type = ] data_type  
    , [ @sensitive = ] sensitive  
    , [ @value = ] value  
    , [ @description = ] description  
```  
  
## <a name="arguments"></a>Argumentos  
 [@folder_name =] *folder_name*  
 Nombre de la carpeta que contiene el entorno. *folder_name* es **nvarchar(128)** .  
  
 [@environment_name =] *environment_name*  
 El nombre del entorno. *environment_name* es **nvarchar(128)** .  
  
 [@variable_name =] *variable_name*  
 Nombre de la variable de entorno. El parámetro *variable_name* es de tipo **nvarchar(128)**.  
  
 [@data_type =] *data_type*  
 Tipo de datos de la variable. Los tipos de datos de las variables de entorno admitidos incluyen **Boolean**, **Byte**, **DateTime**, **Double**, **Int16**, **Int32**, **Int64**, **Single**, **String**, **UInt32** y **UInt64**. Los tipos de datos de las variables de entorno no admitidos incluyen **Char**, **DBNull**, **Object** y **Sbyte**. El tipo de datos del parámetro *data_type* es **nvarchar(128)**.  
  
 [@sensitive =] *sensitive*  
 Indica si la variable contiene un valor confidencial o no. Use un valor de `1` para indicar que el valor de la variable de entorno es confidencial o un valor de `0` para indicar que no lo es. Un valor confidencial se cifra cuando se almacena. Un valor que no es confidencial se almacena en texto no cifrado.*Sensitive* es de tipo **bit**.  
  
 [@value =] *value*  
 Valor de la variable de entorno. El parámetro *value* es de tipo **sql_variant**.  
  
 [@description =] *description*  
 Descripción de la variable de entorno. El parámetro *value* es de tipo **nvarchar(1024)**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos READ y MODIFY en el entorno  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El nombre de la carpeta, del entorno o de la variable de entorno no es válido  
  
-   El nombre de la variable ya existe en el entorno  
  
-   El usuario no tiene los permisos adecuados.  
  
## <a name="remarks"></a>Observaciones  
 Se puede usar una variable de entorno para asignar eficazmente un valor a un parámetro de proyecto o parámetro de paquete para su uso en la ejecución de un paquete. Las variables de entorno permiten organizar los valores de parámetro. Los nombres de variable deben ser únicos dentro de un entorno.  
  
 El procedimiento almacenado valida el tipo de datos de la variable para garantizar que es compatible con el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
> [!TIP]  
>  Considere la posibilidad de usar el tipo de datos **Int16** en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en lugar del tipo de datos **Sbyte**, que no es compatible.  
  
 El valor que se pasa a este procedimiento almacenado con el parámetro *value* se convierte de un tipo de datos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] a un tipo de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como se indica en la siguiente tabla:  
  
|Tipo de datos de Integration Services|Tipo de datos de SQL Server|  
|------------------------------------|--------------------------|  
|**Boolean**|**bit**|  
|**Byte**|**binary**, **varbinary**|  
|**DateTime**|**datetime**, **datetime2**, **datetimeoffset**, **smalldatetime**|  
|**Double**|Valor numérico exacto: **decimal**, **numeric**; valor numérico aproximado: **float**, **real**|  
|**Int16**|**smallint**|  
|**Int32**|**int**|  
|**Int64**|**bigint**|  
|**Single**|Valor numérico exacto: **decimal**, **numeric**; valor numérico aproximado: **float**, **real**|  
|**String**|**varchar**, **nvarchar**, **char**|  
|**UInt32**|**int** (**int** es la asignación disponible más próxima a **Uint32**).|  
|**UInt64**|**bigint** (**int** es la asignación disponible más próxima a **Uint64**).|  
  
  
