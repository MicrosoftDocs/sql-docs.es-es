---
description: ROLLBACK WORK (Transact-SQL)
title: ROLLBACK WORK (Transact-SQL)
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROLLBACK WORK
- ROLLBACK_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- erasing data modifications [SQL Server]
- ROLLBACK WORK statement
- roll back transactions [SQL Server]
- rolling back transactions, ROLLBACK WORK
- savepoints [SQL Server]
ms.assetid: 2071dbd3-53d5-4510-be8d-26e80f2553b4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 67512d8fca35bfda9696192d511450d064f612d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194302"
---
# <a name="rollback-work-transact-sql"></a>ROLLBACK WORK (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Revierte una transacción especificada por el usuario al principio de la misma.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ROLLBACK [ WORK ]  
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Comentarios  
 Esta instrucción funciona de forma idéntica a ROLLBACK TRANSACTION, con la diferencia de que ROLLBACK TRANSACTION acepta nombres de transacción definidos por el usuario. Se especifique o no la palabra clave opcional WORK, esta sintaxis de ROLLBACK es compatible con ISO.  
  
 Al anidar transacciones, ROLLBACK WORK siempre revierte las transacciones hasta la instrucción BEGIN TRANSACTION más externa y disminuye la función del sistema @@TRANCOUNT a 0.  
  
## <a name="permissions"></a>Permisos  
 Los permisos ROLLBACK WORK corresponden, de forma predeterminada, a cualquier usuario válido.  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
