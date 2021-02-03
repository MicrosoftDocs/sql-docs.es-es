---
description: GeomFromGML (tipo de datos geography)
title: GeomFromGML (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GeomFromGML_TSQL
- GeomFromGML
dev_langs:
- TSQL
helpviewer_keywords:
- GeomFromGML (geography Data Type)
- GeomFromGML method
ms.assetid: 470d0997-3cb0-4d34-9a45-b332fe432b14
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f94438c4ce5ecc7cb1a76d85451776bf22e7c1f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165024"
---
# <a name="geomfromgml-geography-data-type"></a>GeomFromGML (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea una instancia de **geography** dada una representación en el subconjunto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del lenguaje de marcado de geografía (GML).
  
Para obtener más información sobre GML, vea las siguientes especificaciones de Open Geospatial Consortium: [Especificaciones de OGC, Lenguaje de marcado de geografía](https://go.microsoft.com/fwlink/?LinkId=93629)
  
Este método del tipo de datos **geography** admite instancias de **FullGlobe** o instancias espaciales mayores que un hemisferio.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
GeomFromGml ( GML_input, SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *GML_input*  
 Es una entrada XML desde la que GML devolverá un valor.  
  
 *SRID*  
 Es una expresión **int** que representa el identificador de referencia espacial (SRID) de la instancia de **geography** que se va a devolver.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
## <a name="remarks"></a>Observaciones  
 Este método produce una excepción **FormatException** si la entrada no tiene el formato correcto.  
  
 Este método producirá una excepción **ArgumentException** si la entrada contiene un borde opuesto.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa `GeomFromGml()` para crear una instancia de `geography`.  
  
```  
DECLARE @g geography;  
DECLARE @x xml;  
SET @x = '<LineString xmlns="https://www.opengis.net/gml"><posList>47.656 -122.36 47.656 -122.343</posList></LineString>';  
SET @g = geography::GeomFromGml(@x, 4326);  
SELECT @g.ToString();  
```  
  
 En el ejemplo siguiente se usa `GeomFromGml()` para crear una instancia de `FullGlobe``geography`.  
  
```  
DECLARE @g geography;  
DECLARE @x xml;  
SET @x = '<FullGlobe xmlns="https://schemas.microsoft.com/sqlserver/2011/geography" />';  
SET @g = geography::GeomFromGml(@x, 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geografía estáticos extendidos](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  
