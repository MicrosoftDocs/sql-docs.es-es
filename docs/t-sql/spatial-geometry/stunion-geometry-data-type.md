---
description: STUnion (tipo de datos geometry)
title: STUnion (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STUnion (geometry Data Type)
- STUnion_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STUnion (geometry Data Type)
ms.assetid: 5b168118-137d-402f-9173-fee3f365a89c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 857428b0dee48336ae41f22e79c82a02f2f7b241
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88458929"
---
# <a name="stunion-geometry-data-type"></a>STUnion (tipo de datos geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Devuelve un objeto que representa la unión de una instancia de **geometry** con otra instancia de **geometry**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STUnion ( other_geometry )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *other_geometry*  
 Es otra instancia de **geometry** para formar la unión con la instancia en la que se invoca `STUnion()`.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Observaciones  
 Este método siempre devuelve NULL si no coinciden los identificadores de referencia espacial (SRID) de las instancias de **geometry**. El resultado puede contener segmentos de arco circulares solo si las instancias de entrada contienen segmentos de arco circulares.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-computing-the-union-of-two-polygon-instances"></a>A. Calcular la unión de dos instancias de Polygon  
 En el ejemplo siguiente se usa `STUnion()` para calcular la unión de dos instancias de `Polygon`.  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 0 2, 2 2, 2 0, 0 0))', 0);  
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);  
SELECT @g.STUnion(@h).ToString();  
```  
  
### <a name="b-computing-the-union-of-a-polygon-instance-with-a-curvepolygon-instance"></a>B. Calcular la unión de una instancia de Polygon con una instancia de CurvePolygon  
 En el siguiente ejemplo se devuelve una instancia de `GeometryCollection` que contiene un segmento de arco circular.  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(CIRCULARSTRING(0 -4, 4 0, 0 4, -4 0, 0 -4))';  
 DECLARE @h geometry = 'POLYGON((5 -1, 5 -3, 7 -3, 7 -1, 5 -1))';  
 SELECT @g.STUnion(@h).ToString();
 ```  
  
 `STUnion()` devuelve un resultado que contiene un segmento de arco circular porque la instancia que invocó `STUnion()` contiene un segmento de arco circular.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

