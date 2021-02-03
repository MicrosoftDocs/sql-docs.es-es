---
description: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
title: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/29/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DISTRIBUTED
- BEGIN_DISTRIBUTED_TRANSACTION_TSQL
- DISTRIBUTED_TSQL
- BEGIN DISTRIBUTED TRANSACTION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN DISTRIBUTED TRANSACTION statement
- distributed transactions [SQL Server], starting
- MS DTC, transaction management
- distributed transactions [SQL Server], BEGIN DISTRIBUTED TRANSACTION statement
- servers [SQL Server], distributed transactions
- starting distributed transactions
- remote servers [SQL Server], distributed transactions
- starting transactions
ms.assetid: c3bc2716-39d3-4061-8c6a-8734899231ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: e0d31150846696532f241c291a80ef226175c25c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200252"
---
# <a name="begin-distributed-transaction-transact-sql"></a>BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Especifica el inicio de una transacción distribuida de [!INCLUDE[tsql](../../includes/tsql-md.md)] administrada mediante el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC).  
    
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
BEGIN DISTRIBUTED { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *transaction_name*  
 Se trata de un nombre de transacción definida por el usuario que se utiliza para realizar el seguimiento de la transacción distribuida en las utilidades de MS DTC. *transaction_name* debe seguir las reglas de los identificadores y ser \<= 32 caracteres.  
  
 @*tran_name_variable*  
 Se trata del nombre de una variable definida por el usuario que contiene el nombre de una transacción utilizada para realizar el seguimiento de la transacción distribuida en las utilidades de MS DTC. La variable debe declararse con un tipo de datos **char**, **varchar**, **nchar** o **nvarchar**.  
  
## <a name="remarks"></a>Comentarios  
 La instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que ejecuta la instrucción BEGIN DISTRIBUTED TRANSACTION es el originador de la transacción y controla su realización. Posteriormente, cuando en la sesión se ejecuta una instrucción COMMIT TRANSACTION o ROLLBACK TRANSACTION, la instancia que controla la transacción solicita a MS DTC que administre la realización de la transacción distribuida entre todas las instancias participantes.  
  
 El aislamiento de instantáneas de nivel de instantánea no admite transacciones distribuidas.  
  
 La principal manera en que las instancias remotas del [!INCLUDE[ssDE](../../includes/ssde-md.md)] se dan de alta en una transacción distribuida es cuando una sesión ya dada de alta en la transacción distribuida ejecuta una consulta distribuida que hace referencia a un servidor vinculado.  
  
 Por ejemplo, si BEGIN DISTRIBUTED TRANSACTION se ejecuta en ServerA, la sesión llama a un procedimiento almacenado en ServerB y a otro procedimiento almacenado en ServerC. El procedimiento almacenado en ServerC ejecuta una consulta distribuida en ServerD y, a continuación, los cuatro equipos participan en la transacción distribuida. La instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] en ServerA es la instancia de origen que controla la transacción.  
  
 Las sesiones que participan en transacciones distribuidas de [!INCLUDE[tsql](../../includes/tsql-md.md)] no obtienen un objeto de transacción que puedan pasar a otra sesión para que se dé de alta explícitamente en la transacción distribuida. La única forma en que un servidor remoto puede darse de alta en la transacción consiste en que constituya el destino de una llamada a un procedimiento almacenado remoto o a una consulta distribuida.  
  
 Cuando se ejecuta una consulta distribuida en una transacción local, la transacción se promueve automáticamente al nivel de transacción distribuida si el origen de datos OLE DB de destino es compatible con ITransactionLocal. Si el origen de datos OLE DB de destino no es compatible con ITransactionLocal, únicamente se permite realizar operaciones de solo lectura en la consulta distribuida.  
  
 Una sesión que ya se ha dado de alta en la transacción distribuida realiza una llamada a un procedimiento almacenado remoto que hace referencia a un servidor remoto.  
  
 La opción **sp_configure remote proc trans** controla si las llamadas a procedimientos almacenados remotos en una transacción local provocan automáticamente que la transacción local se promueva al nivel de transacción distribuida administrada mediante MS DTC. Se puede usar la opción REMOTE_PROC_TRANSACTIONS de SET de nivel de conexión para anular la configuración predeterminada de la instancia establecida por **sp_configure remote proc trans**. Con esta opción activada, la llamada a un procedimiento almacenado remoto hace que una transacción local se promueva al nivel de transacción distribuida. La conexión que crea la transacción de MS DTC se convierte en el originador de la transacción. COMMIT TRANSACTION inicia una confirmación coordinada de MS DTC. Si la opción **sp_configure remote proc trans** está activada, las llamadas realizadas a procedimientos almacenados remotos en transacciones locales se protegen automáticamente como parte de transacciones distribuidas sin necesidad de volver a escribir las aplicaciones para que emitan específicamente BEGIN DISTRIBUTED TRANSACTION en lugar de BEGIN TRANSACTION.  
  
 Para obtener más información sobre el entorno y el proceso de transacciones distribuidas, vea la documentación del Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
 Este ejemplo elimina un candidato de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] tanto en la instancia local del [!INCLUDE[ssDE](../../includes/ssde-md.md)] como en la instancia de un servidor remoto. Ambas bases de datos, local y remota, confirmarán o revertirán la transacción.  
  
> [!NOTE]  
>  A menos que MS DTC esté instalado actualmente en el equipo que ejecuta la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)], este ejemplo produce un mensaje de error. Para obtener más información sobre cómo instalar Microsoft DTC, vea la documentación de Microsoft DTC (Coordinador de transacciones distribuidas).  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN DISTRIBUTED TRANSACTION;  
-- Delete candidate from local instance.  
DELETE AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
-- Delete candidate from remote instance.  
DELETE RemoteServer.AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
