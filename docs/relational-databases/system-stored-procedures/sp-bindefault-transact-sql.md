---
description: sp_bindefault (Transact-SQL)
title: sp_bindefault (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindefault
- sp_bindefault_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindefault
ms.assetid: 3da70c10-68d0-4c16-94a5-9e84c4a520f6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4aacb6bff81f204e89e8704406d61ffb69ae84fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199197"
---
# <a name="sp_bindefault-transact-sql"></a>sp_bindefault (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Enlaza un valor predeterminado con una columna o un tipo de datos de alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Se recomienda crear las definiciones predeterminadas mediante la palabra clave DEFAULT de las instrucciones [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_bindefault [ @defname = ] 'default' ,   
    [ @objname = ] 'object_name'   
    [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @defname = ] 'default'` Es el nombre del valor predeterminado creado por CREATE DEFAULT. el *valor predeterminado* es **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
`[ @objname = ] 'object_name'` Es el nombre de la tabla y columna, o el tipo de datos del alias al que se va a enlazar el valor predeterminado. *object_name* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado. *object_name* no se pueden definir con los tipos definidos por el usuario **VARCHAR (Max**), **nvarchar (Max)**, **varbinary (Max)**, **XML** o CLR.  
  
 Si *object_name* es un nombre de una parte, se resuelve como un tipo de datos de alias. Si es un nombre de dos o tres partes, se resuelve primero como una tabla y una columna. y si se produce un error en esta resolución, se resuelve como un tipo de datos de alias. De forma predeterminada, las columnas existentes del tipo de datos de alias heredan de forma *predeterminada*, a menos que se haya enlazado un valor predeterminado directamente a la columna. Un valor predeterminado no se puede enlazar a una columna de tipo **Text**, **ntext**, **Image**, **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)**, **XML**, **timestamp** o CLR definido por el usuario, una columna con la propiedad Identity, una columna calculada o una columna que ya tenga una restricción default.  
  
> [!NOTE]  
>  *object_name* puede contener corchetes **[]** como identificadores delimitados. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Solo se usa cuando se enlaza un valor predeterminado a un tipo de datos de alias. *futureonly_flag* es de tipo **VARCHAR (15)** y su valor predeterminado es NULL. Cuando este parámetro se establece en **futureonly**, las columnas existentes de ese tipo de datos no pueden heredar el nuevo valor predeterminado. Este parámetro nunca se utiliza al enlazar un valor predeterminado con una columna. Si *futureonly_flag* es null, el nuevo valor predeterminado se enlaza a todas las columnas del tipo de datos de alias que actualmente no tengan ningún valor predeterminado o que utilicen el valor predeterminado existente del tipo de datos de alias.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Puede usar **sp_bindefault** para enlazar un nuevo valor predeterminado a una columna, aunque se prefiere usar la restricción predeterminada, o a un tipo de datos de alias sin desenlazar un valor predeterminado existente. El valor predeterminado anterior se reemplaza. No puede enlazar un valor predeterminado con un tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un tipo definido por el usuario CLR. Si el valor predeterminado no es compatible con la columna con la que se debe enlazar, el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] devuelve un mensaje de error cuando intenta insertar el valor predeterminado y no cuando lo enlaza.  
  
 Las columnas existentes del tipo de datos de alias heredan el nuevo valor predeterminado, a menos que un valor predeterminado esté enlazado directamente a ellos o *futureonly_flag* se especifique como **futureonly**. Las nuevas columnas del tipo de datos de alias siempre heredan el valor predeterminado.  
  
 Al enlazar un valor predeterminado a una columna, la información relacionada se agrega a la vista de catálogo **Sys. Columns** . Cuando se enlaza un valor predeterminado a un tipo de datos de alias, la información relacionada se agrega a la vista de catálogo **Sys. Types** .  
  
## <a name="permissions"></a>Permisos  
 El usuario debe poseer la tabla o ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-binding-a-default-to-a-column"></a>A. Enlazar un valor predeterminado con una columna  
 Se ha definido un valor predeterminado denominado `today` en la base de datos actual mediante CREATE DEFAULT y en el siguiente ejemplo se enlaza el valor predeterminado con la columna `HireDate` de la tabla `Employee`. Cuando se agregue una fila a la tabla `Employee` y no se proporcionen datos para la columna `HireDate`, dicha columna obtiene el valor predeterminado `today`.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-default-to-an-alias-data-type"></a>B. Enlazar un valor predeterminado con un tipo de datos de alias  
 El valor predeterminado denominado `def_ssn` y el tipo de datos de alias denominado `ssn` ya existen. En el siguiente ejemplo se enlaza el valor predeterminado `def_ssn` con `ssn`. Al crear una tabla, el valor predeterminado es heredado por todas las columnas a las que se asigna el tipo de datos de alias `ssn`. Las columnas existentes de tipo **SSN** también heredan la **def_ssn** predeterminada, a menos que se especifique **futureonly** para *futureonly_flag* valor, o a menos que la columna tenga un valor predeterminado enlazado directamente a ella. Los valores predeterminados enlazados con columnas siempre tienen preferencia sobre los enlazados con tipos de datos.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Usar el futureonly_flag  
 En el siguiente ejemplo se enlaza el valor predeterminado `def_ssn` con el tipo de datos de alias `ssn`. Dado que se especifica **futureonly** , no se ven afectadas las columnas existentes de tipo `ssn` .  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Usar identificadores delimitados  
 En el ejemplo siguiente se muestra el uso de identificadores delimitados, `[t.1]` , en *object_name*.  
  
```  
USE master;  
GO  
CREATE TABLE [t.1] (c1 int);   
-- Notice the period as part of the table name.  
EXEC sp_bindefault 'default1', '[t.1].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name,   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_unbindefault &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
