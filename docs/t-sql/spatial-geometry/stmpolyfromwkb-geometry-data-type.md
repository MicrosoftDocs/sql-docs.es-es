---
description: STMPolyFromWKB (tipo de datos geometry)
title: STMPolyFromWKB (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STMPolyFromWKB (geometry Data Type)
- STMPolyFromWKB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STMPolyFromWKB (geometry Data Type)
ms.assetid: cac25868-08ef-46fc-9c3d-a15e43794a7a
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: a2a31338f8ae4a0ef8deca035c275c0d4e3623e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181422"
---
# <a name="stmpolyfromwkb-geometry-data-type"></a>STMPolyFromWKB (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una instancia de **geometryMultiPolygon** a partir de una representación Well-Known Binary (WKB) de Open Geospatial Consortium (OGC).
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
STMPolyFromWKB ( 'WKB_multipolygon' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *WKB_multipolygon*  
 Es la representación WKB de la instancia de **geometryMultiPolygon** que se quiere devolver. *WKB_multipolygon* es una expresión **varbinary(max)**.  
  
 *SRID*  
 Es una expresión de tipo **int** que representa el identificador de referencia espacial (SRID) de la instancia de **geometryMultiPolygon** que se quiere devolver.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
 Tipo OGC: **MultiPolygon**  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa `STMPolyFromWKB()` para crear una instancia de `geometry`:  
  
```  
DECLARE @g geometry;   
SET @g = geometry::STMPolyFromWKB(0x0106000000020000000103000000010000000400000000000000000014400000000000001440000000000000244000000000000014400000000000002440000000000000244000000000000014400000000000001440010300000001000000050000000000000000002440000000000000244000000000000059400000000000002440000000000000694000000000000069400000000000003E400000000000003E4000000000000024400000000000002440, 0);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geometría estáticos de OGC](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
  

