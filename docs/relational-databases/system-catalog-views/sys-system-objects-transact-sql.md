---
description: sys.system_objects (Transact-SQL)
title: sys.system_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.system_objects
- system_objects
- system_objects_TSQL
- sys.system_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_objects catalog view
ms.assetid: 069e9045-97f2-4463-8e8f-c73855f3ea0a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3023fdedcac48278f34fc7759cdcc013c0a6dea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189335"
---
# <a name="syssystem_objects-transact-sql"></a>sys.system_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila para todos los objetos del sistema del ámbito del esquema que se incluyen con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Todos los objetos del sistema están incluidos en esquemas denominados sys o INFORMATION_SCHEMA.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nombre de objeto.|  
|object_id|**int**|Número de identificación del objeto. Es único en una base de datos.|  
|principal_id|**int**|Identificador del propietario individual si es diferente del propietario del esquema. De forma predeterminada, los objetos contenidos en el esquema pertenecen al propietario del esquema. No obstante, se puede especificar otro propietario mediante la instrucción ALTER AUTHORIZATION para cambiar la propiedad.<br /><br /> Es NULL si no existe otro propietario individual.<br /><br /> Es NULL si el tipo de objeto es uno de los siguientes:<br /><br /> C = restricción CHECK<br /><br /> D = DEFAULT (restricción o independiente)<br /><br /> F = Restricción FOREIGN KEY<br /><br /> PK = Restricción PRIMARY KEY<br /><br /> R = Regla (estilo antiguo, independiente)<br /><br /> TA = Desencadenador de ensamblado (CLR)<br /><br /> TR = Desencadenador SQL<br /><br /> UQ = Restricción UNIQUE|  
|schema_id|**int**|IDENTIFICADOR del esquema en el que se encuentra el objeto.<br /><br /> Para todos los objetos del sistema en el ámbito del esquema que se incluyen con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este valor siempre estará en (schema_id('sys'), schema_id('INFORMATION_SCHEMA')).|  
|parent_object_id|**int**|Identificador del objeto al que pertenece este objeto.<br /><br /> 0 = No es un objeto secundario.|  
|tipo|**char(2)**|Tipo de objeto:<br /><br /> AF = Función de agregado (CLR)<br /><br /> C = restricción CHECK<br /><br /> D = DEFAULT (restricción o independiente)<br /><br /> F = Restricción FOREIGN KEY<br /><br /> FN = Función escalar de SQL<br /><br /> FS = Función escalar del ensamblado (CLR)<br /><br /> FT = Función con valores de tabla de ensamblado (CLR)<br /><br /> IF = Función SQL insertada con valores de tabla<br /><br /> IT = tabla interna<br /><br /> P = Procedimiento almacenado de SQL<br /><br /> PC = Procedimiento almacenado del ensamblado (CLR)<br /><br /> PG = Guía de plan<br /><br /> PK = Restricción PRIMARY KEY<br /><br /> R = Regla (estilo antiguo, independiente)<br /><br /> RF = Procedimiento de filtro de replicación<br /><br /> S = Tabla base del sistema<br /><br /> SN = Sinónimo<br /><br /> SQ = Cola de servicio<br /><br /> TA = Desencadenador DML del ensamblado (CLR)<br /><br /> TF = Función con valores de tabla SQL<br /><br /> TR = Desencadenador DML de SQL<br /><br /> TT = Tipo de tabla<br /><br /> U = Tabla (definida por el usuario)<br /><br /> UQ = Restricción UNIQUE<br /><br /> V = Vista<br /><br /> X = Procedimiento almacenado extendido|  
|type_desc|**nvarchar(60)**|Descripción del tipo de objeto. AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> DEFAULT_CONSTRAINT<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> INTERNAL_TABLE<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> RULE<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> SYSTEM_TABLE<br /><br /> SYNONYM<br /><br /> SERVICE_QUEUE<br /><br /> CLR_TRIGGER<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> TABLE_TYPE<br /><br /> USER_TABLE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> VIEW<br /><br /> EXTENDED_STORED_PROCEDURE|  
|create_date|**datetime**|Fecha de creación del objeto.|  
|modify_date|**datetime**|Fecha en que se modificó el objeto por última vez con una instrucción ALTER. Si el objeto es una tabla o una vista, modify_date también cambia cuando se crea o modifica un índice clúster en la tabla o la vista.|  
|is_ms_shipped|**bit**|Un componente interno de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea el objeto.|  
|is_published|**bit**|El objeto se publica.|  
|is_schema_published|**bit**|Solo se ha publicado el esquema del objeto.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md) (Vistas de catálogo de objetos [Transact-SQL])  
  
  
