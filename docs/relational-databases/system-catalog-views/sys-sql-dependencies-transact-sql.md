---
description: sys.sql_dependencies (Transact-SQL)
title: sys.sql_dependencies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sql_dependencies
- sql_dependencies_TSQL
- sys.sql_dependencies_TSQL
- sys.sql_dependencies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_dependencies catalog view
ms.assetid: 1779aa87-a0b8-470a-a286-d7cc0b93ad2e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2032bc1ba6658b18649f5908bebd4ae9405522b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99150711"
---
# <a name="syssql_dependencies-transact-sql"></a>sys.sql_dependencies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada dependencia de una entidad a la que se hace referencia conforme a su referencia en la expresión o instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que definen otro objeto que hace la referencia.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use [Sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) .  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Identifica la clase de la entidad a la que se hace referencia:<br /><br /> 0 = objeto o columna (solo referencias no enlazadas a esquema)<br /><br /> 0 = Objeto o columna (referencias enlazadas a esquemas)<br /><br /> 2 = Tipos (referencias enlazadas a esquemas)<br /><br /> 3 = Colecciones de esquemas XML (referencias enlazadas a esquemas)<br /><br /> 4 = Función de partición (referencias enlazadas a esquemas)|  
|**class_desc**|**nvarchar(60)**|Descripción de la clase de la entidad a la que se hace referencia:<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_NON_SCHEMA_BOUND**<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_SCHEMA_BOUND**<br /><br /> **TYPE_REFERENCE**<br /><br /> **XML_SCHEMA_COLLECTION_REFERENCE**<br /><br /> **PARTITION_FUNCTION_REFERENCE**|  
|**object_id**|**int**|Id. del objeto que hace la referencia.|  
|**column_id**|**int**|Si el Id. que hace la referencia es una columna, se obtiene el Id. de la columna que hace la referencia; en caso contrario, el valor es 0.|  
|**referenced_major_id**|**int**|Id. de la entidad a la que se hace referencia, interpretado por el valor de clase de acuerdo con:<br /><br /> 0, 1 = Id. de objeto del objeto o la columna.<br /><br /> 2 = Id. del tipo.<br /><br /> 3 = Id. de la colección de esquemas XML.|  
|**referenced_minor_id**|**int**|Id. secundario de la entidad a la que se hace referencia, interpretado por el valor de clase de acuerdo con lo siguiente.<br /><br /> Si la clase =:<br /><br /> 0, **referenced_minor_id** es un identificador de columna; o bien, si no es una columna, es 0.<br /><br /> 1, **referenced_minor_id** es un identificador de columna; o bien, si no es una columna, es 0.<br /><br /> De lo contrario, **referenced_minor_id** = 0.|  
|**is_selected**|**bit**|Objeto o columna seleccionados.|  
|**is_updated**|**bit**|Objeto o columna actualizados.|  
|**is_select_all**|**bit**|El objeto se utiliza en la cláusula SELECT* (solo nivel de objeto).|  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
