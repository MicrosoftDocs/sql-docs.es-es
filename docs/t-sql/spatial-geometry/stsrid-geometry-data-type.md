---
description: STSrid (tipo de datos geometry)
title: STSrid (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STSrid (geometry Data Type)
- STSrid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STSrid (geometry Data Type)
ms.assetid: 5e0de983-a0fe-48b7-9e08-30588d7271e2
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 15f91de88c027d96d56dfaeb80859e8433727c9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184582"
---
# <a name="stsrid-geometry-data-type"></a>STSrid (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  **STSrid** es un número entero que representa el identificador de referencia espacial de la instancia.  
  
Esta propiedad se puede modificar.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
STSrid  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **int**  
  
 Tipo de CLR: **SqlInt32**  
  
## <a name="examples"></a>Ejemplos  
 En el primer ejemplo se crea una instancia de **geometry** con el valor SRID 13 y se usa `STSrid` para confirmar dicho SRID.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0))', 13);  
SELECT @g.STSrid;  
```  
  
 El segundo ejemplo usa `STSrid` para cambiar el valor SRID de la instancia a 23 y, a continuación, confirma la modificación de dicho valor.  
  
```  
SET @g.STSrid = 23;  
SELECT @g.STSrid;  
```  
  
## <a name="see-also"></a>Vea también  
 [STX &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/stx-geometry-data-type.md)   
 [STY &#40;tipo de datos geometry&#41;](../../t-sql/spatial-geometry/sty-geometry-data-type.md)   
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

