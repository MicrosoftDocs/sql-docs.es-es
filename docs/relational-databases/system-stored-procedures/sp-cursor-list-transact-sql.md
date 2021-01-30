---
description: sp_cursor_list (Transact-SQL)
title: sp_cursor_list (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_list
- sp_cursor_list_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_list
ms.assetid: 7187cfbe-d4d9-4cfa-a3bb-96a544c7c883
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3a60d447d7072c6386faef08d1dd7398f906064d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205183"
---
# <a name="sp_cursor_list-transact-sql"></a>sp_cursor_list (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Informa de los atributos de los cursores del servidor abiertos actualmente para la conexión.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_cursor_list [ @cursor_return = ] cursor_variable_name OUTPUT   
     , [ @cursor_scope = ] cursor_scope  
[;]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @cursor_return =] *cursor_variable_name* salida  
 Es el nombre de una variable de cursor declarada. *cursor_variable_name* es **cursor** y no tiene ningún valor predeterminado. El cursor es un cursor desplazable, dinámico y de solo lectura.  
  
 [ @cursor_scope =] *cursor_scope*  
 Especifica el nivel de los cursores que se notificarán. *cursor_scope* es de **tipo int**, no tiene ningún valor predeterminado y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|1|Informa de todos los cursores locales.|  
|2|Informa de todos los cursores globales.|  
|3|Informa de los cursores locales y globales.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="cursors-returned"></a>Cursores devueltos  
 sp_cursor_list devuelve su informe como un parámetro de salida del cursor de [!INCLUDE[tsql](../../includes/tsql-md.md)], no como un conjunto de resultados. Esto permite a los lotes, procedimientos almacenados y desencadenadores de [!INCLUDE[tsql](../../includes/tsql-md.md)] trabajar con la salida de fila en fila. También significa que no se puede llamar al procedimiento directamente desde funciones de API de base de datos. El parámetro de salida de tipo cursor debe estar enlazado a una variable del programa, pero las API de base de datos no admiten el enlace de parámetros o variables de cursor.  
  
 Éste es el formato del cursor devuelto por sp_cursor_list. El formato del cursor es el mismo que el que devuelve sp_describe_cursor.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|reference_name|**sysname**|El nombre que se utiliza para hacer referencia al cursor. Si la referencia al cursor se realizó utilizando el nombre dado en una instrucción DECLARE CURSOR, el nombre de referencia es el mismo que el nombre del cursor. Si la referencia al cursor se realizó a través de una variable, el nombre de la referencia es el de la variable de cursor.|  
|cursor_name|**sysname**|El nombre del cursor desde una instrucción DECLARE CURSOR. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si el cursor se creó estableciendo una variable de cursor en un cursor, **cursor_name** devuelve el nombre de la variable de cursor.  En versiones anteriores, esta columna de salida devuelve un nombre generado por el sistema.|  
|cursor_scope|**smallint**|1 = LOCAL<br /><br /> 2 = GLOBAL|  
|status|**smallint**|Los mismos valores de los que informó la función del sistema CURSOR_STATUS:<br /><br /> 1 = El cursor al que hace referencia el nombre del cursor o ?la variable está abierto. Si se trata de un cursor que no distingue, estático o de conjunto de claves, el conjunto de resultados tiene una fila, como mínimo. Si el cursor es dinámico, el conjunto de resultados tiene cero o más filas.<br /><br /> 0 = El cursor al que hace referencia el nombre del cursor o la variable está abierto pero no tiene filas. Los cursores dinámicos nunca devuelven este valor.<br /><br /> -1 = El cursor al que hace referencia el nombre del cursor o la variable está cerrado.<br /><br /> -2 = Se aplica solo a variables de cursor. No hay ningún cursor asignado a la variable. Posiblemente, un parámetro OUTPUT asignó un cursor a la variable pero el procedimiento almacenado cerró el cursor antes de devolver resultados.<br /><br /> -3 = No existe un cursor o variable de cursor con el nombre especificado, o la variable del cursor no tiene todavía un cursor asignado.|  
|model|**smallint**|1 = Sin distinción (o estático) <br /><br /> 2 = conjunto de claves<br /><br /> 3 = dinámica<br /><br /> 4 = Avance rápido|  
|simultaneidad|**smallint**|1 = solo lectura<br /><br /> 2 = Bloqueos de desplazamiento<br /><br /> 3 = Optimista|  
|scrollable|**smallint**|0 = Solo avance<br /><br /> 1 = Desplazable|  
|open_status|**smallint**|0 = Cerrado<br /><br /> 1 = Abierto|  
|cursor_rows|**int**|El número de filas certificadas en el conjunto de resultados. Para obtener más información, [vea @CURSOR_ROWS @](../../t-sql/functions/cursor-rows-transact-sql.md).|  
|fetch_status|**smallint**|El estado de la última captura de este cursor. Para obtener más información, [vea @FETCH_STATUS @](../../t-sql/functions/fetch-status-transact-sql.md):<br /><br /> 0 = Captura correcta.<br /><br /> -1 = Error en la captura o se sobrepasaron los límites del cursor<br /><br /> -2 = Falta la fila solicitada.<br /><br /> -9 = No se registró ninguna captura en el cursor.|  
|column_count|**smallint**|El número de columnas del conjunto de resultados del cursor.|  
|row_count|**smallint**|El número de filas afectadas por la última operación del cursor. Para obtener más información, [vea @ROWCOUNT @](../../t-sql/functions/rowcount-transact-sql.md).|  
|last_operation|**smallint**|La última operación realizada en el cursor:<br /><br /> 0 = No se realizó ninguna operación en el cursor.<br /><br /> 1 = OPEN<br /><br /> 2 = FETCH<br /><br /> 3 = INSERCIÓN<br /><br /> 4 = UPDATE<br /><br /> 5 = ELIMINAR<br /><br /> 6 = CLOSE<br /><br /> 7 = DEALLOCATE|  
|cursor_handle|**int**|Un valor único que identifica el cursor dentro del ámbito del servidor.|  
  
## <a name="remarks"></a>Observaciones  
 sp_cursor_list presenta una lista de los actuales cursores de servidor abiertos por la conexión y describe los atributos globales de cada cursor, como por ejemplo, las posibilidades de desplazamiento y actualización del mismo. sp_cursor_list enumera los cursores siguientes:  
  
-   Cursores de servidor [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Cursores de servidor de API abiertos por una aplicación ODBC que, a continuación, llamó a SQLSetCursorName para denominar al cursor.  
  
 Utilice sp_describe_cursor_columns para obtener una descripción de los atributos del conjunto de resultados devuelto por el cursor. Utilice sp_describe_cursor_tables para obtener un informe de las tablas base a las que hace referencia el cursor. sp_describe_cursor informa de la misma información que sp_cursor_list, pero solo para un cursor especificado.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los permisos de ejecución corresponden al rol public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se abre un cursor global y se utiliza `sp_cursor_list` para obtener un informe de los atributos del cursor.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a keyset-driven cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person  
WHERE LastName LIKE 'S%';  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_cursor_list.  
DECLARE @Report CURSOR;  
  
-- Execute sp_cursor_list into the cursor variable.  
EXEC master.dbo.sp_cursor_list @cursor_return = @Report OUTPUT,  
      @cursor_scope = 2;  
  
-- Fetch all the rows from the sp_cursor_list output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_cursor_list.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
