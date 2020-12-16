---
description: MultiPolygon
title: MultiPolygon | Microsoft Docs
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- MultiPolygon geometry subtype [SQL Server]
- geometry subtypes [SQL Server]
ms.assetid: 2c5db358-2a16-49d9-aac5-a74e86813932
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1bcba3499332ca0c41dfd6229bfdb1deca2e7979
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475366"
---
# <a name="multipolygon"></a>MultiPolygon
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  Una instancia de **MultiPolygon** es una colección de cero o más instancias de **Polygon** .  
  
## <a name="polygon-instances"></a>Instancias Polygon  
 En la ilustración siguiente se muestran ejemplos de instancias de **MultiPolygon** .  
  
 ![Ejemplos de instancias MultiPolygon de geometry](../../relational-databases/spatial/media/multipolygon.gif "Ejemplos de instancias MultiPolygon de geometry")  
  
 Como se muestra en la ilustración:  
  
-   La Figura 1 es una instancia de **MultiPolygon** con dos elementos **Polygon** . El límite se define mediante los dos anillos exteriores y los tres interiores.  
  
-   La Figura 2 es una instancia de **MultiPolygon** con dos elementos **Polygon** . El límite se define mediante los dos anillos exteriores y los tres interiores. Los dos elementos **Polygon** forman una intersección en un punto tangente.  
  
### <a name="accepted-instances"></a>Instancias aceptadas  
 Una instancia **MultiPolygon** es una instancia aceptada si se cumple una de las siguientes condiciones.  
  
-   Es una instancia **MultiPolygon** vacía.  
  
-   Todas las instancias que comprenden la instancia **MultiPolygon** son instancias **Polygon** aceptadas. Para obtener más información sobre instancias **Polygon** aceptadas, vea [Polygon](../../relational-databases/spatial/polygon.md).  
  
Los ejemplos siguientes muestran instancias **MultiPolygon** aceptadas.  
  
```sql  
DECLARE @g1 geometry = 'MULTIPOLYGON EMPTY';  
DECLARE @g2 geometry = 'MULTIPOLYGON(((1 1, 1 -1, -1 -1, -1 1, 1 1)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
DECLARE @g3 geometry = 'MULTIPOLYGON(((2 2, 2 -2, -2 -2, -2 2, 2 2)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
```  
  
En el siguiente ejemplo se muestra una instancia MultiPolygon que producirá una excepción `System.FormatException`.  
  
```sql  
DECLARE @g geometry = 'MULTIPOLYGON(((1 1, 1 -1, -1 -1, -1 1, 1 1)),((1 1, 3 1, 3 3)))';  
```  
  
La segunda instancia en MultiPolygon es una instancia LineString y no una instancia Polygon aceptada.  
  
### <a name="valid-instances"></a>Instancias válidas  
 Una instancia **MultiPolygon** es válida si es una instancia **MultiPolygon** vacía o si cumple los siguientes criterios.  
  
1.  Todas las instancias que comprenden la instancia **MultiPolygon** son instancias **Polygon** válidas. Para las instancias **Polygon** válidas, vea [Polygon](../../relational-databases/spatial/polygon.md).  
  
2.  Ninguna de las instancias **Polygon** que comprenden la instancia **MultiPolygon** se superponen.  

En el siguiente ejemplo se muestran dos instancias **MultiPolygon** válidas y una instancia **MultiPolygon** no válida.  
  
```sql  
DECLARE @g1 geometry = 'MULTIPOLYGON EMPTY';  
DECLARE @g2 geometry = 'MULTIPOLYGON(((1 1, 1 -1, -1 -1, -1 1, 1 1)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
DECLARE @g3 geometry = 'MULTIPOLYGON(((2 2, 2 -2, -2 -2, -2 2, 2 2)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid();  
```  
  
`@g2` es válido porque las dos instancias de **Polygon** se tocan solo en un punto tangente. `@g3` no es válido porque los interiores de las dos instancias de **Polygon** se superponen.  
  
## <a name="examples"></a>Ejemplos  
### <a name="example-a"></a>Ejemplo A.
El ejemplo siguiente muestra la creación de una instancia de `geometry``MultiPolygon` y devuelve el valor Well-Known Text (WKT) del segundo componente.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTIPOLYGON(((0 0, 0 3, 3 3, 3 0, 0 0), (1 1, 1 2, 2 1, 1 1)), ((9 9, 9 10, 10 9, 9 9)))');  
SELECT @g.STGeometryN(2).STAsText();  
```  
  
## <a name="example-b"></a>Ejemplo B.
Este ejemplo crea instancias de una instancia de `MultiPolygon` vacía.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTIPOLYGON EMPTY');  
```  
  
## <a name="see-also"></a>Vea también  
 [Polygon](../../relational-databases/spatial/polygon.md)   
 [STArea &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/starea-geometry-data-type.md)   
 [STCentroid &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stcentroid-geometry-data-type.md)   
 [STPointOnSurface &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)   
 [Datos espaciales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
