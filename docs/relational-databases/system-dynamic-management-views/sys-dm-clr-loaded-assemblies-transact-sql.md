---
description: sys.dm_clr_loaded_assemblies (Transact-SQL)
title: sys.dm_clr_loaded_assemblies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_clr_loaded_assemblies
- sys.dm_clr_loaded_assemblies_TSQL
- dm_clr_loaded_assemblies_TSQL
- sys.dm_clr_loaded_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_loaded_assemblies dynamic management view
ms.assetid: 8523d8db-d8a0-4b1f-ae19-6705d633e0a6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fea43cf3bdbbedb83b8b848c322986c4f53afebb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188802"
---
# <a name="sysdm_clr_loaded_assemblies-transact-sql"></a>sys.dm_clr_loaded_assemblies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada ensamblado de usuario administrado cargado en el espacio de direcciones del servidor. Use esta vista para comprender y solucionar problemas de los objetos de base de datos administrados de integración CLR que se ejecutan en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Los ensamblados son archivos DLL que se utilizan para definir e implementar objetos de base de datos administrados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Siempre que un usuario ejecuta uno de estos objetos de base de datos administrados, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y CLR cargan el ensamblado en el que se define el objeto de base de datos administrado, así como sus referencias. El ensamblado permanece cargado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para aumentar el rendimiento, de modo que pueda llamarse en el futuro a los objetos de base de datos administrados incluidos en el ensamblado sin tener que volver a cargar el ensamblado. El ensamblado no se descarga hasta que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se encuentra bajo presión de la memoria. Para obtener más información sobre los ensamblados y la integración CLR, vea [entorno hospedado de CLR](../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md). Para obtener más información acerca de los objetos de base de datos administrados, vea [crear objetos de base de datos con Common Language Runtime &#40;CLR&#41; Integration](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|Id. del ensamblado cargado. El **assembly_id** se puede utilizar para buscar más información acerca del ensamblado en la vista de catálogo [Sys. Assemblies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) . Tenga en cuenta que el [!INCLUDE[tsql](../../includes/tsql-md.md)] Catálogo [Sys. Assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) solo muestra ensamblados en la base de datos actual. En la vista **SQS.dm_clr_loaded_assemblies** se muestran todos los ensamblados cargados en el servidor.|  
|**appdomain_address**|**int**|Dirección del dominio de aplicación (**AppDomain**) en el que se carga el ensamblado. Todos los ensamblados que pertenecen a un solo usuario se cargan siempre en el mismo **AppDomain**. El **appdomain_address** se puede usar para buscar más información sobre el **AppDomain** en la vista de [Sys.dm_clr_appdomains](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md) .|  
|**load_time**|**datetime**|Hora a la que se cargó el ensamblado. Tenga en cuenta que el ensamblado permanece cargado hasta que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se encuentra en la presión de memoria y descarga el **AppDomain**. Puede supervisar **load_time** para saber con qué frecuencia se produce [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la presión de memoria y descargar el **AppDomain**.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="remarks"></a>Observaciones  
 La vista **dm_clr_loaded_assemblies. appdomain_address** tiene una relación de varios a uno con  **dm_clr_appdomains. appdomain_address**. La vista **dm_clr_loaded_assemblies. assembly_id** tiene una relación de uno a varios con **Sys. Assemblies. assembly_id**.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestra cómo ver información detallada de todos los ensamblados que están actualmente cargados en la base de datos activa.  
  
```  
 SELECT a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
FROM sys.dm_clr_loaded_assemblies AS l   
INNER JOIN sys.assemblies AS a  
ON l.assembly_id = a.assembly_id;  
```  
  
 En el ejemplo siguiente se muestra cómo ver los detalles del **AppDomain** en el que se carga un ensamblado determinado.  
  
```  
SELECT appdomain_id, creation_time, db_id, user_id, state  
FROM sys.dm_clr_appdomains AS a  
WHERE appdomain_address =   
(SELECT appdomain_address   
 FROM sys.dm_clr_loaded_assemblies  
 WHERE assembly_id = 555);  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica relacionadas con Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
