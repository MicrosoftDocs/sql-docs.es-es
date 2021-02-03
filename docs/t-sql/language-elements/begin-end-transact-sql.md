---
description: BEGIN...END (Transact-SQL)
title: BEGIN...END (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN
- BEGIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- enclosing statements [SQL Server]
- BEGIN statement
- control-of-flow language [SQL Server], BEGIN...END statement
- BEGIN...END keyword
- grouping statements, BEGIN...END statement
- executing Transact-SQL statements together [SQL Server]
- statements [SQL Server], grouping
ms.assetid: fc2c7f76-f1f9-4f91-beef-bc8ef0da2feb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7c950bb2d66a34c35d86346a5996cbf3ab601d16
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199107"
---
# <a name="beginend-transact-sql"></a>BEGIN...END (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Incluye una serie de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] de forma que se pueda ejecutar un grupo de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. BEGIN y END son palabras clave del lenguaje de control de flujo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
BEGIN  
    { sql_statement | statement_block }   
END  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 { *sql_statement* | *statement_block* }  
 Se trata de cualquier instrucción o grupo de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] definidas con un bloque de instrucciones.  
  
## <a name="remarks"></a>Observaciones  
 Los bloques BEGIN...END pueden anidarse.  
  
 Aunque todas las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] son válidas en un bloque BEGIN...END, ciertas instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] no deben agruparse en el mismo proceso por lotes o bloque de instrucciones.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo, `BEGIN` y `END` definen un conjunto de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecutan juntas. Si el bloqueo `BEGIN...END` no se incluye, ambas instrucciones `ROLLBACK TRANSACTION` se ejecutarán y se devolverán ambos mensajes `PRINT`.  
  
```sql
USE AdventureWorks2012
GO  
BEGIN TRANSACTION
GO  
IF @@TRANCOUNT = 0  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM Person.Person WHERE LastName = 'Adams'
    ROLLBACK TRANSACTION
    PRINT N'Rolling back the transaction two times would cause an error.'
END
ROLLBACK TRANSACTION
PRINT N'Rolled back the transaction.'
GO  
/*  
Rolled back the transaction.  
*/  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el siguiente ejemplo, `BEGIN` y `END` definen un conjunto de instrucciones [!INCLUDE[DWsql](../../includes/dwsql-md.md)] que se ejecutan juntas. Si no se incluye el bloque `BEGIN...END`, en el ejemplo siguiente se estará en un bucle continuo.  
  
```sql
-- Uses AdventureWorks  

DECLARE @Iteration Integer = 0  
WHILE @Iteration <10  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM dbo.DimCustomer WHERE LastName = 'Adams'
    SET @Iteration += 1  
END
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Lenguaje de control de flujo &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [END &#40;BEGIN...END&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/end-begin-end-transact-sql.md)
