---
description: sp_helpstats (Transact-SQL)
title: sp_helpstats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpstats
- sp_helpstats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpstats
ms.assetid: 00ab3cfd-2736-4fc0-b1b2-16dd49fb2fe5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c3b0d76b08d8be5aefcbfceee570d893d6ca4659
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198722"
---
# <a name="sp_helpstats-transact-sql"></a>sp_helpstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información estadística acerca de las columnas e índices de la tabla especificada.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] Para obtener información acerca de las estadísticas, consulte las vistas de catálogo [Sys. stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) y [Sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpstats[ @objname = ] 'object_name'   
     [ , [ @results = ] 'value' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @objname = ] 'object_name'` Especifica la tabla en la que se va a proporcionar información de estadísticas. *object_name* es **nvarchar (520)** y no puede ser null. Se puede especificar un nombre de una o dos partes.  
  
`[ @results = ] 'value'` Especifica la extensión de la información que se va a proporcionar. Las entradas válidas son **All** y **stats**. **All** muestra las estadísticas de todos los índices y de las columnas que tienen estadísticas creadas. **Stats** solo muestra las estadísticas no asociadas a un índice. *Value* es de tipo **nvarchar (5)** y su valor predeterminado es stats.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 En la tabla siguiente se describen las columnas del conjunto de resultados.  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**statistics_name**|Nombre de la estadística. Devuelve **sysname** y no puede ser null.|  
|**statistics_keys**|Claves en que se basa la estadística. Devuelve **nvarchar (2078)** y no puede ser null.|  
  
## <a name="remarks"></a>Observaciones  
 Utilice DBCC SHOW_STATISTICS para presentar información detallada de estadística acerca de cualquier índice o estadística en particular. Para obtener más información, vea [DBCC SHOW_STATISTICS &#40;&#41;de Transact-SQL ](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) y [sp_helpindex &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crean estadísticas de una sola columna en todas las columnas posibles de todas las tablas de usuario de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] mediante `sp_createstats`. A continuación, se ejecuta `sp_helpstats` para buscar las estadísticas de resultado creadas en la tabla `Customer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_createstats;  
GO  
EXEC sp_helpstats   
@objname = 'Sales.Customer',  
@results = 'ALL';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `statistics_name               statistics_keys`  
  
 `----------------------------  ----------------`  
  
 `_WA_Sys_00000003_22AA2996     AccountNumber`  
  
 `AK_Customer_AccountNumber     AccountNumber`  
  
 `AK_Customer_rowguid           rowguid`  
  
 `CustomerType                  CustomerType`  
  
 `IX_Customer_TerritoryID       TerritoryID`  
  
 `ModifiedDate                  ModifiedDate`  
  
 `PK_Customer_CustomerID        CustomerID`  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
