---
description: sys.foreign_keys (Transact-SQL)
title: sys.foreign_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- foreign_keys
- sys.foreign_keys
- sys.foreign_keys_TSQL
- foreign_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.foreign_keys catalog view
ms.assetid: e960df1a-13fc-43ee-ba91-34c1b719ac2c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 87b8c4bdcd40fc480ad0c4c886a8e5113329f256
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193958"
---
# <a name="sysforeign_keys-transact-sql"></a>sys.foreign_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada objeto que es una restricción FOREIGN KEY, con **Sys. Object. Type** = F.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Para obtener una lista de las columnas que hereda esta vista, vea [Sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**referenced_object_id**|**int**|Id. del objeto al que se hace referencia.|  
|**key_index_id**|**int**|Id. del índice de clave dentro del objeto al que se hace referencia.|  
|**is_disabled**|**bit**|La restricción FOREIGN KEY está deshabilitada.|  
|**is_not_for_replication**|**bit**|La restricción FOREIGN KEY se creó con la opción NOT FOR REPLICATION.|  
|**is_not_trusted**|**bit**|El sistema no ha comprobado la restricción FOREIGN KEY.|  
|**delete_referential_action**|**tinyint**|Acción referencial que se declaró para FOREIGN KEY cuando se produce una eliminación.<br /><br /> 0 = Sin acción<br /><br /> 1 = Cascada<br /><br /> 2 = Establecer como NULL<br /><br /> 3 = Establecer valor predeterminado|  
|**delete_referential_action_desc**|**nvarchar(60)**|Descripción de la acción referencial que se declaró para FOREIGN KEY cuando se produce una eliminación:<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**update_referential_action**|**tinyint**|Acción referencial que se declaró para FOREIGN KEY cuando se produce una actualización.<br /><br /> 0 = Sin acción<br /><br /> 1 = Cascada<br /><br /> 2 = Establecer como NULL<br /><br /> 3 = Establecer valor predeterminado|  
|**update_referential_action_desc**|**nvarchar(60)**|Descripción de la acción referencial que se declaró para FOREIGN KEY cuando se produce una actualización:<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**is_system_named**|**bit**|1 = El sistema generó el nombre.<br /><br /> 0 = El usuario proporcionó el nombre.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
