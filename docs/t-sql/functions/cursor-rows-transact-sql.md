---
description: '&#x40;&#x40;CURSOR_ROWS (Transact-SQL)'
title: '@@CURSOR_ROWS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@CURSOR_ROWS'
- '@@CURSOR_ROWS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@CURSOR_ROWS function'
- cursors [SQL Server], last-opened
- last-opened cursor
- asynchronous cursors [SQL Server]
ms.assetid: 31bd7a97-7f28-42a8-ba24-24d16d22973d
author: cawrites
ms.author: chadam
ms.openlocfilehash: a179a8baa17142abca1ad8217a05cefbfea91c60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184089"
---
# <a name="x40x40cursor_rows-transact-sql"></a>&#x40;&#x40;CURSOR_ROWS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve el número de filas certificadas que se encuentran en el último cursor abierto en la conexión. Para mejorar el rendimiento, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede rellenar asincrónicamente los cursores estáticos y de conjunto de claves de gran tamaño. Se puede llamar a `@@CURSOR_ROWS` para determinar que el número de filas que cumplan las condiciones del cursor se recuperen en el momento en que se llama a @@CURSOR_ROWS.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
@@CURSOR_ROWS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valores devueltos
**integer**
  
## <a name="return-value"></a>Valor devuelto  
  
|Valor devuelto|Descripción|  
|---|---|
|-*m*|El cursor se rellena de forma asincrónica. El valor devuelto (-*m*) es el número de filas que el conjunto de claves contiene actualmente.|  
|-1|El cursor es dinámico. Como los cursores dinámicos reflejan todos los cambios, el número de filas correspondientes al cursor cambia constantemente. El cursor no necesariamente recupera todas las filas calificadas.|  
|0|No se han abierto cursores, no hay filas calificadas para el último cursor abierto, o éste se ha cerrado o su asignación se ha cancelado.|  
|*n*|El cursor está completamente relleno. El valor devuelto (*n*) es el número total de filas del cursor.|  
  
## <a name="remarks"></a>Observaciones  
`@@CURSOR_ROWS` devuelve un número negativo si el último cursor se ha abierto de forma asincrónica. Los cursores controlados por conjunto de claves o cursores estáticos se abren de forma asincrónica si el valor umbral del cursor de sp_configure es mayor que 0 y el número de filas del conjunto de resultados del cursor es mayor que su valor umbral.
  
## <a name="examples"></a>Ejemplos  
Este ejemplo declara primero un cursor y luego usa `SELECT` para mostrar el valor de `@@CURSOR_ROWS`. La opción tiene el valor `0` antes de que se abra el cursor y luego tiene el valor `-1` para indicar que el conjunto de claves del cursor se rellena de forma asincrónica.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT @@CURSOR_ROWS;  
DECLARE Name_Cursor CURSOR FOR  
SELECT LastName ,@@CURSOR_ROWS FROM Person.Person;  
OPEN Name_Cursor;  
FETCH NEXT FROM Name_Cursor;  
SELECT @@CURSOR_ROWS;  
CLOSE Name_Cursor;  
DEALLOCATE Name_Cursor;  
GO             
```  
  
Los conjuntos de resultados son los siguientes.
  
```
-----------
0  
```

```
LastName
---------------
Sanchez
```

```
-----------
-1
```  
  
## <a name="see-also"></a>Consulte también
[Funciones del cursor &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md)  
[OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)
  
  
