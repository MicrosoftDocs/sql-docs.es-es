---
description: DROP MESSAGE TYPE (Transact-SQL)
title: DROP MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_MESSAGE_TYPE_TSQL
- DROP MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- message types [Service Broker], removing
- deleting message types
- dropping message types
- DROP MESSAGE TYPE statement
- removing message types
ms.assetid: 805e8ad5-8a93-49f0-88e5-e6fca8814dd5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2a31f1cae33f2c2899fc5ec8286f167c184894d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201128"
---
# <a name="drop-message-type-transact-sql"></a>DROP MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Quita un tipo de mensaje existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP MESSAGE TYPE message_type_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *message_type_name*  
 Es el nombre del tipo de mensaje que se va a eliminar. No se pueden especificar nombres de servidor, base de datos o esquema.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, se concede permiso para quitar un tipo de mensaje al propietario del tipo de mensaje, a los miembros de los roles fijos de base de datos ddl_admin o db_owner y a los miembros del rol fijo de servidor sysadmin.  
  
## <a name="remarks"></a>Comentarios  
 No puede quitar un tipo de mensaje si algún contrato hace referencia al tipo de mensaje.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente elimina el tipo de mensaje `//Adventure-Works.com/Expenses/SubmitExpense` de la base de datos.  
  
```sql  
DROP MESSAGE TYPE [//Adventure-Works.com/Expenses/SubmitExpense] ;  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-message-type-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
