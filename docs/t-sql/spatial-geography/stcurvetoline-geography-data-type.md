---
description: STCurveToLine (tipo de datos geography)
title: STCurveToLine (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STCurveToLine_TSQL
- STCurveToLine
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveToLine method (geography)
ms.assetid: 2f863a85-6168-465a-b32f-bb5e3de58dee
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 2c8674e1e7ccafd59fa550fca690553050f68ffb
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479314"
---
# <a name="stcurvetoline-geography-data-type"></a>STCurveToLine (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una aproximación poligonal de una instancia de **geography** que contiene segmentos de arco circulares.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STCurveToLine()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
## <a name="remarks"></a>Observaciones  
 Devuelve una instancia de **LineString** para una instancia de **CircularString** o **CompoundCurve**.  
  
 Devuelve una instancia de **Polygon** para una instancia de **CurvePolygon**.  
  
 Devuelve una copia de instancias de **geography** que no contienen instancias de **CircularString**, **CompoundCurve** o **CurvePolygon**.  
  
 A diferencia de la especificación MM de SQL, este método no usa los valores de z-coordinate en el cálculo de la aproximación poligonal. Los valores de z-coordinate presentes en la instancia de **geography** que realiza la llamada se omiten.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve una instancia `LineString` que es una aproximación poligonal de una instancia `CircularString`:  
  
```
 DECLARE @g1 geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @g2 geography;  
 SET @g2 = @g1.STCurveToLine();  
 SELECT @g1.STNumPoints() AS G1, @g2.STNumPoints() AS G2;
 ```  
  
## <a name="see-also"></a>Vea también  
 [STLength &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/stlength-geography-data-type.md)   
 [STNumPoints &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)   
 [Información general de los tipos de datos espaciales](../../relational-databases/spatial/spatial-data-types-overview.md)  
  
  
