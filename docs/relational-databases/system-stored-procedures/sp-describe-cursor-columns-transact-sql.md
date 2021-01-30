---
description: sp_describe_cursor_columns (Transact-SQL)
title: sp_describe_cursor_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_cursor_columns
- sp_describe_cursor_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor_columns
ms.assetid: 6eaa54af-7ba4-4fce-bf6c-6ac67cc1ac94
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 11c2a7610b13e806c7c40d0fd2ff5a9e0363fffc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158437"
---
# <a name="sp_describe_cursor_columns-transact-sql"></a>sp_describe_cursor_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Informa acerca de los atributos de las columnas en el conjunto de resultados de un cursor de servidor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_describe_cursor_columns   
   [ @cursor_return = ] output_cursor_variable OUTPUT   
    { [ , [ @cursor_source = ] N'local' ,   
          [ @cursor_identity = ] N'local_cursor_name' ]   
    | [ , [ @cursor_source = ] N'global' ,   
          [ @cursor_identity = ] N'global_cursor_name' ]   
    | [ , [ @cursor_source = ] N'variable' ,   
          [ @cursor_identity = ] N'input_cursor_variable' ]   
   }  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @cursor_return =] *output_cursor_variable* salida  
 Es el nombre de una variable de cursor declarada que recibirá la salida del cursor. *output_cursor_variable* es **cursor**, no tiene valor predeterminado y no debe estar asociado a ningún cursor en el momento en que se llama a sp_describe_cursor_columns. El cursor devuelto es un cursor desplazable, dinámico y de solo lectura.  
  
 [ @cursor_source =] {N'local ' | N'global ' | N'variable' }  
 Especifica si el cursor del que se informa está especificado con el nombre de un cursor local, un cursor global o una variable de cursor. El parámetro es **nvarchar (30)**.  
  
 [ @cursor_identity =] N '*local_cursor_name*'  
 Es el nombre de un cursor creado por una instrucción DECLARE CURSOR que tiene la palabra clave LOCAL u obtuvo el valor predeterminado LOCAL. *local_cursor_name* es **nvarchar (128)**.  
  
 [ @cursor_identity =] N '*global_cursor_name*'  
 Es el nombre de un cursor creado por una instrucción DECLARE CURSOR que tiene la palabra clave GLOBAL o cuyo valor predeterminado es GLOBAL. *global_cursor_name* es **nvarchar (128)**.  
  
 *global_cursor_name* también puede ser el nombre de un cursor de servidor de API abierto por una aplicación ODBC y, a continuación, llama a SQLSetCursorName.  
  
 [ @cursor_identity =] N '*input_cursor_variable*'  
 Es el nombre de una variable de cursor asociada a un cursor abierto. *input_cursor_variable* es **nvarchar (128)**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="cursors-returned"></a>Cursores devueltos  
 sp_describe_cursor_columns encapsula su informe como un parámetro de salida de [!INCLUDE[tsql](../../includes/tsql-md.md)] **cursor** . De esta forma, los lotes, los procedimientos almacenados y los desencadenadores de [!INCLUDE[tsql](../../includes/tsql-md.md)] pueden trabajar con la salida de fila en fila. También significa que no se puede llamar al procedimiento directamente desde funciones de una API de base de datos. El parámetro de salida **cursor** debe estar enlazado a una variable de programa, pero las API de base de datos no admiten el enlace de variables o parámetros de **cursor** .  
  
 En la siguiente tabla se muestra el formato del cursor devuelto por sp_describe_cursor_columns.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|column_name|**sysname** (acepta valores NULL)|Nombre asignado a la columna del conjunto de resultados. La columna es NULL en el caso de que se especificara sin una cláusula de acompañamiento AS.|  
|ordinal_position|**int**|Posición relativa de la columna desde la primera columna de la izquierda en el conjunto de resultados. La primera columna está en la posición 0.|  
|column_characteristics_flags|**int**|Máscara de bits que indica la información almacenada en DBCOLUMNFLAGS dentro de OLE DB. Puede tener uno o una combinación de los valores siguientes:<br /><br /> 1 = Marcador<br /><br /> 2 = Longitud fija <br /><br /> 4 = Acepta valores NULL<br /><br /> 8 = Versiones de fila<br /><br /> 16 = Columna que se puede actualizar (configurada para columnas previstas de un cursor sin cláusula FOR UPDATE y, en caso de existir esa columna, solo puede ser una por cursor).<br /><br /> Cuando se combinan valores de bit, se aplican las características de los valores de bit combinados. Por ejemplo, si el valor de bit es 6, la columna es una columna que acepta valores NULL (4) de longitud fija (2).|  
|column_size|**int**|Tamaño máximo posible para un valor de esta columna.|  
|data_type_sql|**smallint**|Número que indica el tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la columna.|  
|column_precision|**tinyint**|Precisión máxima de la columna según el valor de *bPrecision* en OLE DB.|  
|column_scale|**tinyint**|Número de dígitos a la derecha del separador decimal para los tipos de datos **numéricos** o **decimales** según el valor de *bScale* de OLE DB.|  
|order_position|**int**|Si la columna participa en la ordenación del conjunto de resultados, es la posición de la columna en la clave de orden relativa a la columna situada en el extremo izquierdo.|  
|order_direction|**VARCHAR (1)**(admite valores NULL)|A = La columna se encuentra en la clave de orden y la ordenación es ascendente.<br /><br /> D = La columna se encuentra en la clave de orden y la ordenación es descendente.<br /><br /> NULL = La columna no participa en la ordenación.|  
|hidden_column|**smallint**|0 = Esta columna aparece en la lista de selección.<br /><br /> 1 = Reservado para uso futuro.|  
|columnid|**int**|Id. de columna para la columna base. Si se generó la columna del conjunto de resultados a partir de una expresión, columnid es -1.|  
|objectid|**int**|Id. de objeto del objeto o la tabla base que proporciona la columna. Si se generó la columna del conjunto de resultados a partir de una expresión, objectid es -1.|  
|dbid|**int**|Id. de la base de datos que contiene la tabla base que proporciona la columna. Si se generó la columna del conjunto de resultados a partir de una expresión, dbid es -1.|  
|dbname|**sysname**<br /><br /> (acepta valores NULL)|Nombre de la base de datos que contiene la tabla base que proporciona la columna. Si se generó la columna del conjunto de resultados a partir de una expresión, dbname es NULL.|  
  
## <a name="remarks"></a>Observaciones  
 sp_describe_cursor_columns describe los atributos de las columnas en el conjunto de resultados de un cursor de servidor, como, por ejemplo, el nombre y el tipo de datos de cada cursor. Utilice sp_describe_cursor para obtener una descripción de los atributos globales del cursor de servidor. Utilice sp_describe_cursor_tables para obtener un informe de las tablas base a las que hace referencia el cursor. Para obtener un informe de los cursores de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)] visibles en la conexión, use sp_cursor_list.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se abre un cursor global y se utiliza `sp_describe_cursor_columns` para obtener un informe de las columnas utilizadas en el cursor.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person;  
GO  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor_columns.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor_columns into the cursor variable.  
EXEC master.dbo.sp_describe_cursor_columns  
    @cursor_return = @Report OUTPUT  
    ,@cursor_source = N'global'   
    ,@cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor_columns output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor_columns.  
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
 [sp_describe_cursor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-transact-sql.md)   
 [sp_cursor_list &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)   
 [sp_describe_cursor_tables &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-tables-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
