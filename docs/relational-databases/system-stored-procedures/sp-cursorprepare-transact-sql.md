---
description: sp_cursorprepare (Transact-SQL)
title: sp_cursorprepare (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_prepare
ms.assetid: 6207e110-f4bf-4139-b3ec-b799c9cb3ad7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df4b0ff31a5b8a1723b6c8f130aeab9b32b03166
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209747"
---
# <a name="sp_cursorprepare-transact-sql"></a>sp_cursorprepare (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Compila la instrucción de cursor o lote en un plan de ejecución, pero no crea el cursor. sp_cursorexecute puede utilizar después la instrucción compilada. Este procedimiento, junto con sp_cursorexecute, tiene la misma función que sp_cursoropen, pero se divide en dos fases. sp_cursorprepare se invoca especificando el identificador 3 en un paquete de flujo TDS.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_cursorprepare prepared_handle OUTPUT, params , stmt , options  
    [ , scrollopt[ , ccopt]]  
```  
  
## <a name="arguments"></a>Argumentos  
 *prepared_handle*  
 Identificador de *controlador* preparado generado por SQL Server que devuelve un valor entero.  
  
> [!NOTE]  
>  Posteriormente, *prepared_handle* se proporciona a un procedimiento sp_cursorexecute para abrir un cursor. Una vez creado un identificador, existe hasta que cierre sesión o hasta que lo quite explícitamente a través de un procedimiento sp_cursorunprepare.  
  
 *params*  
 Identifica instrucciones con parámetros. La definición de *params* de variables se sustituye por los marcadores de parámetros de la instrucción. *params* es un parámetro necesario que requiere un valor de entrada **ntext**, **nchar** o **nvarchar** . Escriba un valor NULL si la instrucción no tiene parámetros.  
  
> [!NOTE]  
>  Use una cadena **ntext** como valor de entrada cuando *stmt* tiene parámetros y el valor de PARAMETERIZED_STMT *scrollopt* es on.  
  
 *instrucción*  
 Define el conjunto de resultados del cursor. El parámetro *stmt* es obligatorio y llama a para un valor de entrada **ntext**, **nchar** o **nvarchar** .  
  
> [!NOTE]  
>  Las reglas para especificar el valor de *stmt* son las mismas que para Sp_cursoropen, con la excepción de que el tipo de datos de la cadena *stmt* debe ser **ntext**.  
  
 *options*  
 Parámetro opcional que devuelve una descripción de las columnas del conjunto de resultados del cursor. *Options* requiere el siguiente valor de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
 *scrollopt*  
 Opción de desplazamiento. *scrollopt* es un parámetro opcional que requiere uno de los siguientes valores de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 Dado que es posible que el valor solicitado no sea adecuado para el cursor definido por *stmt*, este parámetro actúa como entrada y salida. En casos como este, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna un valor adecuado.  
  
 *ccopt*  
 Opción de control de simultaneidad. *ccopt* es un parámetro opcional que requiere uno de los siguientes valores de entrada **int** .  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (conocido anteriormente como LOCKCC)|  
|0x0004|**Optimista** (conocido anteriormente como OPTCC)|  
|0x0008|OPTIMISTIC (conocido anteriormente como OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Al igual que con *scrollpt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede asignar un valor diferente al solicitado.  
  
## <a name="remarks"></a>Observaciones  
 El parámetro de estado de RPC es uno de los siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|0|Correcto|  
|0x0001|Error|  
|1FF6|No pudo devolver los metadatos.<br /><br /> Nota: la razón de esto es que la instrucción no produce un conjunto de resultados; por ejemplo, es una instrucción INSERT o DDL.|  
  
## <a name="examples"></a>Ejemplos  
  El siguiente es un ejemplo del uso de sp_cursorprepare y sp_cursorexecute

```sql
declare @handle int , @p5 int, @p6 int
exec sp_cursorprepare @handle OUTPUT, 
    N'@dbid int', 
    N'select * from sys.databases where database_id < @dbid',
    1,
    @p5 output,
    @p6 output


declare @p1 int  
set @P1 = @handle 
declare @p2 int   
declare @p3 int  
declare @p4 int  
set @P6 = 4 
exec sp_cursorexecute @p1, @p2 OUTPUT, @p3 output , @p4 output, @p5 OUTPUT, @p6

exec sp_cursorfetch @P2

exec sp_cursorunprepare @handle
exec sp_cursorclose @p2
```
 
 Cuando *stmt* tiene parámetros y el valor de PARAMETERIZED_STMT *scrollopt* es on, el formato de la cadena es el siguiente:  
  
 { *\<local variable name>**\<data type>* } [ ,... *n* ]  
  
## <a name="see-also"></a>Consulte también  
 [sp_cursorexecute &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursoropen &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorunprepare &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorunprepare-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
