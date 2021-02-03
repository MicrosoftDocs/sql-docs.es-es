---
description: STCurveToLine (tipo de datos de geometría)
title: STCurveToLine (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveToLine method (geometry)
ms.assetid: abc80b32-4152-4e10-b816-798b901e0ac5
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 81b1a222937a19a8fe18e3cb2261581011206d1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189501"
---
# <a name="stcurvetoline-geometry-data-type"></a>STCurveToLine (tipo de datos de geometría)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una aproximación poligonal de una instancia de **geometry** que contiene segmentos de arco circulares.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STCurveToLine ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Observaciones  
 Devuelve una instancia vacía de **GeometryCollection** para variables vacías de la instancia de **geometry** y devuelve **NULL** para variables de **geometry** sin inicializar.  
  
 La aproximación poligonal devuelta por el método depende de la instancia de **geometry** que se use para llamar al método:  
  
-   Devuelve una instancia de **LineString** para una instancia de **CircularString** o **CompoundCurve**.  
  
-   Devuelve una instancia de **Polygon** para una instancia de **CurvePolygon**.  
  
-   Devuelve una copia de la instancia de **geometry** si esa instancia no es una instancia de **CircularString**, **CompoundCurve** o **CurvePolygon**. Por ejemplo, el método `STCurveToLine` devuelve una instancia de **Point** para una instancia de **geometry** que sea una instancia de **Point**.  
  
 A diferencia de la especificación SQL/MM, el método `STCurveToLine` no usa los valores de z-coordinate para calcular la aproximación poligonal. El método omite los valores de z-coordinate presentes en la instancia de **geometry** que realiza la llamada.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-an-uninitialized-geometry-variable-and-empty-instance"></a>A. Usar una variable geometry sin inicializar y una instancia vacía  
 En el siguiente ejemplo, la primera instrucción **SELECT** usa una instancia de **geometry** sin inicializar para llamar al método `STCurveToLine`, mientras que la segunda instrucción **SELECT** usa una instancia de **geometry** vacía. Así, el método devuelve **NULL** en la primera instrucción y una colección **GeometryCollection** en la segunda instrucción.  
  
```
 DECLARE @g geometry; 
 SET @g = @g.STCurveToLine(); 
 SELECT @g.STGeometryType(); 
 SET @g = geometry::Parse('LINESTRING EMPTY'); 
 SELECT @g.STGeometryType();
 ```  
  
### <a name="b-using-a-linestring-instance"></a>B. Utilizar una instancia de LineString  
 La instrucción **SELECT** del ejemplo siguiente usa una instancia de **LineString** para llamar al método STCurveToLine. Así, el método devuelve una instancia de **LineString**.  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('LINESTRING(1 3, 5 5, 4 3, 1 3)'); 
 SET @g = @g.STCurveToLine(); 
 SELECT @g.STGeometryType();
 ```  
  
### <a name="c-using-a-circularstring-instance"></a>C. Utilizar una instancia de CircularString  
 La primera instrucción **SELECT** del ejemplo siguiente usa una instancia de **CircularString** para llamar al método STCurveToLine. Así, el método devuelve una instancia de **LineString**. Esta instrucción **SELECT** además compara las longitudes de las dos instancias, que son aproximadamente iguales.  Por último, la segunda instrucción **SELECT** devuelve el número de puntos de cada instancia.  Solo devuelve 5 puntos para la instancia de **CircularString**, pero devuelve 65 puntos para la instancia de **LineString**.  
  
```
 DECLARE @g1 geometry, @g2 geometry; 
 SET @g1 = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)'); 
 SET @g2 = @g1.STCurveToLine(); 
 SELECT @g1.STGeometryType() AS [G1 Type], @g2.STGeometryType() AS [G2 Type], @g1.STLength() AS [G1 Perimeter], @g2.STLength() AS [G2 Perimeter], @g2.ToString() AS [G2 Def]; 
 SELECT @g1.STNumPoints(), @g2.STNumPoints();
 ```  
  
### <a name="d-using-a-curvepolygon-instance"></a>D. Utilizar una instancia de CurvePolygon  
 La instrucción **SELECT** del ejemplo siguiente usa una instancia de **CurvePolygon** para llamar al método STCurveToLine. Así, el método devuelve una instancia de **Polygon**.  
  
```
 DECLARE @g1 geometry, @g2 geometry; 
 SET @g1 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))'); 
 SET @g2 = @g1.STCurveToLine(); 
 SELECT @g1.STGeometryType() AS [G1 Type], @g2.STGeometryType() AS [G2 Type];
 ```  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los tipos de datos espaciales](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [STLength &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STNumPoints &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)   
 [STGeometryType &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)  
  
  

