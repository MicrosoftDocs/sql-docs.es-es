---
description: sp_depends (Transact-SQL)
title: sp_depends (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_depends
- sp_depends_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_depends
ms.assetid: d9934590-c6ae-4936-91c3-146055ef2c57
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6251c556475165504075ae60a84b6d4f28766aef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206024"
---
# <a name="sp_depends-transact-sql"></a>sp_depends (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información acerca de las dependencias de los objetos de la base de datos, tales como las vistas y procedimientos que dependen de una tabla o de una vista, y las tablas y vistas de las que depende la vista o el procedimiento. Las referencias a objetos que no se encuentran en la base de datos actual no se notifican.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilice [Sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md) y [Sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_depends [ @objname = ] '<object>'   
  
<object> ::=  
{  
    [ database_name. [ schema_name ] . | schema_name.  
    object_name  
}  
```  
  
## <a name="arguments"></a>Argumentos  
 *database_name*  
 Es el nombre de la base de datos.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece el objeto.  
  
 *object_name*  
 Es el objeto de base de datos cuyas dependencias se van a examinar. El objeto puede ser una tabla, una vista, un procedimiento almacenado, una función definida por el usuario o un desencadenador. o *bject_name* es **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **sp_depends** muestra dos conjuntos de resultados.  
  
 En el siguiente conjunto de resultados se muestran los objetos de los que *\<object>* depende.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar (257** **)**|Nombre del elemento que tiene una dependencia.|  
|**type**|**nvarchar (16)**|Tipo del elemento.|  
|**Actualice**|**nvarchar (7)**|Indica si el elemento está actualizado.|  
|**seleccionadas**|**nvarchar (8)**|Indica si el elemento se utiliza en una instrucción SELECT.|  
|**column**|**sysname**|Columna o parámetro con el que existe la dependencia.|  
  
 En el siguiente conjunto de resultados se muestran los objetos de los que dependen *\<object>* .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar (257** **)**|Nombre del elemento que tiene una dependencia.|  
|**type**|**nvarchar (16)**|Tipo del elemento.|  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-dependencies-on-a-table"></a>A. Enumerar dependencias en una tabla.  
 El ejemplo siguiente enumera los objetos de base de datos que dependen de la tabla `Sales.Customer` en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Se especifican los nombres de esquema y de tabla.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_depends @objname = N'Sales.Customer' ;  
```  
  
### <a name="b-listing-dependencies-on-a-trigger"></a>B. Enumerar dependencias en un desencadenador.  
 En el ejemplo siguiente se enumeran los objetos de base de datos de los que depende el desencadenador `iWorkOrder`.  
  
```  
EXEC sp_depends @objname = N'AdventureWorks2012.Production.iWorkOrder' ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.sql_dependencies &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)  
  
  
