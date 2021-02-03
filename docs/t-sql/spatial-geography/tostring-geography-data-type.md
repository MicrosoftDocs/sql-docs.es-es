---
description: ToString (tipo de datos geography)
title: ToString (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ToString (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- ToString method
ms.assetid: 045c12fa-8fc6-441a-9500-7021cb4ff13e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d7d2d4b6d4f786c7962155e11b7fe89bbb491a7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187747"
---
# <a name="tostring-geography-data-type"></a>ToString (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve la representación Well-Known Text (WKT) de Open Geospatial Consortium (OGC) de una instancia de **geography** ampliada con los valores Z (elevación) y M (medida) pertenecientes a la instancia.  
  
 Este método del tipo de datos geography admite instancias de **FullGlobe** o instancias espaciales mayores que un hemisferio.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.ToString ()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **nvarchar(max)**  
  
 Tipo de valor devuelto de CLR: **SqlString**  
  
## <a name="remarks"></a>Comentarios  
 Este método devuelve la cadena "Null" cuando se le llama con instancias NULL. En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], el conjunto de resultados posibles en el servidor se ha ampliado a las instancias de **FullGlobe**. Este método devolverá el mismo valor que `AsTextZM()`.  
  
 Este método no es preciso.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una instancia de `LineString` y se usa `ToString()` para devolver la descripción de texto de la instancia.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>Vea también  
 [Métodos extendidos en instancias de geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [AsTextZM &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/astextzm-geography-data-type.md)  
  
  
