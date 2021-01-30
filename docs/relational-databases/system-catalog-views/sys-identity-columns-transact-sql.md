---
description: sys.identity_columns (Transact-SQL)
title: sys.identity_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- identity_columns
- sys.identity_columns
- sys.identity_columns_TSQL
- identity_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.identity_columns catalog view
ms.assetid: 97ee01e6-9c9e-4fd9-884b-68b4084669d5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3c375ef30ef9c5a3c76b61bb2eb06a4aa3bb611a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193830"
---
# <a name="sysidentity_columns-transact-sql"></a>sys.identity_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada columna que es una columna de identidad.  
  
 La vista **Sys.identity_columns** hereda filas de la vista **Sys. Columns** . La vista **Sys.identity_columns** devuelve las columnas de la **vista sys. Columns** , además de las columnas **seed_value**, **increment_value**, **last_value** y **is_not_for_replication** . Para obtener más información, vea [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<columns inherited from sys.columns>**||La vista **Sys.identity_columns** devuelve todas las columnas de la vista **Sys. Columns** . También devuelve las columnas adicionales descritas a continuación. Para obtener una descripción de las columnas que la vista **Sys.identity_columns** hereda de **Sys. Columns**, vea [Sys. Columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).|  
|**seed_value**|**sql_variant**|Valor de inicialización de esta columna de identidad. El tipo de datos del valor de inicialización es el mismo que el de la propia columna.|  
|**increment_value**|**sql_variant**|Valor de incremento de esta columna de identidad. El tipo de datos del valor de inicialización es el mismo que el de la propia columna.|  
|**last_value**|**sql_variant**|Último valor generado para esta columna de identidad. El tipo de datos del valor de inicialización es el mismo que el de la propia columna.|  
|**is_not_for_replication**|**bit**|La columna de identidad se declara NOT FOR REPLICATION. **Nota:** Esta columna no se aplica a Azure Synapse Analytics.|  
  
> [!NOTE]  
>  Para crear un número que se incremente automáticamente y que se pueda usar en varias tablas, o que se pueda llamar desde las aplicaciones sin hacer referencia a ninguna tabla, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
