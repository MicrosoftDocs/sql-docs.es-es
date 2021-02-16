---
description: catalog.revoke_permission (base de datos de SSISDB)
title: catalog.revoke_permission (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- revoke_permission stored procedure [Integration Services]
- catalog.revoke_permission stored procedure [Integration Services]
ms.assetid: 850b9c26-5c7c-47b9-a61c-5cf9bb5948cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0f2a25972d62f519f7a2c465df78b4101640944c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355334"
---
# <a name="catalogrevoke_permission-ssisdb-database"></a>catalog.revoke_permission (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Revoca un permiso de un objeto protegible en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```sql
catalog.revoke_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @object_type = ] *object_type*  
 Tipo del objeto protegible. Entre los tipos de objetos protegibles se incluyen la carpeta (`1`), el proyecto (`2`), el entorno (`3`) y la operación (`4`). El parámetro *object_type* es **smallint** _._  
  
 [ @object_id = ] *object_id*  
 Es el identificador único (id.) del objeto protegible. El parámetro *object_id* es **bigint**.  
  
 [ @principal_id = ] *principal_id*  
 Identificador de la entidad de seguridad cuyo permiso se va a revocar. El parámetro *principal_id* es **int**.  
  
 [ @permission_type = ] *permission_type*  
 Tipo de permiso. El parámetro *permission_type* es **smallint**.  
  
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
  
-   Permisos ASSIGN_PERMISSIONS en el objeto  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="remarks"></a>Observaciones  
 Si se especifica permission_type, el procedimiento almacenado quita el permiso asignado explícitamente a la entidad de seguridad del objeto. Aunque cuando no haya ninguna de esas instancias, el procedimiento devuelve un valor de código correcto (`0`). Si se omite permission_type, el procedimiento almacenado quita todos los permisos de la entidad de seguridad del objeto.  
  
> [!NOTE]  
>  La entidad de seguridad todavía puede tener el permiso especificado en el objeto si es miembro de un rol que tiene el permiso especificado.  
  
 Este procedimiento almacenado le permite revocar los tipos de permiso descritos en la tabla siguiente:  
  
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
  
  
