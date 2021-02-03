---
description: DB_NAME (Transact-SQL)
title: DB_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DB_NAME
- DB_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database names [SQL Server], DB_NAME
- names [SQL Server], databases
- viewing database names
- displaying database names
- DB_NAME function
ms.assetid: e21fb33a-a3ea-49b0-bb6b-8f789a675a0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae307c97f04e5c260b9a921286a4d6f3e5b4b524
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211764"
---
# <a name="db_name-transact-sql"></a>DB_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve el nombre de una base de datos especificada.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DB_NAME ( [ database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*database_id*  

El número de identificación de la base de datos cuyo nombre va a devolver `DB_NAME`. Si la llamada a `DB_NAME` omite *database_id*, `DB_NAME` devuelve el nombre de la base de datos actual.
  
## <a name="return-types"></a>Tipos de valores devueltos
**nvarchar(128)**
  
## <a name="permissions"></a>Permisos  

Si el autor de la llamada de `DB_NAME` no posee una base de datos **master** o distinta de **tempdb** determinada, como mínimo se requieren los permisos `ALTER ANY DATABASE` o `VIEW ANY DATABASE` de nivel de servidor para ver la fila `DB_ID` correspondiente. Para la base de datos **master**, `DB_ID` necesita el permiso `CREATE DATABASE` como mínimo. La base de datos a la que se conecta el autor de la llamada siempre aparece en **sys.databases**.
  
> [!IMPORTANT]  
>  El rol público tiene el permiso `VIEW ANY DATABASE` de forma predeterminada, lo que permite a todos los inicios de sesión ver información de la base de datos. Para evitar que un inicio de sesión detecte una base de datos, use `REVOKE` para revocar el permiso `VIEW ANY DATABASE` del público, o bien use `DENY` para denegar el permiso `VIEW ANY DATABASE` para inicios de sesión individuales.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-the-current-database-name"></a>A. Devolver el nombre de la base de datos actual  
En este ejemplo se devuelve el nombre de la base de datos actual.
  
```sql
SELECT DB_NAME() AS [Current Database];  
GO  
```  
  
### <a name="b-returning-the-database-name-of-a-specified-database-id"></a>B. Devolver el nombre de la base de datos de un identificador de base de datos específico  
En este ejemplo se devuelve el nombre de la base de datos con el identificador `3`.
  
```sql
USE master;  
GO  
SELECT DB_NAME(3) AS [Database Name];  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-return-the-current-database-name"></a>C. Devolver el nombre de la base de datos actual  
  
```sql
SELECT DB_NAME() AS [Current Database];  
```  
  
### <a name="d-return-the-name-of-a-database-by-using-the-database-id"></a>D. Devolver el nombre de la base de datos usando el identificador de base de datos  
En este ejemplo se devuelve el nombre y el identificador de base de datos de cada base de datos.
  
```sql
SELECT DB_NAME(database_id) AS [Database], database_id  
FROM sys.databases;  
```  
  
## <a name="see-also"></a>Vea también
[DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)  
[Funciones de metadatos &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
  
  

