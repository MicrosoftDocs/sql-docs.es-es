---
description: BEGIN TRANSACTION (Transact-SQL)
title: BEGIN TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN_TRANSACTION_TSQL
- TRANSACTION_TSQL
- TRANSACTION
- BEGIN TRANSACTION
- BEGIN TRAN
- BEGIN_TRAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction logs [SQL Server], BEGIN TRANSACTION statement
- marked transactions [SQL Server], BEGIN TRANSACTION statement
- BEGIN TRANSACTION statement
- local transactions [SQL Server]
- marking transactions [SQL Server]
- transactions [SQL Server], starting
- transaction names [SQL Server]
- starting point marked for transactions
- starting transactions
ms.assetid: c6258df4-11f1-416a-816b-54f98c11145e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bad9134c5fbdfd3908491a510c6bedafbb93d977
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183346"
---
# <a name="begin-transaction-transact-sql"></a>BEGIN TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Marca el punto de inicio de una transacción local explícita. Las transacciones explícitas empiezan con la instrucción BEGIN TRANSACTION y acaban con la instrucción COMMIT o ROLLBACK.  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
--Applies to SQL Server and Azure SQL Database
 
BEGIN { TRAN | TRANSACTION }   
    [ { transaction_name | @tran_name_variable }  
      [ WITH MARK [ 'description' ] ]  
    ]  
[ ; ]  
```  
 
```syntaxsql
--Applies to Azure Synapse Analytics and Parallel Data Warehouse
 
BEGIN { TRAN | TRANSACTION }   
[ ; ]  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *transaction_name*  
 **SE APLICA A:** SQL Server (a partir de 2008), Azure SQL Database
 
 Es el nombre asignado a la transacción. *transaction_name* debe cumplir las reglas de los identificadores, pero no se admiten identificadores de más de 32 caracteres. Utilice nombres de transacciones solamente en la pareja más externa de instrucciones BEGIN…COMMIT o BEGIN…ROLLBACK anidadas. *transaction_name* siempre distingue mayúsculas de minúsculas, incluso cuando la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no distingue mayúsculas de minúsculas.  
  
 @*tran_name_variable*  
 **SE APLICA A:** SQL Server (a partir de 2008), Azure SQL Database
 
 Se trata del nombre de una variable definida por el usuario que contiene un nombre de transacción válido. La variable debe declararse con un tipo de datos **char**, **varchar**, **nchar** o **nvarchar**. Si se pasan más de 32 caracteres a la variable, solo se utilizarán los primeros 32; el resto de caracteres se truncará.  
  
 WITH MARK [ '*description*' ]  
**SE APLICA A:** SQL Server (a partir de 2008), Azure SQL Database

Especifica que la transacción está marcada en el registro. *description* es una cadena que describe la marca. Un valor *description* de más de 128 caracteres se trunca en 128 antes de almacenarse en la tabla msdb.dbo.logmarkhistory.  
  
 Si utiliza WITH MARK, debe especificar un nombre de transacción. WITH MARK permite restaurar un registro de transacciones hasta una marca con nombre.  
  
## <a name="general-remarks"></a>Notas generales
BEGIN TRANSACTION incrementa @@TRANCOUNT en 1.
  
BEGIN TRANSACTION representa un punto en el que los datos a los que hace referencia una conexión son lógica y físicamente coherentes. Si se producen errores, se pueden revertir todas las modificaciones realizadas en los datos después de BEGIN TRANSACTION para devolver los datos al estado conocido de coherencia. Cada transacción dura hasta que se completa sin errores y se emite COMMIT TRANSACTION para hacer que las modificaciones sean una parte permanente de la base de datos, o hasta que se produzcan errores y se borren todas las modificaciones con la instrucción ROLLBACK TRANSACTION.  
  
BEGIN TRANSACTION inicia una transacción local para la conexión que emite la instrucción. Según la configuración del nivel de aislamiento de la transacción actual, la transacción bloquea muchos recursos adquiridos para aceptar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] emitidas por la conexión hasta que la misma finaliza con una instrucción COMMIT TRANSACTION o ROLLBACK TRANSACTION. Las transacciones que quedan pendientes durante mucho tiempo pueden impedir que otros usuarios tengan acceso a estos recursos bloqueados y pueden impedir también el truncamiento del registro.  
  
 Aunque BEGIN TRANSACTION inicia una transacción local, ésta no se guardará en el registro de transacciones hasta que la aplicación realice posteriormente una acción que se deba almacenar en el registro, como la ejecución de una instrucción INSERT, UPDATE o DELETE. Una aplicación puede realizar acciones tales como adquirir bloqueos para proteger el nivel de aislamiento de transacción de instrucciones SELECT, pero no se guarda ningún dato en el registro hasta que la aplicación realiza una acción de modificación.  
  
 Asignar un nombre a varias transacciones en un conjunto de transacciones anidadas afecta mínimamente a la transacción. Solamente el nombre de la primera transacción (la más externa) se registra en el sistema. Revertir a otro nombre (que no sea un nombre de punto de retorno válido) genera un error. De hecho, no se revierte ninguna de las instrucciones ejecutadas antes de la operación de revertir en el momento en que se produce este error. Solo se revierten las instrucciones cuando se revierte la transacción externa.  
  
 La transacción local iniciada por la instrucción BEGIN TRANSACTION aumenta al nivel de transacción distribuida si se realizan las siguientes acciones antes de confirmarla o revertirla:  
  
