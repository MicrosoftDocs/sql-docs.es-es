---
description: ROLLBACK TRANSACTION (Transact-SQL)
title: ROLLBACK TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROLLBACK TRANSACTION
- ROLLBACK
- ROLLBACK_TSQL
- ROLLBACK_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- ROLLBACK TRANSACTION statement
- erasing data modifications [SQL Server]
- rolling back transactions, ROLLBACK TRANSACTION
- roll back transactions [SQL Server]
- savepoints [SQL Server]
ms.assetid: 6882c5bc-ff74-476a-984b-164aeb036c66
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a35054d5fcea54d00c63f3e007092933128c7d81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194342"
---
# <a name="rollback-transaction-transact-sql"></a>ROLLBACK TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Revierte una transacción explícita o implícita hasta el inicio de la transacción o hasta un punto de retorno dentro de la transacción. Puede usar ROLLBACK TRANSACTION para borrar todas las modificaciones de datos realizadas desde el inicio de la transacción o hasta un punto de retorno. También libera los recursos que mantiene la transacción.  
  
  No se incluyen los cambios realizados en variables locales o variables de tabla. No se borran con esta instrucción.
  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ROLLBACK { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable  
     | savepoint_name | @savepoint_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *transaction_name*  
 Es el nombre asignado a la transacción en BEGIN TRANSACTION. *transaction_name* debe cumplir las reglas de los identificadores, aunque solo se usan los 32 primeros caracteres del nombre de la transacción. Cuando se anidan transacciones, *transaction_name* debe ser el nombre de la instrucción BEGIN TRANSACTION más externa. *transaction_name* siempre distingue mayúsculas de minúsculas, incluso cuando la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no distingue mayúsculas de minúsculas.  
  
 **@** *tran_name_variable*  
 Se trata del nombre de una variable definida por el usuario que contiene un nombre de transacción válido. La variable debe declararse con un tipo de datos **char**, **varchar**, **nchar** o **nvarchar**.  
  
 *savepoint_name*  
 Es el parámetro *savepoint_name* de una instrucción SAVE TRANSACTION. *savepoint_name* debe cumplir las reglas de los identificadores. Use *savepoint_name* cuando una operación de reversión condicional solo deba afectar a parte de la transacción.  
  
 **@** *savepoint_variable*  
 Es el nombre de una variable definida por el usuario que contiene un nombre de punto de retorno válido. La variable debe declararse con un tipo de datos **char**, **varchar**, **nchar** o **nvarchar**.  
  
## <a name="error-handling"></a>Tratamiento de errores  
 Una instrucción ROLLBACK TRANSACTION no produce ningún mensaje para el usuario. Si necesita indicar advertencias en procedimientos almacenados o en desencadenadores, utilice las instrucciones RAISERROR o PRINT. RAISERROR es la instrucción más adecuada para indicar errores.  
  
## <a name="general-remarks"></a>Notas generales  
 ROLLBACK TRANSACTION sin *savepoint_name* o *transaction_name* revierte al principio de la transacción. Cuando se trata de transacciones anidadas, esta misma instrucción revierte todas las transacciones internas hasta la instrucción BEGIN TRANSACTION más externa. En ambos casos, ROLLBACK TRANSACTION disminuye la función del sistema @@TRANCOUNT a 0. ROLLBACK TRANSACTION *savepoint_name* no disminuye @@TRANCOUNT.  
  
 ROLLBACK TRANSACTION no puede hacer referencia a *savepoint_name* en transacciones distribuidas que se inician de forma explícita con BEGIN DISTRIBUTED TRANSACTION o que se escalan desde una transacción local.  
  
 Una transacción no se puede revertir después de ejecutar una instrucción COMMIT TRANSACTION, excepto cuando COMMIT TRANSACTION está asociada a una transacción anidada incluida en la transacción que se revierte. En esta instancia, la transacción anidada se revierte, incluso si ha emitido una instrucción COMMIT TRANSACTION para ella.  
  
 En una transacción se permiten nombres de puntos de retorno duplicados pero una instrucción ROLLBACK TRANSACTION que use este nombre solo revierte las transacciones realizadas hasta la instrucción SAVE TRANSACTION más reciente que también emplee este nombre.  
  
## <a name="interoperability"></a>Interoperabilidad  
 En procedimientos almacenados, las instrucciones ROLLBACK TRANSACTION sin *savepoint_name* ni *transaction_name* revierten todas las instrucciones a la instrucción BEGIN TRANSACTION más externa. Una instrucción ROLLBACK TRANSACTION de un procedimiento almacenado que provoca que @@TRANCOUNT muestre un valor diferente cuando finaliza el procedimiento almacenado que el valor @@TRANCOUNT en el momento de la llamada al procedimiento almacenado, genera un mensaje informativo. Este mensaje no afecta a los siguientes procesos.  
  
 Si se emite la instrucción ROLLBACK TRANSACTION en un desencadenador:  
  
-   Se revierten todas las modificaciones de datos realizadas hasta ese punto de la transacción actual, incluidas las que realizó el desencadenador.  
  
-   El desencadenador continúa la ejecución del resto de las instrucciones después de la instrucción ROLLBACK. Si alguna de estas instrucciones modifica datos, no se revierten las modificaciones. La ejecución de las instrucciones restantes no activa ningún desencadenador anidado.  
  
-   Tampoco se ejecutan las instrucciones del lote después de la instrucción que activó el desencadenador.  
  
@@TRANCOUNT se incrementa en uno al especificar un desencadenador, incluso cuando está en modo de confirmación automática. (El sistema trata a un desencadenador como a una transacción anidada implícita.)  
  
Las instrucciones ROLLBACK TRANSACTION de los procedimientos almacenados no afectan a las siguientes instrucciones del lote que llamó al procedimiento; se ejecutan las siguientes instrucciones del lote. Las instrucciones ROLLBACK TRANSACTION de los desencadenadores finalizan el lote que contiene la instrucción que activó el desencadenador; no se ejecutan las siguientes instrucciones del lote.  
  
El efecto de ROLLBACK en los cursores se define mediante estas reglas:  
  
1.  Con CURSOR_CLOSE_ON_COMMIT establecido en ON, ROLLBACK cierra todos los cursores abiertos pero sin desasignarlos.  
  
2.  Con CURSOR_CLOSE_ON_COMMIT establecido en OFF, ROLLBACK no afecta a los cursores STATIC o INSENSITIVE sincrónicos abiertos o a los cursores STATIC asincrónicos que se hayan llenado completamente. Se cierran los cursores de otros tipos que estén abiertos, pero sin cancelar su asignación.  
  
3.  Un error que finaliza un lote y genera una operación de reversión interna cancela la asignación de todos los cursores declarados en el lote que contiene la instrucción errónea. Se cancela la asignación de todos los cursores independientemente de su tipo o de la configuración de CURSOR_CLOSE_ON_COMMIT. Esto incluye los cursores declarados en procedimientos almacenados a los que llama el lote con errores. Los cursores declarados en un lote  antes del lote con el error están sujetos a las reglas 1 y 2. Un error de interbloqueo es un ejemplo de este tipo de error. Una instrucción ROLLBACK emitida en un desencadenador también genera automáticamente este tipo de error.  
  
## <a name="locking-behavior"></a>Comportamiento del bloqueo  
 Una instrucción ROLLBACK TRANSACTION que especifica un parámetro *savepoint_name* libera todos los bloqueos adquiridos más allá del punto de retorno, a excepción de las extensiones y las conversiones. Estos bloqueos no se liberan y no vuelven a convertirse a su modo de bloqueo previo.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra el efecto de revertir una transacción con nombre. Después de crear una tabla, las instrucciones siguientes inician una transacción con nombre, insertan dos filas y luego revierten la transacción con nombre en la variable @TransactionName. Otra instrucción fuera de la transacción con nombre inserta dos filas. La consulta devuelve los resultados de las instrucciones anteriores.   
  
```sql    
USE tempdb;  
GO  
CREATE TABLE ValueTable ([value] INT);  
GO  
  
DECLARE @TransactionName VARCHAR(20) = 'Transaction1';  
  
BEGIN TRAN @TransactionName  
       INSERT INTO ValueTable VALUES(1), (2);  
ROLLBACK TRAN @TransactionName;  
  
INSERT INTO ValueTable VALUES(3),(4);  
  
SELECT [value] FROM ValueTable;  
  
DROP TABLE ValueTable;  
```  
[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]  
```  
value  
-----   
3    
4  
```  
  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
