---
description: catalog.remove_data_tap
title: catalog.remove_data_tap | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: b77db3e6-478c-441a-a838-82c4de750275
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f350aa9ee715212dcfc5e6163f758110cde58023
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342187"
---
# <a name="catalogremove_data_tap"></a>catalog.remove_data_tap 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita una derivación de datos de la salida de un componente que está en una ejecución. El identificador único de la derivación de datos está asociado a una instancia de la ejecución.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.remove_data_tap [ @data_tap_id = ] data_tap_id  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @data_tap_id = ] *data_tap_id*  
 Identificador único de la derivación de datos que se crea usando el procedimiento almacenado catalog.add_data_tap. *data_tap_id* es **bigint**.  
  
## <a name="remarks"></a>Observaciones  

- Si un paquete contiene varias tareas Flujo de datos con el mismo nombre, la derivación de datos se agrega a la primera tarea Flujo de datos con el nombre especificado.  
  
- Para quitar derivaciones de datos, la instancia de la ejecución debe estar en el estado creado (un valor 1 en la columna **status** de la vista [catalog.operations &#40;Base de datos SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)).  
  
## <a name="return-codes"></a>Códigos de retorno  
 0 (correcto)  
  
 Cuando se produce un error en el procedimiento almacenado, se genera un error.  
  
## <a name="result-set"></a>Tipo de cursor  
 None  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Permisos MODIFY en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la lista siguiente se describen las condiciones que hacen que el procedimiento almacenado genere un error.  
  
-   El usuario no tiene permisos MODIFY.  
  
## <a name="see-also"></a>Consulte también  
 [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md)   
 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)  
  
  
