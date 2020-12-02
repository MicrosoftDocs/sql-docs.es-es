---
description: catalog.create_environment (base de datos de SSISDB)
title: catalog.create_environment (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 66367092-9f6e-40e6-90bd-81efb078ab70
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3657b4d171dd3cef40fbde37c8a13dceaebd0b20
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129895"
---
# <a name="catalogcreate_environment-ssisdb-database"></a>catalog.create_environment (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un entorno en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.create_environment [ @folder_name = ] folder_name  
     , [ @environment_name = ] environment_name  
  [  , [ @environment_description = ] environment_description ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [@folder_name =] *folder_name*  
 Nombre de la carpeta que contendrá el entorno. *folder_name* es **nvarchar(128)** .  
  
 [@environment_name =] *environment_name*  
 El nombre del entorno. *environment_name* es **nvarchar(128)** .  
  
 [@environment_description=] *environment_description*  
 Una descripción opcional del entorno. *environment_description* es **nvarchar(1024)**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos READ y MODIFY en la carpeta  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   rol de base de datos  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El nombre de la carpeta no se encuentra  
  
-   Un entorno con el mismo nombre ya existe en la carpeta especificada  
  
## <a name="remarks"></a>Observaciones  
 El nombre del entorno debe ser único en la carpeta.  
  
  
