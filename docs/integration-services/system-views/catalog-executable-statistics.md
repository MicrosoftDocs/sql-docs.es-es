---
description: catalog.executable_statistics
title: catalog.executable_statistics | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 3dda28d6-10d8-4294-9b5e-a6048c07faf9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3d29248d96eef9c133d997efd67a4518478a0e1c
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835398"
---
# <a name="catalogexecutable_statistics"></a>catalog.executable_statistics 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra una fila para cada aplicación ejecutable que se ejecuta, incluida cada iteración de un ejecutable.  
  
 Un ejecutable es una tarea o un contenedor que se agrega al flujo de control de un paquete.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|Statistics_id|bigint|Identificador único de los datos.|  
|Execution_id|bigint|Identificador único de la instancia de la ejecución.<br /><br /> La vista catalog.executions proporciona información adicional sobre las ejecuciones. Para obtener más información, consulte [catalog.executions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md).|  
|Executable_id|bigint|Identificador único del componente del paquete.<br /><br /> La vista catalog.executables proporciona información adicional sobre los ejecutables. Para obtener más información, consulte [catalog.executables](../../integration-services/system-views/catalog-executables.md).|  
|Execution_path|nvarchar(max)|La ruta de acceso completa de la ejecución del componente del paquete, incluida cada iteración del componente.|  
|Start_time|datetimeoffset(7)|Hora en que el ejecutable entra en la fase previa a la ejecución.|  
|End_time|datetimeoffset(7)|Hora en que el ejecutable entra en la fase posterior a la ejecución.|  
|Execution_duration|int|Tiempo que el ejecutable empleó en la ejecución. El valor se expresa en milisegundos.|  
|Execution_result|SMALLINT|Los posibles valores son los siguientes:<br /><br /> 0 (Correcto)<br /><br /> 1 (Error)<br /><br /> 2 (Finalización)<br /><br /> 3 (Cancelado)|  
|Execution_value|sql_variant|Valor devuelto por la ejecución. Es un valor definido por el usuario.|  
  
## <a name="permissions"></a>Permisos  
 La vista requiere uno de los siguientes permisos:  
  
-   Permiso READ en la instancia de la ejecución.  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol del servidor de **sysadmin**  
  
  
