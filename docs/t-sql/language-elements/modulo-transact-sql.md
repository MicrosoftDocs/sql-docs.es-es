---
description: '% (Módulo) (Transact-SQL)'
title: '% (Módulo) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- modulo
- modulus
- '% (Modulo)'
- '% (Modulus)'
- MOD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- '% (modulo operator)'
- '% (modulus operator)'
- remainder of division operation
- modulo operator (%)
- modulus operator (%)
ms.assetid: f93c662e-f405-486e-bf23-a2d03907b5bd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1c523954ddcf3b326823f01c81bf89a8e56f4342
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351643"
---
# <a name="-modulus-transact-sql"></a>% (Módulo) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve el resto de un número dividido entre otro.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
dividend % divisor  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *dividend*  
 Es la expresión numérica que se va a dividir. *dividend* debe ser una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) válida de cualquiera de los tipos de datos de las categorías de tipos de datos enteros y de moneda, o bien del tipo de datos **numeric**.  
  
 *divisor*  
 Es la expresión numérica entre la que se divide el dividendo. *divisor* debe ser cualquier expresión válida de cualquiera de los tipos de datos de las categorías de tipos de datos enteros y de moneda, o bien del tipo de datos **numeric**.  
  
## <a name="result-types"></a>Tipos de resultado  
 Determinados por los tipos de datos de los dos argumentos.  
  
## <a name="remarks"></a>Observaciones  
 El operador aritmético de módulo se puede usar en la lista de selección de la instrucción SELECT con cualquier combinación de nombres de columnas, constantes numéricas o cualquier expresión válida de las categorías de tipos de datos entero y monetario o del tipo de datos **numeric**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-simple-example"></a>A. Ejemplo sencillo  
 En el ejemplo siguiente se divide el número 38 por 5. Esto produce 7 como parte entera del resultado y muestra cómo el módulo devuelve un resto de 3.  
  
```sql  
SELECT 38 / 5 AS Integer, 38 % 5 AS Remainder;
```  
  
### <a name="b-example-using-columns-in-a-table"></a>B. Ejemplo que usa columnas de una tabla  
 En el siguiente ejemplo se devuelve el número de Id. del producto, el precio unitario del producto y el módulo (resto) de la división del precio de cada producto, convertido a un valor entero, por el número de productos del pedido.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(100)ProductID, UnitPrice, OrderQty,  
   CAST((UnitPrice) AS INT) % OrderQty AS Modulo  
FROM Sales.SalesOrderDetail;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-simple-example"></a>C: ejemplo sencillo  
 En el siguiente ejemplo se muestran los resultados del operador `%` al dividir 3 entre 2.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(1) 3%2 FROM dimEmployee;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
1         
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [%= &#40;Modulus Assignment&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/modulo-equals-transact-sql.md)  (%= [Asignación de módulo] [Transact-SQL])  
 [Operadores compuestos &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


