---
description: STNumCurves (tipo de datos geometry)
title: STNumCurves (tipo de datos geometry) | Microsoft Docs
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
- STNumCurves method (geometry)
ms.assetid: 20c2fa0b-656b-4519-b34c-cc8f094290d4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 01d0c51f531fb6ff1b75974ea0630728df9facee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158637"
---
# <a name="stnumcurves-geometry-data-type"></a>STNumCurves (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Este método devuelve el número de curvas en una instancia de **geometry** cuando la instancia es un tipo de datos espacial unidimensional. Los tipos de datos espaciales unidimensionales incluyen **LineString**, **CircularString** y **CompoundCurve**. `STNumCurves`() solo funciona en tipos simples; no funciona con colecciones de **geometry** como **MultiLineString**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STNumCurves()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Observaciones  
 Una instancia vacía unidimensional de **geometry** devuelve 0. Se devuelve **NULL** cuando la instancia de **geometry** no es una instancia unidimensional o es una instancia no inicializada.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-stnumcurves-on-a-circularstring-instance"></a>A. Usar STNumCurves() en una instancia de CircularString  
 En el siguiente ejemplo se muestra cómo obtener el número de curvas de una instancia de `CircularString`:  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)');  
 SELECT @g.STNumCurves();
 ```  
  
### <a name="b-using-stnumcurves-on-a-compoundcurve-instance"></a>B. Usar STNumCurves() en una instancia de CompoundCurve  
 En el siguiente ejemplo se utiliza `STNumCurves()` para devolver el número de curvas de una instancia de `CompoundCurve`.  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))');  
 SELECT @g.STNumCurves();
 ```  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los tipos de datos espaciales](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

