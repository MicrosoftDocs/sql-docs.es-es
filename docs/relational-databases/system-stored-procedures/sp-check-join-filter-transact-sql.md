---
description: sp_check_join_filter (Transact-SQL)
title: sp_check_join_filter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- filter_TSQL
- sp_check_TSQL
- sp_check_join_filter
- sp_check_join_filter_TSQL
- join
- join_TSQL
- filter
- sp_check
helpviewer_keywords:
- sp_check_join_filter
ms.assetid: e9699d59-c8c9-45f6-a561-f7f95084a540
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 74e4114eca77cbbac1c2e2c2471fb0ee10a55ae1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206560"
---
# <a name="sp_check_join_filter-transact-sql"></a>sp_check_join_filter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Se utiliza para comprobar un filtro de combinación entre dos tablas a fin de determinar si la cláusula del filtro de combinación es válida. Este procedimiento almacenado también devuelve información sobre el filtro de combinación proporcionado, incluso si se puede utilizar con particiones precalculadas para la tabla dada. Este procedimiento almacenado se ejecuta en el publicador de la publicación. Para obtener más información, vea [Optimización del rendimiento de los filtros con parámetros con particiones calculadas previamente](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_check_join_filter [ @filtered_table = ] 'filtered_table'  
        , [@join_table = ] 'join_table'  
        , [ @join_filterclause = ] 'join_filterclause'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @filtered_table = ] 'filtered_table'` Es el nombre de una tabla filtrada. *filtered_table* es de tipo **nvarchar (400)** y no tiene ningún valor predeterminado.  
  
`[ @join_table = ] 'join_table'` Es el nombre de una tabla que se combina con *filtered_table*. *join_table* es de tipo **nvarchar (400)** y no tiene ningún valor predeterminado.  
  
`[ @join_filterclause = ] 'join_filterclause'` Es la cláusula de filtro de combinación que se está probando. *join_filterclause* es de tipo **nvarchar (1000)** y no tiene ningún valor predeterminado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Es si la publicación es apta para las particiones precalculadas; donde **1** significa que se pueden usar las particiones especifica y **0** significa que no se pueden usar.|  
|**has_dynamic_filters**|**bit**|Indica si la cláusula de filtro proporcionada incluye al menos una función de filtrado con parámetros; donde **1** significa que se utiliza una función de filtrado con parámetros y **0** significa que no se utiliza dicha función.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Lista de las funciones de la cláusula de filtro que definen un filtro con parámetros para un artículo; las funciones están separadas por puntos y comas.|  
|**uses_host_name**|**bit**|Si se utiliza la función [host_name ()](../../t-sql/functions/host-name-transact-sql.md) en la cláusula de filtro, donde **1** significa que esta función está presente.|  
|**uses_suser_sname**|**bit**|Si se utiliza la función [SUSER_SNAME ()](../../t-sql/functions/suser-sname-transact-sql.md) en la cláusula de filtro, donde **1** significa que esta función está presente.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_check_join_filter** se utiliza en la replicación de mezcla.  
  
 **sp_check_join_filter** se pueden ejecutar en cualquier tabla relacionada, incluso si no están publicadas. Este procedimiento almacenado se puede utilizar para comprobar una cláusula de filtro de combinación antes de definir un filtro de combinación entre dos artículos.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_check_join_filter**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
