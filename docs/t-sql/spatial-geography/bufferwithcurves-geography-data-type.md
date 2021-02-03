---
description: BufferWithCurves (tipo de datos de geografía)
title: BufferWithCurves (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BufferWithCurves
- BufferWithCurves_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geography)
ms.assetid: abf0a11c-c99c-4faa-bf80-3ae8e04d7bfb
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: e34c83606e632657ab0e4846a6444ecb0a5050ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200235"
---
# <a name="bufferwithcurves-geography-data-type"></a>BufferWithCurves (tipo de datos de geografía)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una instancia de **geography** que representa el conjunto de todos los puntos cuya distancia desde la instancia de **geography** que realiza la llamada es menor o igual que el parámetro *distance*.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.BufferWithCurves ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *distance*  
 Es un valor de tipo **float** que indica la distancia máxima a la que los puntos que forman el búfer pueden estar de la instancia de geography.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
## <a name="exceptions"></a>Excepciones  
 En los siguientes casos, se producirá una excepción **ArgumentException**.  
  
-   No se pasa ningún parámetro al método: `@g.BufferWithCurves()`  
  
-   Se pasa al método un parámetro no numérico: `@g.BufferWithCurves('a')`  
  
-   **NULL** se pasa al método, como en `@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>Comentarios  
 En la siguiente tabla se muestran los resultados devueltos para distintos valores de distancia.  
  
|Valor de distancia|Dimensiones de tipo|Tipo espacial devuelto|  
|--------------------|---------------------|---------------------------|  
|distancia < 0|Cero o uno|Instancia de **GeometryCollection** vacía|  
|distancia \< 0|Dos o más|Instancia de **CurvePolygon** o **GeometryCollection** con un búfer negativo.<br /><br /> Nota: Un búfer negativo puede crear una instancia de **GeometryCollection** vacía.|
|distancia = 0|Todas las dimensiones|Copia de la instancia de **geography** que hace la llamada|  
|distancia > 0|Todas las dimensiones|Instancia de **CurvePolygon** o **GeometryCollection**|  
  
> [!NOTE]  
>  Puesto que *distance* es de tipo **float**, un valor muy pequeño puede resultar igual a cero en los cálculos.  Cuando ocurre esto, se devuelve una copia de la instancia de **geography** que llama.  
  
 Si se pasa al método un parámetro **string**, se convertirá en un valor de tipo **float** o producirá una excepción `ArgumentException`.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geography-instance"></a>A. Llamada a BufferWithCurves() con un valor de parámetro < 0 en una instancia de geografía de una dimensión  
 En el siguiente ejemplo se devuelve una instancia de `GeometryCollection` vacía:  
  
 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(-1).ToString();
``` 
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geography-instance"></a>B. Llamada a BufferWithCurves() con un valor de parámetro < 0 en una instancia de geografía de dos dimensiones  
 En el siguiente ejemplo se devuelve una instancia de `CurvePolygon` con un búfer negativo:  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. Llamada a BufferWithCurves() con un valor de parámetro < 0 que devuelve una instancia de GeometryCollection vacía  
 En el siguiente ejemplo se muestra lo que sucede cuando el parámetro *distance* es igual a -2:  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 Esta instrucción **SELECT** devuelve `GEOMETRYCOLLECTION EMPTY`.  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. Llamada a BufferWithCurves() con un valor de parámetro = 0  
 En el siguiente ejemplo se devuelve una copia de la instancia de **geography** que realiza la llamada:  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. Llamada a BufferWithCurves() con un valor de parámetro distinto de cero que es sumamente pequeño  
 En el siguiente ejemplo también se devuelve una copia de la instancia de **geography** que realiza la llamada:  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. Llamada a BufferWithCurves() con un valor de parámetro > 0  
 En el ejemplo siguiente se devuelve una instancia de `CurvePolygon`:  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
### <a name="g-passing-a-valid-string-parameter"></a>G. Se pasa un parámetro de cadena válido  
 En el ejemplo siguiente se devuelve la misma instancia de `CurvePolygon` mencionada anteriormente, pero se pasa un parámetro de cadena al método:  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves('2').ToString();
```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. Se pasa un parámetro de cadena no válido  
 En el siguiente ejemplo se producirá un error:  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)'  
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 Observe que en los dos ejemplos anteriores se pasa un literal de cadena al método `BufferWithCurves()`. El primer ejemplo funciona porque el literal de cadena se puede convertir en un valor numérico. Sin embargo, el segundo ejemplo inicia una excepción `ArgumentException`.  
  
## <a name="see-also"></a>Vea también  
 [Métodos extendidos en instancias de geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [BufferWithCurves &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md)  
  
  
