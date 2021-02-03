---
description: STBuffer (tipo de datos geometry)
title: STBuffer (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STBuffer (geometry Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geometry Data Type)
ms.assetid: ca6bf2dc-1d38-4503-b87e-f2ea033d36ba
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 787612ae8cdb2cc5751818c31f111831e4013cbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187223"
---
# <a name="stbuffer-geometry-data-type"></a>STBuffer (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve un objeto geométrico que representa la unión de todos los puntos cuya distancia desde una instancia de **geometry** es menor o igual que un valor especificado.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STBuffer ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *distance*  
 Es un valor de tipo **float** (**double** en .NET Framework) que especifica la distancia desde la instancia de geometry en torno a la cual se puede calcular el búfer.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Comentarios  
 `STBuffer()` calcula un búfer como [BufferWithTolerance](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md) mediante la especificación de *tolerance* = distance \* .001 y *relative* = **false**.  
  
 Cuando *distance* > 0, se devuelve una instancia de **Polygon** o **MultiPolygon**.  
  
> [!NOTE]  
>  Puesto que distance es de tipo **float**, un valor muy pequeño puede resultar igual a cero en los cálculos.  Cuando ocurre esto, se devuelve una copia de la instancia de **geometry** que llama.  Vea [float y real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Cuando *distance* = 0, se devuelve una copia de la instancia de **geometry** que llama.  
  
 Cuando *distance* < 0:  
  
-   Se devuelve una instancia de **GeometryCollection** vacía si las dimensiones de la instancia son 0 o 1.  
  
-   se devuelve un búfer negativo si las dimensiones de la instancia son 2 o más.  
  
    > [!NOTE]  
    >  Un búfer negativo también puede crear una instancia de **GeometryCollection** vacía.  
  
 Un búfer negativo quita todos los puntos incluidos en la distancia dada del límite de la geometría.  
  
 El error entre el búfer teórico y el calculado es max(tolerance, extents * 1.E-7), donde tolerance = distance \* ,001. Para más información sobre las extensiones, vea la [referencia del método del tipo de datos geometry](./spatial-types-geometry-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-calling-stbuffer-with-parameter_value--0-on-one-dimensional-geometry-instance"></a>A. Llamar a STBuffer() con parameter_value < 0 en una instancia de geometría dimensional  
 En el siguiente ejemplo se devuelve una instancia de `GeometryCollection` vacía:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="b-calling-stbuffer-with-parameter_value--0-on-a-polygon-instance"></a>B. Llamar a STBuffer() con parameter_value < 0 en una instancia de Polygon  
 En el siguiente ejemplo se devuelve una instancia de `Polygon` con un búfer negativo:  
  
```
 DECLARE @g geometry = 'POLYGON((1 1, 1 5, 5 5, 5 1, 1 1))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="c-calling-stbuffer-with-parameter_value--0-on-a-curvepolygon-instance"></a>C. Llamar a STBuffer() con parameter_value < 0 en una instancia de CurvePolygon  
 En el siguiente ejemplo se devuelve una instancia de `Polygon` con un búfer negativo desde una instancia de `CurvePolygon`:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
> [!NOTE]  
>  Se devuelve una instancia de `Polygon` en lugar de una instancia de `CurvePolygon`.  Para devolver una instancia de `CurvePolygon`, vea [BufferWithCurves &#40;tipo de datos Geometry&#41;](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md).  
  
### <a name="d-calling-stbuffer-with-a-negative-parameter-value-that-returns-an-empty-instance"></a>D. Llamar a STBuffer() con un valor de parámetro negativo que devuelve una instancia vacía  
 En el ejemplo siguiente se muestra lo que sucede cuando el parámetro *distance* es igual a -2 para el ejemplo anterior.  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-2).ToString();
 ```  
  
 Esta instrucción **SELECT** devuelve `GEOMETRYCOLLECTION EMPTY.`.  
  
### <a name="e-calling-stbuffer-with-parameter_value--0"></a>E. Llamar a STBuffer() con parameter_value = 0  
 En el siguiente ejemplo se devuelve una copia de la instancia de `geometry` que realiza la llamada:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(0).ToString();
 ```  
  
### <a name="f-calling-stbuffer-with-a-non-zero-parameter-value-that-is-extremely-small"></a>F. Llamar a STBuffer() con un valor de parámetro distinto de cero que es muy pequeño  
 En el siguiente ejemplo también se devuelve una copia de la instancia de `geometry` que realiza la llamada:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.STBuffer(@distance).ToString();
 ```  
  
### <a name="g-calling-stbuffer-with-parameter_value--0"></a>G. Llamar a STBuffer() con parameter_value > 0  
 En el ejemplo siguiente se devuelve una instancia de `Polygon`:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(2).ToString();
 ```  
  
### <a name="h-calling-stbuffer-with-a-string-parameter-value"></a>H. Llamar a STBuffer() con un valor de parámetro de cadena  
 En el ejemplo siguiente se devuelve la misma instancia de `Polygon` mencionada anteriormente, pero se pasa un parámetro de cadena al método:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('2').ToString();
 ```  
  
 En el siguiente ejemplo se producirá un error:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('a').ToString();
 ```  
  
> [!NOTE]  
>  En los dos ejemplos anteriores se pasó un literal de cadena a `STBuffer()`.  El primer ejemplo funciona porque el literal de cadena se puede convertir en un valor numérico. Sin embargo, el segundo ejemplo inicia una excepción `ArgumentException`.  
  
### <a name="i-calling-stbuffer-on-a-multipoint-instance"></a>I. Llamar a STBuffer() en una instancia de MultiPoint  
 En el siguiente ejemplo se devuelven dos instancias de `MultiPolygon` y una de `Polygon`:  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.STBuffer(1).ToString(); 
 SELECT @g.STBuffer(1.5).ToString(); 
 SELECT @g.STBuffer(1.6).ToString();
 ```  
  
 Las primeras dos instrucciones **SELECT** devuelven una instancia de `MultiPolygon` porque el parámetro *distance* es menor o igual a la mitad de la distancia entre los dos puntos (1 1) y (1 4). La tercera instrucción **SELECT** devuelve una instancia de `Polygon` porque las instancias almacenadas en búfer de los dos puntos (1 1) y (1 4) se superponen.  
  
## <a name="see-also"></a>Vea también  
 [BufferWithTolerance &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md)   
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
