---
description: sp_help_downloadlist (Transact-SQL)
title: sp_help_downloadlist (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_downloadlist_TSQL
- sp_help_downloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_downloadlist
ms.assetid: 745b265b-86e8-4399-b928-c6969ca1a2c8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6f60b3da9c05d38b60573aa272c33a7cb6915116
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208915"
---
# <a name="sp_help_downloadlist-transact-sql"></a>sp_help_downloadlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra todas las filas de la tabla del sistema **sysdownloadlist** para el trabajo proporcionado, o todas las filas si no se especifica ningún trabajo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_downloadlist { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }   
     [ , [ @operation = ] 'operation' ]   
     [ , [ @object_type = ] 'object_type' ]   
     [ , [ @object_name = ] 'object_name' ]   
     [ , [ @target_server = ] 'target_server' ]   
     [ , [ @has_error = ] has_error ]   
     [ , [ @status = ] status ]   
     [ , [ @date_posted = ] date_posted ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Número de identificación del trabajo del que se va a devolver información. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
`[ @operation = ] 'operation'` Operación válida para el trabajo especificado. la *operación* es de tipo **VARCHAR (64)**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**DEFECT**|Operación del servidor que solicita el defecto del servidor de destino al servicio **SQLServerAgent** maestro.|  
|**DELETE**|Operación de trabajo que quita un trabajo completo.|  
|**INSERT**|Operación de trabajo que inserta un trabajo completo o actualiza un trabajo existente. Esta operación incluye todos los pasos y programaciones del trabajo, si corresponde.|  
|**RE-ENLIST**|Operación del servidor que hace que el servidor de destino vuelva a enviar la información de alta, incluidos el intervalo de sondeo y la zona horaria del dominio multiservidor. El servidor de destino también redescarga los detalles de **MSXOperator** .|  
|**SET-POLL**|Operación del servidor que establece el intervalo, en segundos, con el que los servidores de destino sondean el dominio multiservidor. Si se especifica, el *valor* se interpreta como el valor de intervalo necesario y puede ser un valor comprendido entre **10** y **28.800**.|  
|**INICIALES**|Operación de trabajo que solicita el inicio de la ejecución del trabajo.|  
|**DETENER**|Operación de trabajo que solicita la detención de la ejecución del trabajo.|  
|**SYNC-TIME**|Operación de servidor que hace que el servidor de destino sincronice su reloj del sistema con el dominio multiservidor. Como ésta es una operación muy costosa, ejecútela de forma limitada, con poca frecuencia.|  
|**UPDATE**|Operación de trabajo que solo actualiza la información de **sysjobs** para un trabajo, no los pasos o las programaciones de trabajo. **Sp_update_job** llama automáticamente a.|  
  
`[ @object_type = ] 'object_type'` Tipo de objeto para el trabajo especificado. *object_type* es de tipo **VARCHAR (64)** y su valor predeterminado es NULL. *object_type* puede ser un trabajo o un servidor. Para obtener más información acerca de los valores de *object_type* válidos, vea [sp_add_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md).  
  
`[ @object_name = ] 'object_name'` Nombre del objeto. *object_name* es de **tipo sysname y su** valor predeterminado es NULL. Si *object_type* es JOB, *object_name* es el nombre del trabajo. Si *object_type* es SERVER, *object_name* es el nombre del servidor.  
  
`[ @target_server = ] 'target_server'` Nombre del servidor de destino. *target_server* es de tipo **nvarchar (128)** y su valor predeterminado es NULL.  
  
`[ @has_error = ] has_error` Indica si el trabajo debe confirmar los errores. *has_error* es de **tinyint** y su valor predeterminado es null, lo que indica que no se debe confirmar ningún error. **1** indica que se deben confirmar todos los errores.  
  
`[ @status = ] status` El estado del trabajo. *status* es de **tinyint** y su valor predeterminado es NULL.  
  
`[ @date_posted = ] date_posted` Fecha y hora en las que se deben incluir todas las entradas realizadas en el conjunto de resultados o después de la fecha y hora especificadas. *date_posted* es de **tipo DateTime** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Número de identificación entero único de la instrucción.|  
|**source_server**|**nvarchar(30)**|Nombre de equipo del servidor del que proviene la instrucción. En la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versión 7,0, es siempre el nombre de equipo del servidor maestro (MSX).|  
|**operation_code**|**nvarchar(4000)**|Código de operación de la instrucción.|  
|**object_name**|**sysname**|Objeto afectado por la instrucción.|  
|**object_id**|**uniqueidentifier**|Número de identificación del objeto afectado por la instrucción (**job_id** para un objeto de trabajo, o 0x00 para un objeto de servidor) o un valor de datos específico del **operation_code**.|  
|**target_server**|**nvarchar(30)**|Servidor de destino que va a descargar esta instrucción.|  
|**error_message**|**nvarchar(1024)**|Mensaje de error (si existe) del servidor de destino si se encontró algún problema al procesar la instrucción.<br /><br /> Nota: cualquier mensaje de error bloquea todas las descargas posteriores por el servidor de destino.|  
|**date_posted**|**datetime**|Fecha en que la instrucción se envió a la tabla.|  
|**date_downloaded**|**datetime**|Fecha en que el servidor de destino descargó la instrucción.|  
|**status**|**tinyint**|Estado del trabajo:<br /><br /> **0** = sin descargar aún<br /><br /> **1** = descargado correctamente.|  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento corresponden de forma predeterminada a los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestran las filas de `sysdownloadlist` para el trabajo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_downloadlist  
    @job_name = N'NightlyBackups',  
    @operation = N'UPDATE',   
    @object_type = N'JOB',   
    @object_name = N'NightlyBackups',  
    @target_server = N'SEATTLE2',   
    @has_error = 1,   
    @status = NULL,   
    @date_posted = NULL ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
