---
description: sys.numbered_procedure_parameters (Transact-SQL)
title: sys.numbered_procedure_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- numbered_procedure_parameters_TSQL
- sys.numbered_procedure_parameters_TSQL
- numbered_procedure_parameters
- sys.numbered_procedure_parameters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.numbered_procedure_parameters catalog view
ms.assetid: a441d46d-1f30-41c2-8d94-e9442f59786e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 861331152b5e3c25b09d14e326414a4ba25ba78b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191282"
---
# <a name="sysnumbered_procedure_parameters-transact-sql"></a>sys.numbered_procedure_parameters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada parámetro de un procedimiento numerado. Cuando se crea un procedimiento almacenado numerado, el procedimiento base es el número 1. Todos los procedimientos subsiguientes tienen los números 2, 3, etc. **Sys.numbered_procedure_parameters** contiene las definiciones de los parámetros de todos los procedimientos subsiguientes, numerados con 2 y mayores. Esta vista no muestra los parámetros del procedimiento almacenado base (número = 1). El procedimiento almacenado base es similar a un procedimiento almacenado no numerado. Por lo tanto, sus parámetros se representan en [Sys. Parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md).  
  
> [!IMPORTANT]  
>  Los procedimientos numerados son desusados. Por tanto, se desaconseja su uso. Se desencadena un evento DEPRECATION_ANNOUNCEMENT cuando se compila una consulta que utiliza esta vista de catálogo.  
  
> [!NOTE]  
>  Los parámetros XML y CLR no son compatibles con los procedimientos numerados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificador del objeto al que pertenece el parámetro.|  
|**procedure_number**|**smallint**|Número de este procedimiento en el objeto, 2 o mayor.|  
|**name**|**sysname**|Nombre del parámetro. Es único dentro de **procedure_number**.|  
|**parameter_id**|**int**|IDENTIFICADOR del parámetro. Es único en el **procedure_number**.|  
|**system_type_id**|**tinyint**|Id. del tipo de sistema del parámetro.|  
|**user_type_id**|**int**|Id. del tipo, definido por el usuario, del parámetro.|  
|**max_length**|**smallint**|Longitud máxima del parámetro en bytes.<br /><br /> -1 = El tipo de datos de las columnas es varchar(max), nvarchar(max) o varbinary(max).|  
|**precisión**|**tinyint**|Precisión del parámetro si está basado en numerales; de lo contrario es 0.|  
|**scale**|**tinyint**|Escala del parámetro si está basado en numerales; de lo contrario es 0.|  
|**is_output**|**bit**|1 = El parámetro es de salida o retorno; de lo contrario, es 0|  
|**is_cursor_ref**|**bit**|1 = El parámetro es un parámetro de referencia a un cursor.|  
  
> [!NOTE]  
>  Los parámetros XML y CLR no son compatibles con los procedimientos numerados.  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
