---
description: sys.types (Transact-SQL)
title: Sys. Types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- types
- types_TSQL
- sys.types_TSQL
- sys.types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.types catalog view
- table-valued parameters,sys.types
ms.assetid: a5dbc842-71a0-4f62-b5e0-f560a99b7f8c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6018562852e20bc30d1ea2c9c9f22022e96693ab
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464074"
---
# <a name="systypes-transact-sql"></a>sys.types (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada tipo del sistema y definido por el usuario.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del tipo. Es exclusivo en el esquema.|  
|**system_type_id**|**tinyint**|Id. del tipo del sistema interno del tipo.|  
|**user_type_id**|**int**|Id. del tipo. Es único en la base de datos. En el caso de los tipos de datos del sistema, **user_type_id**  =  **system_type_id**.|  
|**schema_id**|**int**|Id. del esquema al que pertenece el tipo.|  
|**principal_id**|**int**|Id. del propietario individual si es distinto al propietario del esquema. De forma predeterminada, los objetos contenidos en el esquema pertenecen al propietario del esquema. No obstante, es posible especificar un propietario alternativo mediante la instrucción ALTER AUTHORIZATION para cambiar la propiedad.<br /><br /> Si no hay un propietario alternativo individual, el valor es NULL.|  
|**max_length**|**smallint**|Longitud máxima del tipo, en bytes.<br /><br /> -1 = el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.<br /><br /> En el caso de las columnas de **texto** , el valor de **max_length** será 16.|  
|**precisión**|**tinyint**|Precisión máxima del tipo si está basado en numerales; de lo contrario, es 0.|  
|**scale**|**tinyint**|Escala máxima del tipo si está basado en numerales; de lo contrario, es 0.|  
|**collation_name**|**sysname**|Nombre de la intercalación del tipo si está basado en caracteres; de lo contrario, es NULL.|  
|**is_nullable**|**bit**|El tipo admite valores NULL.|  
|**is_user_defined**|**bit**|1 = Tipo definido por el usuario.<br /><br /> 0 = tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**is_assembly_type**|**bit**|1 = La implementación del tipo está definida en un ensamblado CLR.<br /><br /> 0 = El tipo está basado en un tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**default_object_id**|**int**|IDENTIFICADOR del valor predeterminado independiente que está enlazado al tipo mediante [sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md).<br /><br /> 0 = No hay valor predeterminado.|  
|**rule_object_id**|**int**|IDENTIFICADOR de la regla independiente enlazada al tipo mediante [sp_bindrule](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md).<br /><br /> 0 = No hay regla.|  
|**is_table_type**|**bit**|Indica que el tipo es una tabla.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vistas de catálogo de tipos escalares &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
