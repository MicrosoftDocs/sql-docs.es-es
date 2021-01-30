---
description: sp_helptext (Transact-SQL)
title: sp_helptext (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helptext
- sp_helptext_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helptext
ms.assetid: 24135456-05f0-427c-884b-93cf38dd47a8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04b88a881f2622b1bccfff675978b1d50a87fb51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158946"
---
# <a name="sp_helptext-transact-sql"></a>sp_helptext (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Muestra definiciones de reglas definidas por el usuario, valores predeterminados, procedimientos almacenados de [!INCLUDE[tsql](../../includes/tsql-md.md)] no cifrados, funciones [!INCLUDE[tsql](../../includes/tsql-md.md)] definidas por el usuario, desencadenadores, columnas calculadas, restricciones CHECK, vistas u objetos del sistema, como un procedimiento almacenado del sistema.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helptext [ @objname = ] 'name' [ , [ @columnname = ] computed_column_name ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @objname = ] 'name'` Es el nombre completo o no completo de un objeto de ámbito de esquema definido por el usuario. Se requieren comillas solo si se especifica un nombre de objeto calificado. Si se proporciona un nombre completo, incluido el nombre de la base de datos, el nombre de la base de datos debe ser el de la base de datos actual. El objeto debe estar en la base de datos actual. *Name* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
`[ @columnname = ] 'computed_column_name'` Es el nombre de la columna calculada para la que se va a mostrar información de definición. La tabla que contiene la columna se debe especificar como *nombre*. *column_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Texto**|**nvarchar(255)**|Definición del objeto|  
  
## <a name="remarks"></a>Observaciones  
 sp_helptext muestra la definición que se utiliza para crear un objeto en varias filas. Cada fila contiene 255 caracteres de la definición de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La definición reside en la columna **Definition** de la vista de catálogo [Sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . Las definiciones de los objetos del sistema están visibles públicamente. La definición de objetos de usuario está visible para el propietario del objeto o para los receptores que dispongan de uno de los siguientes permisos: ALTER, CONTROL, TAKE OWNERSHIP o VIEW DEFINITION.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-the-definition-of-a-trigger"></a>A. Mostrar la definición de un desencadenador  
 En el ejemplo siguiente se muestra la definición del desencadenador `dEmployee` en la [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de datos.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helptext 'HumanResources.dEmployee';  
GO  
```  
  
### <a name="b-displaying-the-definition-of-a-computed-column"></a>B. Mostrar la definición de una columna calculada  
 En el ejemplo siguiente se muestra la definición de la columna calculada `TotalDue` de la tabla `SalesOrderHeader` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
sp_helptext @objname = N'AdventureWorks2012.Sales.SalesOrderHeader', @columnname = TotalDue ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Text`  
  
 `---------------------------------------------------------------------`  
  
 `(isnull(([SubTotal]+[TaxAmt])+[Freight],(0)))`  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
