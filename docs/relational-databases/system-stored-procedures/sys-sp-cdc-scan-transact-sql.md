---
description: sys.sp_cdc_scan (Transact-SQL)
title: sys.sp_cdc_scan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_scan_TSQL
- sp_cdc_scan
- sys.sp_cdc_scan
- sp_cdc_scan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_scan
ms.assetid: 46e4294c-97b8-47d6-9ed9-b436a9929353
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b839f65e6361c88cb842e5d18de9173db8c3727f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159765"
---
# <a name="syssp_cdc_scan-transact-sql"></a>sys.sp_cdc_scan (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ejecuta la operación de recorrido del registro de captura de datos modificados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.sp_cdc_scan [ [ @maxtrans = ] max_trans ]   
     [ , [ @maxscans = ] max_scans ]   
     [ , [ @continuous = ] continuous ]   
     [ , [ @pollinginterval = ] polling_interval ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @maxtrans = ] max_trans` Número máximo de transacciones que se van a procesar en cada ciclo de examen. *max_trans* es de **tipo int** y su valor predeterminado es 500.  
  
`[ @maxscans = ] max_scans` Número máximo de ciclos de recorrido que se ejecutarán para extraer todas las filas del registro. *max_scans* es de **tipo int** y su valor predeterminado es 10.  
  
`[ @continuous = ] continuous` Indica si el procedimiento almacenado debe finalizar después de ejecutar un único ciclo de examen (0) o ejecutarse de forma continua, en pausa durante el tiempo especificado por *polling_interval* antes de volver a ejecutar el ciclo de examen (1). *Continuous* es **tinyint** con un valor predeterminado de 0.  
  
`[ @pollinginterval = ] polling_interval` Número de segundos entre los ciclos de recorrido del registro. *polling_interval* es de tipo **BIGINT** y su valor predeterminado es 0.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 sys.sp_MScdc_capture_job llama internamente a sys.sp_cdc_scan si la captura de datos modificados utiliza el trabajo de captura del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El procedimiento no se puede ejecutar explícitamente cuando una operación de examen del registro de captura de datos modificados ya está activa, o cuando la base de datos está habilitada para la replicación transaccional. Los administradores que deseen personalizar el comportamiento del trabajo de captura configurado automáticamente deben utilizar este procedimiento almacenado.  
  
## <a name="permissions"></a>Permisos  
 Requiere pertenencia al rol fijo de base de datos db_owner.  
  
## <a name="see-also"></a>Consulte también  
 [dbo.cdc_jobs &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)  
  
  
