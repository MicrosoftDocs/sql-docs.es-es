---
description: sp_describe_cursor_tables (Transact-SQL)
title: sp_describe_cursor_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_cursor_tables_TSQL
- sp_describe_cursor_tables
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor_tables
ms.assetid: 02c0f81a-54ed-4ca4-aa4f-bb7463a9ab9a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5c2e609eb3a521a991848b3b795b04b2ffbd01f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200801"
---
# <a name="sp_describe_cursor_tables-transact-sql"></a>sp_describe_cursor_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Informa acerca de los objetos o las tablas base a las que hace referencia un cursor de servidor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_describe_cursor_tables   
     [ @cursor_return = ] output_cursor_variable OUTPUT   
     { [ , [ @cursor_source = ] N'local'  
     , [ @cursor_identity = ] N'local_cursor_name' ]   
   | [ , [ @cursor_source = ] N'global'  
     , [ @cursor_identity = ] N'global_cursor_name' ]   
   | [ , [ @cursor_source = ] N'variable'  
     , [ @cursor_identity = ] N'input_cursor_variable' ]   
     }   
[;]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @cursor_return =] *output_cursor_variable* salida  
 Es el nombre de una variable de cursor declarada que recibirá la salida del cursor. *output_cursor_variable* es **cursor**, no tiene valor predeterminado y no debe estar asociado a ningún cursor en el momento en que se llama a sp_describe_cursor_tables. El cursor devuelto es un cursor desplazable, dinámico y de solo lectura.  
  
 [ @cursor_source =] {N'local ' | N'global ' | N'variable' }  
 Especifica si el cursor del que se informa está especificado con el nombre de un cursor local, un cursor global o una variable de cursor. El parámetro es **nvarchar (30)**.  
  
 [ @cursor_identity =] N '*local_cursor_name*'  
 Es el nombre de un cursor creado por una instrucción DECLARE CURSOR que tiene la palabra clave LOCAL, o bien el valor predeterminado es LOCAL. *local_cursor_name* es **nvarchar (128)**.  
  
 [ @cursor_identity =] N '*global_cursor_name*'  
 Es el nombre de un cursor creado por una instrucción DECLARE CURSOR que tiene la palabra clave GLOBAL u obtuvo el valor predeterminado GLOBAL. *global_cursor_name* también puede ser el nombre de un cursor de servidor de API abierto por una aplicación ODBC que, a continuación, ha llamado al cursor llamando a SQLSetCursorName. *global_cursor_name* es **nvarchar (128)**.  
  
 [ @cursor_identity =] N '*input_cursor_variable*'  
 Es el nombre de una variable de cursor asociada a un cursor abierto. *input_cursor_variable* es **nvarchar (128)**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="cursors-returned"></a>Cursores devueltos  
 sp_describe_cursor_tables encapsula su informe como un parámetro de salida de [!INCLUDE[tsql](../../includes/tsql-md.md)] **cursor** . De esta forma, los lotes, los procedimientos almacenados y los desencadenadores de [!INCLUDE[tsql](../../includes/tsql-md.md)] pueden trabajar con la salida de fila en fila. También significa que no se puede llamar al procedimiento directamente desde funciones de una API. El parámetro de salida **cursor** debe estar enlazado a una variable de programa, pero las API no admiten parámetros de **cursor** de enlace o variables.  
  
 En la tabla siguiente se muestra el formato del cursor devuelto por sp_describe_cursor_tables.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|table owner|**sysname**|Id. de usuario del propietario de la tabla.|  
|Table_name|**sysname**|Nombre del objeto o tabla base. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los cursores de servidor siempre devuelven el objeto especificado por el usuario, no las tablas base.|  
|Optimizer_hints|**smallint**|Mapa de bits que incluye una o varias de las siguientes opciones:<br /><br /> 1 = Bloqueo de fila (ROWLOCK)<br /><br /> 4 = Bloqueo de página (PAGELOCK)<br /><br /> 8 = Bloqueo de tabla (TABLOCK)<br /><br /> 16 = Bloqueo exclusivo de tabla (TABLOCKX)<br /><br /> 32 = Bloqueo de actualización (UPDLOCK)<br /><br /> 64 = Sin bloqueo (NOLOCK)<br /><br /> 128 = Opción de primera fila rápida (FASTFIRST)<br /><br /> 4096 = Lectura de semántica repetible cuando se utiliza con DECLARE CURSOR (HOLDLOCK)<br /><br /> Cuando se especifican varias opciones, el sistema utiliza la más restrictiva. Sin embargo, sp_describe_cursor_tables muestra las marcas que se especifican en la consulta.|  
|lock_type|**smallint**|Tipo de bloqueo de desplazamiento requerido explícita o implícitamente para cada tabla base de este cursor. El valor puede ser uno de los siguientes:<br /><br /> 0 = Ninguna<br /><br /> 1 = Compartido<br /><br /> 3 = Actualización|  
|server_name|**sysname, aceptan valores NULL**|Nombre del servidor vinculado en el que reside la tabla. NULL cuando se utiliza OPENQUERY u OPENROWSET.|  
|Objectid|**int**|Id. de objeto de la tabla. 0 cuando se utiliza OPENQUERY u OPENROWSET.|  
|dbid|**int**|Identificador de la base de datos en la que reside la tabla. 0 cuando se utiliza OPENQUERY u OPENROWSET.|  
|dbname|**sysname**, **aceptan valores NULL**|Nombre de la base de datos en la que reside la tabla. NULL cuando se utiliza OPENQUERY u OPENROWSET.|  
  
## <a name="remarks"></a>Observaciones  
 sp_describe_cursor_tables describe las tablas base a las que hace referencia un cursor de servidor. Utilice sp_describe_cursor_columns para obtener una descripción de los atributos del conjunto de resultados devuelto por el cursor. Para obtener una descripción de las características globales del cursor, como por ejemplo sus posibilidades de desplazamiento y actualización, use sp_describe_cursor. Para obtener un informe de los [!INCLUDE[tsql](../../includes/tsql-md.md)] cursores de servidor que están visibles en la conexión, use sp_cursor_list.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se abre un cursor global y se utiliza `sp_describe_cursor_tables` para informar acerca de las tablas a las que hace referencia el cursor.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person  
WHERE LastName LIKE 'S%';  
  
OPEN abc;  
GO  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor_tables.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor_tables into the cursor variable.  
EXEC master.dbo.sp_describe_cursor_tables  
      @cursor_return = @Report OUTPUT,  
      @cursor_source = N'global', @cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor_tables output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor_tables.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Cursores](../../relational-databases/cursors.md)   
 [CURSOR_STATUS &#40;&#41;de Transact-SQL ](../../t-sql/functions/cursor-status-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [sp_cursor_list &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)   
 [sp_describe_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-transact-sql.md)   
 [sp_describe_cursor_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-columns-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
