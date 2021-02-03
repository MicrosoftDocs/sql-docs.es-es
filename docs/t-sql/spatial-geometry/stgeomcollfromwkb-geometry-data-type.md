---
description: STGeomCollFromWKB (tipo de datos geometry)
title: STGeomCollFromWKB (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STGeomCollFromWKB (geometry Data Type)
- STGeomCollFromWKB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STGeomCollFromWKB (geometry Data Type)
ms.assetid: 6c55032c-7f5e-4181-8e67-c0265032db63
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9d080ad31bc41ad64c9a20a7570742a93b7b7000
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190766"
---
# <a name="stgeomcollfromwkb-geometry-data-type"></a>STGeomCollFromWKB (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una instancia de **geometrycollection** a partir de una representación Well-Known Binary (WKB) de Open Geospatial Consortium (OGC).
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
STGeomCollFromWKB ( 'WKB_geometrycollection' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *WKB_geometrycollection*  
 Es la representación WKB de la instancia de **geometrycollection** que se quiere devolver. *WKB_geometrycollection* es una expresión **varbinary(max)**.  
  
 *SRID*  
 Es una expresión **int** que representa el identificador de referencia espacial (SRID) de la instancia de **geometry** que se quiere devolver.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Comentarios  
 El tipo OGC de la instancia de **geometry** devuelta por `STGeomCollFromWKB()` se establece en **GeomCollection**, **MultiPolygon**, **MultiLineString** o **MultiPoint**, en función de la entrada WKB correspondiente.  
  
 Este método produce una excepción FormatException si la entrada no tiene el formato correcto.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa `STGeomCollFromWKB()` para crear una instancia de **geometry**.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomCollFromWKB(0x0107000000020000000103000000010000000400000000000000000014400000000000001440000000000000244000000000000014400000000000002440000000000000244000000000000014400000000000001440010100000000000000000024400000000000002440, 0);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geometría estáticos de OGC](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