-   Se ejecuta una instrucción INSERT, DELETE o UPDATE que hace referencia a una tabla remota de un servidor vinculado. La instrucción INSERT, UPDATE o DELETE causa un error si el proveedor OLE DB usado para obtener acceso al servidor vinculado no es compatible con la interfaz ITransactionJoin.  
  
-   Se realiza una llamada a un procedimiento almacenado remoto cuando la opción REMOTE_PROC_TRANSACTIONS es ON.  
  
 La copia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se convierte en el controlador de la transacción y utiliza el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC) para administrar la transacción distribuida.  
  
 Una transacción se puede ejecutar explícitamente como una transacción distribuida utilizando BEGIN DISTRIBUTED TRANSACTION. Para obtener más información, vea [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
 Cuando SET IMPLICIT_TRANSACTIONS está configurado en ON, una instrucción BEGIN TRANSACTION abrirá dos transacciones anidadas. Para más información, vea [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)  
  
## <a name="marked-transactions"></a>Transacciones marcadas  
 La opción WITH MARK coloca el nombre de la transacción en el registro de transacciones. Al restaurar una base de datos a su estado anterior, se puede utilizar la transacción marcada en lugar de la fecha y la hora. Para más información, vea [Usar transacciones marcadas para recuperar bases de datos relacionadas sistemáticamente &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md) y [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 Además, se necesitan las marcas del registro de transacciones si tiene la intención de recuperar un conjunto de bases de datos relacionadas a un estado coherente lógicamente. Una transacción distribuida puede colocar marcas en los registros de transacción de las bases de datos relacionadas. Recuperar el conjunto de bases de datos relacionadas hasta estas marcas da como resultado un conjunto de bases de datos coherente en cuanto a las transacciones. La colocación de las marcas en las bases de datos relacionadas requiere procedimientos especiales.  
  
 La marca se coloca en el registro de transacciones solamente si la transacción marcada actualiza la base de datos. No se marcan las transacciones que no modifican los datos.  
  
 Se puede anidar BEGIN TRAN *new_name* WITH MARK en una transacción existente que no esté marcada. De ese modo, *new_name* se convierte en el nombre de marca de la transacción, aunque esa transacción ya tenga uno. En el siguiente ejemplo, `M2` es el nombre de la marca.  
  
```sql  
BEGIN TRAN T1;  
UPDATE table1 ...;  
BEGIN TRAN M2 WITH MARK;  
UPDATE table2 ...;  
SELECT * from table1;  
COMMIT TRAN M2;  
UPDATE table3 ...;  
COMMIT TRAN T1;  
```  
  
 Al anidar transacciones, intentar marcar una transacción ya marcada da lugar a un mensaje de advertencia (no de error):  
  
 "BEGIN TRAN T1 WITH MARK ...;"  
  
 "UPDATE tabla1 ...;"  
  
 "BEGIN TRAN M2 WITH MARK ...;"  
  
 "Servidor: mensaje 3920, nivel 16, estado 1, línea 3"  
  
 "La opción WITH MARK solo se aplica a la primera instrucción BEGIN TRAN WITH MARK."  
  
 "Se omitirá la opción."  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-an-explicit-transaction"></a>A. Usar una transacción explícita
**SE APLICA A:** SQL Server (a partir de la versión 2008), Azure SQL Database, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Almacenamiento de datos en paralelo

En este ejemplo se usa AdventureWorks. 

```sql
BEGIN TRANSACTION;  
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT;  
```

### <a name="b-rolling-back-a-transaction"></a>B. Revertir una transacción
**SE APLICA A:** SQL Server (a partir de la versión 2008), Azure SQL Database, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], Almacenamiento de datos en paralelo

En el ejemplo siguiente se muestra el efecto de revertir una transacción. En este ejemplo, la instrucción ROLLBACK revertirá la instrucción INSERT, pero la tabla creada seguirá existiendo.

```sql
CREATE TABLE ValueTable (id INT);  
BEGIN TRANSACTION;  
       INSERT INTO ValueTable VALUES(1);  
       INSERT INTO ValueTable VALUES(2);  
ROLLBACK;  

```

### <a name="c-naming-a-transaction"></a>C. Asignar un nombre a una transacción 
**SE APLICA A:** SQL Server (a partir de 2008), Azure SQL Database

En el siguiente ejemplo se muestra cómo asignar un nombre a una transacción.  
  
```sql
DECLARE @TranName VARCHAR(20);  
SELECT @TranName = 'MyTransaction';  
  
BEGIN TRANSACTION @TranName;  
USE AdventureWorks2012;  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
  
COMMIT TRANSACTION @TranName;  
GO  
```  
  
### <a name="d-marking-a-transaction"></a>D. Marcar una transacción  
**SE APLICA A:** SQL Server (a partir de 2008), Azure SQL Database

En el siguiente ejemplo se muestra cómo marcar una transacción. Se marca la transacción `CandidateDelete`.  
  
```sql  
BEGIN TRANSACTION CandidateDelete  
    WITH MARK N'Deleting a Job Candidate';  
GO  
USE AdventureWorks2012;  
GO  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
GO  
COMMIT TRANSACTION CandidateDelete;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
