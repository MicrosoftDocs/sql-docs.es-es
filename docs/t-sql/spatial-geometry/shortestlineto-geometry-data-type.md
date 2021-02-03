---
description: ShortestLineTo (tipo de datos geometry)
title: ShortestLineTo (tipo de datos geometry) | Microsoft Docs
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
- ShortestLineTo method (geometry)
ms.assetid: 39a2d0e4-4f93-4e94-a27e-6ad9537cfe74
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 937a631ca32ab7d63dece2603d5cf1357fdd1567
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200408"
---
# <a name="shortestlineto-geometry-data-type"></a>ShortestLineTo (tipo de datos geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una instancia de **LineString** con dos puntos que representan la distancia más corta entre las dos instancias de **geometry**. La longitud de la instancia de **LineString** devuelta es la distancia entre las dos instancias de **geometry**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.ShortestLineTo ( geometry_other )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *otra_geometría*  
 La segunda instancia de **geometry** a la que la instancia de **geometry** que realiza la llamada está intentando determinar la distancia más corta.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Observaciones  
 El método devuelve una instancia de **LineString** con los extremos en los bordes de las dos instancias de **geometry** que no se cruzan y que se comparan. La longitud de la instancia de **LineString** devuelta es igual a la distancia más corta entre las dos instancias de **geometry**. Se devuelve una instancia vacía de **LineString** cuando las dos instancias de **geometry** se cruzan.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-calling-shortestlineto-on-non-intersecting-instances"></a>A. Llamar a ShortestLineTo() en las instancias que no se cruzan  
 En este ejemplo se busca la distancia más corta entre una instancia de `CircularString` y una instancia de `LineString`, y se devuelven las instancias de `LineString` que conectan los dos extremos:  
  
```
 DECLARE @g1 geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 DECLARE @g2 geometry = 'LINESTRING(-4 7, 7 10, 3 7)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
 ```  
  
### <a name="b-calling-shortestlineto-on-intersecting-instances"></a>B. Llamar a ShortestLineTo() en las instancias que se cruzan  
 En este ejemplo se devuelve una instancia de `LineString` vacía porque la instancia de `LineString` se cruza con la instancia de `CircularString`:  
  
```
 DECLARE @g1 geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 DECLARE @g2 geometry = 'LINESTRING(0 5, 7 10, 3 7)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
 ```  
  
## <a name="see-also"></a>Consulte también  
 [ShortestLineTo &#40;geography Data Type&#41;](../../t-sql/spatial-geography/shortestlineto-geography-data-type.md) (ShortestLineTo [tipo de datos geography])  
  
  

