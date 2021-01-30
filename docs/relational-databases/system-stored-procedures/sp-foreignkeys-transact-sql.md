---
description: sp_foreignkeys (Transact-SQL)
title: sp_foreignkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_foreignkeys_TSQL
- sp_foreignkeys
dev_langs:
- TSQL
helpviewer_keywords:
- sp_foreignkeys
ms.assetid: 935fe385-19ff-41a4-8d0b-30618966991d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 377ef64c76238e2e840107dbbe6b32dc9ba4c00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189569"
---
# <a name="sp_foreignkeys-transact-sql"></a>sp_foreignkeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve las claves externas que hacen referencia a las claves principales de la tabla en el servidor vinculado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_foreignkeys [ @table_server = ] 'table_server'   
     [ , [ @pktab_name = ] 'pktab_name' ]   
     [ , [ @pktab_schema = ] 'pktab_schema' ]   
     [ , [ @pktab_catalog = ] 'pktab_catalog' ]   
     [ , [ @fktab_name = ] 'fktab_name' ]   
     [ , [ @fktab_schema = ] 'fktab_schema' ]   
     [ , [ @fktab_catalog = ] 'fktab_catalog' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_server = ] 'table_server'` Es el nombre del servidor vinculado del que se va a devolver información de la tabla. *table_server* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @pktab_name = ] 'pktab_name'` Es el nombre de la tabla con una clave principal. *pktab_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @pktab_schema = ] 'pktab_schema'` Es el nombre del esquema con una clave principal. *pktab_schema* es de **tipo sysname y su** valor predeterminado es NULL. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contiene el nombre del propietario.  
  
`[ @pktab_catalog = ] 'pktab_catalog'` Es el nombre del catálogo con una clave principal. *pktab_catalog* es de **tipo sysname y su** valor predeterminado es NULL. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contiene el nombre de la base de datos.  
  
`[ @fktab_name = ] 'fktab_name'` Es el nombre de la tabla con una clave externa. *fktab_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @fktab_schema = ] 'fktab_schema'` Es el nombre del esquema con una clave externa. *fktab_schema* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @fktab_catalog = ] 'fktab_catalog'` Es el nombre del catálogo con una clave externa. *fktab_catalog* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Varios productos DBMS admiten nombres de tres partes para las tablas (_Catálogo_**.** _esquema_**.** _tabla_), que se representa en el conjunto de resultados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**PKTABLE_CAT**|**sysname**|Catálogo de la tabla en que reside la clave principal.|  
|**PKTABLE_SCHEM**|**sysname**|Esquema de la tabla en que reside la clave principal.|  
|**PKTABLE_NAME**|**sysname**|Nombre de la tabla (con la clave principal). Este campo siempre devuelve un valor.|  
|**PKCOLUMN_NAME**|**sysname**|Nombre de la columna o columnas de clave principal para cada columna de la **TABLE_NAME** devuelta. Este campo siempre devuelve un valor.|  
|**FKTABLE_CAT**|**sysname**|Catálogo de la tabla en que reside la clave externa.|  
|**FKTABLE_SCHEM**|**sysname**|Esquema de la tabla en que reside la clave externa.|  
|**FKTABLE_NAME**|**sysname**|Nombre de la tabla (con una clave externa). Este campo siempre devuelve un valor.|  
|**FKCOLUMN_NAME**|**sysname**|Nombre de las columnas de clave externa por cada columna de TABLE_NAME devuelta. Este campo siempre devuelve un valor.|  
|**KEY_SEQ**|**smallint**|Número de secuencia de la columna en una clave principal con varias columnas. Este campo siempre devuelve un valor.|  
|**UPDATE_RULE**|**smallint**|Acción aplicada a la clave externa si la operación de SQL es una actualización. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve 0, 1 o 2 para estas columnas:<br /><br /> 0=CASCADE cambia a clave externa.<br /><br /> 1=NO ACTION cambia si la clave externa está presente.<br /><br /> 2=SET_NULL; establece la clave externa como NULL.|  
|**DELETE_RULE**|**smallint**|Acción aplicada a la clave externa si la operación de SQL es una eliminación. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve 0, 1 o 2 para estas columnas:<br /><br /> 0=CASCADE cambia a clave externa.<br /><br /> 1=NO ACTION cambia si la clave externa está presente.<br /><br /> 2=SET_NULL; establece la clave externa como NULL.|  
|**FK_NAME**|**sysname**|Identificador de la clave externa. Es NULL si no es aplicable al origen de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve el nombre de la restricción FOREIGN KEY.|  
|**PK_NAME**|**sysname**|Identificador de la clave principal. Es NULL si no es aplicable al origen de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve el nombre de la restricción PRIMARY KEY.|  
|**DEFERRABILITY**|**smallint**|Indica si se puede diferir la comprobación de restricciones.|  
  
 En el conjunto de resultados, las columnas FK_NAME y PK_NAME siempre devuelven NULL.  
  
## <a name="remarks"></a>Observaciones  
 **sp_foreignkeys** consulta el conjunto de filas FOREIGN_KEYS de la interfaz **IDBSchemaRowset** del proveedor OLE DB que corresponde a *table_server*. Los parámetros *TABLE_NAME*, *TABLE_SCHEMA*, *TABLE_CATALOG* y *Column* se pasan a esta interfaz para restringir las filas devueltas.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve información de clave externa acerca de la tabla `Department` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] del servidor vinculado `Seattle1`.  
  
```  
EXEC sp_foreignkeys @table_server = N'Seattle1',   
   @pktab_name = N'Department',   
   @pktab_catalog = N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_catalogs &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_primarykeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_tables_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
