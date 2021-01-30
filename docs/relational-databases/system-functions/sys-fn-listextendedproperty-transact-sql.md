---
description: sys.fn_listextendedproperty (Transact-SQL)
title: sys.fn_listextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_listextendedproperty
- fn_listextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_listextendedproperty function
- displaying extended properties
- database extended properties [SQL Server]
- viewing extended properties
- column extended properties [SQL Server]
- sys.fn_listextendedproperties function
- database objects [SQL Server], extended properties
- extended properties [SQL Server], columns
- table extended properties [SQL Server]
ms.assetid: 59bbb91f-a277-4a35-803e-dcb91e847a49
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8b60e2fabcccd2040c3dceb1307b7f7e7907bda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201935"
---
# <a name="sysfn_listextendedproperty-transact-sql"></a>sys.fn_listextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve valores de propiedad extendidos de objetos de bases de datos.  
 
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_listextendedproperty (   
    { default | 'property_name' | NULL }   
  , { default | 'level0_object_type' | NULL }   
  , { default | 'level0_object_name' | NULL }   
  , { default | 'level1_object_type' | NULL }   
  , { default | 'level1_object_name' | NULL }   
  , { default | 'level2_object_type' | NULL }   
  , { default | 'level2_object_name' | NULL }   
  )   
```  
  
## <a name="arguments"></a>Argumentos  
 {valor predeterminado | '*property_name*' | ACEPTA  
 Es el nombre de la propiedad. *property_name* es de **tipo sysname**. Las entradas válidas son default, NULL o un nombre de propiedad.  
  
 {valor predeterminado | '*level0_object_type*' | ACEPTA  
 Es el usuario o el tipo definido por el usuario. *level0_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, TRIGGER, TYPE, USER y NULL.  
  
> [!IMPORTANT]  
>  USER y TYPE como tipos de nivel 0 se quitarán en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan. En lugar de USER, use SCHEMA como tipo de nivel 0. Para TYPE, use SCHEMA como tipo de nivel 0 y TYPE como tipo de nivel 1.  
  
 {valor predeterminado | '*level0_object_name*' | ACEPTA  
 Nombre del tipo de objeto de nivel 0 especificado. *level0_object_name* es de **tipo sysname y su** valor predeterminado es NULL. Las entradas válidas son default, NULL o un nombre de objeto.  
  
 {valor predeterminado | '*level1_object_type*' | ACEPTA  
 Tipo de objeto de nivel 1. *level1_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE, TYPE, VIEW, XML SCHEMA COLLECTION y NULL.  
  
> [!NOTE]  
>  El valor predeterminado se asigna a NULL y default al tipo de objeto DEFAULT.  
  
 {valor predeterminado | '*level1_object_name*' | ACEPTA  
 Nombre del tipo de objeto de nivel 1 especificado. *level1_object_name* es de **tipo sysname y su** valor predeterminado es NULL. Las entradas válidas son default, NULL o un nombre de objeto.  
  
 {valor predeterminado | '*level2_object_type*' | ACEPTA  
 Es el tipo de objeto de nivel 2. *level2_object_type* es de tipo **VARCHAR (128)** y su valor predeterminado es NULL. Las entradas válidas son DEFAULT, default (se asigna a NULL) y NULL. Las entradas válidas para *level2_object_type* son Column, Constraint, Event Notification, index, Parameter, Trigger y null.  
  
 {valor predeterminado | '*level2_object_name*' | ACEPTA  
 Es el nombre del tipo de objeto de nivel 2 especificado. *level2_object_name* es de **tipo sysname y su** valor predeterminado es NULL. Las entradas válidas son default, NULL o un nombre de objeto.  
  
## <a name="tables-returned"></a>Tablas devueltas  
 Este es el formato de las tablas que devuelve fn_listextendedproperty.  
  
|Nombre de la columna|Tipo de datos|  
|-----------------|---------------|  
|objtype|**sysname**|  
|objname|**sysname**|  
|name|**sysname**|  
|value|**sql_variant**|  
  
 Si la tabla devuelta está vacía, el objeto no tiene propiedades extendidas o el usuario no tiene permisos para mostrar las propiedades extendidas del objeto. Cuando se devuelven propiedades extendidas de la propia base de datos, el valor de las columnas objtype y objname será NULL.  
  
## <a name="remarks"></a>Observaciones  
 Si el valor de *property_name* es null o default, fn_listextendedproperty devuelve todas las propiedades del objeto especificado.  
  
 Cuando se especifica el tipo de objeto y el valor del nombre de objeto correspondiente es NULL o el valor predeterminado, fn_listextendedproperty devuelve todas las propiedades extendidas de todos los objetos del tipo especificado.  
  
 Los objetos se distinguen en función de sus niveles, siendo el nivel 0 el más alto y el nivel 2 el más bajo. Si se especifica el nombre y el tipo de un objeto de nivel inferior (nivel 1 ó 2), se deben proporcionar valores distintos de NULL o default para el nombre y el tipo del objeto primario. En caso contrario, la función devuelve un conjunto de resultados vacío.  
  
 **objName** se corrige como Latin1_General_CI_AI. Sin embargo, puede solucionar este problema invalidando la intercalación en la comparación.  
  
```  
SELECT o.[object_id] AS 'table_id', o.[name] 'table_name',  
0 AS 'column_order', NULL AS 'column_name', NULL AS 'column_datatype',  
NULL AS 'column_length', Cast(e.value AS varchar(500)) AS 'column_description'  
FROM AdventureWorks.sys.objects AS o  
LEFT JOIN sys.fn_listextendedproperty(N'MS_Description', N'user',N'HumanResources',N'table', N'Employee', null, default) AS e  
    ON o.name = e.objname COLLATE SQL_Latin1_General_CP1_CI_AS  
WHERE o.name = 'Employee';  
```  
  
## <a name="permissions"></a>Permisos  
 Los permisos necesarios para mostrar las propiedades extendidas de los objetos varían en función del tipo de objeto.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-extended-properties-on-a-database"></a>A. Mostrar las propiedades extendidas de una base de datos  
 En el ejemplo siguiente se muestran todas las propiedades extendidas establecidas en el propio objeto de base de datos.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty(default, default, default, default, default, default, default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype    objname     name            value`  
  
 `---------  ---------   -----------     ----------------------------`  
  
 `NULL       NULL        MS_Description  AdventureWorks2008 Sample OLTP Database`  
  
 `(1 row(s) affected)`  
  
### <a name="b-displaying-extended-properties-on-all-columns-in-a-table"></a>B. Mostrar las propiedades extendidas de todas las columnas de una tabla  
 En el ejemplo siguiente se enumeran las propiedades extendidas de las columnas de la `ScrapReason` tabla. que se incluye en el esquema `Production`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Production', 'table', 'ScrapReason', 'column', default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype objname      name            value`  
  
 `------- -----------  -------------   ------------------------`  
  
 `COLUMN ScrapReasonID MS_Description  Primary key for ScrapReason records.`  
  
 `COLUMN Name          MS_Description  Failure description.`  
  
 `COLUMN ModifiedDate  MS_Description  Date the record was last updated.`  
  
 `(3 row(s) affected)`  
  
### <a name="c-displaying-extended-properties-on-all-tables-in-a-schema"></a>C. Mostrar las propiedades extendidas de todas las tablas de un esquema  
 En el ejemplo siguiente se enumeran las propiedades extendidas de todas las tablas contenidas en el `Sales` esquema.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Sales', 'table', default, NULL, NULL);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_addextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
