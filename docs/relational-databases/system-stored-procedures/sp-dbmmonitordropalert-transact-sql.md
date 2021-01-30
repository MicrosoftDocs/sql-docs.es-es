---
description: sp_dbmmonitordropalert (Transact-SQL)
title: sp_dbmmonitordropalert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitordropalert_TSQL
- sp_dbmmonitordropalert
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], monitoring
- sp_dbmmonitordropalert
ms.assetid: fe4a134b-25bf-464e-a5c4-358de215b65a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9f872e37c0a9ae9d869b9483e5818db3b7d238a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212386"
---
# <a name="sp_dbmmonitordropalert-transact-sql"></a>sp_dbmmonitordropalert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita la advertencia para una métrica de rendimiento especificada, estableciendo el umbral en NULL.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dbmmonitordropalert database_name   
    [ , alert_id ]   
```  
  
## <a name="arguments"></a>Argumentos  
 *database_name*  
 Especifica la base de datos para la que se debe quitar el umbral de advertencia especificado.  
  
 *alert_id*  
 Valor entero que identifica la advertencia que se va a quitar. Si se omite este argumento, se quitan todas las advertencias de la base de datos. Para quitar la advertencia para una métrica de rendimiento específica, debe especificar uno de los valores siguientes:  
  
|Value|Métrica de rendimiento|Umbral de advertencia|  
|-----------|------------------------|-----------------------|  
|1|Transacción no enviada más antigua|Especifica el número de minutos de transacciones que se pueden acumular en la cola de envío antes de que se genere una advertencia en la instancia del servidor principal. Esta advertencia ayuda a medir el potencial de pérdida de datos en términos de tiempo y es especialmente relevante para el modo de alto rendimiento. No obstante, la advertencia también es relevante para el modo de alta seguridad cuando la creación de reflejo se detiene o suspende debido a que los asociados se han desconectado.|  
|2|Registro sin enviar|Especifica cuántos kilobytes (KB) de registro sin enviar generan una advertencia en la instancia del servidor principal. Esta advertencia ayuda a medir el potencial de pérdida de datos en términos de KB y es especialmente relevante para el modo de alto rendimiento. No obstante, la advertencia también es relevante para el modo de alta seguridad cuando la creación de reflejo se detiene o suspende debido a que los asociados se han desconectado.|  
|3|Registro sin restaurar|Especifica cuántos KB de registro sin restaurar generan una advertencia en la instancia del servidor reflejado. Esta advertencia ayuda a medir el tiempo de conmutación por error. El *tiempo de la conmutación por error* se compone principalmente del tiempo que el servidor reflejado anterior necesita para poner al día los registros pendientes en su cola rehecha, más un breve tiempo adicional.|  
|4|Sobrecarga de confirmación del servidor reflejado|Especifica el número de milisegundos de retardo medio por transacción que se tolera antes de que se genere una advertencia en el servidor principal. Este retardo es la cantidad de sobrecarga en la que se incurre mientras la instancia del servidor principal espera a la instancia del servidor reflejado para escribir la entrada de registro de la transacción en la cola de puesta al día. Este valor solo es relevante en el modo de alta seguridad.|  
|5|Período de retención|Metadatos que controlan cómo se conservan las filas largas en la tabla de estado de la creación de reflejo de la base de datos.|  
  
> [!NOTE]  
>  Este procedimiento quita los umbrales de advertencia, independientemente de si se especificaron mediante **sp_dbmmonitorchangealert** o monitor de creación de reflejo de la base de datos.  
  
 Para obtener información acerca de los identificadores de eventos correspondientes a las advertencias, consulte [usar alertas y umbrales de advertencia de las métricas de rendimiento de la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se quita la configuración del período de retención de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC sp_dbmmonitordropalert AdventureWorks2012, 5;  
```  
  
 En el ejemplo siguiente se quitan todos los umbrales de advertencia y el período de retención de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC sp_dbmmonitordropalert AdventureWorks2012 ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)  
  
  
