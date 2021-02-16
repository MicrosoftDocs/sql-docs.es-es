---
description: catalog.rename_environment (base de datos de SSISDB)
title: catalog.rename_environment (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: c73d7452-31c5-4f4e-afcc-e9eca760c826
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5cff0cdf01fbfbb0af768a18ab54614c2a46f5bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346517"
---
# <a name="catalogrename_environment-ssisdb-database"></a>catalog.rename_environment (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia el nombre de un entorno en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.rename_environment [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @new_environment_name= ] new_environment_name  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @folder_name = ] *folder_name*  
 Nombre de la carpeta que contiene el entorno. *folder_name* es **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Nombre original del entorno. *environment_name* es **nvarchar(128)** .  
  
 [ @new_environment_name = ] *new_environment_name*  
 Nuevo nombre del entorno. *new_environment_name* es **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos MODIFY en el entorno  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El nombre del entorno original no es válido  
  
-   El nuevo nombre ya se ha utilizado en un entorno existente  
  
## <a name="remarks"></a>Observaciones  
 Las referencias de entorno de los proyectos no se actualizan automáticamente al cambiar el nombre del entorno. Las referencias del entorno deben actualizadas como corresponda. Este procedimiento almacenado tendrá éxito aunque las referencias del entorno se interrumpan cambiando el nombre del entorno. Las referencias del entorno se deben actualizar después de que se completa este procedimiento almacenado.  
  
> [!NOTE]  
>  Cuando una referencia de entorno no es válida, la validación y ejecución de los paquetes correspondientes que utilizan esas referencias no se ejecutarán correctamente.  
  
  
