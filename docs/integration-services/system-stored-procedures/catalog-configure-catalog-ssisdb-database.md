---
description: catalog.configure_catalog (base de datos de SSISDB)
title: catalog.configure_catalog (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 72690c61-f462-4c25-9fce-08a687b0bd41
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4506270730f8681315501e76ad1a338ff65b18c0
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129912"
---
# <a name="catalogconfigure_catalog-ssisdb-database"></a>catalog.configure_catalog (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configura el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] estableciendo una propiedad de catálogo en un valor especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql
catalog.configure_catalog [ @property_name = ] property_name , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @property_name = ] *property_name*  
 Nombre de la propiedad de catálogo. *property_name* es **nvarchar(255)**. Para obtener más información sobre las propiedades disponibles, vea [catalog.catalog_properties & #40; Base de datos SSISDB & #41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
 [ @property_value = ] *property_value*  
 Valor de la propiedad de catálogo. *property_value* es **nvarchar(255)**. Para obtener más información sobre los valores de las propiedades, vea [catalog.catalog_properties & #40; Base de datos SSISDB & #41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento almacenado determina si *property_value* es válido para cada *property_name*.  
  
 Este procedimiento almacenado solo se puede realizar cuando no hay ninguna ejecución activa, como ejecuciones pendientes, en cola, en ejecución y en pausa.  
  
 Mientras se está configurando el catálogo, todos los demás procedimientos almacenados de catálogo producirán un error y mostrarán el mensaje de error "El servidor se está configurando actualmente".
  
 Cuando el catálogo está configurado, se escribe una entrada en el registro de operaciones.  
  
## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita uno de los permisos siguientes:  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errores y advertencias  
 En la siguiente lista se describen algunas condiciones que pueden producir un error o una advertencia:  
  
-   La propiedad no existe  
  
-   El valor de la propiedad no es válido  
  
  
