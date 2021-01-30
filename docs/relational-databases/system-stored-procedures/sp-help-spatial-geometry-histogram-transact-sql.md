---
description: sp_help_spatial_geometry_histogram (Transact-SQL)
title: sp_help_spatial_geometry_histogram (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_histogram
- sp_help_spatial_geometry_histogram_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_histogram
ms.assetid: 036aaf61-df3e-40f7-aa4e-62983c5a37bd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 75609570d3c021b350fc06cbaf35f32dee4bd8ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183604"
---
# <a name="sp_help_spatial_geometry_histogram-transact-sql"></a>sp_help_spatial_geometry_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Facilita la introducción de los parámetros del cuadro de límite y de la cuadrícula para un índice espacial.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_spatial_geometry_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @xmin = ] 'minx' ]   
     [ , [ @ymin = ] 'miny' ]   
     [ ,.[ @xmax = ] 'maxx' ]  
     [ , [ @ymax = ] 'maxy' ]  
     [ , [ @sample = ] 'sample' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @tabname = ] 'tabname'` Es el nombre completo o no completo de la tabla para la que se ha especificado el índice espacial.  
  
 Se requieren comillas únicamente si se especifica una tabla certificada. Si se proporciona un nombre completo, incluido el nombre de la base de datos, el nombre de la base de datos debe ser el de la base de datos actual. *tabname* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @colname = ] 'colname'` Es el nombre de la columna espacial especificada. *colname* es un **sysname** y no tiene ningún valor predeterminado.  
  
`[ @resolution = ] 'resolution'` Es la resolución del cuadro de límite. Los valores válidos van del 10 al 5000. la *resolución* es de **tinyint** y no tiene ningún valor predeterminado.  
  
`[ @xmin = ] 'xmin'` Es la propiedad del cuadro de límite X mínimo. *xmin* es de **tipo float** y no tiene ningún valor predeterminado.  
  
`[ @ymin = ] 'ymin'` Es la propiedad del cuadro de límite Y mínimo. *YMIN* es de **tipo float** y no tiene ningún valor predeterminado.  
  
`[ @xmax = ] 'xmax'` Es la propiedad del cuadro de límite X máximo. *Xmax* es de **tipo float** y no tiene ningún valor predeterminado.  
  
`[ @ymax = ] 'ymax'` Es la propiedad del cuadro de límite Y máximo. *YMAX* es de **tipo float** y no tiene ningún valor predeterminado.  
  
`[ @sample = ] 'sample'` Es el porcentaje de la tabla que se utiliza. Los valores válidos son de 0 a 100. el *ejemplo* es **float**. El valor predeterminado es 100.  
  
## <a name="property-valuereturn-value"></a>Valor de propiedad y valor devuelto  
 Se devuelve un valor de tabla. En la siguiente cuadrícula se describe el contenido de la columna de la tabla.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|Representa el identificador único de cada celda, el recuento empieza por 1.|  
|**móvil**|**geometry**|Es un polígono rectangular que representa cada celda. La forma de la celda es idéntica a la de la celda usada para los índices espaciales.|  
|**row_count**|**bigint**|Indica el número de objetos espaciales que tocan la celda o que están contenidos en ella.|  
  
## <a name="permissions"></a>Permisos  
 El usuario debe ser miembro del rol **Public** . Requiere el permiso READ ACCESS en el servidor y el objeto.  
  
## <a name="remarks"></a>Observaciones  
 La pestaña Resultados espaciales de SSMS muestra una representación gráfica de los resultados. Puede consultar los resultados en la ventana espacial para obtener un número aproximado de elementos de resultados. Los objetos de la tabla pueden ocupar más de una celda. Por tanto, la suma de las celdas puede ser mayor que el número de objetos reales.  
  
 Puede que se agregue una fila adicional al conjunto de resultados que contiene el número de objetos situados fuera del cuadro de límite o que están en contacto con el borde de dicho cuadro. El valor de **CellID** de esta fila es 0 y la **celda** de esta fila contiene una **LineString** que representa el rectángulo de selección. Esta fila representa todo el espacio fuera del cuadro de límite.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una tabla de ejemplo y, a continuación, se llama a **sp_help_spatial_geometry_histogram** en la tabla.  
  
 `USE AdventureWorksDW2012`  
  
 `GO`  
  
 `-- Set database compatibility for circular arc segments`  
  
 `ALTER DATABASE AdventureWorksDW2012`  
  
 `SET COMPATIBILITY_LEVEL = 110;`  
  
 `GO`  
  
 `-- Create table to execute sp_help_spatial_geometry_histogram on`  
  
 `CREATE TABLE TownSites`  
  
 `(`  
  
 `Location geometry NULL,`  
  
 `SiteName nvarchar(50) NULL`  
  
 `)`  
  
 `GO`  
  
 `-- Insert site data into table`  
  
 `DECLARE @g geometry;`  
  
 `SET @g = geometry::Parse('POINT(0 0)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Booth Map';`  
  
 `SET @g = geometry::Parse('POLYGON((1 1, 1 2, 2 2, 2 1, 1 1))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Town Hall';`  
  
 `SET @g = geometry::Parse('CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-1 0, 0 -1, 1 0),(1 0, 1 2, -1 0)))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Park';`  
  
 `SET @g = geometry::Parse('CIRCULARSTRING(1 5, 2 2, 5 1)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Road';`  
  
 `-- Call proc to see data within bounding box`  
  
 `EXEC sp_help_spatial_geometry_histogram @tabname = TownSites, @colname = Location, @resolution = 64, @xmin = -2, @ymin = -2, @xmax = 3, @ymax = 3, @sample = 100;`  
  
 `GO`  
  
 `DROP TABLE TownSites;`  
  
 `GO`  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de índice espacial &#40;Transact-SQL&#41;](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
