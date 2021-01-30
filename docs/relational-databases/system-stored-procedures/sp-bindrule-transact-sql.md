---
description: sp_bindrule (Transact-SQL)
title: sp_bindrule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindrule_TSQL
- sp_bindrule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindrule
ms.assetid: 2606073e-c52f-498d-a923-5026b9d97e67
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 266707eca11f6664d816a958e9a3051d3648339b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197693"
---
# <a name="sp_bindrule-transact-sql"></a>sp_bindrule (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Enlaza una regla a una columna o a un tipo de datos de alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] En su lugar, use[restricciones UNIQUE y restricciones check](../../relational-databases/tables/unique-constraints-and-check-constraints.md) . Las restricciones CHECK se crean mediante la palabra clave CHECK de las instrucciones [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_bindrule [ @rulename = ] 'rule' ,   
     [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rulename = ] 'rule'` Es el nombre de una regla creada con la instrucción CREATE RULE. *Rule* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
`[ @objname = ] 'object_name'` Es la tabla y la columna, o el tipo de datos del alias al que se va a enlazar la regla. Una regla no se puede enlazar a una columna **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml**, de tipo definido por el usuario CLR o **timestamp**. Una regla no se puede enlazar a una columna calculada.  
  
 *object_name* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado. Si *object_name* es un nombre de una parte, se resuelve como un tipo de datos de alias. Si es un nombre de dos o tres partes, se resuelve primero como una tabla y una columna, y si esta resolución genera un error, se resuelve como un tipo de datos de alias. De forma predeterminada, las columnas existentes del tipo de datos de alias heredan la *regla* , a menos que se haya enlazado una regla directamente a la columna.  
  
> [!NOTE]  
>  *object_name* puede contener los caracteres de corchete **[** y **]** como caracteres de identificador delimitados. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
>  Las reglas creadas a partir de expresiones que utilizan tipos de datos de alias pueden enlazarse a columnas o tipos de datos de alias, pero no se compilan cuando se hace referencia a ellos. Evite el uso de reglas creadas a partir de tipos de datos de alias.  
  
`[ @futureonly = ] 'futureonly_flag'` Solo se usa cuando se enlaza una regla a un tipo de datos de alias. *future_only_flag* es de tipo **VARCHAR (15)** y su valor predeterminado es NULL. Este parámetro cuando se establece en **futureonly** evita que las columnas existentes de un tipo de datos de alias hereden la nueva regla. Si *futureonly_flag* es null, la nueva regla se enlaza a todas las columnas del tipo de datos de alias que actualmente no tienen ninguna regla o que usan la regla existente del tipo de datos del alias.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Puede enlazar una nueva regla a una columna (aunque se prefiere usar una restricción CHECK) o a un tipo de datos de alias con **sp_bindrule** sin desenlazar una regla existente. Se reemplaza la regla anterior. Si se enlaza una regla a una columna con una restricción CHECK existente, se evalúan todas las restricciones. No se puede enlazar una regla a un tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La regla se aplica cuando se intenta ejecutar una instrucción INSERT, no en la operación de enlace. Puede enlazar una regla de caracteres a una columna de tipo de datos **numérico** , aunque una operación de inserción no sea válida.  
  
 Las columnas existentes del tipo de datos de alias heredan la nueva regla a menos que se especifique *futureonly_flag* como **futureonly**. Las nuevas columnas definidas con el tipo de datos de alias siempre heredan la regla. Sin embargo, si la cláusula ALTER COLUMN de una instrucción ALTER TABLE cambia el tipo de datos de una columna a un tipo de datos de alias enlazado a una regla, la columna no hereda la regla enlazada al tipo de datos. La regla debe estar enlazada específicamente a la columna mediante **sp_bindrule**.  
  
 Al enlazar una regla a una columna, la información relacionada se agrega a la tabla **Sys. Columns** . Al enlazar una regla a un tipo de datos de alias, la información relacionada se agrega a la tabla **Sys. Types** .  
  
## <a name="permissions"></a>Permisos  
 Para enlazar una regla a una columna de una tabla, se debe tener el permiso ALTER en la tabla. Se requiere el permiso CONTROL en el tipo de datos de alias, o el permiso ALTER en el esquema al que pertenece el tipo, para enlazar una regla a un tipo de datos de alias.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-binding-a-rule-to-a-column"></a>A. Enlazar una regla a una columna  
 Partiendo de la base de que se ha creado una regla denominada `today` en la base de datos actual mediante la instrucción CREATE RULE, este ejemplo enlaza la regla a la columna `HireDate` de la tabla `Employee`. Cuando se agrega una fila a `Employee`, los datos de la columna `HireDate` se comprueban con la regla `today`.  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-rule-to-an-alias-data-type"></a>B. Enlazar una regla a un tipo de datos de alias  
 Suponiendo que existe una regla denominada `rule_ssn` y un tipo de datos de alias denominado `ssn`, este ejemplo enlaza `rule_ssn` a `ssn`. En una instrucción CREATE TABLE, las columnas del tipo `ssn` heredan la regla `rule_ssn`. Las columnas existentes de tipo `ssn` también heredan la `rule_ssn` regla, a menos que se especifique **futureonly** para *futureonly_flag* o `ssn` que tenga una regla enlazada directamente a ella. Las reglas enlazadas a columnas siempre tienen prioridad sobre las enlazadas a tipos de datos.  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'rule_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Usar el futureonly_flag  
 En el ejemplo siguiente se enlaza la regla `rule_ssn` al tipo de datos de alias `ssn`. Como se especifica `futureonly`, esto no afecta a ninguna de las columnas existentes de tipo `ssn`.  
  
```  
USE master;  
GO  
EXEC sp_bindrule rule_ssn, 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Usar identificadores delimitados  
 En el ejemplo siguiente se muestra el uso de identificadores delimitados en *object_name* parámetro.  
  
```  
USE master;  
GO  
CREATE TABLE [t.2] (c1 int) ;  
-- Notice the period as part of the table name.  
EXEC sp_bindrule rule1, '[t.2].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
