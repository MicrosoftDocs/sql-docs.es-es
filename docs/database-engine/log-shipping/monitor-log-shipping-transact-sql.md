---
title: Supervisar el trasvase de registros (Transact-SQL) | Microsoft Docs
description: Obtenga información sobre qué tablas almacenan el historial que contiene información de supervisión y procedimientos almacenados para supervisar el trasvase de registros en SQL Server.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], status
- history tables [SQL Server]
- historical information [SQL Server], log shipping
- log shipping [SQL Server], monitoring
- alerts [SQL Server], log shipping
- status information [SQL Server], log shipping
- monitoring log shipping [SQL Server]
ms.assetid: acf3cd99-55f7-4287-8414-0892f830f423
author: cawrites
ms.author: chadam
ms.openlocfilehash: 389a5810611b4721fb78b3fd97f484e5ba7b1974
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346980"
---
# <a name="monitor-log-shipping-transact-sql"></a>Supervisar el trasvase de registros (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Después de configurar el trasvase de registros, puede supervisar la información acerca del estado de todos los servidores de trasvase de registros. El historial y el estado de las operaciones de trasvase de registros siempre se guardan de forma local en los trabajos de trasvase de registros. El historial y el estado de la operación de copia de seguridad se almacenan en el servidor principal, y el historial y el estado de las operaciones de copia y restauración se almacenan en el servidor secundario. Si ha implementado un servidor de supervisión remoto, esta información se almacena también en dicho servidor.  
  
 Puede configurar alertas que se activarán si las operaciones de trasvase de registros no se producen según lo programado. Los errores se indican mediante un trabajo de alerta que vigila el estado de las operaciones de copias de seguridad y restauración. Puede definir alertas que envíen una notificación a un operador cuando aparezcan estos errores. Si hay un servidor de supervisión configurado, el trabajo de alerta se ejecuta en el servidor de supervisión que detecta los errores de todas las operaciones de la configuración de trasvase de registros. Si no se ha especificado ningún servidor de supervisión, el trabajo de alerta se ejecuta en la instancia de servidor principal, que supervisa la operación de copia de seguridad. Si no se ha especificado ningún servidor de supervisión, el trabajo de alerta se ejecuta también en cada instancia de servidor secundario para supervisar las operaciones locales de copia y restauración.  
  
> [!IMPORTANT]  
>  Para supervisar una configuración de trasvase de registros, deberá agregar el servidor de supervisión cuando habilite el trasvase de registros. Si agrega un servidor de supervisión más adelante, deberá quitar la configuración de trasvase de registros y reemplazarla por una configuración nueva que incluya un servidor de supervisión. Para obtener más información, vea [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md). Además, una vez configurado el servidor de supervisión, no puede modificarse sin quitar primero el trasvase de registros.  
  
## <a name="history-tables-containing-monitoring-information"></a>Tablas del historial que contienen información de supervisión  
 Las tablas del historial de supervisión contienen metadatos que se almacenan en el servidor de supervisión. También se almacena localmente una copia de la información específica de un servidor primario o secundario determinado.  
  
 Estas tablas se pueden consultar para supervisar el estado de una sesión de trasvase de registros. Por ejemplo, para conocer el estado del trasvase de registros, compruebe el estado y el historial del trabajo de copia de seguridad, trabajo de copia y trabajo de restauración. Puede ver el historial y los detalles de errores específicos del trasvase de registros mediante la consulta de las siguientes tablas de supervisión.  
  
|Tabla|Descripción|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|Almacena el Id. del trabajo de alerta.|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|Almacena los detalles de errores de los trabajos de trasvase de registros. Puede consultar esta tabla para ver los errores de una sesión de agente. Opcionalmente, puede ordenar los errores por la fecha y la hora en que se registraron. Cada error se registra como una secuencia de excepciones y se pueden registrar varios errores (secuencias) por sesión de agente.|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|Contiene los detalles del historial de los agentes de trasvase de registros. Puede consultar esta tabla para ver los detalles del historial de una sesión de agente.|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|Almacena un registro de supervisión para la base de datos principal en cada configuración de trasvase de registros, incluida información acerca del último archivo de copia de seguridad y el último archivo restaurado que es útil para la supervisión.|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|Almacena un registro de supervisión para cada base de datos secundaria, incluida información acerca del último archivo de copia de seguridad y el último archivo restaurado que es útil para la supervisión.|  
  
## <a name="stored-procedures-for-monitoring-log-shipping"></a>Procedimientos almacenados para supervisar el trasvase de registros  
 La información de supervisión e historial se almacena en tablas de **msdb**, a las que se puede tener acceso mediante procedimientos almacenados de trasvase de registros. Ejecute estos procedimientos almacenados en los servidores indicados en la siguiente tabla.  
  
|Procedimiento almacenado|Descripción|Ejecute este procedimiento en|  
|----------------------|-----------------|---------------------------|  
|[sp_help_log_shipping_monitor_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)|Devuelve registros de supervisión para la base de datos principal especificada desde la tabla **log_shipping_monitor_primary** .|Servidor de supervisión o servidor principal|  
|[sp_help_log_shipping_monitor_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)|Devuelve registros de supervisión para la base de datos secundaria especificada desde la tabla **log_shipping_monitor_secondary** .|Servidor de supervisión o servidor secundario|  
|[sp_help_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)|Devuelve el Id. del trabajo de alerta.|Servidor de supervisión, o bien servidor primario o secundario si no hay ningún servidor de supervisión definido|  
|[sp_help_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)|Recupera la configuración de la base de datos principal y muestra los valores de las tablas **log_shipping_primary_databases** y **log_shipping_monitor_primary** .|Servidor principal|  
|[sp_help_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)|Recupera nombres de las bases de datos secundarias para una base de datos principal.|Servidor principal|  
|[sp_help_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)|Recupera la configuración de la base de datos secundaria de las tablas **log_shipping_secondary**, **log_shipping_secondary_databases** y **log_shipping_monitor_secondary** .|Servidor secundario|  
|[sp_help_log_shipping_secondary_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)|Este procedimiento almacenado recupera la configuración de una base de datos principal determinada en el servidor secundario.|Servidor secundario|  
  
## <a name="see-also"></a>Consulte también  
 [Ver el informe de trasvase de registros &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)   
 [Tablas y procedimientos almacenados de trasvase de registros](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
