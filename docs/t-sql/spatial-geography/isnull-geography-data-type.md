---
description: IsNull (tipo de datos geography)
title: IsNull (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IsNull (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- IsNull method
ms.assetid: c031074f-bfda-4584-a3bf-4e7c324f237f
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5cf656f1f05188a6b765aa9eef4c36131fda56b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205409"
---
# <a name="isnull-geography-data-type"></a>IsNull (tipo de datos geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Propiedad que especifica si la instancia de **geography** es NULL. Devuelve 'TRUE' si la instancia es NULL; en caso contrario, devuelve 0.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.IsNull  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Tipo de CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Observaciones  
 `IsNull` se puede usar para comprobar si una instancia de **geography** es NULL. Esto puede producir resultados un tanto confusos, devolviendo 0 si la instancia no es NULL y NULL si la instancia es NULL.  
  
 Este método lo usa principalmente la infraestructura [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; se recomienda usar el predicado IS NULL de T-SQL para comprobar si una instancia de **geography** es NULL. Para más información sobre el predicado IS NULL de T-SQL, vea [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
  
## <a name="see-also"></a>Consulte también  
 [Métodos extendidos en instancias de geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
