---
description: STIsRing (tipo de datos geometry)
title: STIsRing (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIsRing_TSQL
- STIsRing (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIsRing (geometry Data Type)
ms.assetid: ea0063be-1c74-4cc4-ac6f-b65321ddfa54
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b0f0cb68a9923491e64fa63f52b6d481abb73c14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205008"
---
# <a name="stisring-geometry-data-type"></a>STIsRing (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve 1 si una instancia de **geometry** cumple los siguientes requisitos:
-   Es una instancia de **LineString**.  
-   Está cerrada.  
-   Es sencilla.  
-   Devuelve 0 si la instancia de **LineString** no cumple los requisitos.  

 Para que una instancia de **geometry** esté cerrada y sea sencilla, [STIsClosed()](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md) y [STIsSimple()](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md) deben devolver 1 cuando se les invoca desde la instancia. Para determinar el tipo de instancia de una instancia de **geometry**, use [STGeometryType()](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STIsRing ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Tipo de valor devuelto de CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Observaciones  
 Este método devuelve NULL si la instancia de **LineString** no es un polígono.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una instancia de `LineString` y se usa `STIsRing()` para comprobar si la instancia es un anillo.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0, 0 0)', 0);  
SELECT @g.STIsRing();  
```  
  
## <a name="see-also"></a>Consulte también  
 [STIsClosed &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STGeometryType &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)   
 [STIsSimple &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md)   
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

