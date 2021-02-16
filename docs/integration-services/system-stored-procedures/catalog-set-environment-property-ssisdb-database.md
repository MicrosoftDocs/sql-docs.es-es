---
description: catalog.set_environment_property (base de datos de SSISDB)
title: catalog.set_environment_property (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a345675b-d32e-4624-96cf-ec656730b114
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 892823abe1f7607a034f9cc22b34dd7a81c12ffc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273226"
---
# <a name="catalogset_environment_property-ssisdb-database"></a>catalog.set_environment_property (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece la propiedad de un entorno del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.set_environment_property [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @property_name = ] property_name  
    , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @folder_name = ] *folder_name*  
 Nombre de la carpeta que contiene el entorno. *folder_name* es **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 El nombre del entorno. *environment_name* es **nvarchar(128)** .  
  
 [ @property_name = ] *property_name*  
 Nombre de una propiedad del entorno. *property_name* es **nvarchar(128)**.  
  
 [ @property_value = ] *property_value*  
 Valor de la propiedad del entorno. *property_value* es **nvarchar(1024)**.  
  
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
  
-   El nombre de la carpeta no es válido  
  
-   El nombre de la propiedad no es válido  
  
-   El nombre del entorno no es válido  
  
## <a name="remarks"></a>Observaciones  
 En esta versión, solo se puede establecer la propiedad `Description`. El valor de la propiedad `Description` no puede superar los 4.000 caracteres.  
  
  
