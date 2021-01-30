---
description: managed_backup.fn_get_health_status managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_get_health_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_health_status_TSQL
- smart_admin.fn_get_health_status_TSQL
- smart_admin.fn_get_health_status
- fn_get_health_status
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_health_status
- fn_get_health_status
ms.assetid: b376711d-444a-4b5e-b483-8df323b4e31f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b139114cb7be4094c652f1fbf69313d4f7f1f2db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207394"
---
# <a name="managed_backupfn_get_health_status-transact-sql"></a>managed_backup.fn_get_health_status managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve una tabla de cero, una o más filas del recuento agregado de los errores notificados por Eventos extendidos para un período de tiempo concreto.  
  
 La función se usa para notificar el estado de mantenimiento de los servicios en Smart admin.  Actualmente [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] se admite en el paraguas de administración inteligente. Por tanto, los errores devueltos se relacionan con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
managed_backup.fn_get_health_status([@begin_time = ] 'time_1' , [ @end_time = ] 'time_2')  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 [@begin_time]  
 El inicio del período de tiempo desde el que se calcula el recuento agregado de los errores.  El @begin_time parámetro es DateTime. El valor predeterminado es NULL. Si el valor es NULL, la función procesará los eventos notificados al menos 30 minutos antes de la hora actual.  
  
 [ @end_time]  
 El fin del período de tiempo desde el que se calcula el recuento agregado de los errores. El @end_time  parámetro es de tipo DateTime y su valor predeterminado es NULL. Si el valor es NULL, la función procesará los eventos extendidos al menos hasta el momento actual.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|number_of_storage_connectivity_errors|int|Número de errores de conexión cuando el programa se conecta a la cuenta de almacenamiento de Azure.|  
|number_of_sql_errors|int|Número de errores devueltos cuando el programa se conecta al Motor de SQL Server.|  
|number_of_invalid_credential_errors|int|Número de errores devueltos cuando el programa intenta autenticarse utilizando las credenciales SQL.|  
|number_of_other_errors|int|Número de errores en otras categorías además de conectividad, SQL o credencial.|  
|number_of_corrupted_or_deleted_backups|int|Número de archivos de copia de seguridad dañados o eliminados.|  
|number_of_backup_loops|int|Número de veces que el agente de copia de seguridad examina todas las bases de datos configuradas con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|number_of_retention_loops|int|El número de veces que las bases de datos se examinan para evaluar el período de retención establecido.|  
  
## <a name="best-practices"></a>Prácticas recomendadas  
 Estos recuentos agregados se pueden utilizar para supervisar el estado del sistema. Por ejemplo, si la columna number_ of_retention_loops es 0 en 30 minutos, es posible que la administración de la retención esté tardando mucho o incluso que no funcione correctamente. Las columnas de errores que no son cero pueden indicar que hay problemas y los registros de Eventos extendidos se deben comprobar para conocerlos. Como alternativa, use el procedimiento almacenado **managed_backup. sp_get_backup_diagnostics** para obtener una lista de eventos extendidos para buscar los detalles del error.  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere permisos **Select** en la función.  
  
## <a name="examples"></a>Ejemplos  
  
-   El ejemplo siguiente devuelve los recuentos de errores agregados para los 30 últimos minutos desde el momento en que se ejecutó.  
  
    ```  
    SELECT *  
    FROM managed_backup.fn_get_health_status(NULL, NULL)  
  
    ```  
  
-   El ejemplo siguiente devuelve el recuento de errores agregados para la semana actual:  
  
    ```  
    Use msdb  
    Go  
    DECLARE @startofweek datetime  
    DECLARE @endofweek datetime  
    SET @startofweek = DATEADD(Day, 1-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)   
    SET @endofweek = DATEADD(Day, 7-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)  
    SELECT *  
    FROM managed_backup.fn_get_health_status(@startofweek, @endofweek)  
  
    ```  
  
  
