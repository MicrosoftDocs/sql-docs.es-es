---
description: DROP ASYMMETRIC KEY (Transact-SQL)
title: DROP ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP ASYMMETRIC KEY
- DROP_ASYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], removing
- removing asymmetric keys
- encryption [SQL Server], asymmetric keys
- DROP ASYMMETRIC KEY statement
- dropping asymmetric keys
- deleting asymmetric keys
- cryptography [SQL Server], asymmetric keys
ms.assetid: bf94ac07-9b62-4318-b55b-1eed8f3a1ac6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 75c47b56394c8bd2f9ea7e8fffa3de72584ebe34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200377"
---
# <a name="drop-asymmetric-key-transact-sql"></a>DROP ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Quita una clave asimétrica de la base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP ASYMMETRIC KEY key_name [ REMOVE PROVIDER KEY ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *key_name*  
 Es el nombre de la clave asimétrica que se va quitar de base de datos.  
  
 REMOVE PROVIDER KEY  
 Quita una clave de Administración extensible de claves (EKM) de un dispositivo EKM. Para más información sobre la Administración extensible de claves, vea [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
## <a name="remarks"></a>Comentarios  
 Una clave asimétrica con la que se ha cifrado la clave simétrica en la base de datos, o con la que está asignado un usuario o inicio de sesión, no se puede quitar. Antes de quitar dicha clave, debe quitar los usuarios o inicios de sesión asignados a la clave. También debe quitar o cambiar las claves simétricas cifradas con la clave asimétrica. Puede usar la opción DROP ENCRYPTION de [ALTER SYMMETRIC KEY](../../t-sql/statements/alter-symmetric-key-transact-sql.md) para eliminar el cifrado con una clave asimétrica.  
  
 Con la vista de catálogo [sys.asymmetric_keys](../../relational-databases/system-catalog-views/sys-asymmetric-keys-transact-sql.md) se puede acceder a los metadatos de claves asimétricas. Las propias claves no se pueden ver directamente desde dentro de la base de datos.  
  
 Si la clave asimétrica está asignada a una clave de Administración extensible de claves (EKM) en un dispositivo EKM y no se especifica la opción de REMOVE PROVIDER KEY, la clave se quitará de la base de datos pero no del dispositivo. Se emitirá una advertencia.  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso CONTROL en la clave asimétrica.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se elimina la clave asimétrica `MirandaXAsymKey6` de la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
DROP ASYMMETRIC KEY MirandaXAsymKey6;  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)  
  
  
