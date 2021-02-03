---
description: STUnion (tipo de datos geography)
title: STUnion (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STUnion (geography Data Type)
- STUnion_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STUnion method
ms.assetid: 9bf87691-efd8-4c53-bd2f-eefe0acd19ca
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1990e524b3816c20c374dca2f01e604411530303
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186445"
---
# <a name="stunion-geography-data-type"></a>STUnion (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve un objeto que representa la unión de una instancia de **geography** con otra instancia de **geography**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STUnion ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *other_geography*  
 Es otra instancia de **geography** para formar la unión con la instancia en la que se invoca STUnion().  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
## <a name="exceptions"></a>Excepciones  
 Este método produce una excepción **ArgumentException** si la instancia contiene un borde opuesto.  
  
## <a name="remarks"></a>Observaciones  
 Este método siempre devuelve NULL si no coinciden los identificadores de referencia espacial (SRID) de las instancias de **geography**.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite instancias espaciales mayores que un hemisferio. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el conjunto de resultados posibles devuelto en el servidor se ha ampliado a las instancias de **FullGlobe**.  
  
 El resultado puede contener segmentos de arco circulares solo si las instancias de entrada contienen segmentos de arco circulares.  
  
 Este método no es preciso.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-computing-the-union-of-two-polygons"></a>A. Calcular la unión de dos polígonos  
 En el ejemplo siguiente se usa `STUnion()` para calcular la unión de dos instancias de `Polygon`.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('POLYGON((-122.351 47.656, -122.341 47.656, -122.341 47.661, -122.351 47.661, -122.351 47.656))', 4326);  
SELECT @g.STUnion(@h).ToString();  
```  
  
### <a name="b-producing-a-fullglobe-result"></a>B. Generar un resultado de FullGlobe  
 En el siguiente ejemplo se genera un `FullGlobe` cuando `STUnion()` une dos instancias `Polygon`.  
  
```
 DECLARE @g geography = 'POLYGON ((-122.358 47.653, -122.358 47.658,-122.348 47.658, -122.348 47.649, -122.358 47.653))';  
 DECLARE @h geography = 'POLYGON ((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STUnion(@h).ToString();
 ```  
  
### <a name="c-producing-a-triagonal-hole-from-a-union-of-a-curvepolygon-and-a-triagonal-hole"></a>C. Generar un hueco triagonal a partir de la unión de un objeto CurvePolygon y un hueco triagonal.  
 En el siguiente ejemplo se genera un hueco triagonal a partir de la unión de un `CurvePolygon` y una instancia de `Polygon`.  
  
```
 DECLARE @g geography = 'POLYGON ((-0.5 0, 0 1, 0.5 0.5, -0.5 0))';  
 DECLARE @h geography = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 0, 0.7 0.7, 0 1), (0 1, 0 0)))';  
 SELECT @g.STUnion(@h).ToString();
 ```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
