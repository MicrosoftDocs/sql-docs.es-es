---
description: STCurveN (tipo de datos Geography)
title: STCurveN (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STCurveN
- STCurveN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveN method (geography)
ms.assetid: 99ef7100-2c4b-4f07-8d66-b343da94b023
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: fdb5dc0e5bae929b931c219937d89ea2afb9b828
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207524"
---
# <a name="stcurven-geography-data-type"></a>STCurveN (tipo de datos Geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve la curva especificada a partir de una instancia de **geography** que es **LineString**, **CircularString** o **CompoundCurve**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STCurveN( n )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *n*  
 Es una expresión **int** entre 1 y el número de curvas de la instancia de **geography**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
## <a name="exceptions"></a>Excepciones  
 Si n < 1, se produce una excepción **ArgumentOutOfRangeException**.  
  
## <a name="remarks"></a>Observaciones  
 Cuando se dan los criterios siguientes, se devuelve **NULL**.  
  
-   Se declara la instancia de **geography**, pero no se crea.  
  
-   La instancia de **geography** está vacía.  
  
-   n supera el número de curvas de la instancia de **geography** (vea [STNumCurves &#40;geography Data Type&#41;](../../t-sql/spatial-geography/stnumcurves-geography-data-type.md)).  
  
-   La dimensión para la instancia de **geography** no es equivalente (vea [STDimension &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/stdimension-geography-data-type.md)).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-stcurven-on-a-circularstring"></a>A. Usar STCurveN() en una CircularString  
 En el siguiente ejemplo se devuelve la segunda curva de una instancia de **CircularString**:  
  
```
 DECLARE @g geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 El ejemplo devuelve  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
### <a name="b-using-stcurven-on-a-compoundcurve"></a>B. Usar STCurveN() en una CompoundCurve  
 En el siguiente ejemplo se devuelve la segunda curva de una instancia de **CompoundCurve**:  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 El ejemplo devuelve  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
### <a name="c-using-stcurven-on-a-compoundcurve-containing-three-circularstrings"></a>C. Usar STCurveN() en una CompoundCurve que tiene tres CircularStrings  
 En el siguiente ejemplo se usa una instancia de **CompoundCurve** que combina tres instancias independientes de **CircularString** en la misma secuencia de la curva, como en el ejemplo anterior:  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE (CIRCULARSTRING (-122.358 47.653, -122.348 47.649, -122.348 47.658), CIRCULARSTRING(-122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 El ejemplo devuelve  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
 `STCurveN()` devuelve los mismos resultados sea cual sea el formato de Well-known text (WKT) que se utilice.  
  
### <a name="d-testing-for-validity-before-calling-stcurve"></a>D. Comprobar la validez antes de llamar a STCurve()  
 En el ejemplo siguiente se muestra cómo asegurarse de que *n* es válido antes de llamar al método STCurveN():  
  
```
 DECLARE @g geography;  
 DECLARE @n int;  
 SET @n = 2;  
 SET @g = geography::Parse('LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
 IF @n >= 1 AND @n <= @g.STNumCurves()  
 BEGIN  
 SELECT @g.STCurveN(@n).ToString();  
 END
  ```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
