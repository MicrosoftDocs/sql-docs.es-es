---
description: ¿Cuáles son las funciones de base de datos SQL de Microsoft?
title: ¿Cuáles son las funciones de base de datos SQL de Microsoft? | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- built-in functions [SQL Server]
- function [SQL Server] See functions [SQL Server]
- functions [Transact-SQL]
- functions [SQL Server], about functions
- scalar functions
- functions [SQL Server]
ms.assetid: 17186213-5ab5-40b0-b470-b660af1ec44c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1364ff8238e8aea9f303b7a6aa7ef114b4544d3e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179225"
---
# <a name="what-are-the-sql-database-functions"></a>¿Cuáles son las funciones de base de datos SQL?
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Obtenga información sobre las categorías de las funciones integradas que se pueden usar con las bases de datos SQL. Puede usar las funciones integradas o crear las suyas propias.
  
## <a name="aggregate-functions"></a>Funciones de agregado

Las funciones de agregado realizan un cálculo en un conjunto de valores y devuelven un valor único. Se pueden usar en la lista de selección o en la cláusula HAVING de una instrucción SELECT. Puede usar una agregación en combinación con la cláusula GROUP BY para calcular la agregación en las categorías de filas. Use la cláusula OVER para calcular la agregación en un intervalo de valor específico. La cláusula OVER no puede seguir las agregaciones GROUPING o GROUPING_ID.

Todas las funciones de agregación son deterministas; es decir, siempre devuelven el mismo resultado cuando se ejecutan con los mismos valores de entrada. Para más información, vea [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

## <a name="analytic-functions"></a>Funciones analíticas
Las funciones analíticas calculan un valor agregado basándose en un grupo de filas. A diferencia de las funciones de agregado, estas funciones pueden devolver varias filas para cada grupo. Puede usar funciones analíticas para calcular medias móviles, totales acumulados, porcentajes o resultados de N valores superiores dentro de un grupo.

## <a name="ranking-functions"></a>Funciones de categoría
Las funciones de categoría devuelven un valor de categoría para cada fila de una partición. Según la función que se utilice, algunas filas pueden recibir el mismo valor que otras. Las funciones de categoría son no deterministas.

## <a name="rowset-functions"></a>Funciones de conjuntos de filas
Las funciones de conjuntos de filas devuelven un objeto que se puede usar como referencias de tabla en una instrucción SQL.

## <a name="scalar-functions"></a>Funciones escalares
Operan sobre un valor y después devuelven otro valor. Las funciones escalares se pueden utilizar donde la expresión sea válida.

### <a name="categories-of-scalar-functions"></a>Categorías de las funciones escalares
  
|Categoría de la función|Descripción|  
|-----------------------|-----------------|  
|[Funciones de configuración](configuration-functions-transact-sql.md)|Devuelven información acerca de la configuración actual.|  
|[Funciones de conversión](conversion-functions-transact-sql.md)|Admiten conversión y conversión de tipos de datos.|  
|[Funciones del cursor](cursor-functions-transact-sql.md)|Devuelven información acerca de los cursores.|  
|[Tipos de datos y funciones de fecha y hora](date-and-time-data-types-and-functions-transact-sql.md)|Llevan a cabo operaciones sobre un valor de entrada de fecha y hora, y devuelven un valor numérico, de cadena o de fecha y hora.|  
|[Funciones JSON](json-functions-transact-sql.md)|Validan, consultan o cambian datos JSON.|  
|[Funciones lógicas](logical-functions-choose-transact-sql.md)|Realizan operaciones lógicas.|  
|[Funciones matemáticas](mathematical-functions-transact-sql.md)|Realizan cálculos basados en valores de entrada proporcionados como parámetros a las funciones y devuelven valores numéricos.|  
|[Funciones de metadatos](metadata-functions-transact-sql.md)|Devuelven información acerca de la base de datos y los objetos de la base de datos.|  
|[Funciones de seguridad](security-functions-transact-sql.md)|Devuelven información acerca de usuarios y roles.|  
|[Funciones de cadena](string-functions-transact-sql.md)|Realizan operaciones en el valor de entrada de una cadena (**char** o **varchar**) y devuelven una cadena o un valor numérico.|  
|[Funciones del sistema](../../relational-databases/system-functions/system-functions-category-transact-sql.md)|Realizan operaciones y devuelven información acerca de valores, objetos y configuraciones de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Funciones estadísticas del sistema](system-statistical-functions-transact-sql.md)|Devuelven información estadística acerca del sistema.|  
|[Funciones de texto e imagen](./text-and-image-functions-textptr-transact-sql.md)|Realizan operaciones sobre los valores de entrada o columnas de texto o imagen, y devuelven información acerca del valor.|  
  
## <a name="function-determinism"></a>Determinismo de función  
 Las funciones integradas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son deterministas o no deterministas. Las funciones son deterministas cuando devuelven siempre el mismo resultado cada vez que se llaman con un conjunto específico de valores de entrada. Las funciones son no deterministas cuando es posible que devuelvan distintos resultados cada vez que se llaman con un mismo conjunto específico de valores de entrada. Para más información, vea [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="function-collation"></a>Intercalación de funciones  
 Las funciones que toman una entrada de cadena de caracteres y devuelven una salida de cadena de caracteres utilizan la intercalación de la cadena de entrada para la salida.  
  
 Las funciones que toman entradas que no son de caracteres y devuelven una cadena de caracteres utilizan la intercalación predeterminada de la base de datos actual para la salida.  
  
 Las funciones que toman varias entradas de cadena de caracteres y devuelven una cadena de caracteres utilizan las reglas de prioridad de intercalación para establecer la intercalación de la cadena de salida. Para más información, vea [Prioridad de intercalación &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)   
 [Using Stored Procedures &#40;MDX&#41;](../../mdx/using-stored-procedures-mdx.md) (Usar procedimientos almacenados [MDX])  
  
