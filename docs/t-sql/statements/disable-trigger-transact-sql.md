---
description: DISABLE TRIGGER (Transact-SQL)
title: DISABLE TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DISABLE_TSQL
- DISABLE
- DISABLE TRIGGER
- DISABLE_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DML triggers, disabling
- DDL triggers, disabling
- DISABLE TRIGGER statement
- triggers [SQL Server], disabling
- disabling triggers
ms.assetid: e6529f06-e442-437e-a7bf-41790bc092c5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 01302c2d9acdf1ce89246be18fd54c3cae72ebff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159346"
---
# <a name="disable-trigger-transact-sql"></a>DISABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Deshabilita un desencadenador.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DISABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre del esquema al que pertenece el desencadenador. *schema_name* no se puede especificar para los desencadenadores DDL o de inicio de sesión.  
  
 *trigger_name*  
 Es el nombre del desencadenador que se va a deshabilitar.  
  
 ALL  
 Indica que todos los desencadenadores definidos en el ámbito de la cláusula ON están deshabilitados.  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea desencadenadores en las bases de datos que se publican para la replicación de mezcla. Si se especifica ALL en las bases de datos publicadas, se deshabilitarán los desencadenadores y se interrumpirá la replicación. Compruebe que la base de datos actual no se publica para replicación de mezcla antes de especificar ALL.  
  
 *object_name*  
 Es el nombre de la tabla o la vista en la que se creó el desencadenador DML *trigger_name* para su ejecución.  
  
 DATABASE  
 En el caso de un desencadenador DDL, indica que *trigger_name* se creó o se modificó para ejecutarse en el ámbito de la base de datos.  
  
 ALL SERVER  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 En el caso de un desencadenador DDL, indica que *trigger_name* se creó o se modificó para ejecutarse en el ámbito de servidor. ALL SERVER también se aplica a los desencadenadores de inicio de sesión.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
## <a name="remarks"></a>Observaciones  
 De forma predeterminada, los desencadenadores se habilitan cuando se crean. Al deshabilitar un desencadenador no se quita. Sigue siendo un objeto de la base de datos actual. Sin embargo, el desencadenador no se activa cuando se ejecuta una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] en la que se programó. Los desencadenadores se pueden volver a habilitar con [ENABLE TRIGGER](../../t-sql/statements/enable-trigger-transact-sql.md). Los desencadenadores DML definidos en tablas también se pueden habilitar o deshabilitar mediante el uso de [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 Si se cambia el desencadenador mediante la instrucción **ALTER TRIGGER**, se habilita el desencadenador.  
  
## <a name="permissions"></a>Permisos  
 Para deshabilitar un desencadenador DML, el usuario debe contar, como mínimo, con permiso ALTER sobre la tabla o vista en la que se creó el desencadenador.  
  
 Para deshabilitar un desencadenador DDL con ámbito de servidor (ON ALL SERVER) o un desencadenador de inicio de sesión, el usuario debe tener el permiso CONTROL SERVER en el servidor. Para deshabilitar un desencadenador DDL con ámbito en la base de datos (ON DATABASE) el usuario debe contar, como mínimo, con un permiso ALTER ANY DATABASE DDL TRIGGER en la base de datos actual.  
  
## <a name="examples"></a>Ejemplos  
Los ejemplos siguientes se describen en la base de datos AdventureWorks2012.
  
### <a name="a-disabling-a-dml-trigger-on-a-table"></a>A. Deshabilitar un desencadenador DML en una tabla  
 En el ejemplo siguiente se deshabilita el desencadenador `uAddress`, que se creó en la tabla `Person`.  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-disabling-a-ddl-trigger"></a>B. Deshabilitar un desencadenador DDL  
 En el ejemplo siguiente se crea un desencadenador DDL `safety`, con ámbito en la base de datos, y después se deshabilita.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "safety" to drop or alter tables!'   
   ROLLBACK;  
GO  
DISABLE TRIGGER safety ON DATABASE;  
GO  
```  
  
### <a name="c-disabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. Deshabilitar todos los desencadenadores que se definieron con el mismo ámbito  
 En el ejemplo siguiente se deshabilitan todos los desencadenadores DDL creados en el ámbito de servidor.  
  
```sql  
DISABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  
