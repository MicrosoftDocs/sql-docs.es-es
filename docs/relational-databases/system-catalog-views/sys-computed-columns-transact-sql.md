---
description: sys.computed_columns (Transact-SQL)
title: sys.computed_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.computed_columns_TSQL
- sys.computed_columns
- computed_columns_TSQL
- computed_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.computed_columns catalog view
ms.assetid: c962c619-e18f-4315-9251-8d9862462299
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0f6f74e61ce938afcacffb3304cf40365cd4dc0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97459901"
---
# <a name="syscomputed_columns-transact-sql"></a>sys.computed_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila para cada columna que se encuentra en **Sys. Columns** , que es una columna calculada.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<Inherited columns>**||La vista **Sys.computed_columns** devuelve todas las columnas de la vista **Sys. Columns** . También devuelve las columnas adicionales descritas a continuación. Para obtener una descripción de las columnas que la vista **Sys.computed_columns** hereda de **Sys. Columns**, vea [Sys. Columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md). El valor de la columna **is_computed** siempre se establece en 1 en la vista **Sys.computed_columns** .|  
|**definir**|**nvarchar(max)**|Texto SQL que define esta columna calculada.|  
|**uses_database_collation**|**bit**|1 = La definición de columna depende de la intercalación predeterminada de la base de datos para su correcta evaluación; de lo contrario, 0. Esta dependencia evita el cambio de la intercalación predeterminada de la base de datos.|  
|**is_persisted**|**bit**|Se almacena la columna calculada.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
