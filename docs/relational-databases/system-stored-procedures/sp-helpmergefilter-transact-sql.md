---
description: sp_helpmergefilter (Transact-SQL)
title: sp_helpmergefilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergefilter
- sp_helpmergefilter_TSQL
helpviewer_keywords:
- sp_helpmergefilter
ms.assetid: f133a094-0009-4771-b93b-e86a5c01e40b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 862f23713da4bcd611310149b509e34658766006
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179242"
---
# <a name="sp_helpmergefilter-transact-sql"></a>sp_helpmergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los filtros de mezcla. Este procedimiento almacenado se ejecuta en el publicador de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergefilter [ @publication= ] 'publication'   
    [ , [ @article= ] 'article']  
    [ , [ @filtername= ] 'filtername']  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo. *article* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve los nombres de todos los artículos.  
  
`[ @filtername = ] 'filtername'` Es el nombre del filtro sobre el que se va a devolver información. *filtername* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información acerca de todos los filtros definidos en el artículo o la publicación.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**join_filterid**|**int**|Id. del filtro de combinación.|  
|**FilterName**|**sysname**|Nombre del filtro.|  
|**join article name**|**sysname**|Nombre del artículo de combinación.|  
|**join_filterclause**|**nvarchar (2000)**|Cláusula de filtro que califica la combinación.|  
|**join_unique_key**|**int**|Indica si la combinación se hace sobre una clave exclusiva.|  
|**base table owner**|**sysname**|Nombre del propietario de la tabla base.|  
|**base table name**|**sysname**|Nombre de la tabla base.|  
|**join table owner**|**sysname**|Nombre del propietario de la tabla que se combina con la tabla base.|  
|**join table name**|**sysname**|Nombre de la tabla que se combina con la tabla base.|  
|**article name**|**sysname**|Nombre del artículo de la tabla que se combina con la tabla base.|  
|**filter_type**|**tinyint**|Tipo de filtro de mezcla, que puede ser uno de los siguientes:<br /><br /> **1** = solo filtro de combinación<br /><br /> **2** = relación de registros lógicos<br /><br /> **3** = ambos|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergefilter** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** y del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpmergefilter**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_changemergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
