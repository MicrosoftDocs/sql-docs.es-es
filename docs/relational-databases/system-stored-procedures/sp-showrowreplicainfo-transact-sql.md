---
description: sp_showrowreplicainfo (Transact-SQL)
title: sp_showrowreplicainfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_showrowreplicainfo_TSQL
- sp_showrowreplicainfo
helpviewer_keywords:
- sp_showrowreplicainfo
ms.assetid: 6a9dbc1a-e1e1-40c4-97cb-8164a2288f76
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f2a66e8e6123adef864ca2beadef31cd4a9cbffe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193487"
---
# <a name="sp_showrowreplicainfo-transact-sql"></a>sp_showrowreplicainfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información acerca de una fila en una tabla que se utiliza como un artículo en la replicación de mezcla. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_showrowreplicainfo [ [ @ownername = ] 'ownername' ]  
    [ , [ @tablename =] 'tablename' ]   
        , [ @rowguid =] rowguid   
    [ , [ @show = ] 'show' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @ownername = ] 'ownername'` Es el nombre del propietario de la tabla. *nombrepropietario* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro resulta útil para diferenciar las tablas en caso de que la base de datos contenga varias tablas con el mismo nombre pero con propietarios distintos.  
  
`[ @tablename = ] 'tablename'` Es el nombre de la tabla que contiene la fila para la que se devuelve la información. *TableName* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @rowguid = ] rowguid` Es el identificador único de la fila. *ROWGUID* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado.  
  
`[ @show = ] 'show'` Determina la cantidad de información que se va a devolver en el conjunto de resultados. *Show* es de tipo **nvarchar (20)** y su valor predeterminado es Both. Si es **Row**, solo se devuelve información de la versión de fila. Si son **columnas**, solo se devuelve información de la versión de la columna. Si es, se devuelve la **información de fila** y de columna.  
  
## <a name="result-sets-for-row-information"></a>Conjuntos de resultados para información de fila  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**server_name**|**sysname**|Nombre del servidor que hospeda la base de datos que realizó la entrada de versión de fila.|  
|**db_name**|**sysname**|Nombre de la base de datos que realizó esta entrada.|  
|**db_nickname**|**binario (6)**|Alias de la base de datos que realizó esta entrada.|  
|**version**|**int**|Versión de la entrada.|  
|**current_state**|**nvarchar (9)**|Devuelve información sobre el estado actual de la fila.<br /><br /> los datos de la fila **y** representan el estado actual de la fila.<br /><br /> los datos de la fila **n** no representan el estado actual de la fila.<br /><br /> **\<n/a>** -No es aplicable.<br /><br /> **\<unknown>** -No se puede determinar el estado actual.|  
|**rowversion_table**|**NCHAR (17)**|Indica si las versiones de fila se almacenan en la tabla [MSmerge_contents](../../relational-databases/system-tables/msmerge-contents-transact-sql.md) o en la tabla [MSmerge_tombstone](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md) .|  
|**comment**|**nvarchar(255)**|Información adicional acerca de esta entrada de versión de fila. Este campo suele estar vacío.|  
  
## <a name="result-sets-for-column-information"></a>Conjuntos de resultados para información de columna  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**server_name**|**sysname**|Nombre del servidor que hospeda la base de datos que realizó la entrada de versión de columna.|  
|**db_name**|**sysname**|Nombre de la base de datos que realizó esta entrada.|  
|**db_nickname**|**binario (6)**|Alias de la base de datos que realizó esta entrada.|  
|**version**|**int**|Versión de la entrada.|  
|**colname**|**sysname**|Nombre de la columna del artículo que representa la entrada de la versión de columna.|  
|**comment**|**nvarchar(255)**|Información adicional acerca de esta entrada de versión de columna. Este campo suele estar vacío.|  
  
## <a name="result-set-for-both"></a>Conjuntos de resultados para ambos  
 Si se elige el valor **both** para *Mostrar*, se devuelven los conjuntos de resultados de filas y columnas.  
  
## <a name="remarks"></a>Observaciones  
 **sp_showrowreplicainfo** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 solo los miembros del rol fijo de base de datos **db_owner** en la base de datos de publicación o los miembros de la lista de acceso a la publicación (PAL) de la base de datos de publicación pueden ejecutar **sp_showrowreplicainfo** .  
  
## <a name="see-also"></a>Consulte también  
 [Detectar y resolver conflictos de replicación de mezcla](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
