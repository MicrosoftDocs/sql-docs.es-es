---
description: STMPointFromText (tipo de datos geography)
title: STMPointFromText (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STMPointFromText (geography Data Type)
- STMPointFromText_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STMPointFromText method
ms.assetid: fe91a9f5-8de6-464e-88db-00650eae79b0
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9a69396422cc690e3034f6c61a926d80dfe71e68
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195323"
---
# <a name="stmpointfromtext-geography-data-type"></a>STMPointFromText (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve una instancia de **geography** a partir de una representación Well-Known Text (WKT) de Open Geospatial Consortium (OGC), ampliada con los valores Z (elevación) y M (medida) pertenecientes a la instancia.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
STMPointFromText ( 'multipoint_tagged_text', SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *multipoint_tagged_text*  
 Es la representación WKT de la instancia de **geographyMultiPoint** que se quiere devolver. *multipoint_tagged_text* es una expresión **nvarchar(max)**.  
  
 *SRID*  
 Es una expresión **int** que representa el identificador de referencia espacial (SRID) de la instancia de **geographyMultiPoint** que se quiere devolver.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
 Tipo OGC: **MultiPoint**  
  
## <a name="remarks"></a>Observaciones  
 Este método produce una excepción **FormatException** si la entrada no tiene el formato correcto.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa `STMPointFromText()` para crear una instancia de `geography`.  
  
```  
DECLARE @g geography;   
SET @g = geography::STMPointFromText('MULTIPOINT(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geografía estáticos de OGC](../../t-sql/spatial-geography/ogc-static-geography-methods.md)  
  
  
