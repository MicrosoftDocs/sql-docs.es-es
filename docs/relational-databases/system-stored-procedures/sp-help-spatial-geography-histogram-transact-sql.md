---
description: sp_help_spatial_geography_histogram (Transact-SQL)
title: sp_help_spatial_geography_histogram (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geography_histogram_TSQL
- sp_help_spatial_geography_histogram
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geography_histogram
ms.assetid: 5c5bd319-055d-4cd6-8c5a-06354cc056cc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3554c8b385a14f588c4f7f8dd6248ba36ad7b16d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212069"
---
# <a name="sp_help_spatial_geography_histogram-transact-sql"></a>sp_help_spatial_geography_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Facilita la escritura de parámetros de cuadrícula para un índice espacial.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_spatial_geography_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @sample = ] 'tablesample' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @tabname = ] 'tabname'` Es el nombre completo o no completo de la tabla para la que se ha especificado el índice espacial.  
  
 Se requieren comillas únicamente si se especifica una tabla certificada. Si se proporciona un nombre completo, incluido el nombre de la base de datos, el nombre de la base de datos debe ser el de la base de datos actual. *tabname* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @colname = ] 'columnname'` Es el nombre de la columna espacial especificada. *columnName* es un **sysname** y no tiene ningún valor predeterminado.  
  
`[ @resolution = ] 'resolution'` Es la resolución del cuadro de límite. Los valores válidos van del 10 al 5000. la *resolución* es de **tinyint** y no tiene ningún valor predeterminado.  
  
`[ @sample = ] 'sample'` Es el porcentaje de la tabla que se utiliza. Los valores válidos son de 0 a 100. *tablesample* es un valor **float**. El valor predeterminado es 100.  
  
## <a name="property-valuereturn-value"></a>Valor de propiedad y valor devuelto  
 Se devuelve un valor de tabla. En la siguiente cuadrícula se describe el contenido de la columna de la tabla.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|Representa el identificador único de cada celda, con el contador inicial igual a 1.|  
|**móvil**|**geography**|Es un polígono rectangular que representa cada celda. La forma de la celda es idéntica a la de la celda usada para los índices espaciales.|  
|**row_count**|**bigint**|Indica el número de objetos espaciales que tocan la celda o que están contenidos en ella.|  
  
## <a name="permissions"></a>Permisos  
 El usuario debe ser miembro del rol **Public** . Requiere el permiso READ ACCESS en el servidor y el objeto.  
  
## <a name="remarks"></a>Observaciones  
 La pestaña Resultados espaciales de SSMS muestra una representación gráfica de los resultados. Puede consultar los resultados en la ventana espacial para obtener un número aproximado de elementos de resultados.  
  
> [!NOTE]  
>  Los objetos de la tabla pueden ocupar más de una celda. Por tanto, la suma de las celdas de la tabla puede ser mayor que el número de objetos reales.  
  
 El rectángulo de selección para el tipo de **geografía** es todo el globo.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se llama a  **sp_help_spatial_geography_histogram** en la `Person.Address` tabla de la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de datos.  
  
```  
EXEC sp_help_spatial_geography_histogram @tabname = Person.Address, @colname = SpatialLocation, @resolution = 64, @sample = 30;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de índice espacial &#40;Transact-SQL&#41;](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
