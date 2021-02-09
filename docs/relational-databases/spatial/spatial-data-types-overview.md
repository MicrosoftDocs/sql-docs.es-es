---
description: Información general de los tipos de datos espaciales
title: Información general de los tipos de datos espaciales | Microsoft Docs
ms.date: 07/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- geometry data type [SQL Server], understanding
- geography data type [SQL Server], spatial data
- planar spatial data [SQL Server], geometry data type
- spatial data types [SQL Server]
ms.assetid: 1615db50-69de-4778-8be6-4e058c00ccd4
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 87ed2882d8276b3a92add3349b73bf38fe821dc9
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589317"
---
# <a name="spatial-data-types-overview"></a>Información general de los tipos de datos espaciales

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  
Hay dos tipos de datos espaciales. El tipo de datos **geometry** admite datos planares o euclidianos (planisferio). El tipo de datos **geometry** se ajusta tanto a las *características simples de Open Geospatial Consortium (OGC) para la especificación SQL* versión 1.1.0 como a SQL MM (estándar ISO).
Además, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el tipo de datos **geography**, que almacena datos elipsoidales (tierra redonda), como coordenadas de latitud y longitud GPS.

## <a name="spatial-data-objects"></a>Objetos de datos espaciales

Los tipos de datos **geometry** y **geography** admiten 16 objetos de datos espaciales o tipos de instancia. Pero solo se pueden *crear instancias* de 11 de estos tipos de instancia; puede crear y trabajar con estas instancias (o crear instancias de ellas) en una base de datos. Estas instancias obtienen determinadas propiedades de sus tipos de datos primarios.

La figura siguiente muestra la jerarquía de geometry en la que se basan los tipos de datos **geometry** y **geography**. Los tipos a partir de los que pueden crearse instancias de **geometry** y **geography** se indican en azul.  

![geom_hierarchy](../../relational-databases/spatial/media/geom-hierarchy.png)

Existe un tipo adicional a partir del que pueden crearse instancias para el tipo de datos geography: **FullGlobe**. Los tipos **geometry** y **geography** pueden reconocer una instancia concreta siempre y cuando se trate de una instancia bien formada, aunque no se haya definido explícitamente. Por ejemplo, si define una instancia **Point** usando explícitamente el método STPointFromText(), **geometry** y **geography** reconocen la instancia como **Point**, con tal de que la entrada de método esté bien formada. Si define la misma instancia mediante el método `STGeomFromText()` , los tipos de datos **geometry** y **geography** reconocen la instancia como **Point**.  

Los subtipos de los tipos Geometry y Geography se dividen en tipos simples y de colección.  Algunos métodos como `STNumCurves()` funcionan solo con los tipos simples.  

Los tipos simples son:

- [Point](../../relational-databases/spatial/point.md)  
- [LineString](../../relational-databases/spatial/linestring.md)  
- [CircularString](../../relational-databases/spatial/circularstring.md)  
- [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)  
- [Polygon](../../relational-databases/spatial/polygon.md)  
- [CurvePolygon](../../relational-databases/spatial/curvepolygon.md)  

Los tipos de colección son:

- [MultiPoint](../../relational-databases/spatial/multipoint.md)  
- [MultiLineString](../../relational-databases/spatial/multilinestring.md)  
- [MultiPolígono](../../relational-databases/spatial/multipolygon.md)  
- [GeometryCollection](../../relational-databases/spatial/geometrycollection.md)  

## <a name="geometry-and-geography-data-type-differences"></a>Diferencias entre los tipos de datos geometry y geography

A menudo, los dos tipos de datos espaciales se comportan de forma similar. Hay algunas diferencias importantes en el modo en que se almacenan y se manipulan los datos.  

### <a name="how-connecting-edges-are-defined"></a>Definición de bordes de conexión

Los datos de definición para los tipos **LineString** y **Polygon** son solo los vértices. El borde de conexión entre dos vértices en un tipo Geometría es una línea recta. Sin embargo, el borde de conexión entre dos vértices en un tipo de geografía es un arco elíptico grande corto entre los dos vértices. Una elipse grande es la intersección del elipsoide con un plano a través de su centro. Un arco elíptico grande es un segmento de arco de la elipse grande.  

### <a name="how-circular-arc-segments-are-defined"></a>Definición de los segmentos de arco circular

Los segmentos de arco circular para los tipos Geometry se definen en el plano de coordenadas cartesianas XY (se ignoran los valores Z). Los segmentos de arco circular para los tipos Geography se definen mediante segmentos de curva en una esfera de referencia. Los paralelos de la esfera de referencia se pueden definir mediante dos arcos circulares complementarios en los que los puntos de ambos arcos tienen un ángulo de latitud constante.  

### <a name="measurements-in-spatial-data-types"></a>Medidas en tipos de datos espaciales

En el sistema plano, o de tierra plana, las medidas de distancias y las áreas se proporcionan en la misma unidad de medida que las coordenadas. Usando el tipo de datos **geometry**, la distancia entre (2, 2) y (5, 6) es 5 unidades, independientemente de las unidades usadas.  

