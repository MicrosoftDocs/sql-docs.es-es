---
title: sys.dm_pdw_nodes_exec_sql_text (Transact-SQL) | Microsoft Docs
description: Vista de administración dinámica que devuelve el texto del lote SQL que se identifica mediante el sql_handle especificado.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: b4e4c686411d40a2c161c670821e6566460db4a4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644078"
---
# <a name="syspdw_nodes_dm_exec_sql_text-transact-sql"></a>sys.pdw_nodes_dm_exec_sql_text (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Devuelve el texto del lote SQL que se identifica mediante el *sql_handle* especificado. Esta función con valores de tabla reemplaza a la función del sistema **fn_get_sql**.  
   
## <a name="table-returned"></a>Tabla devuelta  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|IDENTIFICADOR numérico único asociado al nodo.|
|**DBID**|**smallint**|Identificador de la base de datos.<br /><br /> En el caso de instrucciones SQL no planeadas y preparadas, el identificador de la base de datos donde se compilaron las instrucciones.|  
|**objectid**|**int**|Identificador del objeto.<br /><br /> Este valor es NULL para las instrucciones SQL ad hoc y preparadas.|  
|**número**|**smallint**|En un procedimiento almacenado numerado, esta columna devuelve el número del procedimiento almacenado. Para obtener más información, vea [sys.numbered_procedures &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md).<br /><br /> Este valor es NULL para las instrucciones SQL ad hoc y preparadas.|  
|**cifra**|**bit**|1: el texto SQL está cifrado.<br /><br /> 0: el texto SQL no está cifrado.|  
|**text**|**nvarchar(max)**|Texto de la consulta de SQL.<br /><br /> Este valor es NULL para objetos cifrados.|  

## <a name="remarks"></a>Observaciones  
Se aplican los mismos comentarios en [Sys.dm_exec_sql_text](./sys-dm-exec-sql-text-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 Requiere el rol de servidor **sysadmin** o `VIEW SERVER STATE` el permiso en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

  ## <a name="next-steps"></a>Pasos siguientes
 Para obtener más sugerencias sobre desarrollo, consulte [información general sobre el desarrollo de Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop).