---
description: STArea (tipo de datos geometry)
title: STArea (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STArea (geometry Data Type)
- STArea_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STArea (geometry Data Type)
ms.assetid: a7dd6083-c649-4ac3-885d-1234e0db62f1
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 005cdc3e931c9b504348f6c5a01bae3fce18acb5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188746"
---
# <a name="starea-geometry-data-type"></a>STArea (tipo de datos geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el área expuesta total de una instancia de **geometry**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STArea ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **float**  
  
 Tipo de valor devuelto de CLR: **SqlDouble**  
  
## <a name="remarks"></a>Comentarios  
 `STArea()` devuelve 0 si una instancia de **geometry** contiene únicamente figuras no dimensionales o unidimensionales, o si está vacía. `STArea()` devuelve **NULL** si la instancia de **geometry** no se ha inicializado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-computing-the-area-of-a-polygon-instance"></a>A. Calcular el área de una instancia de Polygon  
 En el siguiente ejemplo se crea una instancia de `Polygon``geometry` y se calcula el área del polígono.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);  
SELECT @g.STArea();  
```  
  
### <a name="b-computing-the-area-of-a-curvepolygon-instance"></a>B. Calcular el área de una instancia de CurvePolygon  
 En el siguiente ejemplo se calcula el área de una instancia de `CurvePolygon`.  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 2, 2 0, 4 2, 4 2, 0 2))');  
 SELECT @g.STArea() AS Area;
 ```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  
