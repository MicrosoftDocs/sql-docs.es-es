---
description: STContains (tipo de datos geography)
title: STContains (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- STContains method (geography)
ms.assetid: b10e8f0a-2926-449a-82ea-be42543420ca
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f077d3aa3826fa0e2053ba23ddc94574d2823b09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185980"
---
# <a name="stcontains--geography-data-type"></a>STContains (tipo de datos geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Especifica si la instancia de **geography** que hace la llamada contiene espacialmente a la instancia de **geography** que se pasa al método.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STContains ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *other_geography*  
 Es otra instancia de **geography** con la que se compara la instancia en la que se invoca `STContains()`.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Tipo de valor devuelto de CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Comentarios  
 Devuelve 1 si la instancia de **geography** que hace la llamada contiene espacialmente a la instancia de **geography** que se pasa al método, y devuelve 0 si no lo hace. Devuelve **null** si el SRID de las dos instancias de **geography** no coincide.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa `STContains()` para comprobar si, dadas dos instancias de `geography`, la primera de ellas contiene a la segunda.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::Parse('CURVEPOLYGON (COMPOUNDCURVE (CIRCULARSTRING (-122.200928 47.454094, -122.810669 47.00648, -122.942505 46.687131, -121.14624 45.786679, -119.119263 46.183634), (-119.119263 46.183634, -119.273071 47.107523, -120.640869 47.569114, -122.200928 47.454094)))');  
SET @h = geography::Parse('POINT(-121.703796 46.893985)');  
```  
  
 `SELECT @g.STContains(@h);`  
  
  
