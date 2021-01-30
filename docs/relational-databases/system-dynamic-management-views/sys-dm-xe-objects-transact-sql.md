---
description: sys.dm_xe_objects (Transact-SQL)
title: sys.dm_xe_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xe_objects
- sys.dm_xe_objects
- sys.dm_xe_objects_TSQL
- dm_xe_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_objects dynamic management view
- extended events [SQL Server], views
ms.assetid: 5d944b99-b097-491b-8cbd-b0e42b459ec0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9e797a7010a23bfc8b13673d1efa1a90d849e6ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99129338"
---
# <a name="sysdm_xe_objects-transact-sql"></a>sys.dm_xe_objects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada objeto expuesto por un paquete de eventos. Los objetos pueden ser alguno de los siguientes:  
  
-   Ceso. Los eventos indican los puntos de interés en una ruta de ejecución. Todos los eventos contienen información sobre un punto de interés.  
  
-   Acciones. Las acciones se ejecutan sincrónicamente cuando se activan los eventos. Una acción puede anexar información del tiempo de ejecución a un evento.  
  
-   Destinos. Los destinos utilizan eventos, sincrónicamente en el subproceso que activa el evento o de forma asincrónica en un subproceso proporcionado por el sistema.  
  
-   Predicados. Los orígenes de predicado recuperan los valores de los orígenes de eventos para utilizarlos en operaciones de comparación. Las comparaciones de predicado comparan tipos de datos concretos y devuelven un valor booleano.  
  
-   Tipos. Los tipos encapsulan la longitud y las características de la colección byte, necesaria para interpretar los datos.  

 |Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(60)**|El nombre del objeto. el nombre es único dentro de un paquete para un tipo de objeto específico. No admite valores NULL.|  
|object_type|**nvarchar(60)**|Tipo del objeto. object_type es uno de los siguientes:<br /><br /> event<br /><br /> action<br /><br /> Destino<br /><br /> pred_source<br /><br /> pred_compare<br /><br /> tipo<br /><br /> No admite valores NULL.|  
|package_guid|**uniqueidentifier**|GUID del paquete que expone esta acción. Hay una relación de varios a uno con sys.dm_xe_packages.package_id. No admite valores NULL.|  
|description|**nvarchar(256)**|Descripción de la acción. la descripción la establece el autor del paquete. No admite valores NULL.|  
|capabilities|**int**|Mapa de bits que describe las capacidades del objeto. Acepta valores NULL.|  
|capabilities_desc|**nvarchar(256)**|Enumera todas las capacidades del objeto. Acepta valores NULL.<br /><br /> **Capacidades que se aplican a todos los tipos de objeto**<br /><br /> -<br />                                **Privado**. El único objeto disponible para uso interno, y al que no se puede acceder mediante la DLL CREATE/ALTER EVENT SESSION. Los destinos y los eventos de auditoría pertenecen a esta categoría, así como un pequeño número de objetos usados internamente.<br /><br /> ===============<br /><br /> **Capacidades de eventos**<br /><br /> -<br />                                **No_block**. El evento está en una ruta de acceso de código crítica que se no puede bloquear por ningún motivo. Los eventos con esta capacidad no se pueden agregar a ninguna sesión de evento que especifique NO_EVENT_LOSS.<br /><br /> ===============<br /><br /> **Capacidades que se aplican a todos los tipos de objeto**<br /><br /> -<br />                                **Process_whole_buffers**. El destino consume búferes de eventos a la vez, en lugar de evento por evento.<br /><br /> -<br />                        **Singleton**. Solamente puede haber una instancia del destino en un proceso. Aunque varias sesiones de eventos pueden hacer referencia al mismo destino singleton, en realidad solo hay una instancia, y esa instancia verá cada evento distinto una sola vez. Esto es importante si se agrega el destino a varias sesiones que recopilan el mismo evento.<br /><br /> -<br />                                **Sincrónicos**. El destino se ejecuta en el subproceso que produce el evento, antes de que se devuelva el control a la línea de código de llamada.|  
|type_name|**nvarchar(60)**|Nombre de los objetos pred_source y pred_compare. Acepta valores NULL.|  
|type_package_guid|**uniqueidentifier**|GUID del paquete que expone el tipo en el que este objeto funciona. Acepta valores NULL.|  
|type_size|**int**|Tamaño del tipo de datos, en bytes. Solo es válido para los tipos de objetos válidos. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
### <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|sys.dm_xe_objects.package_guid|sys.dm_xe_packages.guid|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

