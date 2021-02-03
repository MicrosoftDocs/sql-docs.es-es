---
description: STX (tipo de datos geometry)
title: STX (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 06/23/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STX (geometry Data Type)
- STX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STX (geometry Data Type)
ms.assetid: 2aef77e8-0460-43f9-bad6-2aae6d8c36f9
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: c66bc8f83328eaaaad62d7dceb291d5aaaaa5bd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191843"
---
# <a name="stx-geometry-data-type"></a>STX (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Propiedad de la coordenada X de una instancia de **Point**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STX  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **float**  
  
 Tipo CLR: **SqlDouble**  
  
## <a name="remarks"></a>Observaciones  
 El valor de esta propiedad será NULL si la instancia de **geometry** no es un punto.  
  
 Esta propiedad es de solo lectura.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una instancia de `Point` y se utiliza `STX` para recuperar la coordenada x de la instancia.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(3 8)', 0);  
SELECT @g.STX;  
```  
  
## <a name="see-also"></a>Consulte también  
 [STY &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/sty-geometry-data-type.md)   
 [STSrid &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stsrid-geometry-data-type.md)   
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

