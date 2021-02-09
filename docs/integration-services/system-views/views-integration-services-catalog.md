---
description: Vistas (catálogo de Integration Services)
title: Vistas (catálogo de Integration Services) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- views [Integration Services]
ms.assetid: d0294d43-4852-46dc-9afa-d0c19ea9aa03
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b4e067dc38f6bd511237ab05491feda4307106c4
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835996"
---
# <a name="views-integration-services-catalog"></a>Vistas (catálogo de Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  En esta sección se describen las vistas [!INCLUDE[tsql](../../includes/tsql-md.md)] que están disponibles para administrar proyectos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] implementados en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Consulte las vistas de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para inspeccionar los objetos, valores y datos operativos almacenados en el catálogo de **SSISDB**.  
  
 El nombre predeterminado del catálogo es SSISDB. Entre los objetos que se almacenan en el catálogo se incluyen proyectos, paquetes, parámetros, entornos y el historial de operaciones.  
  
 Puede utilizar las vistas de base de datos y los procedimientos almacenados directamente, o escribir código personalizado que llame a la API administrada. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y la API administrada consultan las vistas y llaman a los procedimientos almacenados que se describen en esta sección para realizar muchas de sus tareas.  
  
## <a name="in-this-section"></a>En esta sección  
 [catalog.catalog_properties &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)  
 Muestra las propiedades del catálogo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.effective_object_permissions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md)  
 Muestra los permisos efectivos de la entidad de seguridad actual para todos los objetos del catálogo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environment_variables &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
 Muestra los detalles de las variables de entorno para todos los entornos del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environments &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
 Muestra los detalles de todos los entornos del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Los entornos contienen variables a las que pueden hacer referencia los proyectos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.execution_parameter_values &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
 Muestra los valores de parámetro reales utilizados por los paquetes [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] durante una instancia de ejecución.  
  
 [catalog.executions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
 Muestra las instancias de ejecución del paquete en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Los paquetes que se ejecutan con la tarea Ejecutar paquete se ejecutan en la misma instancia de ejecución que el paquete primario.  
  
 [catalog.explicit_object_permissions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md)  
 Muestra solo los permisos asignados explícitamente al usuario.  
  
 [catalog.extended_operation_info &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
 Muestra la información extendida sobre todas las operaciones del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.folders &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-folders-ssisdb-database.md)  
 Muestra las carpetas del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.object_parameters &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
 Muestra los parámetros para todos los paquetes y proyectos en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.object_versions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
 Muestra las versiones de objetos en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. En esta versión, solo las versiones de proyectos se admiten en esta vista.  
  
 [catalog.operation_messages &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
 Muestra los mensajes registrados durante las operaciones del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.operations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
 Muestra los detalles de todas las operaciones del catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.packages &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
 Muestra los detalles para todos los paquetes que aparecen en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.environment_references &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
 Muestra las referencias del entorno para todos los proyectos en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.projects &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
 Muestra los detalles para todos los proyectos que aparecen en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 [catalog.validations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md)  
 Muestra los detalles de todas las validaciones de paquete y proyecto en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
[catalog.master_properties &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)  
Muestra las propiedades del patrón de escalabilidad horizontal de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].

[catalog.worker_agents &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md)  
Muestra la información del trabajo de escalabilidad horizontal de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
