---
description: BufferWithCurves (tipo de datos Geometry)
title: BufferWithCurves (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geometry)
ms.assetid: 8ffaba3f-d2dd-4e57-9f41-3ced9f14b600
author: MladjoA
ms.author: mlandzic
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 18691c1a3e02d93d7c98a6b290672af5c77f15c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347531"
---
# <a name="bufferwithcurves-geometry-data-type"></a>BufferWithCurves (tipo de datos Geometry)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Devuelve una instancia de **geometry** que representa el conjunto de todos los puntos cuya distancia desde la instancia de **geometry** que realiza la llamada es menor o igual que el parámetro *distance*.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
.BufferWithCurves ( distance )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos  
 *distance*  
 Es un valor de tipo **float** que indica la distancia máxima a la que los puntos que forman el búfer pueden estar de la instancia de **geometry**.  
  
## <a name="return-types"></a>Tipos de valor devuelto
Tipo de valor devuelto de SQL Server: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="exceptions"></a>Excepciones  
 En los siguientes casos, se producirá una excepción **ArgumentException**.  
  
-   No se pasa ningún parámetro al método, como `@g.BufferWithCurves()`  
  
-   Se pasa al método un parámetro no numérico, como `@g.BufferWithCurves('a')`  
  
-   **NULL** se pasa al método, como en `@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>Observaciones  
 En la siguiente ilustración se muestra un ejemplo de una instancia de geometría devuelta por este método.  
  
 ![Diagrama que muestra el ejemplo de una instancia de geometría devuelta por este método.](../../t-sql/spatial-geometry/media/bufferedcurve.gif)
  
 En la siguiente tabla se muestran los resultados devueltos para distintos valores de distancia.  
  
|Valor de distancia|Dimensiones de tipo|Tipo espacial devuelto|  
|--------------------|---------------------|---------------------------|  
|distancia < 0|Cero o uno|Instancia de **GeometryCollection** vacía|  
|distancia < 0|Dos o más|Instancia de **CurvePolygon** o **GeometryCollection** con un búfer negativo. **Nota:** un búfer negativo puede crear una instancia de **GeometryCollection** vacía.|  
|distancia = 0|Todas las dimensiones|Copia de la instancia de **geometry** que hace la llamada|  
|distancia > 0|Todas las dimensiones|Instancia de **CurvePolygon** o **GeometryCollection**|  
  
> [!NOTE]  
>  Puesto que *distance* es de tipo **float**, un valor muy pequeño puede resultar igual a cero en los cálculos. Cuando ocurre esto, se devuelve una copia de la instancia de **geometry** que llama. Vea [float y real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Un búfer negativo quita todos los puntos incluidos en la distancia dada del límite de la geometría. En la siguiente ilustración se muestra un búfer negativo como el área del círculo sombreada en claro. La línea de puntos es el límite del polígono original y la línea continua es el límite del polígono resultante.  
  
 Si se pasa al método un parámetro **string**, se convertirá en un valor de tipo **float** o producirá una excepción `ArgumentException`.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geometry-instance"></a>A. Llamada a BufferWithCurves() con un valor de parámetro < 0 en una instancia de geometría de una dimensión  
 En el siguiente ejemplo se devuelve una instancia de `GeometryCollection` vacía:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(-1).ToString(); 
 ```
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geometry-instance"></a>B. Llamada a BufferWithCurves() con un valor de parámetro < 0 en una instancia de geometría de dos dimensiones  
 En el siguiente ejemplo se devuelve una instancia de `CurvePolygon` con un búfer negativo:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. Llamada a BufferWithCurves() con un valor de parámetro < 0 que devuelve una instancia de GeometryCollection vacía  
 En el siguiente ejemplo se muestra lo que sucede cuando el parámetro *distance* es igual a -2:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 Esta instrucción **SELECT** devuelve `GEOMETRYCOLLECTION EMPTY`.  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. Llamada a BufferWithCurves() con un valor de parámetro = 0  
 En el siguiente ejemplo se devuelve una copia de la instancia de **geometry** que realiza la llamada:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. Llamada a BufferWithCurves() con un valor de parámetro distinto de cero que es sumamente pequeño  
 En el siguiente ejemplo también se devuelve una copia de la instancia de **geometry** que realiza la llamada:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 DECLARE @distance float = 1e-20; 
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. Llamada a BufferWithCurves() con un valor de parámetro > 0  
 En el ejemplo siguiente se devuelve una instancia de `CurvePolygon`:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
  
### <a name="g-passing-a-valid-string-parameter"></a>G. Se pasa un parámetro de cadena válido  
 En el ejemplo siguiente se devuelve la misma instancia de `CurvePolygon` mencionada anteriormente, pero se pasa un parámetro de cadena al método:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves('2').ToString();
 ```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. Se pasa un parámetro de cadena no válido  
 En el siguiente ejemplo se producirá un error:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)' 
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 Observe que en los dos ejemplos anteriores se pasa un literal de cadena al método `BufferWithCurves()`. El primer ejemplo funciona porque el literal de cadena se puede convertir en un valor numérico. Sin embargo, el segundo ejemplo inicia una excepción `ArgumentException`.  
  
### <a name="i-calling-bufferwithcurves-on-multipoint-instance"></a>I. Llamada a BufferWithCurves() en una instancia de tipo MultiPoint  
 En el siguiente ejemplo se devuelven dos instancias de `GeometryCollection` y una de `CurvePolygon`:  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.BufferWithCurves(1).ToString(); 
 SELECT @g.BufferWithCurves(1.5).ToString(); 
 SELECT @g.BufferWithCurves(1.6).ToString();
 ```  
  
 Las primeras dos instrucciones **SELECT** devuelven una instancia de `GeometryCollection` porque el parámetro *distance* es menor o igual a la mitad de la distancia entre los dos puntos (1 1) y (1 4). La tercera instrucción **SELECT** devuelve una instancia de `CurvePolygon` porque las instancias almacenadas en búfer de los dos puntos (1 1) y (1 4) se superponen.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos extendidos en instancias de geometry](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
 
