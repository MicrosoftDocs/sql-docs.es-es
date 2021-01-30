---
description: sys.dm_clr_appdomains (Transact-SQL)
title: sys.dm_clr_appdomains (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_clr_appdomains
- sys.dm_clr_appdomains
- dm_clr_appdomains_TSQL
- sys.dm_clr_appdomains_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_appdomains dynamic management dynamic management view
ms.assetid: 9fe0d4fd-950a-4274-a493-85e776278045
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7579a0648bbc5eee90512bb70dfd667728b56834
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208111"
---
# <a name="sysdm_clr_appdomains-transact-sql"></a>sys.dm_clr_appdomains (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada dominio de aplicación en el servidor. El dominio de aplicación (**AppDomain**) es una construcción del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR) que es la unidad de aislamiento para una aplicación. Puede utilizar esta vista para comprender y solucionar problemas de objetos de integración CLR que se ejecutan en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Existen varios tipos de objetos de base de datos administrados de integración CLR. Para obtener información general sobre estos objetos, vea [crear objetos de base de datos con la integración de Common Language Runtime (CLR)](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md). Cada vez que se ejecutan estos objetos, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea un **AppDomain** en el que puede cargar y ejecutar el código necesario. El nivel de aislamiento de un **AppDomain** es un **AppDomain** por cada base de datos por propietario. Es decir, todos los objetos CLR propiedad de un usuario se ejecutan siempre en el mismo **AppDomain** por base de datos (si un usuario registra objetos de base de datos de CLR en diferentes bases de datos, los objetos de base de datos de CLR se ejecutarán en dominios de aplicación diferentes). Un **AppDomain** no se destruye una vez finalizada la ejecución del código. sino que se almacena en la memoria caché de cara a futuras ejecuciones. De este modo se aumenta el rendimiento.  
  
 Para obtener más información, consulte [dominios de aplicación](../../sql-server/what-s-new-in-sql-server-2016.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**appdomain_address**|**varbinary(8**|Dirección del **AppDomain**. Todos los objetos de base de datos administrados que pertenecen a un usuario se cargan siempre en el mismo **AppDomain**. Puede utilizar esta columna para buscar todos los ensamblados cargados actualmente en este **AppDomain** en **Sys.dm_clr_loaded_assemblies**.|  
|**appdomain_id**|**int**|IDENTIFICADOR del **AppDomain**. Cada **AppDomain** tiene un identificador único.|  
|**appdomain_name**|**VARCHAR (386)**|Nombre del **AppDomain** asignado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**creation_time**|**datetime**|Hora a la que se creó el **AppDomain** . Dado que los **AppDomains** se almacenan en caché y se reutilizan para mejorar el rendimiento, **creation_time** no es necesariamente el momento en que se ejecutó el código.|  
|**db_id**|**int**|IDENTIFICADOR de la base de datos en la que se creó este **AppDomain** . El código almacenado en dos bases de datos diferentes no puede compartir un **AppDomain**.|  
|**user_id**|**int**|IDENTIFICADOR del usuario cuyos objetos se pueden ejecutar en este **AppDomain**.|  
|**state**|**nvarchar(128)**|Un descriptor para el estado actual del **AppDomain**. Un AppDomain puede encontrarse en estados diferentes desde la creación a la eliminación. Vea la sección Comentarios de este tema para obtener más información.|  
|**strong_refcount**|**int**|Número de referencias seguras a este **AppDomain**. Esto refleja el número de lotes que se están ejecutando actualmente y que utilizan este **AppDomain**. Tenga en cuenta que la ejecución de esta vista creará un **refcount fuerte**; Aunque no sea un código que se esté ejecutando actualmente, **strong_refcount** tendrá un valor de 1.|  
|**weak_refcount**|**int**|Número de referencias débiles a este **AppDomain**. Esto indica cuántos objetos dentro del **AppDomain** están almacenados en caché. Al ejecutar un objeto de base de datos administrado, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo almacena en la memoria caché dentro del **AppDomain** para su reutilización futura. De este modo se aumenta el rendimiento.|  
|**cost**|**int**|Costo del **AppDomain**. Cuanto mayor sea el costo, más probable será que este **AppDomain** se descargue bajo presión de memoria. El costo suele depender de la cantidad de memoria necesaria para volver a crear este **AppDomain**.|  
|**value**|**int**|Valor de **AppDomain**. Cuanto menor sea el valor, más probable es que este **AppDomain** se descargue bajo presión de memoria. El valor suele depender del número de conexiones o lotes que usa este **AppDomain**.|  
|**total_processor_time_ms**|**bigint**|Tiempo total del procesador, en milisegundos, consumido por todos los subprocesos durante la ejecución en el dominio de aplicación actual desde que se inició el proceso. Esto es equivalente a **System. AppDomain. MonitoringTotalProcessorTime**.|  
|**total_allocated_memory_kb**|**bigint**|Tamaño total, en kilobytes, de todas las asignaciones de memoria realizadas por el dominio de aplicación desde que se creó, sin restar memoria recopilada. Esto es equivalente a **System. AppDomain. MonitoringTotalAllocatedMemorySize**.|  
|**survived_memory_kb**|**bigint**|Número de kilobytes que sobrevivieron a la última colección de bloqueo completa a que debe hacer referencia el dominio de aplicación actual. Esto es equivalente a **System. AppDomain. MonitoringSurvivedMemorySize**.|  
  
## <a name="remarks"></a>Observaciones  
 Existe una relación de uno a uno entre **dm_clr_appdomains. appdomain_address** y **dm_clr_loaded_assemblies. appdomain_address**.  
  
 En las tablas siguientes se enumeran los posibles valores de **Estado** , sus descripciones y cuándo se producen en el ciclo de vida de **AppDomain** . Puede usar esta información para seguir el vida de un **AppDomain** y para ver la descarga de instancias de **AppDomain** sospechosas o repetitivas, sin tener que analizar el registro de eventos de Windows.  
  
## <a name="appdomain-initialization"></a>Inicialización de AppDomain  
  
|Estado|Descripción|  
|-----------|-----------------|  
|E_APPDOMAIN_CREATING|Se está creando el **AppDomain** .|  
  
## <a name="appdomain-usage"></a>Uso de AppDomain  
  
|Estado|Descripción|  
|-----------|-----------------|  
|E_APPDOMAIN_SHARED|El **AppDomain** en tiempo de ejecución está listo para que lo usen varios usuarios.|  
|E_APPDOMAIN_SINGLEUSER|**AppDomain** está listo para su uso en operaciones DDL. Se diferencian de E_APPDOMAIN_SHARED en que AppDomains compartidos se usan para ejecuciones de integración CLR en contraposición con operaciones de DDL. Dichos AppDomains están aislados de las operaciones simultáneas.|  
|E_APPDOMAIN_DOOMED|El **AppDomain** está programado para descargarse, pero actualmente hay subprocesos que se ejecutan en él.|  
  
## <a name="appdomain-cleanup"></a>Limpieza de AppDomain  
  
|Estado|Descripción|  
|-----------|-----------------|  
|E_APPDOMAIN_UNLOADING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha solicitado que CLR Descargue el **AppDomain**, normalmente porque se ha modificado o quitado el ensamblado que contiene los objetos de base de datos administrados.|  
|E_APPDOMAIN_UNLOADED|CLR ha descargado el **AppDomain**. Esto suele ser el resultado de un procedimiento de extensión debido a **ThreadAbort**, **OutOfMemory**, o a una excepción no controlada en el código de usuario.|  
|E_APPDOMAIN_ENQUEUE_DESTROY|**AppDomain** se ha descargado en CLR y ha establecido que lo destruya [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_DESTROY|El **AppDomain** está en proceso de ser destruido por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_ZOMBIE|Ha destruido el **AppDomain** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; sin embargo, no todas las referencias al **AppDomain** se han limpiado.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo ver los detalles de un **AppDomain** para un ensamblado determinado:  
  
```  
select appdomain_id, creation_time, db_id, user_id, state  
from sys.dm_clr_appdomains a  
where appdomain_address =   
(select appdomain_address   
 from sys.dm_clr_loaded_assemblies  
   where assembly_id = 500);  
```  
  
 En el ejemplo siguiente se muestra cómo ver todos los ensamblados en un **AppDomain** determinado:  
  
```  
select a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
from sys.dm_clr_loaded_assemblies as l   
inner join sys.assemblies as a  
on l.assembly_id = a.assembly_id  
where l.appdomain_address =   
(select appdomain_address   
from sys.dm_clr_appdomains  
where appdomain_id = 15);  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_clr_loaded_assemblies &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)   
 [Vistas de administración dinámica relacionadas con Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
