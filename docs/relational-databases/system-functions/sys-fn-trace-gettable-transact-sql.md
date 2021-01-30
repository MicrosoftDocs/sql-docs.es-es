---
description: sys.fn_trace_gettable (Transact-SQL)
title: sys.fn_trace_gettable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_trace_gettable
- fn_trace_gettable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_trace_gettable function
- sys.fn_trace_gettable function
ms.assetid: c2590159-6ec5-4510-81ab-e935cc4216cd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ba7550423c7d477a8a85d75b73294fbb97fbde6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208075"
---
# <a name="sysfn_trace_gettable-transact-sql"></a>sys.fn_trace_gettable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el contenido de uno o más archivos de seguimiento en forma de tabla.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use eventos extendidos en su lugar.  
   
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_trace_gettable ( 'filename' , number_files )  
```  
  
## <a name="arguments"></a>Argumentos  
 '*nombreDeArchivo*'  
 Especifica el archivo de seguimiento inicial que se va a leer. *filename* es **nvarchar (256)** y no tiene ningún valor predeterminado.  
  
 *number_files*  
 Especifica el número de archivos de sustitución que se van a leer. Este número incluye el archivo inicial especificado en *filename*. *number_files* es de **tipo int**.  
  
## <a name="remarks"></a>Observaciones  
 Si se especifica *number_files* como **valor predeterminado**, **fn_trace_gettable** Lee todos los archivos de sustitución incremental hasta que llega al final del seguimiento. **fn_trace_gettable** devuelve una tabla con todas las columnas válidas para el seguimiento especificado. Para obtener más información, vea [sp_trace_setevent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).  
  
 Tenga en cuenta que la función fn_trace_gettable no cargará los archivos de sustitución incremental (cuando esta opción se especifica mediante el argumento *number_files* ), donde el nombre del archivo de seguimiento original termina con un carácter de subrayado y un valor numérico. (Esto no se aplica al carácter de subrayado y al número que se anexan automáticamente cuando se revierte un archivo). Como solución alternativa, puede cambiar el nombre de los archivos de seguimiento para quitar los guiones bajos en el nombre de archivo original. Por ejemplo, si el archivo original se denomina **Trace_Oct_5. TRC** y el archivo de sustitución incremental se denomina **Trace_Oct_5_1. TRC**, puede cambiar el nombre de los archivos a **TraceOct5. TRC** y **TraceOct5_1. TRC**.  
  
 Esta función puede leer un seguimiento que todavía esté activo en la instancia en la que se ejecuta.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER TRACE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-fn_trace_gettable-to-import-rows-from-a-trace-file"></a>A. Usar fn_trace_gettable para importar filas de un archivo de seguimiento  
 En el siguiente ejemplo se llama a `fn_trace_gettable` en la cláusula `FROM` de una instrucción `SELECT...INTO`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * INTO temp_trc  
FROM fn_trace_gettable('c:\temp\mytrace.trc', default);  
GO  
```  
  
### <a name="b-using-fn_trace_gettable-to-return-a-table-with-an-identity-column-that-can-be-loaded-into-a-sql-server-table"></a>B. Usar fn_trace_gettable para devolver una tabla con una columna IDENTITY que se pueda cargar en una tabla de SQL Server  
 Este ejemplo llama a la función como parte de una instrucción `SELECT...INTO` y devuelve una tabla con una columna `IDENTITY` que se puede cargar en la tabla `temp_trc`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT IDENTITY(int, 1, 1) AS RowNumber, * INTO temp_trc  
FROM fn_trace_gettable('c:\temp\mytrace.trc', default);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_generateevent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)  
  
  
