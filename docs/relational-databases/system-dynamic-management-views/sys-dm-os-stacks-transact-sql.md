---
description: sys.dm_os_stacks (Transact-SQL)
title: sys.dm_os_stacks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_stacks
- dm_os_stacks_TSQL
- sys.dm_os_stacks
- sys.dm_os_stacks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_stacks dynamic management view
ms.assetid: a69b06c4-28f0-4535-8fa1-9f132db4d916
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04f9fe453b2f3e74a96ebd20565d92038bff4bae
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2020
ms.locfileid: "97325391"
---
# <a name="sysdm_os_stacks-transact-sql"></a>sys.dm_os_stacks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa esta vista de administración dinámica internamente para hacer lo siguiente:  
  
-   Realizar el seguimiento de datos de depuración como asignaciones pendientes.  
  
-   Suponer o validar la lógica que usan los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en sitios donde el componente supone que se ha realizado una determinada llamada.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**stack_address**|**varbinary(8**|Dirección única para esta asignación de pilas. No admite valores NULL.|  
|**frame_index**|**int**|Cada línea representa una llamada de función que, cuando se ordena en orden ascendente por índice de marco para un **stack_address** determinado, devuelve la pila de llamadas completa. No admite valores NULL.|  
|**frame_address**|**varbinary(8**|Dirección de la llamada a función. No admite valores NULL.|  
  
## <a name="remarks"></a>Observaciones  
 **Sys.dm_os_stacks** requiere que los símbolos del servidor y otros componentes estén presentes en el servidor para mostrar la información correctamente.  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   


## <a name="see-also"></a>Consulte también  
  [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  
