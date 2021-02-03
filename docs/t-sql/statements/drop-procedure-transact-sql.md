---
description: DROP PROCEDURE (Transact-SQL)
title: DROP PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP PROCEDURE
- DROP_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing stored procedures
- dropping procedure groups
- deleting stored procedures
- deleting procedure groups
- DROP PROCEDURE statement
- dropping stored procedures
- stored procedures [SQL Server], removing
- removing procedure groups
ms.assetid: 1c2d7235-7b9b-4336-8f17-429e7d82c2c3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 438738baa1734e38a06eecebb6a2cfbc8a97f026
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237007"
---
# <a name="drop-procedure-transact-sql"></a>DROP PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quita uno o más procedimientos almacenados o grupos de procedimientos de la base de datos actual en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP { PROC | PROCEDURE } [ IF EXISTS ] { [ schema_name. ] procedure } [ ,...n ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP { PROC | PROCEDURE } { [ schema_name. ] procedure_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *IF EXISTS*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Quita el procedimiento condicionalmente solo si ya existe.  
  
 *schema_name*  
 El nombre del esquema al que pertenece el procedimiento. No se puede especificar un nombre de servidor o un nombre de base de datos.  
  
 *procedure*  
 Nombre del procedimiento almacenado o grupo de procedimientos almacenados que se van a quitar. No se pueden quitar procedimientos concretos de un grupo de procedimientos numerados, ya que de este modo se quita el grupo de procedimientos completo.  
  
## <a name="best-practices"></a>Prácticas recomendadas  
 Antes de quitar un procedimiento almacenado, compruebe los objetos dependientes y modifique estos objetos como corresponda. La acción de quitar un procedimiento almacenado puede hacer que los objetos dependientes y los scripts sufran errores cuando estos objetos no están actualizados. Para más información, vea [Ver las dependencias de un procedimiento almacenado](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
## <a name="metadata"></a>Metadatos  
 Para consultar una lista de procedimientos existentes, vea la vista de catálogo **sys.objects**. Para consultar la definición del procedimiento, vea la vista de catálogo **sys.sql_modules**.  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Se necesita el permiso **CONTROL** en el procedimiento o el permiso **ALTER** en el esquema al que corresponde el procedimiento, o la pertenencia al rol fijo de servidor **db_ddladmin**.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita el procedimiento almacenado `dbo.uspMyProc` de la base de datos actual.  
  
```sql  
DROP PROCEDURE dbo.uspMyProc;  
GO  
```  
  
 En el siguiente ejemplo se quitan varios procedimientos almacenados de la base de datos actual.  
  
```sql  
DROP PROCEDURE dbo.uspGetSalesbyMonth, dbo.uspUpdateSalesQuotes, dbo.uspGetSalesByYear;  
```  
  
 En este ejemplo se quita el procedimiento almacenado `dbo.uspMyProc`, si existe, pero no se produce un error si el procedimiento no existe. Esta sintaxis es nueva en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
  
```sql  
DROP PROCEDURE IF EXISTS dbo.uspMyProc;  
GO  
```  
  
  
## <a name="see-also"></a>Consulte también  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Eliminar un procedimiento almacenado](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)  
