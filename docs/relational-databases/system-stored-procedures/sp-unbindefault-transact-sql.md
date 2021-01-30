---
description: sp_unbindefault (Transact-SQL)
title: sp_unbindefault (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unbindefault_TSQL
- sp_unbindefault
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindefault
ms.assetid: c96a6c5e-f3ca-4c1e-b64b-0d8ef6986af8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ecc8a6c7a012a36da4deeacf8b4e66e6096c8227
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202345"
---
# <a name="sp_unbindefault-transact-sql"></a>sp_unbindefault (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Desenlaza o quita un valor predeterminado de una columna o de un tipo de datos alias en la base de datos actual.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Se recomienda crear las definiciones predeterminadas mediante la palabra clave DEFAULT en las instrucciones [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_unbindefault [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @objname = ] 'object_name'` Es el nombre de la tabla y columna, o el tipo de datos del alias del que se va a desenlazar el valor predeterminado. *object_name* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta resolver los identificadores de dos partes en nombres de columna en primer lugar, y después en tipos de datos de alias.  
  
 Cuando se desenlaza un valor predeterminado de un tipo de datos alias, también se desenlaza cualquier columna de ese tipo de datos que tenga el mismo valor predeterminado. Las columnas de ese tipo de datos cuyos valores predeterminados estén directamente enlazados a ellas no se ven afectadas.  
  
> [!NOTE]  
>  *object_name* pueden contener corchetes **[]** como caracteres de identificador delimitados. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Solo se usa cuando se desenlaza un valor predeterminado de un tipo de datos de alias. *futureonly_flag* es de tipo **VARCHAR (15)** y su valor predeterminado es NULL. Cuando *futureonly_flag* es **futureonly**, las columnas existentes del tipo de datos no pierden el valor predeterminado especificado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Para mostrar el texto de un valor predeterminado, ejecute **sp_helptext** con el nombre del parámetro predeterminado.  
  
## <a name="permissions"></a>Permisos  
 Para desenlazar un valor predeterminado de una columna de la tabla, es necesario el permiso ALTER en la tabla. Para desenlazar un valor predeterminado de un tipo de datos alias, se requiere el permiso CONTROL en el tipo o el permiso ALTER del esquema al que pertenece el tipo.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-unbinding-a-default-from-a-column"></a>A. Desenlazar un valor predeterminado de una columna  
 En el siguiente ejemplo se desenlaza el valor predeterminado de la columna `hiredate` de una tabla `employees`.  
  
```  
EXEC sp_unbindefault 'employees.hiredate';  
```  
  
### <a name="b-unbinding-a-default-from-an-alias-data-type"></a>B. Desenlazar un valor predeterminado de un tipo de datos alias  
 En el siguiente ejemplo se desenlaza el valor predeterminado del tipo de datos alias `ssn`. Desenlaza las columnas de ese tipo existentes y futuras.  
  
```  
EXEC sp_unbindefault 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Usar el futureonly_flag  
 En el siguiente ejemplo se desenlazan futuros usos del tipo de datos alias `ssn` sin que ello afecte a las columnas `ssn` existentes.  
  
```  
EXEC sp_unbindefault 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Usar identificadores delimitados  
 En el ejemplo siguiente se muestra el uso de identificadores delimitados en *object_name* parámetro.  
  
```  
CREATE TABLE [t.3] (c1 int); -- Notice the period as part of the table   
-- name.  
CREATE DEFAULT default2 AS 0;  
GO  
EXEC sp_bindefault 'default2', '[t.3].c1' ;  
-- The object contains two periods;  
-- the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
EXEC sp_unbindefault '[t.3].c1';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
