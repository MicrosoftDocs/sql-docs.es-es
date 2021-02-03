---
description: STNumPoints (tipo de datos geometry)
title: STNumPoints (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STNumPoints_TSQL
- STNumPoints (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STNumPoints (geometry Data Type)
ms.assetid: a19520fc-7f91-4a2c-856f-4d8b99a7e496
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: aef97340f845f15cc8a86e0d4758b5fa7dc33229
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181716"
---
# <a name="stnumpoints-geometry-data-type"></a>STNumPoints (tipo de datos geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve la suma del número de puntos de cada una de las figuras de una instancia de **geometry**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STNumPoints ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **int**  
  
 Tipo de valor devuelto de CLR: **SqlInt32**  
  
## <a name="remarks"></a>Observaciones  
 Este método cuenta los puntos de la descripción de una instancia de **geometry**. Los puntos duplicados se cuentan. Si esta instancia es de tipo **collection**, este método devuelve la suma de los puntos de cada uno de sus elementos.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una instancia de `LineString` y se utiliza `STNumPoints()` para determinar el número de puntos que se utilizaron en la descripción de la instancia.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STNumPoints();  
```  
  
## <a name="see-also"></a>Vea también  
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  
