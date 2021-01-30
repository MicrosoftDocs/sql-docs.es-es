---
description: sp_purge_jobhistory (Transact-SQL)
title: sp_purge_jobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_purge_jobhistory_TSQL
- sp_purge_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_purge_jobhistory
ms.assetid: 237f9bad-636d-4262-9bfb-66c034a43e88
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25858da6a748f16071ee9d9693ada310c2ce6170
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189301"
---
# <a name="sp_purge_jobhistory-transact-sql"></a>sp_purge_jobhistory (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quita los registros de historial de un trabajo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_purge_jobhistory   
   {   [ @job_name = ] 'job_name' |   
     | [ @job_id = ] job_id }  
   [ , [ @oldest_date = ] oldest_date ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_name = ] 'job_name'` Nombre del trabajo para el que se van a eliminar los registros de historial. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos.  
  
> [!NOTE]  
>  Los miembros del rol fijo de servidor **sysadmin** o los miembros del rol fijo de base de datos **SQLAgentOperatorRole** pueden ejecutar **sp_purge_jobhistory** sin especificar un *job_name* o *job_id*. Cuando los usuarios de **sysadmin** no especifican estos argumentos, se elimina el historial de trabajos de todos los trabajos locales y multiservidor en el tiempo especificado por *oldest_date*. Cuando los usuarios de **SQLAgentOperatorRole** no especifican estos argumentos, se elimina el historial de trabajos de todos los trabajos locales en el tiempo especificado por *oldest_date*.  
  
`[ @job_id = ] job_id` Número de identificación del trabajo para los registros que se van a eliminar. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL. Se debe especificar *job_id* o *job_name* , pero no se pueden especificar ambos. Vea la nota en la descripción de **\@ job_name** para obtener información sobre cómo los usuarios de **sysadmin** o **SQLAgentOperatorRole** pueden usar este argumento.  
  
`[ @oldest_date = ] oldest_date` Registro más antiguo que se va a conservar en el historial. *oldest_date* es de **tipo DateTime** y su valor predeterminado es NULL. Cuando se especifica *oldest_date* , **sp_purge_jobhistory** solo quita los registros que son más antiguos que el valor especificado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Cuando **sp_purge_jobhistory** se completa correctamente, se devuelve un mensaje.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **SQLAgentOperatorRole** pueden ejecutar este procedimiento almacenado. Los miembros de **sysadmin** pueden purgar el historial de trabajos de todos los trabajos locales y multiservidor. Los miembros de **SQLAgentOperatorRole** solo pueden purgar el historial de trabajos de todos los trabajos locales.  
  
 A otros usuarios, incluidos los miembros de **SQLAgentUserRole** y miembros de **SQLAgentReaderRole**, se les debe conceder EXPLÍCITAmente el permiso Execute en **sp_purge_jobhistory**. Una vez que disponen del permiso EXECUTE en este procedimiento almacenado, estos usuarios solo pueden purgar el historial de los trabajos que les pertenecen.  
  
 Los roles fijos de base de datos **SQLAgentUserRole**, **SQLAgentReaderRole** y **SQLAgentOperatorRole** se encuentran en la base de datos **msdb** . Para obtener más información sobre sus permisos, vea [Agente SQL Server roles fijos de base de datos](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-remove-history-for-a-specific-job"></a>A. Quitar el historial de un trabajo determinado  
 En el ejemplo siguiente se quita el historial de un trabajo denominado `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-remove-history-for-all-jobs"></a>B. Quitar el historial de todos los trabajos  
  
> [!NOTE]  
>  Solo los miembros del rol fijo de servidor **sysadmin** y de los miembros de **SQLAgentOperatorRole** pueden quitar el historial de todos los trabajos. Cuando los usuarios de **sysadmin** ejecutan este procedimiento almacenado sin parámetros, se purga el historial de trabajos de todos los trabajos locales y multiservidor. Cuando los usuarios de **SQLAgentOperatorRole** ejecutan este procedimiento almacenado sin parámetros, solo se purga el historial de trabajos de todos los trabajos locales.  
  
 En el ejemplo siguiente se ejecuta el procedimiento sin parámetros para quitar todos los registros de historial.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_help_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobhistory &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobhistory-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)  
  
  
