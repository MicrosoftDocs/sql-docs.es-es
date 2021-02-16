---
description: catalog.clear_object_parameter_value (base de datos de SSISDB)
title: catalog.clear_object_parameter_value (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: dcbbb714-a051-4805-9e2b-2c2fb647c890
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9b8b3397514fc0dd2e7a4d8b5dfa704ab14a5989
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346879"
---
# <a name="catalogclear_object_parameter_value-ssisdb-database"></a>catalog.clear_object_parameter_value (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Borra el valor de un parámetro para un paquete o proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] existente almacenado en el servidor.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.clear_object_parameter [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @object_type = ] object_type   
    , [ @object_name = ] object_name   
    , [ @parameter_ name = ] parameter_name  
```  
  
## <a name="arguments"></a>Argumentos  
 [ \@folder_name = ] *folder_name*  
 Nombre de la carpeta que contiene el proyecto. *folder_name* es **nvarchar(128)** .  
  
 [ \@project_name = ] *project_name*  
 Nombre del proyecto. *project_name* es **nvarchar(128)** .  
  
 [ \@object_type = ] *object_type*  
 Tipo del objeto. Los valores válidos incluyen `20` para un proyecto y `30` para un paquete. *object_type* es **smallInt**.  
  
 [ \@ object _name = ] *object _name*  
 Nombre del paquete. *object _name* es **nvarchar(260)**.  
  
 [ \@parameter_ name = ] *parameter_name*  
 El nombre del parámetro. *parameter_ name* es **nvarchar(128)**.  
  
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
 En la lista siguiente se describen algunas condiciones que pueden hacer que el procedimiento almacenado clear_object_parameter produzca un error:  
  
-   Se ha especificado un tipo de objeto no válido o no se ha podido encontrar el nombre de objeto en el proyecto.  
  
-   El proyecto no existe o no es accesible, o bien, el nombre del proyecto no es válido.  
  
-   Se ha especificado un nombre de parámetro no válido.  
  
-   El usuario no tiene los permisos adecuados.  
  
  
