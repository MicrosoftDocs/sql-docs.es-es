---
description: CollectionAggregate (tipo de datos geography)
title: CollectionAggregate (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CollectionAggregate method (geography)
ms.assetid: e49a644a-dbf2-46c3-98f5-4b3ec197e2ad
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d8fc50abd671381da2de9dba0d9e7c0e30f73f11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159665"
---
# <a name="collectionaggregate-geography-data-type"></a>CollectionAggregate (tipo de datos geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Crea una instancia de **GeometryCollection** a partir de un conjunto de objetos **geography**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
ConvexHullAggregate ( geography_operand )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *geography_operand*  
 Es una columna de tabla de tipo **geography** que representa un conjunto de objetos **geography** que se van a enumerar en la instancia de **GeometryCollection**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
## <a name="exception"></a>Excepción  
 Produce una excepción `FormatException` cuando hay valores de entrada no válidos. Vea [STIsValid &#40;geography Data Type&#41;](../../t-sql/spatial-geography/stisvalid-geography-data-type.md) (STIsValid [tipo de datos geography]).  
  
## <a name="remarks"></a>Comentarios  
 El método devuelve **null** cuando la entrada está vacía o esta tiene unos SRID diferentes. Vea [Identificadores de referencia espacial &#40;SRID&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).  
  
 El método omite las entradas **null**.  
  
> [!NOTE]  
>  El método devuelve **null** si todos los valores introducidos son **null**.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve una instancia de `GeometryCollection` que contiene un conjunto de objetos **geography**.  
  
 ```
 USE AdventureWorks2012  
 GO  
 SELECT geography::CollectionAggregate(SpatialLocation).ToString() AS SpatialLocation  
 FROM Person.Address  
 WHERE City LIKE ('Bothell')
 ```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geografía estáticos extendidos](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  
