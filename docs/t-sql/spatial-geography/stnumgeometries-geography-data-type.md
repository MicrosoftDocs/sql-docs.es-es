---
description: STNumGeometries (tipo de datos geography)
title: STNumGeometries (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STNumGeometries (geography Data Type)
- STNumGeometries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STNumGeometries method
ms.assetid: 6ae7fac2-62f1-420f-9fc9-a09606be9605
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 21dfb949f48d8717062cc2df6abb7ac3d30c7d23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202120"
---
# <a name="stnumgeometries-geography-data-type"></a>STNumGeometries (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el número de **geometries** que componen una instancia de **geography**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STNumGeometries ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **int**  
  
 Tipo de valor devuelto de CLR: **SqlInt32**  
  
## <a name="remarks"></a>Observaciones  
 Este método devuelve 1 si la instancia de **geography** no es una instancia de **MultiPoint**, **MultiLineString**, **MultiPolygon** o **GeometryCollection** y 0 si la instancia de **geography** está vacía.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crea una instancia de `MultiPoint` y se usa `STNumGeometries()` para averiguar cuántas **geometries** contiene dicha instancia.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('MULTIPOINT((-122.360 47.656), (-122.343 47.656))', 4326);  
SELECT @g.STNumGeometries();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
