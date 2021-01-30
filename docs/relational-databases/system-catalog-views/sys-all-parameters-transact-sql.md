---
description: sys.all_parameters (Transact-SQL)
title: sys.all_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- all_parameters_TSQL
- sys.all_parameters
- all_parameters
- sys.all_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_parameters catalog view
ms.assetid: eecbb68e-9b4c-4243-94e2-8096a9cc7892
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 17c1c62c582a4e8937e82b7ba911955a6805ab26
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205547"
---
# <a name="sysall_parameters-transact-sql"></a>sys.all_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Muestra la unión de todos los parámetros que pertenecen a objetos definidos por el usuario u objetos del sistema.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificador del objeto al que pertenece el parámetro.|  
|**name**|**sysname**|Nombre del parámetro. Es único en el objeto. Si el objeto es una función escalar, el nombre del parámetro es una cadena vacía en la fila que representa el valor devuelto.|  
|**parameter_id**|**int**|Id. del parámetro. Es único en el objeto. Si el objeto es una función escalar, **parameter_id** = 0 representa el valor devuelto.|  
|**system_type_id**|**tinyint**|ID. del tipo de sistema del parámetro.|  
|**user_type_id**|**int**|Id. de tipo del parámetro, definido por el usuario.<br /><br /> Para devolver el nombre del tipo, únase a la vista de catálogo [Sys. Types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) en esta columna.|  
|**max_length**|**smallint**|Longitud máxima del parámetro, en bytes.<br /><br /> -1 = el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.|  
|**precisión**|**tinyint**|Precisión del parámetro si es numérico; de otro modo, es 0.|  
|**scale**|**tinyint**|Escala del parámetro si es numérico; de otro modo, es 0.|  
|**is_output**|**bit**|1 = El parámetro es de salida (o de retorno); de otro modo, es 0.|  
|**is_cursor_ref**|**bit**|1 = Se trata de un parámetro de referencia de cursor.|  
|**has_default_value**|**bit**|1 = El parámetro tiene un valor predeterminado.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo mantiene valores predeterminados para objetos CLR en esta vista de catálogo; por lo tanto, esta columna siempre tiene valor 0 para objetos [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para ver el valor predeterminado de un parámetro en un [!INCLUDE[tsql](../../includes/tsql-md.md)] objeto, consulte la columna de **definición** de la vista de catálogo [Sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) o utilice la función del sistema [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .|  
|**is_xml_document**|**bit**|1 = El contenido es un documento XML completo.<br /><br /> 0 = el contenido es un fragmento de documento o el tipo de datos de la columna no es **XML**.|  
|**default_value**|**sql_variant**|Si **has_default_value** es 1, el valor de esta columna es el valor predeterminado del parámetro; de lo contrario, es NULL.|  
|**xml_collection_id**|**int**|Es el Id. de la colección de esquemas XML utilizada para validar el parámetro.<br /><br /> Distinto de cero si el tipo de datos del parámetro es **XML** y se escribe el XML.<br /><br /> 0 = No hay una colección de esquemas XML o el parámetro no es XML.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Sys. Parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)   
 [sys.system_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md)  
  
  
