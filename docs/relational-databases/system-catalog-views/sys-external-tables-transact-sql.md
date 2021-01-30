---
description: sys.external_tables (Transact-SQL)
title: sys.external_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: fac4720c-b679-4ab2-864b-ff7810a9b559
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: caa4ea9fa1ae72760652912b3001f40e7c9e0c09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193972"
---
# <a name="sysexternal_tables-transact-sql"></a>sys.external_tables (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada tabla externa de la base de datos actual.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Para obtener una lista de las columnas que hereda esta vista, vea [Sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).||  
|max_column_id_used|**int**|Se ha usado el ID. de columna máximo en esta tabla.||  
|uses_ansi_nulls|**bit**|La tabla se creó con la opción de base de datos SET ANSI_NULLS establecida en ON.||  
|data_source_id|**int**|IDENTIFICADOR de objeto del origen de datos externo.||  
|file_format_id|**int**|En el caso de las tablas externas a través de un origen de datos externo de HADOOP, es el identificador de objeto del formato de archivo externo.||  
|ubicación|**nvarchar(4000)**|En el caso de las tablas externas a través de un origen de datos externo de HADOOP, esta es la ruta de acceso de los datos externos en HDFS.||  
|reject_type|**tinyint**|En el caso de las tablas externas a través de un origen de datos externo de HADOOP, esta es la forma en que se cuentan las filas rechazadas al consultar los datos externos.|VALOR: el número de filas rechazadas.<br /><br /> PORCENTAJE: el porcentaje de filas rechazadas.|  
|reject_value|**float**|Para tablas externas a través de un origen de datos externo de HADOOP:<br /><br /> Para *reject_type =* valor, es el número de rechazos de filas que se permiten antes de que se produzca un error en la consulta.<br /><br /> En *reject_type* = Percentage, es el porcentaje de rechazos de filas que se permiten antes de que se produzca un error en la consulta.||  
|reject_sample_value|**int**|En *reject_type* = Percentage, es el número de filas que se van a cargar, ya sea correcta o incorrectamente, antes de calcular el porcentaje de filas rechazadas.|NULL si reject_type = valor.|  
|distribution_type|**int**|En el caso de las tablas externas en un SHARD_MAP_MANAGER origen de datos externo, se trata de la distribución de datos de las filas en las tablas base subyacentes.|0-particionado<br /><br /> 1-replicado<br /><br /> 2-Round Robin|  
|distribution_desc|**nvarchar(120)**|En el caso de las tablas externas a través de un origen de datos externo SHARD_MAP_MANAGER, este es el tipo de distribución que se muestra como una cadena.||  
|sharding_column_id|**int**|En el caso de las tablas externas a través de un origen de datos externo SHARD_MAP_MANAGER y una distribución particionada, este es el ID. de columna de la columna que contiene los valores de clave de particionamiento.||  
|remote_schema_name|**sysname**|En el caso de las tablas externas a través de un origen de datos externo SHARD_MAP_MANAGER, este es el esquema en el que la tabla base se encuentra en las bases de datos remotas (si es diferente del esquema en el que se define la tabla externa).||  
|remote_object_name|**sysname**|En el caso de las tablas externas en un SHARD_MAP_MANAGER origen de datos externo, es el nombre de la tabla base en las bases de datos remotas (si es diferente del nombre de la tabla externa).||  
  
## <a name="permissions"></a>Permisos  
 La visibilidad de los metadatos en las vistas de catálogo se limita a los elementos protegibles y que son propiedad de un usuario o sobre los que el usuario tiene algún permiso. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [sys.external_file_formats &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_data_sources &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)  
  
  
