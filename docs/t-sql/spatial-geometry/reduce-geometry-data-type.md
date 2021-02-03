---
description: Reduce (tipo de datos geometry)
title: Reduce (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Reduce_TSQL
- Reduce
dev_langs:
- TSQL
helpviewer_keywords:
- Reduce method
ms.assetid: 132184bf-c4d2-4a27-900d-8373445dce2a
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: dc6fbf1f3952e7ab59688c928eff3770776c308a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187717"
---
# <a name="reduce-geometry-data-type"></a>Reduce (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una aproximación de la instancia de **geometry** especificada. La aproximación se produce al ejecutar una extensión del algoritmo de Douglas-Peucker en la instancia con la tolerancia especificada.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.Reduce ( tolerance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *tolerance*  
 Es un valor de tipo **float**. *tolerancia* es la tolerancia que se usa como entrada para el algoritmo de aproximación.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Observaciones  
 Para los tipos de colección, este algoritmo funciona independientemente en cada tipo **geometry** contenido en la instancia.  
  
 Este algoritmo no modifica las instancias de **Point**.  
  
 En las instancias **LineString**, **CircularString** y **CompoundCurve**, el algoritmo de aproximación mantiene los puntos iniciales y finales originales de la instancia. El algoritmo "next" vuelve a agregar de forma iterativa el punto desde la instancia original que se más desvíe del resultado. El proceso continúa hasta que ningún punto se desvía más de la tolerancia especificada.  
  
 `Reduce()` devuelve una instancia de **LineString**, **CircularString** o **CompoundCurve** para las instancias de **CircularString**.  `Reduce()` devuelve una instancia de **CompoundCurve** o **LineString** para las instancias de **CompoundCurve**.  
  
 En las instancias de **Polygon**, el algoritmo de aproximación se aplica independientemente a cada anillo. El método generará una excepción `FormatException` si la instancia de **Polygon** devuelta no es válida; por ejemplo, se crea una instancia de **MultiPolygon** no válida si se aplica `Reduce()` para simplificar cada anillo de la instancia y los anillos resultantes se superponen.  En las instancias de **CurvePolygon** con un anillo exterior y sin anillos interiores, `Reduce()` devuelve una instancia de **CurvePolygon**, **LineString** o **Point**.  Si **CurvePolygon** tiene anillos interiores, se devuelve una instancia de **CurvePolygon** o **MultiPoint**.  
  
 Cuando se detecta un segmento de arco circular, el algoritmo de aproximación comprueba si se puede aproximar al arco su cuerda en la mitad de la tolerancia indicada. En las cuerdas que cumplen estos criterios, el arco circular se reemplaza en los cálculos por la cuerda. Si una cuerda no satisface este criterio, se conserva el arco circular y se aplica el algoritmo de aproximación a los segmentos restantes.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-reduce-to-simplify-a-linestring"></a>A. Usar Reduce() para simplificar LineString  
 En el ejemplo siguiente se crea una instancia sencilla de `LineString` y se utiliza `Reduce()` para simplificar la instancia.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 0 1, 1 0, 2 1, 3 0, 4 1)', 0);  
SELECT @g.Reduce(.75).ToString();  
```  
  
### <a name="b-using-reduce-with-varying-tolerance-levels-on-a-circularstring"></a>B. Usar Reduce() con niveles de tolerancia que varían en CircularString  
 En el ejemplo siguiente se usa `Reduce()` con tres niveles de tolerancia en una instancia de **CircularString**:  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0)'; 
 SELECT @g.Reduce(5).ToString(); 
 SELECT @g.Reduce(15).ToString(); 
 SELECT @g.Reduce(16).ToString();
 ```  
  
 Este ejemplo produce el siguiente resultado:  
  
 ```
 CIRCULARSTRING (0 0, 8 8, 16 0, 20 -4, 24 0) 
 COMPOUNDCURVE (CIRCULARSTRING (0 0, 8 8, 16 0), (16 0, 24 0)) 
 LINESTRING (0 0, 24 0)
 ```  
  
 Cada una de las instancias devueltas contiene los extremos (0 0) y (24 0).  
  
### <a name="c-using-reduce-with-varying-tolerance-levels-on-a-compoundcurve"></a>C. Usar Reduce() con niveles de tolerancia que varían en CompoundCurve  
 En el ejemplo siguiente se usa `Reduce()` con dos niveles de tolerancia en una instancia de **CompoundCurve**:  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE(CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0),(24 0, 20 4, 16 0))';  
 SELECT @g.Reduce(15).ToString();  
 SELECT @g.Reduce(16).ToString();
 ```  
  
 En este ejemplo, observe que la segunda instrucción **SELECT** devuelve la instancia de **LineString**: `LineString(0 0, 16 0)`.  
  
### <a name="showing-an-example-where-the-original-start-and-end-points-are-lost"></a>Mostrar un ejemplo en el que se pierdan los puntos inicial y final originales  
 En el ejemplo siguiente, se muestra cómo la instancia resultante no conserva los puntos inicial y final originales. Este comportamiento se produce porque, si se conservan los puntos inicial y final originales, el resultado sería una instancia de **LineString** no válida.  
  
```  
DECLARE @g geometry = 'LINESTRING(0 0, 4 0, 2 .01, 1 0)';  
DECLARE @h geometry = @g.Reduce(1);  
SELECT @g.STIsValid() AS Valid  
SELECT @g.ToString() AS Original, @h.ToString() AS Reduced;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geometría estáticos ampliados](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
