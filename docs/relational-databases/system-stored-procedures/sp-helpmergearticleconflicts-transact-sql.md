---
description: sp_helpmergearticleconflicts (Transact-SQL)
title: sp_helpmergearticleconflicts (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergearticleconflicts
- sp_helpmergearticleconflicts_TSQL
helpviewer_keywords:
- sp_helpmergearticleconflicts
ms.assetid: 4678a2b9-9a5f-4193-a20d-2e11fc896c3a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2af24818f4dd4ee28829847fb912d01f47d5473c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179264"
---
# <a name="sp_helpmergearticleconflicts-transact-sql"></a>sp_helpmergearticleconflicts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve los artículos de la publicación que tienen conflictos. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones o en el suscriptor de la base de datos de suscripciones de mezcla.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergearticleconflicts [ [ @publication = ] 'publication' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publsher_db' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación de combinación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve todos los artículos de la base de datos que tienen conflictos.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artículo**|**sysname**|Nombre del artículo.|  
|**source_owner**|**sysname**|Propietario del objeto de origen.|  
|**source_object**|**nvarchar (386)**|Nombre del objeto de origen.|  
|**conflict_table**|**nvarchar(258)**|Nombre de la tabla que almacena los conflictos de inserción o actualización.|  
|**guidcolname**|**sysname**|Nombre de la propiedad RowGuidCol para el objeto de origen.|  
|**centralized_conflicts**|**int**|Indica si los registros de conflictos se almacenan en el publicador dado.|  
  
 Si el artículo solo tiene conflictos de eliminación y no **conflict_table** filas, el nombre del **conflict_table** en el conjunto de resultados es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergearticleconflicts** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** y del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpmergearticleconflicts**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
