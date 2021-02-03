---
description: DATABASE_PRINCIPAL_ID (Transact-SQL)
title: DATABASE_PRINCIPAL_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_PRINCIPAL_ID_TSQL
- DATABASE_PRINCIPAL_ID
dev_langs:
- TSQL
helpviewer_keywords:
- identification numbers [SQL Server], principals
- principal ID numbers [SQL Server]
- DATABASE_PRINCIPAL_ID function
- IDs [SQL Server], principals
ms.assetid: 908c7dd8-c10b-4658-92f6-0224f9835dd9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 911c4733853d3920486c2b1cd37298159b539e2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184038"
---
# <a name="database_principal_id-transact-sql"></a>DATABASE_PRINCIPAL_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Esta función devuelve el número de Id. de una entidad de seguridad de la base de datos actual. Vea [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md) para obtener más información sobre las entidades de seguridad.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATABASE_PRINCIPAL_ID ( 'principal_name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*principal_name*  
Una expresión de tipo **sysname** que representa la entidad de seguridad. Si se omite *principal_name*, `DATABASE_PRINCIPAL_ID` devuelve el identificador del usuario actual. `DATABASE_PRINCIPAL_ID` requiere los paréntesis.
  
## <a name="return-types"></a>Tipos de valores devueltos
**int**  
NULL si la entidad de seguridad de base de datos no existe.
  
## <a name="remarks"></a>Comentarios  
Use `DATABASE_PRINCIPAL_ID` en una lista de selección, en una cláusula WHERE o en cualquier lugar que permita una expresión. Para más información, vea [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-retrieving-the-id-of-the-current-user"></a>A. Recuperar el Id. del usuario actual  
En este ejemplo se devuelve el identificador de la entidad de seguridad de base de datos del usuario actual.
  
```sql
SELECT DATABASE_PRINCIPAL_ID();  
GO  
```  
  
### <a name="b-retrieving-the-id-of-a-specified-database-principal"></a>B. Recuperar el Id. de la entidad de seguridad de base de datos especificada  
En este ejemplo se devuelve el identificador de la entidad de seguridad de base de datos del rol de base de datos `db_owner`.
  
```sql
SELECT DATABASE_PRINCIPAL_ID('db_owner');  
GO  
```  
  
## <a name="see-also"></a>Consulte también
[Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
[Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
[sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
  
  
