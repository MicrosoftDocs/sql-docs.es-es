---
description: 'Vistas de catálogo del testigo de creación de reflejo de la base de datos: sys.database_mirroring_witnesses'
title: sys.database_mirroring_witnesses (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring_witnesses
- sys.database_mirroring_witnesses_TSQL
- database_mirroring_witnesses
- database_mirroring_witnesses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_witnesses catalog view
- witness [SQL Server], sys.database_mirroring_witnesses catalog view
ms.assetid: 0dd5b794-733b-4a3c-b5a4-62f9f1f0f22d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 63599e8dfbeafc6136e2d4904a546e5ebbd5dd96
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465103"
---
# <a name="database-mirroring-witness-catalog-views---sysdatabase_mirroring_witnesses"></a>Vistas de catálogo del testigo de creación de reflejo de la base de datos: sys.database_mirroring_witnesses
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila para cada rol de testigo desempeñado por un servidor en una asociación de creación de reflejo de la base de datos. 
  
  En una sesión de creación del reflejo de la base de datos, la conmutación automática por error requiere un servidor testigo. Lo ideal es que el testigo resida en un equipo independiente de los servidores principal y reflejado. El testigo no está disponible para la base de datos. En su lugar, supervisa el estado de los servidores principal y reflejado. Si se produce un error en el servidor principal, el testigo puede iniciar la conmutación automática por error al servidor reflejado. 
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nombre de las dos copias de la base de datos en la sesión de creación de reflejo de la base de datos.|  
|**principal_server_name**|**sysname**|Nombre del servidor asociado cuya copia de la base de datos es actualmente la base de datos principal.|  
|**mirror_server_name**|**sysname**|Nombre del servidor asociado cuya copia de la base de datos es actualmente la base de datos reflejada.|  
|**safety_level**|**tinyint**|Configuración de la seguridad de las transacciones para realizar actualizaciones en la base de datos reflejada:<br /><br /> 0 = Estado desconocido<br /><br /> 1 = Desactivada (asincrónica)<br /><br /> 2 = Completa (sincrónica)<br /><br /> Cuando se utiliza un testigo para la conmutación automática por error, se requiere la seguridad de transacciones completa, que es el valor predeterminado.|  
|**safety_level_desc**|**nvarchar(60)**|Descripción de la garantía de seguridad de las actualizaciones de la base de datos reflejada:<br /><br /> DESCONOCIDO<br /><br /> Apagado<br /><br /> FULL|  
|**safety_sequence_number**|**int**|Actualiza el número de secuencia de los cambios en **safety_level**.|  
|**role_sequence_number**|**int**|Número de secuencia de actualización de los cambios en los roles principal/reflejado desempeñados por los asociados de creación de reflejo.|  
|**mirroring_guid**|**uniqueidentifier**|Identificador de la asociación de creación de reflejo.|  
|**family_guid**|**uniqueidentifier**|Identificador de la familia de copias de seguridad de la base de datos. Se utiliza para detectar estados de restauración coincidentes.|  
|**is_suspended**|**bit**|La creación de reflejo de la base de datos se ha suspendido.|  
|**is_suspended_sequence_number**|**int**|Número de secuencia para establecer **is_suspended**.|  
|**partner_sync_state**|**tinyint**|Estado de sincronización de la sesión de reflejo:<br /><br /> 5 = los asociados están sincronizados. La conmutación por error es potencialmente posible. Para obtener información acerca de los requisitos para la conmutación por error, vea [conmutación de roles durante una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).<br /><br /> 6 = los asociados no están sincronizados. La conmutación por error no es posible.|  
|**partner_sync_state_desc**|**nvarchar(60)**|Descripción del estado de sincronización de la sesión de reflejo:<br /><br /> SYNCHRONIZED<br /><br /> UNSYNCHRONIZED|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Testigo de creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-witness.md)   
 [sys.database_mirroring &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
