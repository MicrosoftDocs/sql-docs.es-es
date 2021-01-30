---
description: sys.sp_cdc_stop_job (Transact-SQL)
title: sys.sp_cdc_stop_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_stop_job_TSQL
- sys.sp_cdc_stop_job
- sp_cdc_stop_job_TSQL
- sp_cdc_stop_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_stop_job
ms.assetid: 421fc21c-c7a4-407c-8b31-359273b68c63
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c2a738af88e947528f4c8d2cadf871db7cd8334a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159746"
---
# <a name="syssp_cdc_stop_job-transact-sql"></a>sys.sp_cdc_stop_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Detiene una limpieza de captura datos modificados o un trabajo de captura de la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.sp_cdc_stop_job [ [ @job_type = ] 'job_type' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ [ @job_type = ] 'job_type_' ]` Tipo de trabajo que se va a agregar. *job_type* es de tipo **nvarchar (20)** y su valor predeterminado es **Capture**. Las entradas válidas son **Capture** y **Cleanup**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Un administrador puede utilizar sys.sp_cdc_stop_job para detener explícitamente el trabajo de captura o el trabajo de limpieza.  
  
## <a name="permissions"></a>Permisos  
 Requiere pertenencia al rol fijo de base de datos db_owner.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se detiene el trabajo de limpieza de la base de datos `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_cdc_stop_job @job_type = N'capture';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [dbo.cdc_jobs &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_start_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md)  
  
  
