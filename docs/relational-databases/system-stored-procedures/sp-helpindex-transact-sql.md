---
description: sp_helpindex (Transact-SQL)
title: sp_helpindex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpindex_TSQL
- sp_helpindex
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpindex
ms.assetid: c7f73ba0-ec35-4b10-aa5f-f1487e51fbf7
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2d80fa7cdea6801945d10d127bf05e258e5e2fe0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200320"
---
# <a name="sp_helpindex-transact-sql"></a>sp_helpindex (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de los índices de una tabla o vista.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpindex [ @objname = ] 'name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @objname = ] 'name'` Es el nombre completo o no completo de una vista o tabla definida por el usuario. Solo son necesarias las comillas si se especifica una tabla o nombre de vista calificada. Si se proporciona un nombre completo, incluido el nombre de la base de datos, el nombre de la base de datos debe ser el de la base de datos actual. *Name* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**index_name**|**sysname**|Nombre del índice.|  
|**index_description**|**VARCHAR (210)**|Descripción del índice incluido el grupo de archivos en el que está ubicado.|  
|**index_keys**|**nvarchar (2078)**|Columnas de tabla o vista sobre las que se genera el índice.|  
  
 Aparecerá listada una columna de índice descendente en el conjunto de resultados con un signo menos (-) tras el nombre; la columna predeterminada de índice ascendente aparecerá listada solamente por su nombre.  
  
## <a name="remarks"></a>Observaciones  
 Si los índices se han establecido mediante la opción NORECOMPUTE de UPDATE STATISTICs, esa información se incluye en la columna **index_description** .  
  
 **sp_helpindex** solo expone columnas de índice ordenable; por lo tanto, no expone información sobre índices XML o índices espaciales.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo presenta los tipos de índices de la tabla `Customer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helpindex N'Sales.Customer';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)  
  
  
