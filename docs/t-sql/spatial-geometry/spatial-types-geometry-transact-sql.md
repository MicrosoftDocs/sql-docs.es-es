---
description: 'Tipos espaciales: geometry (Transact-SQL)'
title: geometry (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- geometry
dev_langs:
- TSQL
helpviewer_keywords:
- spatial data types [SQL Server]
- geometry data type [SQL Server], Transact-SQL
ms.assetid: 3fefdf7b-f931-404c-821c-82c0375eaf51
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 4889317b7492a5f51657d38cf2e71001f890e048
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180149"
---
# <a name="spatial-types---geometry-transact-sql"></a>Tipos espaciales: geometry (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  El tipo de datos espaciales planares, **geometry**, se implementa como un tipo de datos CLR (Common Language Runtime) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Este tipo representa datos en un sistema de coordenadas euclídeo (plano).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite un conjunto de métodos para el tipo de datos espaciales **geography**. Este métodos incluyen a su vez métodos de **geometry** definidos por el estándar Open Geospatial Consortium (OGC) y un conjunto de extensiones de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para dicho estándar.  
 
 La tolerancia de error para los métodos geometry puede ser de hasta 1,0e-7 * extensiones. Las extensiones hacen referencia a la distancia máxima aproximada entre los puntos del objeto **geometry**.
  
## <a name="registering-the-geometry-type"></a>Registrar el tipo de datos Geometry  
 El tipo **geometry** está predefinido y está disponible en cada base de datos. Puede crear columnas de tabla de tipo **geometry** y operar con los datos **geometry** de la misma manera que con los demás tipos CLR. Se puede utilizar en columnas calculadas persistentes y no persistentes.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-showing-how-to-add-and-query-geometry-data"></a>A. Mostrar cómo agregar y consultar datos de tipo geometry  
 En los dos ejemplos siguientes se muestra cómo agregar y consultar datos de geometry. En el primer ejemplo se crea una tabla con una columna de identidad y una columna de tipo `geometry`, `GeomCol1`. Una tercera columna representa la columna de tipo `geometry` en su representación Well-Known Text (WKT) de Open Geospatial Consortium (OGC) y utiliza el método `STAsText()` . A continuación se insertan dos filas: una que contiene una instancia de `LineString` de `geometry`y otra que contiene una instancia de `Polygon` .  
  
```sql 
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
    ( id int IDENTITY (1,1),  
    GeomCol1 geometry,   
    GeomCol2 AS GeomCol1.STAsText() );  
GO  
  
INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('LINESTRING (100 100, 20 180, 180 180)', 0));  
  
INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('POLYGON ((0 0, 150 0, 150 150, 0 150, 0 0))', 0));  
GO  
```  
  
### <a name="b-returning-the-intersection-of-two-geometry-instances"></a>B. Devolver la intersección de dos instancias de geometry  
 En el segundo ejemplo se usa el método `STIntersection()` para devolver los puntos de intersección de las dos instancias de `geometry` insertadas previamente.  
  
```sql  
DECLARE @geom1 geometry;  
DECLARE @geom2 geometry;  
DECLARE @result geometry;  
  
SELECT @geom1 = GeomCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geom2 = GeomCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geom1.STIntersection(@geom2);  
SELECT @result.STAsText();  
```  
  
### <a name="c-using-geometry-in-a-computed-column"></a>C. Usar geometry en una columna calculada  
 En el ejemplo siguiente se crea una tabla con una columna calculada persistente mediante un tipo **geometry**.  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable  
(  
    locationId int IDENTITY(1,1),  
    location geometry,  
    deliveryArea as location.STBuffer(10) persisted  
)  
```  
  
## <a name="see-also"></a>Vea también  
  [Datos espaciales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
