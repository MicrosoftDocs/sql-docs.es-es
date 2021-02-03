---
description: ALTER MASTER KEY (Transact-SQL)
title: ALTER MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: fasttrack-edit
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER MASTER KEY
- ALTER_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REGENERATE phrase
- ALTER MASTER KEY statement
- decryption [SQL Server], Database Master Key
- FORCE option
- encryption [SQL Server], Database Master Key
- database master key [SQL Server], modifying
- cryptography [SQL Server], Database Master Key
- modifying Database Master Key
- service master key [SQL Server], modifying
- DROP ENCRYPTION BY SERVICE MASTER KEY phrase
ms.assetid: 8ac501c3-4280-4d5b-b58a-1524fa715b50
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 677de39f127ea8efc853746168eea1d3be7e4eb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204179"
---
# <a name="alter-master-key-transact-sql"></a>ALTER MASTER KEY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cambia las propiedades de la clave maestra de una base de datos.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD = 'password'

<encryption_option> ::=
    ADD ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
    |
    DROP ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
```

```syntaxsql
-- Syntax for Azure SQL Database
-- Note: DROP ENCRYPTION BY SERVICE MASTER KEY is not supported on Azure SQL Database.

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD = 'password'

<encryption_option> ::=
    ADD ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
    |
    DROP ENCRYPTION BY { PASSWORD = 'password' }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Analytics Platform System

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD ='password'

<encryption_option> ::=
    ADD ENCRYPTION BY SERVICE MASTER KEY
    |
    DROP ENCRYPTION BY SERVICE MASTER KEY
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

PASSWORD ='*password*' Especifica una contraseña con la que se va a cifrar o descifrar la clave maestra de la base de datos. *password* debe cumplir los requisitos de la directiva de contraseñas de Windows del equipo que ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="remarks"></a>Observaciones

La opción REGENERATE vuelve a crear la clave maestra de la base de datos y todas las claves que ésta protege. Las claves se descifran primero con la antigua clave maestra y, a continuación, se cifran con la nueva clave maestra. Puesto que esta operación hace un uso intensivo de recursos, debe programarse durante un periodo de baja demanda, a menos que la clave maestra se encuentre en peligro.

[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] usa el algoritmo de cifrado AES para proteger la clave maestra de servicio (SMK) y la clave maestra de la base de datos (DMK). AES es un algoritmo de cifrado más reciente que el algoritmo 3DES empleado en versiones anteriores. Después de actualizar una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , se deben volver a generar las claves SMK y DMK para actualizar las claves maestras al algoritmo AES. Para más información sobre cómo volver a generar la SMK, vea [ALTER SERVICE MASTER KEY](../../t-sql/statements/alter-service-master-key-transact-sql.md).

Cuando se usa la opción FORCE, la regeneración de la clave continuará aunque la clave maestra no se encuentre disponible o el servidor no pueda descifrar todas las claves privadas cifradas. Si no puede abrir la clave maestra, use la instrucción [RESTORE MASTER KEY](../../t-sql/statements/restore-master-key-transact-sql.md) para restaurar la clave maestra desde una copia de seguridad. Utilice la opción FORCE únicamente si la clave maestra es irrecuperable o si se producen errores en el descifrado. Se perderá la información cifrada solamente con una clave irrecuperable.

La opción DROP ENCRYPTION BY SERVICE MASTER KEY quita el cifrado de la clave maestra de la base de datos por parte de la clave maestra de servicio.

ADD ENCRYPTION BY SERVICE MASTER KEY cifra una copia de la clave maestra con la clave maestra de servicio y la almacena en la base de datos actual y en master.

## <a name="permissions"></a>Permisos

Necesita el permiso CONTROL en la base de datos. Si la clave maestra de la base de datos se ha cifrado con una contraseña, es necesario conocer dicha contraseña.

## <a name="examples"></a>Ejemplos

En el siguiente ejemplo se crea una nueva clave maestra de base de datos para `AdventureWorks` y se vuelven a cifrar las claves inferiores en la jerarquía de cifrado.

```sql
USE AdventureWorks2012;
ALTER MASTER KEY REGENERATE WITH ENCRYPTION BY PASSWORD = 'dsjdkflJ435907NnmM#sX003';
GO
```

## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

En el siguiente ejemplo se crea una clave maestra de base de datos para `AdventureWorksPDW2012` y se vuelven a cifrar las claves inferiores en la jerarquía de cifrado.

```sql
USE master;
ALTER MASTER KEY REGENERATE WITH ENCRYPTION BY PASSWORD = 'dsjdkflJ435907NnmM#sX003';
GO
```

## <a name="see-also"></a>Consulte también

- [CREATE MASTER KEY](../../t-sql/statements/create-master-key-transact-sql.md)
- [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md)
- [CLOSE MASTER KEY](../../t-sql/statements/close-master-key-transact-sql.md)
- [BACKUP MASTER KEY](../../t-sql/statements/backup-master-key-transact-sql.md)
- [RESTORE MASTER KEY](../../t-sql/statements/restore-master-key-transact-sql.md)
- [DROP MASTER KEY](../../t-sql/statements/drop-master-key-transact-sql.md)
- [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Adjuntar y separar bases de datos](../../relational-databases/databases/database-detach-and-attach-sql-server.md)
