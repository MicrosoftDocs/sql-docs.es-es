---
description: SELECT @local_variable (Transact-SQL)
title: SELECT @local_variable (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- variable_TSQL
- '@loca_variable'
- '@local'
- variable
- '@loca_variable_TSQL'
- '@local_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- variables [SQL Server], assigning
- SELECT statement [SQL Server], @local_variable
- '@local_variable'
- local variables [SQL Server]
ms.assetid: 8e1a9387-2c5d-4e51-a1fd-a2a95f026d6f
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current ||>= sql-server-2016 ||= azure-sqldw-latest||>= sql-server-linux-2017
ms.openlocfilehash: 69e2495cb78896378196b163c868e7a2d069f1b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194276"
---
# <a name="select-local_variable-transact-sql"></a>SELECT @local_variable (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Establece una variable local en el valor de una expresión.  
  
 Para asignar variables, se recomienda usar [SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md) en lugar de SELECT @*local_variable*.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SELECT { @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression } 
    [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

@*local_variable*  
 Es una variable declarada a la que se va a asignar un valor.  
  
{= \| += \| -= \| \*= \| /= \| %= \| &= \| ^= \| \|= }  
Asigna el valor de la derecha a la variable de la izquierda.  
  
Operador de asignación compuesta:  

| operator | action |  
| -------- | ------ |  
| = | Asigna a la variable la expresión que le sigue. |  
| += | Sumar y asignar |  
| -= | Restar y asignar |  
| \*= | Multiplicar y asignar |  
| /= | Dividir y asignar |  
| %= | Módulo y asignar |  
| &= | AND bit a bit y asignar |  
| ^= | XOR bit a bit y asignar |  
| \|= | OR bit a bit y asignar |  

*expression*  
Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida. Esto incluye una subconsulta escalar.  

## <a name="remarks"></a>Observaciones

SELECT @*local_variable* se suele usar para devolver un único valor a la variable. En cambio, cuando *expression* es el nombre de una columna, puede devolver varios valores. Si la instrucción SELECT devuelve más de un valor, se asigna a la variable el último valor devuelto.  

Si la instrucción SELECT no devuelve filas, la variable mantiene su valor actual. Si *expression* es una subconsulta escalar que no devuelve ningún valor, la variable se establece en NULL.  

Una instrucción SELECT puede inicializar varias variables locales.  

> [!NOTE]
> Una instrucción SELECT que contenga una asignación de variable no se puede utilizar para realizar operaciones típicas de obtención de conjuntos de resultados.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-use-select-local_variable-to-return-a-single-value"></a>A. Usar SELECT @local_variable para devolver un solo valor  
 En el siguiente ejemplo, se asigna el valor `@var1` a la variable `Generic Name`. La consulta realizada en la tabla `Store` no devuelve filas debido a que el valor especificado en `CustomerID` no existe en la tabla. La variable mantiene el valor `Generic Name`.  
  
```sql  
-- Uses AdventureWorks    
  
DECLARE @var1 VARCHAR(30);         
SELECT @var1 = 'Generic Name';         
SELECT @var1 = Name         
FROM Sales.Store         
WHERE CustomerID = 1000 ;        
SELECT @var1 AS 'Company Name';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Company Name  
 ------------------------------  
 Generic Name  
 ```  
  
### <a name="b-use-select-local_variable-to-return-null"></a>B. Usar SELECT @local_variable para devolver NULL  
 En el siguiente ejemplo, se utiliza una subconsulta para asignar un valor a `@var1`. Debido a que el valor solicitado para `CustomerID` no existe, la subconsulta no devuelve ningún valor y la variable se establece en `NULL`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @var1 VARCHAR(30)   
SELECT @var1 = 'Generic Name'   
SELECT @var1 = (SELECT Name   
FROM Sales.Store   
WHERE CustomerID = 1000)   
SELECT @var1 AS 'Company Name' ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Company Name  
----------------------------  
NULL  
```  
  
## <a name="see-also"></a>Consulte también  
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operadores compuestos &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
