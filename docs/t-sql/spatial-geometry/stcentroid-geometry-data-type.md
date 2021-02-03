---
description: STCentroid (tipo de datos geometry)
title: STCentroid (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STCentroid_TSQL
- STCentroid (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STCentroid (geometry Data Type)
ms.assetid: 4dc5a004-7a53-4cce-81dd-9f5e1dd0db78
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: efa17bc7e133aec0b3730c814a35c64bb6ac5ae7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187203"
---
# <a name="stcentroid-geometry-data-type"></a>STCentroid (tipo de datos geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Devuelve el centro geométrico de una instancia de **geometry** que consta de uno o varios polígonos.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STCentroid ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
 Tipo Open Geospatial Consortium (OGC): **Point**  
  
## <a name="remarks"></a>Comentarios  
 `STCentroid()` devuelve null si la instancia de **geometry** no es de tipo **Polygon, CurvePolygon** o **MultiPolygon**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-computing-the-centroid-of-a-polygon-instance"></a>A. Calcular el centroide de una instancia de Polygon  
 En el siguiente ejemplo se usa `STCentroid()` para calcular el centroide de una instancia de `polygon``geometry`:  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);  
SELECT @g.STCentroid().ToString();  
```  
  
### <a name="b-computing-the-centroid-of-a-curvepolygon-instance"></a>B. Calcular el centroide de una instancia de CurvePolygon  
 En el siguiente ejemplo se calcula el centroid para una instancia de `CurvePolygon`:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(CIRCULARSTRING(0 4, 4 0, 8 4, 4 8, 0 4), CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))';  
 SELECT @g.STCentroid().ToString() AS Centroid
 ```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

