---
description: CLOSE SYMMETRIC KEY (Transact-SQL)
title: CLOSE SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CLOSE SYMMETRIC KEY
- CLOSE_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- closing symmetric keys
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], closing
- CLOSE SYMMETRIC KEY statement
- cryptography [SQL Server], symmetric keys
ms.assetid: 3b083cbb-3c6a-4f59-8d34-601db1efcc83
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 88b36782a114ca5a078d438719147892e46749c0
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421938"
---
# <a name="close-symmetric-key-transact-sql"></a>CLOSE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Cierra una clave simétrica o cierra todas las claves simétricas abiertas en la sesión actual.  
  
  ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)] 
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CLOSE { SYMMETRIC KEY key_name | ALL SYMMETRIC KEYS }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Key_name*  
 Es el nombre de la clave simétrica que se va a cerrar.  
  
## <a name="remarks"></a>Comentarios  
 Las claves simétricas abiertas están enlazadas a la sesión, no al contexto de seguridad. Una clave abierta seguirá disponible hasta que se cierre explícitamente o hasta que finalice la sesión. CLOSE ALL SYMMETRIC KEYS cerrará las claves maestras de la base de datos que estaban abiertas en la sesión actual utilizando la instrucción [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md).  Puede consultar la información sobre las claves abiertas en la vista de catálogo [sys.openkeys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Para cerrar una clave simétrica no es necesario ningún permiso explícito.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-closing-a-symmetric-key"></a>A. Cerrar una clave simétrica  
 En el siguiente ejemplo se cierra la clave simétrica `ShippingSymKey04`.  
  
```sql  
CLOSE SYMMETRIC KEY ShippingSymKey04;  
GO  
```  
  
### <a name="b-closing-all-symmetric-keys"></a>B. Cerrar todas las claves simétricas  
 En el siguiente ejemplo se cierran todas las claves simétricas abiertas en la sesión actual y también la clave maestra de la base de datos abierta de forma explícita.  
  
```sql  
CLOSE ALL SYMMETRIC KEYS;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)  
  
  
