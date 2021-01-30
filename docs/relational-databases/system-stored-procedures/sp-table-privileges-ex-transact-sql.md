---
description: sp_table_privileges_ex (Transact-SQL)
title: sp_table_privileges_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_table_privileges_ex
- sp_table_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_table_privileges_ex
ms.assetid: b58d4a07-5c40-4f17-b66e-6d6b17188dda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 41c9548f0c4841b6a9c1845a22c7883cd42ad88b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203682"
---
# <a name="sp_table_privileges_ex-transact-sql"></a>sp_table_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información de privilegios sobre la tabla especificada del servidor vinculado especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_table_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]  
     [ , [@fUsePattern =] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_server = ] 'table_server'` Es el nombre del servidor vinculado del que se va a devolver información. *table_server* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @table_name = ] 'table_name']` Es el nombre de la tabla para la que se va a proporcionar información sobre los privilegios de tabla. *TABLE_NAME* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_schema = ] 'table_schema'` Es el esquema de la tabla. En algunos entornos DBMS es el propietario de la tabla. *TABLE_SCHEMA* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_catalog = ] 'table_catalog'` Es el nombre de la base de datos en la que reside el *TABLE_NAME* especificado. *TABLE_CATALOG* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina si los caracteres "_", "%", "[" y "]" se interpretan como caracteres comodín. Los valores válidos son 0 (coincidencia de patrón desactivada) y 1 (coincidencia de patrón activada). *fUsePattern* es de **bit** y su valor predeterminado es 1.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nombre del calificador de tabla. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla. Este campo puede ser NULL.|  
|**TABLE_SCHEM**|**sysname**|Nombre del propietario de la tabla. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta columna representa el nombre del usuario de la base de datos que creó la tabla. Este campo siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla. Este campo siempre devuelve un valor.|  
|**GRANTOR**|**sysname**|Nombre de usuario de base de datos que ha concedido permisos para este **TABLE_NAME** al **receptor** indicado. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta columna es siempre la misma que la **table_owner**. Este campo siempre devuelve un valor. Además, la columna de GRANTOR puede ser el propietario de la base de datos (**table_owner**) o un usuario al que el propietario de la base de datos haya concedido permiso mediante la cláusula with Grant Option en la instrucción Grant.|  
|**GRANTEE**|**sysname**|Nombre de usuario de base de datos al que se han concedido permisos para este **TABLE_NAME** por el **otorgante** de la lista. Este campo siempre devuelve un valor.|  
|**PRIVILEGIA**|**VARCHAR (** 32 **)**|Uno de los permisos de tabla disponibles. Los permisos de tabla pueden ser uno de los valores siguientes u otros valores que el origen de datos admita al definirse la implementación.<br /><br /> SELECT = **GRANTEE** puede recuperar datos para una o más de las columnas.<br /><br /> INSERT = **GRANTEE** puede proporcionar datos para nuevas filas de una o más de las columnas.<br /><br /> UPDATE = **GRANTEE** puede modificar datos existentes de una o varias columnas.<br /><br /> DELETE = **GRANTEE** puede quitar filas de la tabla.<br /><br /> REFERENCEs = **GRANTEE** puede hacer referencia a una columna de una tabla externa en una relación de clave principal y clave externa. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], las relaciones entre clave principal y clave externa se definen mediante restricciones de tabla.<br /><br /> El ámbito de acción dado a **GRANTEE** por un privilegio de tabla específico depende del origen de datos. Por ejemplo, el permiso UPDATE podría permitir al **receptor** actualizar todas las columnas de una tabla en un origen de datos y solo aquellas columnas para las que el **otorgante** tiene permiso Update en otro origen de datos.|  
|**IS_GRANTABLE**|**VARCHAR (** 3 **)**|Indica si el **receptor** tiene permiso para conceder permisos a otros usuarios. A esto se le suele denominar permiso "conceder por concesión". Puede ser YES, NO o NULL. Un valor desconocido, o NULL, hace referencia a un origen de datos en el que “conceder por concesión” no es aplicable.|  
  
## <a name="remarks"></a>Observaciones  
 Los resultados devueltos se ordenan por **TABLE_QUALIFIER**, **table_owner**, **TABLE_NAME** y **privilegios**.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información de privilegios acerca de las tablas con nombres que comienzan por `Product` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] del servidor vinculado especificado `Seattle1`. ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se supone que es el servidor vinculado).  
  
```  
EXEC sp_table_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Product%',   
   @table_schema = 'Production',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_column_privileges_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-ex-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de consultas distribuidas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)  
  
  
