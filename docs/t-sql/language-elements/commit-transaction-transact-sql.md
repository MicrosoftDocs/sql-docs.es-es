---
description: COMMIT TRANSACTION (Transact-SQL)
title: COMMIT TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COMMIT
- COMMIT TRANSACTION
- COMMIT_TSQL
- COMMIT_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending transactions [SQL Server]
- user-defined transactions [SQL Server]
- committed transactions
- transactions [SQL Server], ending
- marking end of transactions [SQL Server]
- implicit transactions
- distributed transactions [SQL Server], committed
- transactions [SQL Server], committed
- COMMIT TRANSACTION statement
- rolling back transactions, COMMIT TRANSACTION
ms.assetid: f8fe26a9-7911-497e-b348-4e69c7435dc1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b3f3997b0d5dfec86b9a12dc9645a0315e40253e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208247"
---
# <a name="commit-transaction-transact-sql"></a>COMMIT TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Marca el final de una transacción correcta, implícita o explícita. Si @@TRANCOUNT es 1, COMMIT TRANSACTION hace que todas las modificaciones de datos desde el inicio de la transacción sean parte permanente de la base de datos, libera los recursos de la transacción y reduce @@TRANCOUNT a 0. Si @@TRANCOUNT es mayor que 1, COMMIT TRANSACTION solo reduce @@TRANCOUNT en 1 y la transacción se mantiene activa.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Applies to SQL Server (starting with 2008) and Azure SQL Database
  
COMMIT [ { TRAN | TRANSACTION }  [ transaction_name | @tran_name_variable ] ] [ WITH ( DELAYED_DURABILITY = { OFF | ON } ) ]  
[ ; ]  
```  
 
```syntaxsql
-- Applies to Azure Synapse Analytics and Parallel Data Warehouse Database
  
