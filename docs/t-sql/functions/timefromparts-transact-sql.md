---
description: TIMEFROMPARTS (Transact-SQL)
title: TIMEFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TIMEFROMPARTS_TSQL
- TIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- TIMEFROMPARTS function
ms.assetid: 786c65a1-2b3f-4e4b-82b6-4940d62f3801
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: db314c5d21ab27e81133048b90546efbbe5bb100
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205502"
---
# <a name="timefromparts-transact-sql"></a>TIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve un valor **time** para la hora especificada y con la precisión indicada.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
TIMEFROMPARTS ( hour, minute, seconds, fractions, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *hour*  
 Expresión entera que especifica horas.  
  
 *minute*  
 Expresión entera que especifica minutos.  
  
 *segundos*  
 Expresión entera que especifica segundos.  
  
 *fractions*  
 Expresión entera que especifica fracciones.  
  
 *precisión*  
 Literal entero que especifica la precisión del valor **time** que se va a devolver.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **time(** *precision* **)**  
  
## <a name="remarks"></a>Comentarios  
 TIMEROMPARTS devuelve un valor de hora totalmente inicializado. Si los argumentos no son válidos, se generará un error. Si alguno de los parámetros es NULL, se devuelve NULL. Pero si el argumento *precision* es NULL, se generará un error.  
  
 El argumento *fractions* depende del argumento *precision*. Por ejemplo, si *precision* es 7, cada fracción representa 100 nanosegundos; si *precision* es 3, cada fracción representa un milisegundo. Si el valor de *precision* es cero, el valor de *fractions* también debe ser cero; de lo contrario, se generará un error.  
  
 Esta función se puede enviar de forma remota a servidores [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y posteriores. No se puede enviar de forma remota a servidores que tengan una versión anterior a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-simple-example-without-fractions-of-a-second"></a>A. Ejemplo simple sin fracciones de segundo  
  
```sql
SELECT TIMEFROMPARTS ( 23, 59, 59, 0, 0 ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------------------  
23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Ejemplo con fracciones de segundo  
 En este ejemplo se muestra el uso de los parámetros *fractions* y *precision*:  
  
1.  Cuando *fractions* tiene el valor 5 y *precision*, el valor 1, el valor de *fractions* representa 5/10 de un segundo.  
  
2.  Cuando *fractions* tiene el valor 50 y *precision*, el valor 2, el valor de *fractions* representa 50/100 de un segundo.  
  
3.  Cuando *fractions* tiene el valor 500 y *precision*, el valor 3, el valor de *fractions* representa 500/1000 de un segundo.  
  
```sql  
SELECT TIMEFROMPARTS ( 14, 23, 44, 5, 1 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 50, 2 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 500, 3 );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------  
14:23:44.5  
  
(1 row(s) affected)  
  
----------------  
14:23:44.50  
  
(1 row(s) affected)  
  
----------------  
14:23:44.500  
  
(1 row(s) affected)  
```  
  

