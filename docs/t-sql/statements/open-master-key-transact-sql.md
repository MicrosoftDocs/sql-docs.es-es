---
description: OPEN MASTER KEY (Transact-SQL)
title: OPEN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN MASTER KEY DECRYPTION BY PASSWORD
- OPEN_MASTER_KEY_DECRYPTION_BY_PASSWORD_TSQL
- MASTER_KEY_TSQL
- MASTER KEY
- OPEN_MASTER_KEY_TSQL
- MASTER KEY DECRYPTION
- OPEN MASTER KEY
- MASTER_KEY_DECRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- opening Database Master Keys
- encryption [SQL Server], Database Master Key
- cryptography [SQL Server], Database Master Key
- master key decryption
- OPEN MASTER KEY statement
- database master key [SQL Server], opening
ms.assetid: 1674753e-ca1e-4913-9ba4-b442e7106121
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0e9acddda3ac18a3ae82b9d09f6ecc1ee1fc793c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199362"
---
# <a name="open-master-key-transact-sql"></a>OPEN MASTER KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Abre la clave maestra de la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
OPEN MASTER KEY DECRYPTION BY PASSWORD = 'password'   
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 '*password*'  
 Contraseña con la que se cifró la clave maestra de la base de datos.  
  
## <a name="remarks"></a>Comentarios  
 Si la clave maestra de la base de datos se ha cifrado con la clave maestra de servicio, se abre automáticamente cuando sea necesario para el cifrado y el descifrado. En este caso, no es necesario usar la instrucción **OPEN MASTER KEY**.  
  
 La primera vez que se adjunta una base de datos o se restaura en una instancia nueva de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aún no se ha almacenado una copia de la clave maestra de la base de datos (cifrada por la clave maestra de servicio) en el servidor. Debe usar la instrucción **OPEN MASTER KEY** para descifrar la clave maestra de la base de datos (DMK). Una vez que se ha descifrado la clave maestra de la base de datos, tiene la posibilidad de habilitar el descifrado automático en el futuro usando la instrucción **ALTER MASTER KEY REGENERATE** para proporcionar al servidor una copia de la clave maestra de la base de datos cifrada con la clave maestra de servicio (SMK). Cuando una base de datos se haya actualizado desde una versión anterior, se debe volver a generar la DMK para usar el algoritmo AES más reciente. Para obtener más información sobre cómo volver a generar la DMK, vea [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md). El tiempo necesario para volver a generar la DMK con el fin de actualizarse a AES depende del número de objetos protegidos por la DMK. Solo es necesario volver a generar la DMK una vez y no tiene ningún efecto sobre las nuevas generaciones futuras como parte de una estrategia de rotación de claves.  
  
 Puede excluir la clave maestra de una base de datos específica de la administración automática de claves mediante la instrucción ALTER MASTER KEY con la opción DROP ENCRYPTION BY SERVICE MASTER KEY. A continuación, debe abrir de forma explícita la clave maestra de la base de datos con una contraseña.  
  
 Si se revierte una transacción en que la clave maestra de la base de datos se ha abierto de forma explícita, la clave permanece abierta.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso CONTROL en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se abre la clave maestra de la base de datos `AdventureWorks2012`, cifrada con una contraseña.  
  
```sql  
USE AdventureWorks2012;  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '43987hkhj4325tsku7';  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En este ejemplo se abre la clave maestra de la base de datos, que se ha cifrado con una contraseña.  
  
```sql  
USE master;  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '43987hkhj4325tsku7';  
GO  
CLOSE MASTER KEY;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CLOSE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-master-key-transact-sql.md)   
 [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-master-key-transact-sql.md)   
 [RESTORE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-master-key-transact-sql.md)   
 [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [DROP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-master-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

