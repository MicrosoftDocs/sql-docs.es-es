---
description: DROP SYMMETRIC KEY (Transact-SQL)
title: DROP SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SYMMETRIC KEY
- DROP_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], removing
- deleting symmetric keys
- encryption [SQL Server], symmetric keys
- removing symmetric keys
- dropping symmetric keys
- cryptography [SQL Server], symmetric keys
- DROP SYMMETRIC KEY statement
ms.assetid: 6150bc67-08cb-402e-9c24-b04c9654b434
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e18319e45c3d397e4283013ec31bb3c98424c892
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96131090"
---
# <a name="drop-symmetric-key-transact-sql"></a>DROP SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Quita una clave simétrica de la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
DROP SYMMETRIC KEY symmetric_key_name [REMOVE PROVIDER KEY]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *symmetric_key_name*  
 Es el nombre de la clave simétrica que se va a quitar.  
  
 REMOVE PROVIDER KEY  
 Quita una clave de Administración extensible de claves (EKM) de un dispositivo EKM. Para más información sobre la Administración extensible de claves, vea [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
## <a name="remarks"></a>Observaciones  
 Si la clave está abierta en la sesión actual, la instrucción registrará errores.  
  
 Si la clave asimétrica está asignada a una clave de Administración extensible de claves (EKM) en un dispositivo EKM y no se especifica la opción de **REMOVE PROVIDER KEY**, la clave se quitará de la base de datos pero no del dispositivo y se emitirá una advertencia.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL para la clave simétrica.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita una clave simétrica con el nombre `GailSammamishKey6` de la base de datos actual.  
  
```sql  
CLOSE SYMMETRIC KEY GailSammamishKey6;  
DROP SYMMETRIC KEY GailSammamishKey6;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CLOSE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
