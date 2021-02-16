---
description: catalog.create_environment_reference (base de datos de SSISDB)
title: catalog.create_environment_reference (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 48069bea-31cb-4a0e-9849-a07edc94088f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c7c513bf3fcd720418be0eb79a99f75194d0fb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346828"
---
# <a name="catalogcreate_environment_reference-ssisdb-database"></a>catalog.create_environment_reference (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una referencia de entorno para un proyecto en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.create_environment_reference [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @environment_name = ] environment_name  
     , [ @reference_type = ] reference_type  
  [  , [ @environment_folder_name = ] environment_folder_name ]  
  [  , [ @reference_id = ] reference_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @folder_name = ] *folder_name*  
 El nombre de la carpeta del proyecto al que se hace referencia en el entorno. *folder_name* es **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 El nombre del proyecto al que se hace referencia en el entorno. *project_name* es **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 El nombre del entorno al que se hace referencia. *environment_name* es **nvarchar(128)** .  
  
 [ @reference_type = ] *reference_type*  
 Indica si el entorno se puede encontrar en la misma carpeta que el proyecto (referencia relativa) o en una carpeta diferente (referencia absoluta). Utilice el valor `R` para indicar una referencia relativa. Utilice el valor `A` para indicar una referencia absoluta. El parámetro *reference_type* es **char(1)** .  
  
 [ @environment_folder_name = ] *environment_folder_name*  
 El nombre de la carpeta en la que se encuentra el entorno al que se hace referencia. Este valor se requiere para las referencias absolutas. El parámetro *environment_folder_name* es **nvarchar(128)** .  
  
 [ @reference_id = ] *reference_id*  
 Devuelve el identificador único para la nueva referencia. Este parámetro es opcional. *reference_id* es **bigint**.  
  
## <a name="return-code-value"></a>Valor de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Los permisos de lectura y modificación en el proyecto, y el permiso de lectura en el entorno  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   El nombre de la carpeta no es válido  
  
-   El nombre de proyecto no es válido  
  
-   El usuario no tiene los permisos apropiados  
  
-   Una referencia absoluta se especifica mediante el carácter `A` en el parámetro *reference_location*, pero el nombre de la carpeta no se especificó con el parámetro *environment_folder_name*.  
  
## <a name="remarks"></a>Observaciones  
 Un proyecto puede tener referencias de entorno absolutas o relativas. Las referencias relativas se refieren al entorno por nombre y requieren que resida en la misma carpeta que el proyecto. Las referencias absolutas hacen referencia al entorno por nombre y carpeta, y pueden hacer referencia a los entornos que residen en una carpeta diferente que el proyecto. Un proyecto puede hacer referencia a varios entornos.  
  
  
