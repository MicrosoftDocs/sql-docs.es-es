---
description: sp_tables_ex (Transact-SQL)
title: sp_tables_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tables_ex
- sp_tables_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables_ex
ms.assetid: 33755c33-7e1e-4ef7-af14-a9cebb1e2ed4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0126cfd14adde25d88d6990a5d7e78c2141ea21e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182762"
---
# <a name="sp_tables_ex-transact-sql"></a>sp_tables_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información de tabla acerca de las tablas del servidor vinculado especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_tables_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]  
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @table_type = ] 'table_type' ]   
     [ , [@fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_server = ] 'table_server'` Es el nombre del servidor vinculado del que se va a devolver información de la tabla. *table_server* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
``[ , [ @table_name = ] 'table_name']`` Es el nombre de la tabla para la que se va a devolver información de tipo de datos. *TABLE_NAME* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_schema = ] 'table_schema']` Es el esquema de la tabla. *TABLE_SCHEMA* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_catalog = ] 'table_catalog'` Es el nombre de la base de datos en la que reside el *TABLE_NAME* especificado. *TABLE_CATALOG* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_type = ] 'table_type'` Es el tipo de la tabla que se va a devolver. *TABLE_TYPE* es de **tipo sysname, su** valor predeterminado es NULL y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**ALIAS**|Nombre de un alias.|  
|**GLOBAL TEMPORARY**|Nombre de una tabla temporal disponible en todo el sistema.|  
|**LOCAL TEMPORARY**|Nombre de una tabla temporal disponible solo para el trabajo actual.|  
|**SYNONYM**|Nombre de un sinónimo.|  
|**TABLA DEL SISTEMA**|Nombre de una tabla del sistema.|  
|**VISTA DEL SISTEMA**|Nombre de una vista del sistema.|  
|**TABLE**|Nombre de una tabla de usuario.|  
|**VIEW**|Nombre de una vista.|  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina si los caracteres **_**, **%** , **[** y **]** se interpretan como caracteres comodín. Los valores válidos son 0 (coincidencia de patrón desactivada) y 1 (coincidencia de patrón activada). *fUsePattern* es de **bit** y su valor predeterminado es 1.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nombre del calificador de tabla. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En otros productos, representa el nombre del servidor del entorno de base de datos de la tabla. Este campo puede ser NULL.|  
|**TABLE_SCHEM**|**sysname**|Nombre del propietario de la tabla. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta columna representa el nombre del usuario de la base de datos que creó la tabla. Este campo siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla. Este campo siempre devuelve un valor.|  
|**TABLE_TYPE**|**varchar(32)**|Tabla, tabla del sistema o vista.|  
|**COMENTARIOS**|**VARCHAR (254)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no devuelve ningún valor para esta columna.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_tables_ex** se ejecuta consultando el conjunto de filas tables de la interfaz **IDBSchemaRowset** del proveedor de OLE DB correspondiente a *table_server*. Los parámetros *TABLE_NAME*, *TABLE_SCHEMA*, *TABLE_CATALOG* y *Column* se pasan a esta interfaz para restringir las filas devueltas.  
  
 **sp_tables_ex** devuelve un conjunto de resultados vacío si el proveedor de OLE DB del servidor vinculado especificado no admite el conjunto de filas tables de la interfaz **IDBSchemaRowset** .  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve información sobre las tablas contenidas en el esquema `HumanResources` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] del servidor vinculado `LONDON2`.  
  
```  
EXEC sp_tables_ex @table_server = 'LONDON2',   
@table_catalog = 'AdventureWorks2012',   
@table_schema = 'HumanResources',   
@table_type = 'TABLE';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de consultas distribuidas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_catalogs &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_columns_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-columns-ex-transact-sql.md)   
 [sp_column_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_foreignkeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_table_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