En el sistema elipsoidal, o de tierra redonda, las coordenadas se proporcionan en grados de latitud y longitud. Pero las longitudes y las áreas normalmente se miden en metros y metros cuadrados, aunque la medida puede depender del [identificador de referencia espacial](./spatial-reference-identifiers-srids.md) de la instancia de **geography**. La unidad de medida más común para el tipo de datos **geography** es el metro.  

### <a name="orientation-of-spatial-data"></a>Orientación de datos espaciales

La orientación de anillo de un polígono no es un factor importante en el sistema plano. Las *características simples de OGC para la especificación de SQL* no dictan una ordenación de anillos y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no exige la ordenación de anillos.  

En un sistema elipsoidal, un polígono sin una orientación no tiene ningún significado, o es ambiguo. Por ejemplo, ¿un anillo alrededor del ecuador describe el hemisferio norte o el hemisferio sur? Si usamos el tipo de datos **geography** para almacenar la instancia espacial, debemos especificar la orientación del anillo y describir con precisión la ubicación de la instancia.

El interior del polígono de un sistema elipsoidal se define a partir de la "regla del lado izquierdo": si recorriéramos el anillo de un polígono de tipo geography siguiendo los puntos en el orden en que aparecen, el área de la izquierda se considera el interior del polígono y el área de la derecha, el exterior.

Cuando el nivel de compatibilidad es 100 o menor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , el tipo de datos **geography** tiene las siguientes restricciones:

- Cada instancia de **geography** debe ajustarse en un hemisferio único. No se puede almacenar ningún objeto espacial mayor que un hemisferio.

- Cualquier instancia de **geography** de una representación Well-Known Text (WKT) o Well-Known Binary (WKB) de Open Geospatial Consortium (OGC) que genera un objeto mayor que un hemisferio inicia una **ArgumentException**.  

- Los métodos del tipo de datos **geography** que requieren la entrada de dos instancias de **geography** , como STIntersection(), STUnion(), STDifference() y STSymDifference(), devolverán NULL si los resultados de los métodos no se ajustan en un hemisferio único. STBuffer() también devolverá NULL si la salida supera un único hemisferio.  

En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], **FullGlobe** es un tipo especial de Polygon que abarca el mundo entero. Tiene un área, pero no tiene bordes ni vértices.  

### <a name="outer-and-inner-rings-in-geography-data-type"></a>Anillos externos e internos en el tipo de datos `geography`

Las *características simples de OGC para la especificación de SQL* analizan los anillos externos e internos, pero esta distinción tiene poco sentido para el tipo de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]geography **de**; se puede tomar cualquier anillo de un polígono como anillo externo.  

Para obtener más información sobre las especificaciones de OGC, vea los recursos siguientes:

