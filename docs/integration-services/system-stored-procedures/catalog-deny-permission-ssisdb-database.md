---
description: catalog.deny_permission (base de datos de SSISDB)
title: catalog.deny_permission (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: de310bac-2ddc-4ef9-8783-43dcb02a94f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d32af051552c211592bcc4deb2bcd2c1c1c20079
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342267"
---
# <a name="catalogdeny_permission-ssisdb-database"></a>catalog.deny_permission (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Deniega un permiso en un objeto protegible del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql
catalog.deny_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @object_type = ] *object_type*  
 Tipo del objeto protegible. Entre los tipos de objetos protegibles se incluyen la carpeta (`1`), el proyecto (`2`), el entorno (`3`) y la operación (`4`). El parámetro *object_type* es **smallint** _._  
  
 [ @object_id = ] *object_id*  
 Identificador único (Id.) o clave principal del objeto protegible. El parámetro *object_id* es **bigint**.  
  
 [ @principal_id = ] *principal_id*  
 El identificador de la entidad de seguridad que será denegado. El parámetro *principal_id* es **int**.  
  
 [ @permission_type = ] *permission_type*  
 El tipo de permiso que será denegado. El parámetro *permission_type* es **smallint**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto)  
  
 1 (object_class no es válido)  
  
 2 (object_id no existe)  
  
 3 (la entidad de seguridad no existe)  
  
 4 (el permiso no es válido)  
  
 5 (otro error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permiso MANAGE_PERMISSIONS en el objeto  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento almacenado le permite denegar los tipos de permiso descritos en la tabla siguiente:  
  
|Valor de permission_type|Nombre del permiso|Descripción del permiso|Tipos de objetos aplicables|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|Permite a la entidad de seguridad leer información que se considera parte del objeto, como las propiedades. No permite a la entidad de seguridad enumerar o leer el contenido de otros objetos contenidos dentro del objeto.|Carpeta, proyecto, entorno, operación|  
|`2`|MODIFY|Permite a la entidad de seguridad modificar información que se considera parte del objeto, como las propiedades. No permite a la entidad de seguridad modificar otros objetos contenidos dentro del objeto.|Carpeta, proyecto, entorno, operación|  
|`3`|Ejecute|Permite a la entidad de seguridad ejecutar todos los paquetes del proyecto.|Project|  
|`4`|MANAGE_PERMISSIONS|Permite a la entidad de seguridad asignar permisos a los objetos.|Carpeta, proyecto, entorno, operación|  
|`100`|CREATE_OBJECTS|Permite a la entidad de seguridad crear objetos en la carpeta.|Carpeta|  
|`101`|READ_OBJECTS|Permite a la entidad de seguridad leer todos los objetos de la carpeta.|Carpeta|  
|`102`|MODIFY_OBJECTS|Permite a la entidad de seguridad modificar todos los objetos de la carpeta.|Carpeta|  
|`103`|EXECUTE_OBJECTS|Permite a la entidad de seguridad ejecutar todos los paquetes de todos los proyectos de la carpeta.|Carpeta|  
|`104`|MANAGE_OBJECT_PERMISSIONS|Permite a la entidad de seguridad administrar los permisos de todos los objetos de la carpeta.|Carpeta|  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   Si se especifica "permission_type", el procedimiento deniega el permiso especificado que está asignado explícitamente a la entidad de seguridad indicada para el objeto especificado. Aun cuando no haya ninguna de esas instancias, el procedimiento devuelve todavía un valor de código correcto (`0`).  
  
-   Si se omite "permission_type", el procedimiento deniega al objeto especificado todos los permisos de la entidad de seguridad indicada.  
  
  
