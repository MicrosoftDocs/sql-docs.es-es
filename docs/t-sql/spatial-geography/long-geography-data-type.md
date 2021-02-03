---
description: Long (tipo de datos geography)
title: Long (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Long_TSQL
- Long
dev_langs:
- TSQL
helpviewer_keywords:
- Long method
ms.assetid: bedbeced-70b8-4569-84f3-f86bfb04ce50
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 98fa19504777027a5d8e4bca5951b1d5ceec6fe6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207542"
---
# <a name="long-geography-data-type"></a>Long (tipo de datos geography)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Propiedad de longitud de la instancia de **geography**.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
.Long  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-value"></a>Valor devuelto  
 Tipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **float**  
  
 Tipo CLR: **SqlDouble**  
  
## <a name="remarks"></a>Observaciones  
 En el modelo OpenGIS, Long se define solo en instancias de **geography** que constan de un solo punto. Esta propiedad devuelve NULL si las instancias de **geography** contienen más de un punto. Esta propiedad es precisa y de solo lectura.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se crea una instancia de **Point** y se recupera la longitud del punto.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.Long;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos extendidos en instancias de geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
