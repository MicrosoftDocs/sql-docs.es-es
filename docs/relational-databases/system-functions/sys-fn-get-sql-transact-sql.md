---
description: sys.fn_get_sql (Transact-SQL)
title: sys.fn_get_sql (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_sql
- sys.fn_get_sql_TSQL
- fn_get_sql_TSQL
- sys.fn_get_sql
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_sql function
- text [SQL Server], SQL handles
- sys.fn_get_sql function
- valid SQL handles [SQL Server]
- SQL handles
ms.assetid: d5fe49b5-0813-48f2-9efb-9187716b2fd4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: aee9ac3d8d75081ad3474f215d6bc289580e2da0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206057"
---
# <a name="sysfn_get_sql-transact-sql"></a>sys.fn_get_sql (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el texto de la instrucción SQL para el identificador SQL especificado.  
  
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Microsoft SQL Server. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Use sys.dm_exec_sql_text en su lugar. Para obtener más información, vea [sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_get_sql ( SqlHandle )  
```  
  
## <a name="arguments"></a>Argumentos  
 *SqlHandle*  
 Es el valor del identificador. *SqlHandle* es **varbinary (64)** y no tiene ningún valor predeterminado.  
  
## <a name="tables-returned"></a>Tablas devueltas  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|dbid|**smallint**|Id. de la base de datos. En el caso de instrucciones SQL ad hoc y preparadas, identificador de la base de datos en que se compilaron las instrucciones.|  
|objectid|**int**|Identificador del objeto de base de datos. Este valor es NULL para las instrucciones SQL ad hoc.|  
|número|**smallint**|Indica el número del grupo, si se agrupan los procedimientos.<br /><br /> 0 = Las no son procedimientos.<br /><br /> NULL = Instrucciones SQL ad hoc.|  
|encrypted|**bit**|Indica si el objeto está cifrado.<br /><br /> 0 = No cifrado<br /><br /> 1 = Cifrada|  
|texto|**text**|Texto de la instrucción SQL. Este valor es NULL para objetos cifrados.|  
  
## <a name="remarks"></a>Observaciones  
 Puede obtener un identificador SQL válido de la columna sql_handle del sys.dm_exec_requests &#40;la vista de administración dinámica de [Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) .  
  
 Si pasa un identificador que ya no existe en la memoria caché, fn_get_sq **l** devuelve un conjunto de resultados vacío. Si pasa un identificador no válido, el archivo por lotes se detiene y aparece un mensaje de error.  
  
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]No puede almacenar en caché algunas [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones, como instrucciones de copia masiva e instrucciones con literales de cadena mayores de 8 KB. No se pueden recuperar identificadores de esas instrucciones utilizando la función fn_get_sql.  
  
 La columna de **texto** del conjunto de resultados se filtra por texto que puede contener contraseñas. Para obtener más información acerca de los procedimientos almacenados relacionados con la seguridad que no se supervisan, vea [filtrar un seguimiento](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 La función fn_get_sql devuelve información similar al comando [DBCC INPUTBUFFER](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md) . Los siguientes son ejemplos de situaciones en las que se puede utilizar la función fn_get_sql cuando no se puede utilizar DBCC INPUTBUFFER:   
  
-   Cuando los eventos tengan más de 255 caracteres.  
  
-   Cuando tenga que devolver el nivel más alto de anidamiento de un procedimiento almacenado. Por ejemplo, hay dos procedimientos almacenados denominados sp_1 y sp_2. Si sp_1 llama a sp_2 y el identificador se obtiene de la vista de administración dinámica sys.dm_exec_requests mientras se ejecuta sp_2, la función fn_get_sql devuelve información sobre sp_2. Además, la función fn_get_sql devuelve el texto completo del procedimiento almacenado en el nivel más alto de anidamiento.  
  
## <a name="permissions"></a>Permisos  
 El usuario necesita tener el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 Los administradores de base de datos pueden utilizar la función fn_get_sql, como se muestra en el ejemplo siguiente, para facilitar el diagnóstico de procesos problemáticos. Una vez que un administrador identifica el identificador de sesión problemático, puede recuperar el identificador SQL de esa sesión, llamar a la función fn_get_sql con él y luego utilizar los desplazamientos inicial y final para determinar el texto del identificador de sesión SQL problemático.  
  
```  
DECLARE @Handle varbinary(64);  
SELECT @Handle = sql_handle   
FROM sys.dm_exec_requests   
WHERE session_id = 52 and request_id = 0;  
SELECT * FROM sys.fn_get_sql(@Handle);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)   
 [sys.sysprocesos &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
