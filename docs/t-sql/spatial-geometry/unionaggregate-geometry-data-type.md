---
description: UnionAggregate (tipo de datos geometry)
title: UnionAggregate (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- UnionAggregate method (geometry)
ms.assetid: dc7929cc-55ca-4a2c-a4b9-f5452f95bde8
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 0d2ff521148ff199802fac992ddea63b67c26628
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172465"
---
# <a name="unionaggregate-geometry-data-type"></a>UnionAggregate (tipo de datos geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Realiza una operación de unión en un conjunto de objetos geometry.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
UnionAggregate ( geometry_operand )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *geometry_operand*  
 Es una columna de tabla de tipo **geometry** que contiene el conjunto de objetos **geometry** en el que se va a realizar una operación de unión.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
## <a name="exceptions"></a>Excepciones  
 Produce una excepción `FormatException` cuando hay valores de entrada no válidos. Vea [STIsValid &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md).  
  
## <a name="remarks"></a>Observaciones  
 El método devuelve **null** cuando la entrada está vacía o esta tiene unos SRID diferentes. Vea [Identificadores de referencia espacial &#40;SRID&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).  
  
 El método omite las entradas **null**.  
  
> [!NOTE]  
>  El método devuelve **null** si todos los valores introducidos son **null**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve la unión de un conjunto de objetos **geometry** en una variable de tabla.  
 ```
 -- Setup table variable for UnionAggregate example 
 DECLARE @Geom TABLE 
 ( 
 shape geometry, 
 shapeType nvarchar(50) 
 ); 
 INSERT INTO @Geom(shape,shapeType) 
 VALUES('CURVEPOLYGON(CIRCULARSTRING(2 3, 4 1, 6 3, 4 5, 2 3))', 'Circle'), 
 ('POLYGON((1 1, 4 1, 4 5, 1 5, 1 1))', 'Rectangle'); 
 -- Perform UnionAggregate on @Geom.shape column 
 SELECT geometry::UnionAggregate(shape).ToString() 
 FROM @Geom;
``` 
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geometría estáticos ampliados](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
  

