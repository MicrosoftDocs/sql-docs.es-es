---
description: sp_primarykeys (Transact-SQL)
title: sp_primarykeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_primarykeys_TSQL
- sp_primarykeys
dev_langs:
- TSQL
helpviewer_keywords:
- sp_primarykeys
ms.assetid: 0f76dd31-5b7b-4209-9e2e-b9ed5cac164d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 23117c882ee0c2c55e8d29089327e9dd84643878
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199585"
---
# <a name="sp_primarykeys-transact-sql"></a>sp_primarykeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve las columnas de clave principal, una fila por cada columna de clave, para la tabla remota especificada.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_primarykeys [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_server = ] 'table_server'_` Es el nombre del servidor vinculado del que se va a devolver la información de la clave principal. *table_server* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @table_name = ] 'table_name'` Es el nombre de la tabla para la que se va a proporcionar información de la clave principal. *TABLE_NAME* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_schema = ] 'table_schema'` Es el esquema de la tabla. *TABLE_SCHEMA* es de **tipo sysname y su** valor predeterminado es NULL. En el entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], corresponde al propietario de la tabla.  
  
`[ @table_catalog = ] 'table_catalog'` Es el nombre del catálogo en el que reside el *TABLE_NAME* especificado. En el entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], corresponde al nombre de base de datos. *TABLE_CATALOG* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Catálogo de la tabla.|  
|**TABLE_SCHEM**|**sysname**|Esquema de la tabla|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla.|  
|**COLUMN_NAME**|**sysname**|Nombre de la columna.|  
|**KEY_SEQ**|**int**|Número de secuencia de la columna en una clave principal con varias columnas.|  
|**PK_NAME**|**sysname**|Identificador de la clave principal. Devuelve NULL si no es aplicable al origen de datos.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_primarykeys** se ejecuta consultando el conjunto de filas PRIMARY_KEYS de la interfaz **IDBSchemaRowset** del proveedor de OLE DB correspondiente a *table_server*. Los parámetros *TABLE_NAME*, *TABLE_SCHEMA*, *TABLE_CATALOG* y *Column* se pasan a esta interfaz para restringir las filas devueltas.  
  
 **sp_primarykeys** devuelve un conjunto de resultados vacío si el proveedor de OLE DB del servidor vinculado especificado no admite el conjunto de filas PRIMARY_KEYS de la interfaz **IDBSchemaRowset** .  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelven las columnas de clave principal del servidor `LONDON1` para la tabla `HumanResources.JobCandidate` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC sp_primarykeys @table_server = N'LONDON1',   
   @table_name = N'JobCandidate',  
   @table_catalog = N'AdventureWorks2012',   
   @table_schema = N'HumanResources';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de consultas distribuidas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_catalogs &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_foreignkeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_tables_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
