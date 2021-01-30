---
description: sp_refreshview (Transact-SQL)
title: sp_refreshview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refreshview
- sp_refreshview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refreshview
ms.assetid: 9ce1d07c-ee66-4a83-8c73-cd2cc104dd08
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da179448924381de2dddbf5613194c30df1f01bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201264"
---
# <a name="sp_refreshview-transact-sql"></a>sp_refreshview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza los metadatos de la vista no enlazada a esquema especificada. Los metadatos persistentes de una vista pueden quedar desfasados debido a los cambios realizados en los objetos subyacentes de los que depende la vista.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_refreshview [ @viewname = ] 'viewname'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @viewname = ] 'viewname'` Es el nombre de la vista. *ViewName* es de tipo **nvarchar** y no tiene ningún valor predeterminado. *ViewName* puede ser un identificador de varias partes, pero solo puede hacer referencia a vistas en la base de datos actual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o un número distinto de cero (error)  
  
## <a name="remarks"></a>Observaciones  
 Si no se crea una vista con SCHEMABINDING, **sp_refreshview** se debe ejecutar cuando se realicen cambios en los objetos subyacentes de la vista que afecten a la definición de la vista. De lo contrario, la vista podría producir resultados inesperados en las consultas.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER en la vista y el permiso REFERENCES en los tipos definidos por el usuario Common Language Runtime (CLR) y las colecciones de esquemas XML a los que hacen referencia las columnas de la vista.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-updating-the-metadata-of-a-view"></a>A. Actualizar los metadatos de una vista  
 En el siguiente ejemplo se actualizan los metadatos de la vista `Sales.vIndividualCustomer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sp_refreshview N'Sales.vIndividualCustomer';  
```  
  
### <a name="b-creating-a-script-that-updates-all-views-that-have-dependencies-on-a-changed-object"></a>B. Crear un script que actualiza todas las vistas que tienen dependencias en un objeto modificado  
 Suponga que la tabla `Person.Person` se modificó de una forma que afecte a la definición de todas las vistas que se creen en ella. En el siguiente ejemplo se crea un script que actualiza los metadatos de todas las vistas que tienen una dependencia en la tabla `Person.Person`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT 'EXEC sp_refreshview ''' + name + ''''   
FROM sys.objects AS so   
INNER JOIN sys.sql_expression_dependencies AS sed   
    ON so.object_id = sed.referencing_id   
WHERE so.type = 'V' AND sed.referenced_id = OBJECT_ID('Person.Person');  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_refreshsqlmodule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md)  
  
  
