---
description: sp_post_msx_operation (Transact-SQL)
title: sp_post_msx_operation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_post_msx_operation
- sp_post_msx_operation_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_post_msx_operation
ms.assetid: 085deef8-2709-4da9-bb97-9ab32effdacf
author: markingmyname
ms.author: maghan
ms.openlocfilehash: be9ef622f686044800f634837819b4cba92821a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196035"
---
# <a name="sp_post_msx_operation-transact-sql"></a>sp_post_msx_operation (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inserta operaciones (filas) en la tabla del sistema **sysdownloadlist** para que los servidores de destino los descarguen y ejecuten.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_post_msx_operation  
     [ @operation = ] 'operation'  
     [ , [ @object_type = ] 'object' ]   
     { , [ @job_id = ] job_id }   
     [ , [ @specific_target_server = ] 'target_server' ]   
     [ , [ @value = ] value ]  
     [ , [ @schedule_uid = ] schedule_uid ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @operation = ] 'operation'` Tipo de operación de la operación expuesta. la *operación* es **VARCHAR (64)** y no tiene ningún valor predeterminado. Las operaciones válidas dependen de *object_type*.  
  
|Tipo de objeto|Operación|  
|-----------------|---------------|  
|**TRABAJO**|INSERT<br /><br /> UPDATE<br /><br /> Delete<br /><br /> START<br /><br /> STOP|  
|**SERVIDOR**|RE-ENLIST<br /><br /> DEFECT<br /><br /> SYNC-TIME<br /><br /> SET-POLL|  
|**PLANEA**|INSERT<br /><br /> UPDATE<br /><br /> Delete|  
  
`[ @object_type = ] 'object'` Tipo de objeto para el que se va a exponer una operación. Los tipos válidos son **Job**, **Server** y **Schedule**. el *objeto* es **VARCHAR (64)** y su valor predeterminado es **Job**.  
  
`[ @job_id = ] job_id` Número de identificación del trabajo al que se aplica la operación. *job_id* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado. **0x00** indica todos los trabajos. Si el *objeto* es **Server**, *job_id* no es necesario.  
  
`[ @specific_target_server = ] 'target_server'` Nombre del servidor de destino para el que se aplica la operación especificada. Si se especifica *job_id* , pero no se especifica *target_server* , las operaciones se exponen para todos los servidores de trabajo del trabajo. *target_server* es de tipo **nvarchar (30)** y su valor predeterminado es NULL.  
  
`[ @value = ] value` El intervalo de sondeo, en segundos. *value* es de tipo **int** y su valor predeterminado es NULL. Especifique este parámetro solo si la *operación* es **set-Poll**.  
  
`[ @schedule_uid = ] schedule_uid` Identificador único de la programación a la que se aplica la operación. *schedule_uid* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_post_msx_operation** se debe ejecutar desde la base de datos **msdb** .  
  
 siempre se puede llamar a **sp_post_msx_operation** de forma segura porque primero determina si el servidor actual es un agente de Microsoft SQL Server multiservidor y, si es así, si el *objeto* es un trabajo multiservidor.  
  
 Una vez que se ha publicado una operación, aparece en la tabla **sysdownloadlist** . Después de crear y exponer un trabajo, también se deben comunicar los cambios siguientes de ese trabajo a los servidores de destino (TSX). Esto también se realiza mediante la lista de descarga.  
  
 Es muy recomendable administrar la lista de descarga con SQL Server Management Studio. Para obtener más información, vea [ver o modificar trabajos](../../ssms/agent/view-or-modify-jobs.md).  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar este procedimiento almacenado, se debe conceder a los usuarios el rol fijo de servidor **sysadmin** .  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_jobserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_delete_targetserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-targetserver-transact-sql.md)   
 [sp_resync_targetserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-resync-targetserver-transact-sql.md)   
 [sp_start_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_stop_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-stop-job-transact-sql.md)   
 [sp_update_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [sp_update_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
