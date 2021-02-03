---
description: CurveToLineWithTolerance (tipo de datos Geometry)
title: CurveToLineWithTolerance (tipo de datos geometry) | Microsoft Docs
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
- CurveToLineWithTolerance method (geometry)
ms.assetid: 96871075-1998-4cd9-86b1-3fc55577aee4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d1851b51e9068c3bd0fcbe7ac559e3d770fc1b48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198265"
---
# <a name="curvetolinewithtolerance-geometry-data-type"></a>CurveToLineWithTolerance (tipo de datos Geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una aproximación poligonal de una instancia de **geometry** que contiene segmentos de arco circulares.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.CurveToLineWithTolerance ( tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *tolerance*  
 Expresión **double** que define el error máximo entre el segmento del arco circular original y su aproximación lineal.  
  
 *relative*  
 Expresión **bool** que indica si se va a usar un máximo relativo para la desviación. Cuando relative está establecido en falso (0), se establece un máximo absoluto para la desviación que puede tener una aproximación lineal. Cuando relative está establecido en true (1), la tolerancia se calcula como un producto del parámetro de tolerancia y el diámetro del cuadro de límite para el objeto espacial.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="exceptions"></a>Excepciones  
 Al establecer la tolerancia <= 0 se produce una excepción `ArgumentOutOfRange`.  
  
## <a name="remarks"></a>Comentarios  
 Este método puede especificar una cantidad de tolerancia a errores para el resultado **LineString** que se obtiene.  
  
 En la siguiente tabla se muestra el tipo de instancia que `CurveToLineWithTolerance()` devuelve para varios tipos.  
  
|Tipo de la instancia que hace la llamada|Tipo espacial devuelto|  
|----------------------------|---------------------------|  
|Instancia de geometría vacía|Instancia de **GeometryCollection** vacía|  
|**Point** y **MultiPoint**|Instancia de **Point**|  
|**MultiPoint**|Instancia de **Point** o de **MultiPoint**|  
|**CircularString**, **CompoundCurve** o **LineString**|Instancia de **LineString**|  
|**MultiLineString**|Instancia de **LineString** o **MultiLineString**|  
|**CurvePolygon** y **Polygon**|Instancia de **Polygon**|  
|**MultiPolígono**|Instancia de **Polygon** o **MultiPolygon**|  
|**GeometryCollection** con una instancia única que no contiene un segmento de arco circular|La instancia que se encuentra en **GeometryCollection** determina el tipo de instancia que se devuelve.|  
|**GeometryCollection** con una sola instancia de arco de segmento circular unidimensional (**CircularString**, **CompoundCurve**)|Instancia de **LineString**|  
|**GeometryCollection** con una sola instancia de arco de segmento circular bidimensional (**CurvePolygon**)|Instancia de **Polygon**|  
|**GeometryCollection** con varias instancias unidimensionales|Instancia de **MultiLineString**|  
|**GeometryCollection** con varias instancias bidimensionales|Instancia de **MultiPolygon**|  
|**GeometryCollection** con varias instancias de distintas dimensiones|Instancia de **GeometryCollection**|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-different-tolerance-values-on-a-circularstring-instance"></a>A. Utilizar valores de tolerancia diferentes en una instancia de CircularString  
 En el siguiente ejemplo se muestra cómo establecer la tolerancia afecta a la instancia de `LineString` devuelta desde una instancia de `CircularString`:  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.01, 0).STNumPoints();
 ```  
  
### <a name="b-using-the-method-on-a-multilinestring-instance-containing-one-linestring"></a>B. Utilizar el método en una instancia de MultiLineString que contiene un LineString  
 En el siguiente ejemplo se muestra lo que se devuelve de una instancia de `MultiLineString` que solo contiene una instancia de `LineString`:  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('MULTILINESTRING((1 3, 4 8, 6 9))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
 ```  
  
### <a name="c-using-the-method-on-a-multilinestring-instance-containing-multiple-linestrings"></a>C. Utilizar el método en una instancia de MultiLineString que contiene varios LineString  
 En el siguiente ejemplo se muestra lo que se devuelve de una instancia de `MultiLineString` que contiene más de una instancia de `LineString`:  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('MULTILINESTRING((1 3, 4 8, 6 9),(4 4, 9 18))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
 ```  
  
### <a name="d-setting-relative-to-true-for-an-invoking-curvepolygon-instance"></a>D. Establecer relative en true para invocar una instancia de CurvePolygon  
 En el siguiente ejemplo se usa una instancia de `CurvePolygon` para llamar a `CurveToLineWithTolerance()` con *relative* establecido en true:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.CurveToLineWithTolerance(.5,1).ToString();
 ```  
  
### <a name="e-using-the-method-on-a-geometrycollection-instance"></a>E. Usar el método en una instancia de GeometryCollection  
 En el ejemplo siguiente se llama a `CurveToLineWithTolerance()` en un elemento `GeometryCollection` que contiene una instancia de `CurvePolygon` bidimensional y una instancia de `CircularString` unidimensional. `CurveToLineWithTolerance()` convierte los dos tipos de segmento de arco circular en tipos de segmento de línea y los devuelve en un tipo `GeometryCollection`.  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('GEOMETRYCOLLECTION(CURVEPOLYGON( COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), (4 2, 0 2))), CIRCULARSTRING(4 4, 8 6, 9 5))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.1, 0).ToString();
 ```  
  
## <a name="see-also"></a>Vea también  
 [CurveToLineWithTolerance &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/curvetolinewithtolerance-geography-data-type.md)   
 [STCurveToLine &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stcurvetoline-geometry-data-type.md)  
  
  

