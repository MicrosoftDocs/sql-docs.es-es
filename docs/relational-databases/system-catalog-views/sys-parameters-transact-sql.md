---
description: sys.parameters (Transact-SQL)
title: Sys. Parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.parameters_TSQL
- sys.parameters
- parameters
- parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.parameters catalog view
- table-valued parameters,sys.parameters
ms.assetid: 24e2764b-c8e5-4322-97a4-7407d8b8a92b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 18a527e54f2a5b49926334fbb6755ecb6d97fb37
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465024"
---
# <a name="sysparameters-transact-sql"></a>sys.parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada parámetro de un objeto que acepta parámetros. Si el objeto es una función escalar, también hay una fila que describe el valor devuelto. Esa fila tendrá un valor de **parameter_id** de 0.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificador del objeto al que pertenece el parámetro.|  
|**name**|**sysname**|Nombre del parámetro. Es único en el objeto.<br /><br /> Si el objeto es una función escalar, el nombre del parámetro es una cadena vacía en la fila que representa el valor devuelto.|  
|**parameter_id**|**int**|IDENTIFICADOR del parámetro. Es único en el objeto.<br /><br /> Si el objeto es una función escalar, **parameter_id** = 0 representa el valor devuelto.|  
|**system_type_id**|**tinyint**|ID. del tipo de sistema del parámetro.|  
|**user_type_id**|**int**|Id. de tipo del parámetro, definido por el usuario.<br /><br /> Para devolver el nombre del tipo, únase a la vista de catálogo [Sys. Types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) en esta columna.|  
|**max_length**|**smallint**|Longitud máxima del parámetro, en bytes.<br /><br /> Valor =-1 cuando el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.|  
|**precisión**|**tinyint**|Precisión del parámetro si está basado en numerales; de lo contrario es 0.|  
|**scale**|**tinyint**|Escala del parámetro si está basado en numerales; de lo contrario es 0.|  
|**is_output**|**bit**|1 = El parámetro es OUTPUT o RETURN; de lo contrario, es 0.|  
|**is_cursor_ref**|**bit**|1 = El parámetro es un parámetro de referencia a un cursor.|  
|**has_default_value**|**bit**|1 = el parámetro tiene un valor predeterminado.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo mantiene valores predeterminados para objetos de CLR en esta vista de catálogo; por lo tanto, esta columna tiene valor 0 para objetos [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para ver el valor predeterminado de un parámetro en un [!INCLUDE[tsql](../../includes/tsql-md.md)] objeto, consulte la columna de **definición** de la vista de catálogo [Sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) o utilice la función del sistema [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .|  
|**is_xml_document**|**bit**|1 = El contenido es un documento XML completo.<br /><br /> 0 = el contenido es un fragmento de documento o el tipo de datos de la columna no es **XML**.|  
|**default_value**|**sql_variant**|Si **has_default_value** es 1, el valor de esta columna es el valor predeterminado del parámetro; de lo contrario, es NULL.|  
|**xml_collection_id**|**int**|Distinto de cero si el tipo de datos del parámetro es **XML** y se escribe el XML. El valor es el identificador de la colección que contiene el espacio de nombres del esquema XML de validación del parámetro.<br /><br /> 0 = No es una colección de esquemas XML.|  
|**is_readonly**|**bit**|1 =  El parámetro es READONLY; de lo contrario, es 0.|  
|**is_nullable**|**bit**|1 = El parámetro admite valores NULL. (el valor predeterminado).<br /><br /> 0 = El parámetro no admite valores NULL, para que la ejecución de procedimientos almacenados compilados de forma nativa sea más eficaz.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.all_parameters &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-parameters-transact-sql.md)   
 [sys.system_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md)  
  
  
