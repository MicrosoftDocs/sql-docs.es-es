---
description: 'Vistas de catálogo de propiedades extendidas: sys.extended_properties'
title: sys.extended_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.extended_properties
- sys.extended_properties_TSQL
- extended_properties
- extended_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.extended_properties catalog view
ms.assetid: 439b7299-dce3-4d26-b1c7-61be5e0df82a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f127b6ee96f95c0f51b4ce41a101348d7fce5c42
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421957"
---
# <a name="extended-properties-catalog-views---sysextended_properties"></a>Vistas de catálogo de propiedades extendidas: sys.extended_properties
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada propiedad extendida de la base de datos actual.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|clase|**tinyint**|Identifica la clase de elemento en el que existe la propiedad. Puede ser uno de los siguientes:<br /><br /> 0 = Base de datos<br /><br /> 1 = Objeto o columna<br /><br /> 2 = Parámetro<br /><br /> 3 = Esquema<br /><br /> 4 = Entidad de seguridad de base de datos<br /><br /> 5 = Ensamblado<br /><br /> 6 = Tipo<br /><br /> 7 = Índice<br /><br /> 8 = columna de tipo de tabla definida por el usuario<br /><br /> 10 = Colección de esquemas XML<br /><br /> 15 = Tipo de mensaje<br /><br /> 16 = Contrato de servicio<br /><br /> 17 = Servicio<br /><br /> 18 = Enlace de servicio remoto<br /><br /> 19 = Ruta<br /><br /> 20 = Espacio de datos (grupo de archivos o esquema de partición)<br /><br /> 21 = Función de partición<br /><br /> 22 = Archivo de base de datos<br /><br /> 27 =Guía de plan|  
|class_desc|**nvarchar(60)**|Descripción de la clase en la que existe la propiedad extendida. Puede ser uno de los siguientes:<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN<br /><br /> PARÁMETRO<br /><br /> SCHEMA<br /><br /> DATABASE_PRINCIPAL<br /><br /> ASSEMBLY<br /><br /> TYPE<br /><br /> INDEX<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> MESSAGE_TYPE<br /><br /> SERVICE_CONTRACT<br /><br /> SERVICE<br /><br /> REMOTE_SERVICE_BINDING<br /><br /> ROUTE<br /><br /> DATASPACE<br /><br /> PARTITION_FUNCTION<br /><br /> DATABASE_FILE<br /><br /> PLAN_GUIDE|  
|major_id|**int**|Identificador del elemento en el que existe la propiedad extendida, interpretado de acuerdo con su clase. Para la mayoría de los elementos, es el identificador aplicable a lo que la clase representa. La interpretación de los identificadores principales no estándar es la siguiente:<br /><br /> Si class es 0, major_id siempre es 0.<br /><br /> Si class es 1, 2 ó 7, major_id es object_id.|  
|minor_id|**int**|Identificador secundario del elemento en el que existe la propiedad extendida, interpretado de acuerdo con su clase. Para la mayoría de los elementos es 0; en los demás casos, el identificador es el siguiente:<br /><br /> Si class = 1, minor_id es column_id si es una columna o 0 si es un objeto.<br /><br /> Si class = 2, minor_id es parameter_id.<br /><br /> Si class = 7, minor_id es index_id.|  
|name|**sysname**|Nombre de la propiedad, único con class, major_id y minor_id.|  
|valor|**sql_variant**|Valor de la propiedad extendida.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vea también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Propiedades extendidas vistas de catálogo &#40;Transact-SQL&#41;](./catalog-views-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
