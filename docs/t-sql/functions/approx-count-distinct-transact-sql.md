---
description: APPROX_COUNT_DISTINCT (Transact-SQL)
title: APPROX_COUNT_DISTINCT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPROX_COUNT_DISTINCT
dev_langs:
- TSQL
author: joesackmsft
ms.author: josack
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7cb07f1448835b4f8af9384b895776d896134f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202272"
---
# <a name="approx_count_distinct-transact-sql"></a>APPROX_COUNT_DISTINCT (Transact-SQL)

[!INCLUDE [sqlserver2019-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi-asa.md)]

Esta función devuelve el número aproximado de valores no nulos únicos de un grupo. 
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
APPROX_COUNT_DISTINCT ( expression )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*expression*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo, excepto **image**, **sql_variant**, **ntext** o **text**. 

## <a name="return-types"></a>Tipos de valores devueltos
 **bigint**  
  
## <a name="remarks"></a>Observaciones  
`APPROX_COUNT_DISTINCT( expression )` evalúa una expresión para cada fila de un grupo y devuelve el número aproximado de valores no nulos únicos de un grupo. Esta función está diseñada para proporcionar agregaciones de conjuntos de datos de gran tamaño en los que la capacidad de respuesta resulta más fundamental que la precisión absoluta.  

`APPROX_COUNT_DISTINCT` está diseñado para su uso en escenarios de macrodatos y está optimizado para las condiciones siguientes:
- Acceso de conjuntos de datos con miles de filas o más *y*
- Agregación de una columna o columnas con muchos valores distintos

La implementación de la función garantiza una tasa de error de hasta 2 % dentro de una probabilidad del 97 %. 

`APPROX_COUNT_DISTINCT` requiere menos memoria que una operación COUNT DISTINCT exhaustiva.  Dada la menor superficie de memoria, es menos probable que `APPROX_COUNT_DISTINCT` desborde memoria en el disco en comparación con una operación COUNT DISTINCT precisa. Para más información sobre el algoritmo usado para conseguir esto, consulte [HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog).

> [!NOTE]
> Con cadenas que distinguen la intercalación, APPROX_COUNT_DISTINCT usa una combinación binaria y proporciona resultados que se habrían generado en presencia de intercalaciones BIN y no BIN2. 
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-approx_count_distinct"></a>A. Uso de APPROX_COUNT_DISTINCT 
Este ejemplo devuelve el número aproximado de claves de pedido diferentes desde la tabla de pedidos.
  
```sql
SELECT APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders;
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Approx_Distinct_OrderKey
------------------------
15164704
```
  
### <a name="b-using-approx_count_distinct-with-group-by"></a>B. Uso de APPROX_COUNT_DISTINCT con GROUP BY 
Este ejemplo devuelve el número aproximado de claves de pedido diferentes según el estado del pedido desde la tabla de pedidos. 
  
```sql
SELECT O_OrderStatus, APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders
GROUP BY O_OrderStatus
ORDER BY O_OrderStatus; 
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
O_OrderStatus                                                    Approx_Distinct_OrderKey
---------------------------------------------------------------- ------------------------
F                                                                7397838
O                                                                7387803
P                                                                388036
```
    
## <a name="see-also"></a>Vea también
[Funciones de agregado &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md) 