COMMIT [ TRAN | TRANSACTION ] 
[ ; ]  
``` 
 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *transaction_name*  
 **SE APLICA A:** SQL Server y Azure SQL Database
 
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] lo omite. *transaction_name* especifica un nombre de transacción asignado por una instrucción BEGIN TRANSACTION anterior. *transaction_name* debe cumplir con las reglas para identificadores, pero no puede superar los 32 caracteres. *transaction_name* indica a los programadores a qué instrucción BEGIN TRANSACTION anidada está asociada la instrucción COMMIT TRANSACTION.  
  
 *\@tran_name_variable*  
 **SE APLICA A:** SQL Server y Azure SQL Database  
 
Se trata del nombre de una variable definida por el usuario que contiene un nombre de transacción válido. La variable se debe declarar con un tipo de datos char, varchar, nchar o nvarchar. Si se pasan más de 32 caracteres a la variable, solo se usarán 32 caracteres; el resto de los caracteres se truncarán.  
  
 DELAYED_DURABILITY  
 **SE APLICA A:** SQL Server y Azure SQL Database   

 La opción que solicita esta transacción se confirma con la durabilidad diferida. Se omitirá la solicitud si la base de datos se ha modificado con `DELAYED_DURABILITY = DISABLED` o `DELAYED_DURABILITY = FORCED`. Para saber más, vea [Control de la durabilidad de las transacciones](../../relational-databases/logs/control-transaction-durability.md).  
  
## <a name="remarks"></a>Observaciones  
 Es responsabilidad del programador de [!INCLUDE[tsql](../../includes/tsql-md.md)] utilizar COMMIT TRANSACTION solo en el punto donde todos los datos a los que hace referencia la transacción sean lógicamente correctos.  
  
 Si la transacción que se ha confirmado era una transacción [!INCLUDE[tsql](../../includes/tsql-md.md)] distribuida, COMMIT TRANSACTION hace que MS DTC utilice el protocolo de confirmación en dos fases para confirmar los servidores involucrados en la transacción. Si una transacción local afecta a dos o más bases de datos de la misma instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)], la instancia utiliza una confirmación interna en dos fases para confirmar todas las bases de datos involucradas en la transacción.  
  
 Cuando se utiliza en transacciones anidadas, las confirmaciones de las transacciones anidadas no liberan recursos ni hacen permanentes sus modificaciones. Las modificaciones sobre los datos solo quedan permanentes y se liberan los recursos cuando se confirma la transacción más externa. Cada COMMIT TRANSACTION que se ejecute cuando @@TRANCOUNT sea mayor que 1 solo reduce @@TRANCOUNT en 1. Cuando @@TRANCOUNT llega a 0, se confirma la transacción externa completa. Como [!INCLUDE[ssDE](../../includes/ssde-md.md)] ignora *transaction_name*, la ejecución de una instrucción COMMIT TRANSACTION que haga referencia al nombre de una transacción externa cuando haya transacciones internas pendientes solo reduce @@TRANCOUNT en 1.  
  
 La ejecución de COMMIT TRANSACTION cuando @@TRANCOUNT es 0 produce un error; no hay ninguna instrucción BEGIN TRANSACTION asociada.  
  
 No se puede revertir una transacción después de ejecutar una instrucción COMMIT TRANSACTION, porque las modificaciones sobre los datos ya son parte permanente de la base de datos.  
  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] incrementa el recuento de transacciones de una instrucción solo cuando el recuento de transacciones es 0 al inicio de la instrucción.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-committing-a-transaction"></a>A. Confirmación de una transacción  
**SE APLICA A:** SQL Server, Azure SQL Database, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]   

En el siguiente ejemplo se elimina a un candidato a un puesto de trabajo. Usa AdventureWorks. 
  
```sql   
BEGIN TRANSACTION;   
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;   
COMMIT TRANSACTION;   
```  
  
### <a name="b-committing-a-nested-transaction"></a>B. Confirmar una transacción anidada  
**SE APLICA A:** SQL Server y Azure SQL Database    

En el siguiente ejemplo se crea una tabla, se generan tres niveles de transacciones anidadas y después se confirma la transacción anidada. Aunque cada instrucción `COMMIT TRANSACTION` cuenta con un parámetro *transaction_name*, no existe ninguna relación entre las instrucciones `COMMIT TRANSACTION` y `BEGIN TRANSACTION`. Los parámetros *transaction_name* ayudan al programador a asegurarse de que escribe el número apropiado de confirmaciones para reducir `@@TRANCOUNT` hasta 0, confirmando así la transacción más externa. 
  
```sql   
IF OBJECT_ID(N'TestTran',N'U') IS NOT NULL  
    DROP TABLE TestTran;  
GO  
CREATE TABLE TestTran (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
-- This statement sets @@TRANCOUNT to 1.  
BEGIN TRANSACTION OuterTran;  
  
PRINT N'Transaction count after BEGIN OuterTran = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
 
INSERT INTO TestTran VALUES (1, 'aaa');  
 
-- This statement sets @@TRANCOUNT to 2.  
BEGIN TRANSACTION Inner1;  
 
PRINT N'Transaction count after BEGIN Inner1 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
INSERT INTO TestTran VALUES (2, 'bbb');  
  
-- This statement sets @@TRANCOUNT to 3.  
BEGIN TRANSACTION Inner2;  
  
PRINT N'Transaction count after BEGIN Inner2 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
INSERT INTO TestTran VALUES (3, 'ccc');  
  
-- This statement decrements @@TRANCOUNT to 2.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner2;  
 
PRINT N'Transaction count after COMMIT Inner2 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
 
-- This statement decrements @@TRANCOUNT to 1.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner1;  
 
PRINT N'Transaction count after COMMIT Inner1 = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
  
-- This statement decrements @@TRANCOUNT to 0 and  
-- commits outer transaction OuterTran.  
COMMIT TRANSACTION OuterTran;  
  
PRINT N'Transaction count after COMMIT OuterTran = '  
    + CAST(@@TRANCOUNT AS NVARCHAR(10));  
```  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
