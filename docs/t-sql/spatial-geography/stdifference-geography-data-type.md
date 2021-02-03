---
description: STDifference (tipo de datos geography)
title: STDifference (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STDifference_TSQL
- STDifference (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STDifference (geography Data Type)
ms.assetid: 1cde5054-b91a-41bb-812a-08c9308738af
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: a6ac9219a2450835acb3a6495aef4c5db4304048
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192214"
---
# <a name="stdifference-geography-data-type"></a>STDifference (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve un objeto que representa el conjunto de puntos de una instancia de **geography** que queda fuera de otra instancia de **geography**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STDifference ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *other_geography*  
 Es otra instancia de **geography** que indica los puntos que hay que quitar de la instancia en la que se invoca STDifference().  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
## <a name="exceptions"></a>Excepciones  
 Este método produce una excepción **ArgumentException** si la instancia contiene un borde opuesto.  
  
## <a name="remarks"></a>Observaciones  
 Este método siempre devuelve NULL si no coinciden los identificadores de referencia espacial (SRID) de las instancias de **geography**.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el conjunto de resultados posibles devuelto en el servidor se ha ampliado a las instancias de **FullGlobe**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite instancias espaciales mayores que un hemisferio. El resultado puede contener segmentos de arco circulares solo si las instancias de entrada contienen segmentos de arco circulares. Este método no es preciso.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-computing-the-difference-between-two-geography-instances"></a>A. Calcular la diferencia entre dos instancias de geografía  
 En el ejemplo siguiente se usa `STDifference()` para calcular la diferencia entre dos instancias de **geography**.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STDifference(@h).ToString();  
```  
  
### <a name="b-using-a-fullglobe-with-stdifference"></a>B. Utilizar FullGlobe con STDifference()  
 En el ejemplo siguiente se utiliza la instancia de `FullGlobe`. El primer resultado es un elemento `GeometryCollection` vacío y el segundo resultado es una instancia de `Polygon`. `STDifference()` devuelve un elemento `GeometryCollection` vacío cuando una instancia de `FullGlobe` es el parámetro. Todos los puntos de una instancia de `geography` que se invoca están contenidos en una instancia de `FullGlobe`.  
  
```
 DECLARE @g geography = 'POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 DECLARE @h geography = 'FULLGLOBE';  
 SELECT @g.STDifference(@h).ToString(),  
 @h.STDifference(@g).ToString();
 ```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
