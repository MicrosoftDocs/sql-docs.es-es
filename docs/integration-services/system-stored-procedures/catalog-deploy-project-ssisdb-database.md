---
description: catalog.deploy_project (base de datos de SSISDB)
title: catalog.deploy_project (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2e3439b4-7226-4b61-a993-7a1d161eac7e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ef918ad60dd685cf90fc043eba72dcc6dd7987ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342257"
---
# <a name="catalogdeploy_project-ssisdb-database"></a>catalog.deploy_project (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Implementa un proyecto en una carpeta en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] o actualiza un proyecto existente que se ha implementado previamente.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.deploy_project [@folder_name =] folder_name   
      , [ @project_name = ] project_name   
      , [ @project_stream = ] projectstream   
    [ , [ @operation_id = ] operation_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Argumentos  
 [@folder_name =] *folder_name*  
 Es el nombre de la carpeta donde se implementará el proyecto. *folder_name* es **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Nombre del proyecto nuevo o actualizado en la carpeta. *project_name* es **nvarchar(128)** .  
  
 [@projectstream =] *projectstream*  
 Contenido binario de un archivo de implementación de proyecto (extensión .ispac) de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Puede usar una instrucción SELECT con la función OPENROWSET y el proveedor de conjuntos de filas BULK para recuperar el contenido binario del archivo. Para ver un ejemplo, consulte [Deploy Integration Services (SSIS) Projects and Packages (Implementación de proyectos y paquetes de Integration Services [SSIS])](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md). Para obtener más información sobre la cláusula OPENROWSET, consulte [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
 El parámetro *projectstream* es **varbinary(MAX)**  
  
 [@operation_id =] *operation_id*  
 Devuelve el identificador único para la operación de implementación. *operation_id* es **bigint**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos CREATE_OBJECTS en la carpeta para implementar un nuevo proyecto o permisos MODIFY en el proyecto para actualizar un proyecto  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 La siguiente lista describe algunas condiciones que pueden hacer que este procedimiento almacenado produzca un error:  
  
-   Un parámetro hace referencia a un objeto que no existe, un parámetro intenta crear un objeto que ya existe o un parámetro no es válido por algún otro motivo  
  
-   El valor del parámetro *\@project_name* no coincide con el nombre del proyecto en el archivo de implementación.  
  
-   El usuario no tiene permisos suficientes  
  
## <a name="remarks"></a>Observaciones  
 Durante la implementación o la actualización de un proyecto, el procedimiento almacenado no comprueba el nivel de protección de paquetes individuales en el proyecto.  
  
  
