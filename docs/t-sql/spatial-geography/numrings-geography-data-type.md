---
description: NumRings (tipo de datos geography)
title: NumRings (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NumRings_TSQL
- NumRings
dev_langs:
- TSQL
helpviewer_keywords:
- NumRings method
ms.assetid: 0e4e4fa2-b608-4cc4-98ba-0845ddb4214c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1a4827b3b3972864b560f3951a81889321580a10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186418"
---
# <a name="numrings-geography-data-type"></a>NumRings (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el número total de anillos de una instancia de **Polygon**. En el tipo  **geography** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], no se distingue entre los anillos externos y los internos, dado que cualquier anillo puede usarse como anillo externo.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.NumRings ()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Tipo de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **int**  
  
 Tipo de valor devuelto de CLR: **SqlInt32**  
  
## <a name="remarks"></a>Observaciones  
 Este método devolverá NULL si no se trata de una instancia de **Polygon** y devolverá 0 si la instancia está vacía. Este método es preciso.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se crea una instancia de `Polygon` con dos anillos y se confirma que tiene dos anillos.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653), (-122.357 47.654, -122.357 47.657, -122.349 47.657, -122.349 47.650, -122.357 47.654))', 4326);  
SELECT @g.NumRings();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos extendidos en instancias de geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
