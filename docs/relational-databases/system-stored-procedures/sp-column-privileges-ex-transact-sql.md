---
description: sp_column_privileges_ex (Transact-SQL)
title: sp_column_privileges_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_column_privileges_ex
- sp_column_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges_ex
ms.assetid: 98cb6e58-4007-40fc-b048-449fb2e7e6be
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c4ce393362851e17471829272544df86840153fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174504"
---
# <a name="sp_column_privileges_ex-transact-sql"></a>sp_column_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve privilegios de columna para la tabla especificada del servidor vinculado indicado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_column_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_server = ] 'table_server'` Es el nombre del servidor vinculado del que se va a devolver información. *table_server* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @table_name = ] 'table_name'` Es el nombre de la tabla que contiene la columna especificada. *TABLE_NAME* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_schema = ] 'table_schema'` Es el esquema de la tabla. *TABLE_SCHEMA* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_catalog = ] 'table_catalog'` Es el nombre de la base de datos en la que reside el *TABLE_NAME* especificado. *TABLE_CATALOG* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @column_name = ] 'column_name'` Es el nombre de la columna para la que se va a proporcionar información de privilegios. *column_name* es de **tipo sysname y su** valor predeterminado es null (común).  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 En la siguiente tabla se muestran las columnas del conjunto de resultados. Los resultados devueltos se ordenan por **TABLE_QUALIFIER**, **table_owner**, **TABLE_NAME**, **column_name** y **Privilege**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nombre del calificador de tabla. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla. Este campo puede ser NULL.|  
|**TABLE_SCHEM**|**sysname**|Nombre del propietario de la tabla. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta columna representa el nombre del usuario de la base de datos que creó la tabla. Este campo siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla. Este campo siempre devuelve un valor.|  
|**COLUMN_NAME**|**sysname**|Nombre de columna para cada columna del **TABLE_NAME** devuelto. Este campo siempre devuelve un valor.|  
|**GRANTOR**|**sysname**|Nombre de usuario de base de datos que ha concedido permisos para este **column_name** al **receptor** indicado. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta columna es siempre la misma que la **table_owner**. Este campo siempre devuelve un valor.<br /><br /> La **columna de** GRANTOR puede ser el propietario de la base de datos (**table_owner**) o un usuario al que el propietario de la base de datos haya concedido permisos mediante la cláusula with Grant Option en la instrucción Grant.|  
|**GRANTEE**|**sysname**|Nombre de usuario de base de datos al que se han concedido permisos para este **column_name** por el **otorgante** de la lista. Este campo siempre devuelve un valor.|  
|**PRIVILEGIA**|**VARCHAR (** 32 **)**|Uno de los permisos de columna disponibles. Los permisos de columna pueden ser uno de los valores siguientes (u otros valores compatibles con el origen de datos cuando se define la implementación):<br /><br /> SELECT = **GRANTEE** puede recuperar datos para las columnas.<br /><br /> INSERT = **GRANTEE** puede proporcionar datos para esta columna cuando se inserten nuevas filas (por parte del **receptor**) en la tabla.<br /><br /> UPDATE = **GRANTEE** puede modificar datos existentes en la columna.<br /><br /> REFERENCEs = **GRANTEE** puede hacer referencia a una columna de una tabla externa en una relación de clave principal y clave externa. Las relaciones entre clave principal y clave externa se definen con restricciones de tabla.|  
|**IS_GRANTABLE**|**VARCHAR (** 3 **)**|Indica si se permite al **receptor** conceder permisos a otros usuarios (a menudo se hace referencia al permiso "Grant with Grant"). Puede ser YES, NO o NULL. Un valor desconocido, o NULL, hace referencia a un origen de datos en el que no se aplica “conceder por concesión”.|  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve información de privilegios de columna de la tabla `HumanResources.Department` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] del servidor vinculado `Seattle1`.  
  
```  
EXEC sp_column_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Department',   
   @table_schema = 'HumanResources',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_table_privileges_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-ex-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
