---
description: STCurveN (tipo de datos geometry)
title: STCurveN (tipo de datos geometry) | Microsoft Docs
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
- STCurveN method (geometry)
ms.assetid: 64adf1a1-3a41-41fb-b7d1-44390c3e4ea9
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 56ecf81209f125958a72e57b7dc70409d492e5ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187686"
---
# <a name="stcurven-geometry-data-type"></a>STCurveN (tipo de datos geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Devuelve la curva especificada a partir de una instancia de **geometry** que es **LineString**, **CircularString**, **CompoundCurve** o **MultiLineString**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STCurveN ( curve_index )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *curve_index*  
 Es una expresión **int** entre 1 y el número de curvas de la instancia de **geometry**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="exceptions"></a>Excepciones  
 Si *curve_index* < 1, se produce una `ArgumentOutOfRangeException`.  
  
## <a name="remarks"></a>Observaciones  
 Se devuelve **NULL** cuando se da alguna de las condiciones siguientes:  
  
-   La instancia de **geometry** se declara, pero no se crea.  
  
-   La instancia de **geometry** está vacía.  
  
-   *curve_index* supera el número de curvas de la instancia de **geometry**.  
  
-   La instancia de **geometry** es un valor **Point**, **MultiPoint**, **Polygon**, **CurvePolygon** o **MultiPolygon**  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-stcurven-on-a-circularstring-instance"></a>A. Usar STCurveN() en una instancia de CircularString  
 En el siguiente ejemplo se devuelve la segunda curva de una instancia de `CircularString`:  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 El ejemplo anterior de este tema devuelve:  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
### <a name="b-using-stcurven-on-a-compoundcurve-instance-with-one-circularstring-instance"></a>B. Usar STCurveN() en una instancia de CompoundCurve con una instancia de CircularString  
 En el siguiente ejemplo se devuelve la segunda curva de una instancia de `CompoundCurve`:  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE(CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 El ejemplo anterior de este tema devuelve:  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
### <a name="c-using-stcurven-on-a-compoundcurve-instance-with-three-circularstring-instances"></a>C. Usar STCurveN() en una instancia de CompoundCurve con tres instancias de CircularString  
 El siguiente ejemplo utiliza un `CompoundCurve` crea instancias que combina tres instancias `CircularString` independientes en la misma secuencia de la curva como el ejemplo anterior:  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE (CIRCULARSTRING (0 0, 1 2.1082, 3 6.3246), CIRCULARSTRING(3 6.3246, 0 7, -3 6.3246), CIRCULARSTRING(-3 6.3246, -1 2.1082, 0 0))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 El ejemplo anterior de este tema devuelve:  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
 Observe que los resultados son los mismos para los tres ejemplos anteriores. Cualquier formato WKT (texto bien conocido) se utiliza para especificar la misma secuencia de la curva, los resultados que devuelve  `STCurveN()` son iguales cuando se usa una instancia de `CompoundCurve`.  
  
### <a name="d-validating-the-parameter-before-calling-stcurven"></a>D. Validar el parámetro antes de llamar a STCurveN()  
 En el ejemplo siguiente se muestra cómo asegurarse de que `@n` es válido antes de llamar al método `STCurveN()`:  
  
```
 DECLARE @g geometry;  
 DECLARE @n int;  
 SET @n = 3;  
 SET @g = geometry::Parse('CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)');  
 IF @n >= 1 AND @n <= @g.STNumCurves()  
 BEGIN  
 SELECT @g.STCurveN(@n).ToString();  
 END
 ```  
  
## <a name="see-also"></a>Consulte también  
 [STNumCurves &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stnumcurves-geometry-data-type.md)   
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

