---
description: catalog.operations (base de datos de SSISDB)
title: catalog.operations (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- operations view [Integration Services]
- catalog.operations view [Integration Services]
ms.assetid: 9455c5b1-60ff-45fc-8599-cc3abbd6daf5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 05f34ebf6652983dd1c8ea007e9ba68d47ef1d00
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835273"
---
# <a name="catalogoperations-ssisdb-database"></a>catalog.operations (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra los detalles de todas las operaciones del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|operation_id|**bigint**|Identificador (id.) único de la operación.|  
|operation_type|**smallint**|Tipo de operación.|  
|created_time|**datetimeoffset**|Hora de creación de la operación.|  
|object_type|**smallint**|Tipo de objeto afectado por la operación. El objeto puede ser una carpeta (`10`), proyecto (`20`), paquete (`30`), entorno (`40`) o instancia de ejecución (`50`).|  
|object_id|**bigint**|Identificador del objeto afectado por la operación.|  
|object_name|**nvarchar(260)**|El nombre del objeto.|  
|status|**int**|Estado de la operación. Los valores posibles son creado (`1`), en ejecución (`2`), cancelado (`3`), con errores (`4`), pendiente (`5`), finalizado inesperadamente (`6`), correcto (`7`), deteniendo (`8`) y completado (`9`).|  
|start_time|**datetimeoffset**|Hora a la que se inició la operación.|  
|end_time|**datetimeoffsset**|Hora a la que finalizó la operación.|  
|caller_sid|**varbinary(85)**|Identificador de seguridad (SID) del usuario si se utilizó Autenticación de Windows para iniciar sesión.|  
|caller_name|**nvarchar(128)**|Nombre de la cuenta que realizó la operación.|  
|process_id|**int**|Identificador de proceso del proceso externo, si es aplicable.|  
|stopped_by_sid|**varbinary(85)**|Identificador de seguridad (SID) del usuario que detuvo la operación.|  
|stopped_by_name|**nvarchar(128)**|Nombre del usuario que detuvo la operación.|  
|server_name|**nvarchar(128)**|Información del servidor Windows y de la instancia asociada a una instancia especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|machine_name|**nvarchar(128)**|Nombre del equipo en el que se está ejecutando la instancia del servidor.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra una fila por cada operación del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Permite al administrador enumerar todas las operaciones lógicas que se realizaron en el servidor, por ejemplo la implementación de un proyecto o la ejecución de un paquete.  
  
 Esta vista muestra los siguientes tipos de operación, como se muestra en la columna **operation_type**:  
  
|Valor de **operation_type**|Descripción de **operation_type**|Descripción de **object_id**|Descripción de **object_name**|  
|-------------------------------|-------------------------------------|--------------------------------|----------------------------------|  
|`1`|Inicialización de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|**NULL**|**NULL**|  
|`2`|Período de retención<br /><br /> (Trabajo del SQL Agent)|**NULL**|**NULL**|  
|`3`|MaxProjectVersion<br /><br /> (Trabajo del SQL Agent)|**NULL**|**NULL**|  
|`101`|**deploy_project**<br /><br /> (Procedimiento almacenado)|Identificador de proyecto|Nombre de proyecto|  
|`106`|**restore_project**<br /><br /> (Procedimiento almacenado)|Identificador de proyecto|Nombre de proyecto|  
|`200`|**create_execution** y **start_execution**<br /><br /> (Procedimientos almacenados)|Identificador de proyecto|**NULL**|  
|`202`|**stop_operation**<br /><br /> (Procedimiento almacenado)|Identificador de proyecto|**NULL**|  
|`300`|**validate_project**<br /><br /> (Procedimiento almacenado)|Identificador de proyecto|Nombre de proyecto|  
|`301`|**validate_package**<br /><br /> (Procedimiento almacenado)|Identificador de proyecto|Nombre del paquete|  
|`1000`|**configure_catalog**<br /><br /> (Procedimiento almacenado)|**NULL**|**NULL**||  
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Permiso de lectura en la operación  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
> [!NOTE]  
>  Cuando se dispone de permiso para realizar una operación en el servidor, también se dispone de permiso para ver información sobre la operación. Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
  
