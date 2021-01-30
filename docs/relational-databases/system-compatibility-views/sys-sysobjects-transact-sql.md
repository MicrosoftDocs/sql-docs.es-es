---
description: sys.sysobjects (Transact-SQL)
title: Objetos sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysobjects_TSQL
- sysobjects
- sysobjects_TSQL
- sys.sysobjects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysobjects compatibility view
- sysobjects system table
ms.assetid: 44fdc387-67b0-4139-8bf5-ed26cf640cd1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 36c8f1ad3d1d90bedd5a42f93add11a8dfdb2770
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158517"
---
# <a name="syssysobjects-transact-sql"></a>sys.sysobjects (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una fila por cada objeto creado en una base de datos, como restricciones, valores predeterminados, registros, reglas y procedimientos almacenados.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nombre del objeto|  
|id|**int**|Número de identificación del objeto|  
|xtype|**char(2)**|Tipo de objeto. Puede ser uno de los siguientes tipos de objeto:<br /><br /> AF = Función de agregado (CLR)<br /><br /> C = restricción CHECK<br /><br /> D = Valor predeterminado o restricción DEFAULT<br /><br /> F = Restricción FOREIGN KEY<br /><br /> L = Registro<br /><br /> FN = Función escalar<br /><br /> FS = Función escalar del ensamblado (CLR)<br /><br /> FT = Función con valores de tabla de ensamblado (CLR)<br /><br /> IF = Función de tabla en línea<br /><br /> IT = tabla interna<br /><br /> P = Procedimiento almacenado<br /><br /> PC = Procedimiento almacenado del ensamblado (CLR)<br /><br /> PK = Restricción PRIMARY KEY (de tipo K)<br /><br /> RF = Procedimiento almacenado de filtro de replicación<br /><br /> S = Tabla del sistema<br /><br /> SN = Sinónimo<br /><br /> SQ = Cola de servicio<br /><br /> TA = Desencadenador DML del ensamblado (CLR)<br /><br /> TF = Función de tabla<br /><br /> TR = desencadenador DML SQL<br /><br /> TT = Tipo de tabla<br /><br /> U = Tabla de usuario<br /><br /> UQ = Restricción UNIQUE (de tipo K)<br /><br /> V = Vista<br /><br /> X = Procedimiento almacenado extendido|  
|uid|**smallint**|Identificador de esquema del propietario del objeto. Para bases de datos actualizadas a partir de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el identificador de esquema es igual que el identificador de usuario del propietario. Produce un desbordamiento o devuelve NULL si el número de usuarios y roles es superior a 32.767.<br /><br /> Importante: Si usa cualquiera de las siguientes instrucciones DDL, debe utilizar la vista de catálogo **\* Sys. Objects en lugar de sys.sysobjetos. \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)<br /><br /> CREAR &#124; ALTER &#124; DROP USER<br /><br /> CREAR &#124; ALTER &#124; DROP ROLE<br /><br /> CREAR &#124; MODIFICAR &#124; QUITAR ROL DE APLICACIÓN<br /><br /> CREATE SCHEMA<br /><br /> ALTER AUTHORIZATION ON OBJECT|  
|info|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|base_schema_ver|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|replinfo|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|parent_obj|**int**|Número de identificación del objeto primario. Por ejemplo, el identificador de tabla si es un desencadenador o una restricción.|  
|crdate|**datetime**|Fecha de creación del objeto.|  
|ftcatid|**smallint**|Identificador del catálogo de texto completo de todas las tablas de usuario registradas para la indización de texto completo y 0 para todas las tablas de usuario no registradas.|  
|schema_ver|**int**|Número de versión que se incrementa cada vez que cambia el esquema de una tabla. Siempre devuelve 0.|  
|stats_schema_ver|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|tipo|**char(2)**|Tipo de objeto. Puede ser uno de los siguientes valores:<br /><br /> AF = Función de agregado (CLR)<br /><br /> C = restricción CHECK<br /><br /> D = Valor predeterminado o restricción DEFAULT<br /><br /> F = Restricción FOREIGN KEY<br /><br /> FN = Función escalar<br /><br /> FS = Función escalar del ensamblado (CLR)<br /><br /> FT = Función con valores de tabla de ensamblado (CLR) IF = Función de tabla en línea<br /><br /> IT = Tabla interna<br /><br /> K = Restricción PRIMARY KEY o UNIQUE<br /><br /> L = Registro<br /><br /> P = Procedimiento almacenado<br /><br /> PC = Procedimiento almacenado del ensamblado (CLR)<br /><br /> R = Regla<br /><br /> RF = Procedimiento almacenado de filtro de replicación<br /><br /> S = Tabla del sistema<br /><br /> SN = Sinónimo<br /><br /> SQ = Cola de servicio<br /><br /> TA = Desencadenador DML del ensamblado (CLR)<br /><br /> TF = Función de tabla<br /><br /> TR = desencadenador DML SQL<br /><br /> TT = Tipo de tabla<br /><br /> U = Tabla de usuario<br /><br /> V = Vista<br /><br /> X = Procedimiento almacenado extendido|  
|userstat|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|sysstat|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|indexdel|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|refdate|**datetime**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|version|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|deltrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|instrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|updtrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|seltrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|category|**int**|Utilizado para publicación, restricciones e identidad.|  
|caché|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
