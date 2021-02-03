---
description: STBuffer (tipo de datos geography)
title: STBuffer (tipo de datos geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STBuffer (geography Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geography Data Type)
ms.assetid: cb4deab8-642b-44d9-b3d9-85114d64021e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5b8b15017c53da5fe6020742fe5d3b5cdc5be1da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186006"
---
# <a name="stbuffer-geography-data-type"></a>STBuffer (tipo de datos geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve un objeto geography que representa la unión de todos los puntos cuya distancia desde una instancia de **geography** es menor o igual que un valor especificado.  
  
 Este método del tipo de datos geography admite instancias de **FullGlobe** o instancias espaciales mayores que un hemisferio.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STBuffer ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *distance*  
 Es un valor de tipo **float** (**double** en .NET Framework) que especifica la distancia desde la instancia de **geography** en torno a la cual se puede calcular el búfer.  
  
 La distancia máxima del búfer no puede ser superior a 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 circunferencia terráquea) o todo el globo terráqueo.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Tipo de valor devuelto de CLR: **SqlGeography**  
  
## <a name="remarks"></a>Observaciones  
 STBuffer() calcula un búfer de la misma forma que [BufferWithTolerance](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md), mediante la especificación de *tolerance* = abs(distance) \* 0,001 y *relative* = **false**.  
  
 Un búfer negativo quita todos los puntos que se encuentran dentro de la distancia especificada del límite de la instancia de **geography**.  
  
 `STBuffer()` devuelve una instancia de **FullGlobe** en determinados casos; por ejemplo, `STBuffer()` devuelve una instancia de **FullGlobe** cuando la distancia del búfer es mayor que la distancia desde el ecuador a los polos. Un búfer no puede superar el globo terráqueo completo.  
  
 Este método produce una excepción **ArgumentException** en las instancias de **FullGlobe** donde la distancia del búfer supera la siguiente limitación:  
  
 0,999 \* *π* * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 circunferencia terráquea)  
  
 El límite de distancia máxima permite que la construcción del búfer sea lo más flexible posible.  
  
 El error entre el búfer teórico y el calculado es max(tolerance, extents * 1.E-7), donde tolerance = distance \* ,001. Para más información sobre las extensiones, vea [Referencia de los métodos del tipo de datos geography](./stequals-geography-data-type.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una instancia de `LineString``geography`. A continuación, se usa `STBuffer()` para devolver la región que se encuentra en un radio de 1 metro de la instancia.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STBuffer(1).ToString();  
```  
  
## <a name="see-also"></a>Vea también  
 [BufferWithTolerance &#40;tipo de datos geography&#41;](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)   
 [Métodos de OGC en instancias de Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
