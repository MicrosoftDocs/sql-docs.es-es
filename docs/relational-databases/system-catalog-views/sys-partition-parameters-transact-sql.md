---
description: sys.partition_parameters (Transact-SQL)
title: sys.partition_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- partition_parameters_TSQL
- partition_parameters
- sys.partition_parameters_TSQL
- sys.partition_parameters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_parameters catalog view
ms.assetid: 2012ed9d-3ea3-4c29-9b78-dfa54a392dce
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1d750939c6d2a99e695b894164c6af20953297bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203804"
---
# <a name="syspartition_parameters-transact-sql"></a>sys.partition_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada parámetro de una función de partición.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**function_id**|**int**|Id. de la función de partición a la que pertenece este parámetro.|  
|**parameter_id**|**int**|IDENTIFICADOR del parámetro. Es único en la función de partición y empieza por 1.|  
|**system_type_id**|**tinyint**|ID. del tipo de sistema del parámetro. Corresponde al **system_type_id** columna de la vista de catálogo **Sys. Types** .|  
|**max_length**|**smallint**|Longitud máxima del parámetro en bytes.|  
|**precisión**|**tinyint**|Precisión del parámetro si está basado en numerales; de lo contrario es 0.|  
|**scale**|**tinyint**|Escala del parámetro si está basado en numerales; de lo contrario es 0.|  
|**collation_name**|**sysname**|Nombre de la intercalación del parámetro, si está basado en caracteres; en caso contrario, es NULL.|  
|**user_type_id**|**int**|Id. del tipo. Es único en la base de datos. En el caso de los tipos de datos del sistema, **user_type_id**  =  **system_type_id**.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de funciones de partición &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
 [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)  
  
  
