---
description: CURRENT_TRANSACTION_ID (Transact-SQL)
title: CURRENT_TRANSACTION_ID (Transact-SQL)
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_TRANSACTION_ID
- CURRENT_TRANSACTION_ID_TSQL
- sys.CURRENT_TRANSACTION_ID
- sys.CURRENT_TRANSACTION_ID_TSQL
helpviewer_keywords:
- CURRENT_TRANSACTION_ID function
ms.assetid: 82cd9f92-d935-45a0-a433-620d6e15b467
author: cawrites
ms.author: chadam
ms.openlocfilehash: f48cffa6ebde2b0bae948d104636e3839b25a338
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184067"
---
# <a name="current_transaction_id-transact-sql"></a>CURRENT_TRANSACTION_ID (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Esta función devuelve el identificador de transacción de la transacción actual en la sesión actual.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CURRENT_TRANSACTION_ID( )  
  
```  

## <a name="return-types"></a>Tipos de valores devueltos

**bigint**
  
## <a name="return-value"></a>Valor devuelto  
El identificador de transacción de la transacción actual en la sesión actual tomado de [sys.dm_tran_current_transaction &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql.md).
  
## <a name="permissions"></a>Permisos  
Cualquier usuario puede devolver el identificador de transacción de la sesión actual.
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se devuelve el identificador de transacción de la sesión actual:
  
```sql
SELECT CURRENT_TRANSACTION_ID();  
```  
  
## <a name="see-also"></a>Consulte también
[sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
[SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)  
[Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md)  
[CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)  
[SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)
  
  
