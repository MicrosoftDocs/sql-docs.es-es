---
description: sp_linkedservers (Transact-SQL)
title: sp_linkedservers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_linkedservers
- sp_linkedservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_linkedservers
ms.assetid: d8f82f78-8a1f-4831-bcee-7c36c6e7dfbb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a376e29a988e64242fc06d0c550814f51d8ad1f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103161"
---
# <a name="sp_linkedservers-transact-sql"></a>sp_linkedservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la lista de servidores vinculados definidos en el servidor local.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_linkedservers  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o un número distinto de cero (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**SRV_NAME**|**sysname**|Nombre del servidor vinculado.|  
|**SRV_PROVIDERNAME**|**nvarchar (** 128 **)**|Nombre descriptivo del proveedor OLE DB que administra el acceso al servidor vinculado especificado.|  
|**SRV_PRODUCT**|**nvarchar (** 128 **)**|Nombre de producto del servidor vinculado.|  
|**SRV_DATASOURCE**|**nvarchar (** 4000 **)**|Propiedad de origen de datos OLE DB correspondiente al servidor vinculado especificado.|  
|**SRV_PROVIDERSTRING**|**nvarchar (** 4000 **)**|Propiedad de cadena del proveedor OLE DB correspondiente al servidor vinculado.|  
|**SRV_LOCATION**|**nvarchar (** 4000 **)**|Propiedad de ubicación de OLE DB correspondiente al servidor vinculado especificado.|  
|**SRV_CAT**|**sysname**|Propiedad de catálogo de OLE DB correspondiente al servidor vinculado especificado.|  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="see-also"></a>Consulte también  
 [sp_catalogs &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_columns_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-columns-ex-transact-sql.md)   
 [sp_foreignkeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_primarykeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_table_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [sp_tables_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de consultas distribuidas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)  
  
  
