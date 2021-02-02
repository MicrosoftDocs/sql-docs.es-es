---
description: STLineFromText (tipo de datos geometry)
title: STLineFromText (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STLineFromText (geometry Data Type)
- STLineFromText_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STLineFromText (geometry Data Type)
ms.assetid: 430508ad-207b-4dee-a4d1-4ddf25e6b4a9
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 3c5c234cb63698abdc31bc6e482feacde131289c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158357"
---
# <a name="stlinefromtext-geometry-data-type"></a>STLineFromText (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una instancia de **geometry** a partir de una representación Well-Known Text (WKT) de Open Geospatial Consortium (OGC), ampliada con los valores Z (elevación) y M (medida) pertenecientes a la instancia.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
STLineFromText ( 'linestring_tagged_text' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *linestring_tagged_text*  
 Es la representación WKT de la instancia de **geometryLineString** que quiere devolver. *linestring_tagged_text* es una expresión **nvarchar(max)**.  
  
 *SRID*  
 Es una expresión de tipo **int** que representa el identificador de referencia espacial (SRID) de la instancia de **geometryLineString** que quiere devolver.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
 Tipo OGC: **LineString**  
  
## <a name="remarks"></a>Observaciones  
Este método produce una excepción **FormatException** si la entrada no tiene el formato correcto. No se admiten las características simples de la notación de WKT de geometría medida y de tres dimensiones de Open Geospatial Consortium (OGC) para la especificación de SQL, versión 1.2.1. Vea los ejemplos de la representación compatible de los valores Z (elevación) y M (medida).
  
## <a name="examples"></a>Ejemplos  
 En los ejemplos siguientes se usa `STLineFromText()` para crear una instancia de `geometry`.

### <a name="example-1-two-dimension-geometry-wkt"></a>Ejemplo 1: WKT de geometría de dos dimensiones
```  
DECLARE @g geometry;  
SET @g = geometry::STLineFromText('LINESTRING (100 100, 200 200)', 0);  
SELECT @g.ToString();  
```  
  
### <a name="example-2-three-dimension-geometry-wkt"></a>Ejemplo 2: WKT de geometría de tres dimensiones
```  
DECLARE @g geometry;  
SET @g = geometry::STLineFromText('LINESTRING (100 100 100, 200 200 200)', 0);  
SELECT @g.ToString();  
``` 

### <a name="example-3-two-dimension-measured-geometry-wkt"></a>Ejemplo 3: WKT de geometría medida de dos dimensiones
```  
DECLARE @g geometry;  
SET @g = geometry::STLineFromText('LINESTRING (100 100 NULL 100, 200 200 NULL 200)', 0);  
SELECT @g.ToString();  
``` 

### <a name="example-4-three-dimension-measured-geometry-wkt"></a>Ejemplo 4: WKT de geometría medida de tres dimensiones
```  
DECLARE @g geometry;  
SET @g = geometry::STLineFromText('LINESTRING (100 100 100 100, 200 200 200 200)', 0);  
SELECT @g.ToString();  
``` 
## <a name="see-also"></a>Consulte también  
 [Métodos de geometría estáticos de OGC](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
  

