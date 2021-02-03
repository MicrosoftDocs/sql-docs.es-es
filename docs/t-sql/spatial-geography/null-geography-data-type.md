---
description: Null (tipo de datos geography)
title: Null (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Null (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- Null (geography Data Type)
- Null method
ms.assetid: bb464b06-86e0-4b8b-ad78-04bd33b6069c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5be992307d8372d63d595220054e12968551f71f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182250"
---
# <a name="null-geography-data-type"></a>Null (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Una propiedad de solo lectura que proporciona una instancia NULL del tipo **geography**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Null  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo CLR: **SqlGeography**  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se recupera una instancia NULL de `geography`.  
  
```  
DECLARE @g geography;   
SET @g = geography::[Null];  
SELECT @g  
```  
  
## <a name="see-also"></a>Consulte también  
 [Métodos de geografía estáticos extendidos](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  
