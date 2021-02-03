---
description: STPointN (tipo de datos geometry)
title: STPointN (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STPointN_TSQL
- STPointN (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STPointN (geometry Data Type)
ms.assetid: 8f0bb3b7-5cd9-42c2-b9f8-f04628653bd0
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 78db87953b38208585bd9ea68c75766cfd3765b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184629"
---
# <a name="stpointn-geometry-data-type"></a>STPointN (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve un punto especificado de una instancia de **geometry**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STPointN ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es una expresión **int** entre 1 y el número de puntos de la instancia de **geometry**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
 Tipo Open Geospatial Consortium (OGC): **Point**  
  
## <a name="remarks"></a>Comentarios  
 Si se trata de una instancia de **geometry** creada por el usuario, `STPointN()` devuelve el punto especificado por *expression* considerando los puntos en el orden en el que se especificaron inicialmente.  
  
 Si se trata de una instancia de **geometry** generada por el sistema, `STPointN()` devuelve el punto especificado por *expression* considerando todos los puntos en el orden en que se generarían: primero por geometrías, después por anillos dentro de la geometría (si procede) y, luego, por puntos dentro del anillo. Este orden es determinista.  
  
 Si se llama a este método con un valor inferior a 1, se genera una excepción **ArgumentOutOfRangeException**.  
  
 Si se llama a este método con un valor superior al número de puntos de la instancia, devuelve NULL.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una instancia de `LineString` y se usa `STPointN()` para recuperar el segundo punto de la descripción de la instancia.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STPointN(2).ToString();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

