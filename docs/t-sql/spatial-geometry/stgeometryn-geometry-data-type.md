---
description: STGeometryN (tipo de datos geometry)
title: STGeometryN (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STGeometryN_TSQL
- STGeometryN (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STGeometryN (geometry Data Type)
ms.assetid: 348c7047-3442-4590-8879-fe841e79058c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9d74046c6465f09324c8b8b6e154dfbd3a823b95
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190757"
---
# <a name="stgeometryn-geometry-data-type"></a>STGeometryN (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una geometría especificada en una **colección de geometría**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STGeometryN ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Expresión **int** entre 1 y el número de instancias de **geometry** en la colección **geometrycollection**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Observaciones  
 Este método devuelve **null** si el parámetro es mayor que el resultado de `STNumGeometries()` y producirá una excepción **ArgumentOutOfRangeException** si el parámetro *expresión* es menor que 1.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una `MultiPoint``geometry collection` y se usa `STGeometryN()` para buscar la segunda instancia de `geometry` de la colección.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('MULTIPOINT(0 0, 13.5 2, 7 19)', 0);  
SELECT @g.STGeometryN(2).ToString();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

