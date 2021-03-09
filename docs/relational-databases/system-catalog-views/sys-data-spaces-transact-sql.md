---
description: sys.data_spaces (Transact-SQL)
title: sys.data_spaces (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- data_spaces
- sys.data_spaces_TSQL
- sys.data_spaces
- data_spaces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.data_spaces catalog view
ms.assetid: f39d55fe-2c0f-472d-a77f-cebc6fea95b5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 570de88dccaf0f7c64f8a9c98d35d085ee8674b0
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464059"
---
# <a name="sysdata_spaces-transact-sql"></a>sys.data_spaces (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una fila por cada espacio de datos. Puede ser un grupo de archivos, un esquema de partición o un grupo de archivos de datos de FILESTREAM.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nombre del espacio de datos, único en la base de datos.|  
|data_space_id|**int**|Número de identificación del espacio de datos, único en la base de datos.|  
|type|**char(2)**|Tipo de espacio de datos:<br /><br /> FG = Grupo de archivos<br /><br /> FD = Grupo de archivos de datos de FILESTREAM<br /><br /> FX = Grupo de archivos de tablas optimizadas en memoria<br /><br /> **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> PS = Esquema de partición|  
|type_desc|**nvarchar(60)**|Descripción del tipo de espacio de datos:<br /><br /> FILESTREAM_DATA_FILEGROUP<br /><br /> MEMORY_OPTIMIZED_DATA_FILEGROUP<br /><br /> **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> PARTITION_SCHEME<br /><br /> ROWS_FILEGROUP |  
|is_default|**bit**|1 = Espacio de datos predeterminado. El espacio de datos predeterminado se usa cuando no se especifica un grupo de archivos o un esquema de partición en una instrucción CREATE TABLE o CREATE INDEX.<br /><br /> 0 = No es el espacio de datos predeterminado.|  
|is_system|**bit**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> 1 = El espacio de datos se usa para los fragmentos de índice de texto completo.<br /><br /> 0 = El espacio de datos no se usa para los fragmentos de índice de texto completo.|  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Espacios de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-spaces-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
