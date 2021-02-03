---
description: Filter (tipo de datos Geography)
title: Filter (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Filter
- Filter_TSQL
- Filter (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- Filter method
ms.assetid: 82a8f54a-3a47-4e20-b13a-b148029c5448
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 4d3d5975a0e1d01a0b4dfbea123f483d8e004ab2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176318"
---
# <a name="filter-geography-data-type"></a>Filter (tipo de datos Geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Método que proporciona una forma rápida de intersección solo para índices con la que se puede determinar si una instancia de **geography** forma intersección con otra instancia de **geography**, siempre y cuando haya un índice disponible.  
  
 Devuelve 1 si una instancia de **geography** puede cortar otra instancia de **geography**. Este método puede generar una respuesta falsa positiva y el resultado exacto puede depender del plan. Devuelve el valor 0 preciso (respuesta verdadera negativa) si no se encuentra ninguna intersección de las instancias de **geography**.  
  
 En los casos en los que no haya ningún índice disponible o que no se use, el método devolverá los mismos valores que **STIntersects()** cuando se llama con los mismos parámetros.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.Filter ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *other_geography*  
 Es otra instancia de **geography** con la que se compara la instancia en la que se invoca Filter().  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Tipo de valor devuelto de CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Comentarios  
 Este método no es determinista y no es preciso.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa `Filter()` para determinar si dos instancias de `geography` forman intersección.  
  
```  
CREATE TABLE sample (id int primary key, g geography);  
INSERT INTO sample VALUES  
   (0, geography::Point(45, -120, 4326)),  
   (1, geography::Point(45, -120.1, 4326)),  
   (2, geography::Point(45, -120.2, 4326)),  
   (3, geography::Point(45, -120.3, 4326)),  
   (4, geography::Point(45, -120.4, 4326));  
  
CREATE SPATIAL INDEX sample_idx on sample(g);  
SELECT id  
FROM sample   
WHERE g.Filter(geography::Parse(  
   'POLYGON((-120.1 44.9, -119.9 44.9, -119.9 45.1, -120.1 45.1, -120.1 44.9))')) = 1;  
```  
  
## <a name="see-also"></a>Vea también  
 [Métodos extendidos en instancias de geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [STIntersects &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/stintersects-geography-data-type.md)  
  
  