- [Especificaciones de OGC, Acceso a características simples, Parte 1 - Arquitectura común](https://go.microsoft.com/fwlink/?LinkId=93627)  
- [Especificaciones de OGC, acceso a características simples, parte 2: opciones de SQL](https://go.microsoft.com/fwlink/?LinkId=93628)  

## <a name="circular-arc-segments"></a>Segmentos de arco circular

Tres tipos de los que se pueden crear instancias pueden tomar segmentos de arco circular: **CircularString**, **CompoundCurve** y **CurvePolygon**.  Un segmento de arco circular se define mediante tres puntos en un plano bidimensional y el tercer punto no puede ser igual que el primero. Estos son algunos ejemplos de segmentos de arco circular:

![circular_arc_segments](../../relational-databases/spatial/media/7e382f76-59da-4b62-80dc-caf93e637c14.gif)

Los primeros dos ejemplos muestran segmentos típicos de arco circular. Observe que los tres puntos se encuentran en el perímetro de un círculo.  

Los otros dos ejemplos muestran cómo se puede definir el segmento de una línea como segmento de arco circular. Los tres puntos continúan siendo necesarios para definir el segmento de arco circular, a diferencia de un segmento de línea normal, que se puede definir mediante dos puntos únicamente.

Los métodos que funcionan en tipos de segmentos de arco circular usan segmentos de línea recta para aproximarse al arco circular. El número de segmentos de línea utilizado para aproximarse al arco dependerá de su longitud y curvatura. Los valores Z se pueden almacenar para cada uno de los tipos de segmentos de arco circular, pero no se usarán los valores Z en los cálculos.  

> [!NOTE]  
> Si se dan valores Z para los segmentos de arco circular, deben ser iguales para todos los puntos del segmento de arco circular para que se acepte como entrada. Por ejemplo: se acepta `CIRCULARSTRING(0 0 1, 2 2 1, 4 0 1)` , pero no se acepta `CIRCULARSTRING(0 0 1, 2 2 2, 4 0 1)` .  

### <a name="linestring-and-circularstring-comparison"></a>Comparación de LineString y CircularString

En este ejemplo se muestra cómo almacenar triángulos isósceles idénticos tanto con una instancia de **LineString** como con una instancia de **CircularString** :  

```sql
DECLARE @g1 geometry;
DECLARE @g2 geometry;
SET @g1 = geometry::STGeomFromText('LINESTRING(1 1, 5 1, 3 5, 1 1)', 0);
SET @g2 = geometry::STGeomFromText('CIRCULARSTRING(1 1, 3 1, 5 1, 4 3, 3 5, 2 3, 1 1)', 0);
IF @g1.STIsValid() = 1 AND @g2.STIsValid() = 1
  BEGIN
      SELECT @g1.ToString(), @g2.ToString()
      SELECT @g1.STLength() AS [LS Length], @g2.STLength() AS [CS Length]
  END
```  

Tenga en cuenta que una instancia de **CircularString** requiere siete puntos para definir el triángulo. La instancia de **LineString** requiere solo cuatro puntos para definir el triángulo. Esto se debe a que una instancia de **CircularString** almacena los segmentos de arco circular y no los de línea. Los lados del triángulo almacenado en la instancia de **CircularString** son ABC, CDE y EFA. Los lados del triángulo almacenado en la instancia de **LineString** son AC, CE y EA.  

Considere el ejemplo siguiente:  

```sql
SET @g1 = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 4 0)', 0);
SET @g2 = geometry::STGeomFromText('CIRCULARSTRING(0 0, 2 2, 4 0)', 0);
SELECT @g1.STLength() AS [LS Length], @g2.STLength() AS [CS Length];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
LS Length    CS Length
5.65685...   6.28318...
```

Las instancias de **CircularString** usan menos puntos para almacenar límites curvos con mayor precisión que las instancias de **LineString** . Las instancias de **CircularString** son útiles para almacenar límites circulares, como un radio de búsqueda de 20 millas desde un punto específico. Las instancias de **LineString** funcionan bien para almacenar límites que son lineales como un bloque de ciudad cuadrado.  

### <a name="linestring-and-compoundcurve-comparison"></a>Comparación de LineString y CompoundCurve

En los siguientes ejemplos de código se muestra cómo almacenar la misma figura con instancias de **LineString** y **CompoundCurve** :

```sql
SET @g = geometry::Parse('LINESTRING(2 2, 4 2, 4 4, 2 4, 2 2)');
SET @g = geometry::Parse('COMPOUNDCURVE((2 2, 4 2), (4 2, 4 4), (4 4, 2 4), (2 4, 2 2))');
SET @g = geometry::Parse('COMPOUNDCURVE((2 2, 4 2, 4 4, 2 4, 2 2))');
```

En los ejemplos anteriores, una instancia de **LineString** o un instancia de **CompoundCurve** pudieron almacenar la figura.  En el siguiente ejemplo se usa **CompoundCurve** para almacenar un segmento de gráfico circular:  

```sql
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(2 2, 1 3, 0 2),(0 2, 1 0, 2 2))');  
```  

Una instancia de **CompoundCurve** puede almacenar directamente el segmento de arco circular (2 2, 1 3, 0 2), mientras que una instancia de **LineString** tendría que convertir la curva en varios segmentos de línea más pequeños.  

### <a name="circularstring-and-compoundcurve-comparison"></a>Comparación de CircularString y CompoundCurve

En el siguiente ejemplo de código se muestra cómo se puede almacenar el segmento de gráfico circular en una instancia de **CircularString** :  

```sql
DECLARE @g geometry;
SET @g = geometry::Parse('CIRCULARSTRING( 0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)');
SELECT @g.ToString(), @g.STLength();
```

Para almacenar el segmento de gráfico circular mediante una instancia de **CircularString** se requieren tres puntos para cada segmento de línea. Si no se conoce un punto intermedio, se debe calcular o se debe duplicar el extremo del segmento de línea, como muestra el siguiente fragmento de código:  

```sql
SET @g = geometry::Parse('CIRCULARSTRING( 0 0, 3 6.3246, 3 6.3246, 0 7, -3 6.3246, 0 0, 0 0)');
```

Las instancias de **CompoundCurve** permiten componentes tanto de **LineString** como de **CircularString** para que solo se necesiten conocer dos puntos en los segmentos de línea del segmento de gráfico circular.  En este ejemplo de código se muestra cómo usar **CompoundCurve** para almacenar la misma figura:

```sql
DECLARE @g geometry;
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING( 3 6.3246, 0 7, -3 6.3246), (-3 6.3246, 0 0, 3 6.3246))');
SELECT @g.ToString(), @g.STLength();
```

### <a name="polygon-and-curvepolygon-comparison"></a>Comparación de Polygon y CurvePolygon

Las instancias de **CurvePolygon** pueden usar las instancias de **CircularString** y **CompoundCurve** instances when defining their exterior y interior rings. Las instancias de **Polygon** no pueden.

## <a name="see-also"></a>Consulte también

- [Datos espaciales (SQL Server)](./spatial-data-sql-server.md)
- [Referencia de los métodos del tipo de datos geometry](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md)
- [Referencia de los métodos del tipo de datos geography](../../t-sql/spatial-geography/spatial-types-geography.md)
- [STNumCurves &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stnumcurves-geometry-data-type.md)
- [STNumCurves &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/stnumcurves-geography-data-type.md)
- [STGeomFromText &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stgeomfromtext-geometry-data-type.md)
- [STGeomFromText &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)