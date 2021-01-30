---
description: sys.sp_cdc_add_job (Transact-SQL)
title: sys.sp_cdc_add_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_add_job_TSQL
- sys.sp_cdc_add_job
- sys.sp_cdc_add_job_TSQL
- sp_cdc_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_add_job
ms.assetid: c4458738-ed25-40a6-8294-a26ca5a05bd9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c48413f1f588ea4a70c4f8a1190e819758c76b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186692"
---
# <a name="syssp_cdc_add_job-transact-sql"></a>sys.sp_cdc_add_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una limpieza de captura datos de cambios o un trabajo de captura en la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.sp_cdc_add_job [ @job_type = ] 'job_type'  
    [ , [ @start_job = ] start_job ]   
    [ , [ @maxtrans = ] max_trans ]   
    [ , [ @maxscans = ] max_scans ]   
    [ , [ @continuous = ] continuous ]   
    [ , [ @pollinginterval = ] polling_interval ]   
    [ , [ @retention ] = retention ]   
    [ , [ @threshold ] = 'delete_threshold' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_type = ] 'job\_type'` Tipo de trabajo que se va a agregar. *job_type* es de tipo **nvarchar (20)** y no puede ser null. Las entradas válidas son **' Capture '** y **' Cleanup '**.  
  
`[ @start_job = ] start_job` Marca que indica si el trabajo debe iniciarse inmediatamente después de agregarse. *start_job* es de **bit** y su valor predeterminado es 1.  
  
`[ @maxtrans ] = max_trans` Número máximo de transacciones que se van a procesar en cada ciclo de examen. *max_trans* es de **tipo int** y su valor predeterminado es 500. Si se especifica, el valor debe ser un entero positivo.  
  
 *max_trans* solo es válido para los trabajos de captura.  
  
`[ @maxscans ] = max\_scans_` Número máximo de ciclos de recorrido que se ejecutarán para extraer todas las filas del registro. *max_scans* es de **tipo int** y su valor predeterminado es 10.  
  
 *max_scan* solo es válido para los trabajos de captura.  
  
`[ @continuous ] = continuous_` Indica si el trabajo de captura debe ejecutarse continuamente (1) o solo una vez (0). *Continuous* es de **bit** y su valor predeterminado es 1.  
  
 Cuando *Continuous* = 1, el trabajo de [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md) examina el registro y procesa hasta (*max_trans* \* *max_scans*) transacciones. Después, espera el número de segundos especificado en *polling_interval* antes de comenzar el siguiente examen del registro.  
  
 Cuando *Continuous* = 0, el trabajo de **sp_cdc_scan** se ejecuta hasta *max_scans* recorridos del registro, procesando hasta *max_trans* transacción durante cada análisis y, a continuación, se cierra.  
  
 *Continuous* solo es válido para los trabajos de captura.  
  
`[ @pollinginterval ] = polling\_interval_` Número de segundos entre los ciclos de recorrido del registro. *polling_interval* es de tipo **BIGINT** y su valor predeterminado es 5.  
  
 *polling_interval* solo es válido para los trabajos de captura cuando *Continuous* está establecido en 1. Si se especifica, el valor debe ser mayor o igual que 0 y menor que 24 horas (máx.: 86399 segundos). Si se especifica el valor 0, no hay ninguna espera entre los exámenes del registro.  
  
`[ @retention ] = retention_` Número de minutos que las filas de datos de cambio se conservarán en las tablas de cambios. la *retención* es **BIGINT** con un valor predeterminado de 4320 (72 horas). El valor máximo es 52494800 (100 años). Si se especifica, el valor debe ser un entero positivo.  
  
 la *retención* solo es válida para los trabajos de limpieza.  
  
`[ @threshold = ] 'delete\_threshold'` Número máximo de entradas de eliminación que se pueden eliminar mediante una única instrucción en la limpieza. *delete_threshold* es de tipo **BIGINT** y su valor predeterminado es 5000.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Un trabajo de limpieza se crea utilizando los valores predeterminados cuando la primera tabla de la base de datos se habilita para la captura de datos de cambio. Un trabajo de cambio se crea usando los valores predeterminados cuando la primera tabla de la base de datos se habilita para la captura de datos de cambio y no existe ninguna publicación transaccional para la base de datos. Si existe una publicación transaccional, se usará el lector del registro transaccional para controlar el mecanismo de captura, y no se permitirá ni se necesitará realizar otro trabajo de captura.  
  
 Dado que los trabajos de captura y limpieza se crean de forma predeterminada, este procedimiento almacenado solo es necesario cuando un trabajo se ha eliminado y se ha vuelto a crear explícitamente.  
  
 El nombre del trabajo es **CDC.** _\<database\_name\>_ **\_ Cleanup** o **CDC.** _\<database\_name\>_ **\_ Capture**, donde *<database_name>* es el nombre de la base de datos actual. Si ya existe un trabajo con el mismo nombre, el nombre se anexa con un punto (**.**) seguido de un identificador único, por ejemplo: **CDC.AdventureWorks_capture. A1ACBDED-13FC-428C-8302-10100EF74F52**.  
  
 Para ver la configuración actual de un trabajo de limpieza o de captura, use [sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md). Para cambiar la configuración de un trabajo, use [sp_cdc_change_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **db_owner** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-capture-job"></a>A. Crear un trabajo de captura  
 En este ejemplo se crea un trabajo de captura. Este ejemplo supone que el trabajo de limpieza existente se eliminó y se debe volver a crear explícitamente. El trabajo se crea utilizando los valores predeterminados.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job @job_type = N'capture';  
GO  
```  
  
### <a name="b-creating-a-cleanup-job"></a>B. Crear un trabajo de limpieza  
 En el siguiente ejemplo se crea un trabajo de limpieza en la base de datos de [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. El parámetro `@start_job` está establecido en 0 y `@retention` está establecido en 5760 minutos (96 horas). Este ejemplo supone que el trabajo de limpieza existente se eliminó y se debe volver a crear explícitamente.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_add_job  
     @job_type = N'cleanup'  
    ,@start_job = 0  
    ,@retention = 5760;  
```  
  
## <a name="see-also"></a>Consulte también  
 [dbo.cdc_jobs &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
