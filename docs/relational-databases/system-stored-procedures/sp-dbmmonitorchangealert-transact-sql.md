---
description: sp_dbmmonitorchangealert (Transact-SQL)
title: sp_dbmmonitorchangealert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorchangealert_TSQL
- sp_dbmmonitorchangealert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorchangealert
- database mirroring [SQL Server], monitoring
ms.assetid: 1b29f82b-9cf8-4539-8d5c-9a1024db8a50
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e250cc4fb95c32b677db304f5e4de321c14448a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212408"
---
# <a name="sp_dbmmonitorchangealert-transact-sql"></a>sp_dbmmonitorchangealert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega o cambia el umbral de advertencia de una métrica de rendimiento de creación de reflejo especificada.  

  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dbmmonitorchangealert database_name   
    , alert_id   
    , alert_threshold   
    , enabled   
```  
  
## <a name="arguments"></a>Argumentos  
 *database_name*  
 Especifica la base de datos para la que se debe agregar o modificar el umbral de advertencia especificado.  
  
 *alert_id*  
 Valor entero que identifica la advertencia que se va a agregar o modificar. Especifique uno de los valores siguientes:  
  
|Value|Métrica de rendimiento|Umbral de advertencia|  
|-----------|------------------------|-----------------------|  
|1|Transacción no enviada más antigua|Especifica el número de minutos de transacciones que se pueden acumular en la cola de envío antes de que se genere una advertencia en la instancia del servidor principal. Esta advertencia ayuda a medir el potencial de pérdida de datos en términos de tiempo y es especialmente relevante para el modo de alto rendimiento. No obstante, la advertencia también es relevante para el modo de alta seguridad cuando la creación de reflejo se detiene o suspende debido a que los asociados se han desconectado.|  
|2|Registro sin enviar|Especifica cuántos kilobytes (KB) de registro sin enviar generan una advertencia en la instancia del servidor principal. Esta advertencia ayuda a medir el potencial de pérdida de datos en términos de KB y es especialmente relevante para el modo de alto rendimiento. No obstante, la advertencia también es relevante para el modo de alta seguridad cuando la creación de reflejo se detiene o suspende debido a que los asociados se han desconectado.|  
|3|Registro sin restaurar|Especifica cuántos KB de registro sin restaurar generan una advertencia en la instancia del servidor reflejado. Esta advertencia ayuda a medir el tiempo de conmutación por error. El *tiempo de la conmutación por error* se compone principalmente del tiempo que el servidor reflejado anterior necesita para poner al día los registros pendientes en su cola rehecha, más un breve tiempo adicional.|  
|4|Sobrecarga de confirmación del servidor reflejado|Especifica el número de milisegundos de retardo medio por transacción que se tolera antes de que se genere una advertencia en el servidor principal. Este retardo es la cantidad de sobrecarga en la que se incurre mientras la instancia del servidor principal espera a la instancia del servidor reflejado para escribir la entrada de registro de la transacción en la cola de puesta al día. Este valor solo es relevante en el modo de alta seguridad.|  
|5|Período de retención|Metadatos que controlan cómo se conservan las filas largas en la tabla de estado de la creación de reflejo de la base de datos.|  
  
 Para obtener información acerca de los identificadores de eventos correspondientes a las advertencias, consulte [usar alertas y umbrales de advertencia de las métricas de rendimiento de la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
 *alert_threshold*  
 Valor de umbral de la advertencia. Si se devuelve un valor superior a este umbral cuando se actualiza el estado de la creación de reflejos, se escribe una entrada en el registro de eventos de Windows. Este valor representa el número de KB, minutos o milisegundos, en función de la métrica de rendimiento.  
  
> [!NOTE]  
>  Para ver los valores actuales, ejecute el procedimiento almacenado [sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md) .  
  
 *enabled*  
 ¿Está habilitada la advertencia?  
  
 0 = La advertencia está deshabilitada.  
  
 1 = La advertencia está habilitada.  
  
> [!NOTE]  
>  El período de retención siempre está habilitado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se establecen umbrales para cada una de las métricas de rendimiento y el período de retención para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. En la tabla que se muestra a continuación se indican los valores utilizados en este ejemplo.  
  
|*alert_id*|Métrica de rendimiento|Umbral de advertencia|¿Está habilitada la advertencia?|  
|-----------------|------------------------|-----------------------|-----------------------------|  
|1|Transacción no enviada más antigua|30 minutos|Sí|  
|2|Registro sin enviar|10.000 KB|Sí|  
|3|Registro sin restaurar|10.000 KB|Sí|  
|4|Sobrecarga de confirmación del servidor reflejado|1.000 milisegundos|No|  
|5|Período de retención|8 horas|Sí|  
  
```  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 1, 30, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 2, 10000, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 3, 10000, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 4, 1000, 0 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 5, 8, 1 ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorhelpalert &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
  
