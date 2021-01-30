---
description: sp_check_dynamic_filters (Transact-SQL)
title: sp_check_dynamic_filters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- dynamic_filters_TSQL
- sp_check_TSQL
- check
- sp_check_dynamic filter
- check_TSQL
- filters_TSQL
- check_dynamic_filters_TSQL
- dynamic filters
- filters
- check dynamic filters
- sp_check_dynamic filter_TSQL
- sp_check_for_sync_trigger_TSQL
- sp_check
helpviewer_keywords:
- sp_check_dynamic_filters
ms.assetid: dd7760db-a3a5-460f-bd97-b8d436015e19
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f458f3dcf629ee6f77b8237edb0a9defa516b8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158956"
---
# <a name="sp_check_dynamic_filters-transact-sql"></a>sp_check_dynamic_filters (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Muestra información sobre las propiedades del filtro de filas con parámetros para una publicación, en particular, las funciones utilizadas para generar una partición de datos filtrados para una publicación, y sobre si la publicación puede utilizar particiones previamente calculadas. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_check_dynamic_filters [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Es si la publicación es apta para usar particiones precalculadas; donde **1** significa que se pueden usar las particiones precalculadas, y **0** significa que no se pueden usar.|  
|**has_dynamic_filters**|**bit**|Indica si se ha definido al menos un filtro de fila con parámetros en la publicación; donde **1** significa que existen uno o más filtros de fila con parámetros y **0** significa que no existe ningún filtro dinámico.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Lista de funciones utilizadas para filtrar artículos en una publicación, donde las funciones se separan mediante puntos y coma.|  
|**validate_subscriber_info**|**nvarchar (500)**|Lista de funciones utilizadas para filtrar artículos en una publicación, donde las funciones se separan mediante signos más (+).|  
|**uses_host_name**|**bit**|Si se utiliza la función [host_name ()](../../t-sql/functions/host-name-transact-sql.md) en los filtros de fila con parámetros, donde **1** significa que esta función se usa para el filtrado dinámico.|  
|**uses_suser_sname**|**bit**|Si se utiliza la función [SUSER_SNAME ()](../../t-sql/functions/suser-sname-transact-sql.md) en los filtros de fila con parámetros, donde **1** significa que esta función se usa para el filtrado dinámico.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_check_dynamic_filters** se utiliza en la replicación de mezcla.  
  
 Si se ha definido una publicación para utilizar particiones precalculadas, **sp_check_dynamic_filters** comprueba si hay infracciones de las restricciones de las particiones precalculadas. Si se infringió alguna restricción, se devuelve un error. Para obtener más información, vea [Optimización del rendimiento de los filtros con parámetros con particiones calculadas previamente](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 Si se ha definido que una publicación posee filtros de filas con parámetros, pero no se encuentra ninguno, se devuelve un error.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_check_dynamic_filters**.  
  
## <a name="see-also"></a>Consulte también  
 [Administrar particiones para una publicación de combinación con filtros con parámetros](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [sp_check_join_filter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-check-join-filter-transact-sql.md)   
 [sp_check_subset_filter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-check-subset-filter-transact-sql.md)  
  
  
