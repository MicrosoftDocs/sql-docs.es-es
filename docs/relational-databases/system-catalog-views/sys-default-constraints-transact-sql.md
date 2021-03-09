---
description: sys.default_constraints (Transact-SQL)
title: sys.default_constraints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.default_constraints
- sys.default_constraints_TSQL
- default_constraints
- default_constraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.default_constraints catalog view
ms.assetid: 096e3659-edeb-4440-a016-f847acd6166b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9b5e8d50939584a9bc50cf7874be4475a5344ff
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464637"
---
# <a name="sysdefault_constraints-transact-sql"></a>sys.default_constraints (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada objeto que es una definición predeterminada (creada como parte de una instrucción CREATE TABLE o ALTER TABLE en lugar de una instrucción CREATE DEFAULT), con **Sys. Objects. Type** = D.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Para obtener una lista de las columnas que hereda esta vista, vea [Sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**parent_column_id**|**int**|IDENTIFICADOR de la columna en **parent_object_id** a la que pertenece este valor predeterminado.|  
|**definir**|**nvarchar(max)**|Expresión SQL que define este valor predeterminado.|  
|**is_system_named**|**bit**|1 = El nombre ha sido generado por el sistema.<br /><br /> 0 = El usuario proporcionó el nombre.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve la definición de la restricción DEFAULT que se aplica a la columna `VacationHours` de la tabla `HumanResources.Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT d.definition   
FROM sys.default_constraints AS d  
INNER JOIN sys.columns AS c  
ON d.parent_object_id = c.object_id
AND d.parent_column_id = c.column_id  
WHERE d.parent_object_id = OBJECT_ID(N'HumanResources.Employee', N'U')  
AND c.name = 'VacationHours';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
