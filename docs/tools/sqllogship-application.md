---
title: sqllogship (aplicación)
description: La aplicación sqllogship realiza una operación de copia de seguridad, copia o restauración y las tareas de limpieza en una configuración de trasvase de registros en una base de datos de SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- sqllogship
ms.assetid: 8ae70041-f3d9-46e4-8fa8-31088572a9f8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 349b8f5bd757e623b79b043d14ce9de91c34e942
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341982"
---
# <a name="sqllogship-application"></a>sqllogship (aplicación)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  La aplicación **sqllogship** realiza una operación de copia de seguridad, copia o restauración y las tareas de limpieza asociadas en una configuración de trasvase de registros. La operación se realiza en una instancia específica de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para una base de datos determinada.  
  
 ![Icono de vínculo a tema](../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") Para las convenciones de sintaxis, vea [Referencia de la utilidad del símbolo del sistema &#40;motor de base de datos&#41;](../tools/command-prompt-utility-reference-database-engine.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqllogship -server instance_name { -backup primary_id | -copy secondary_id | -restore secondary_id } [ -verboselevel level ] [ -logintimeout timeout_value ] [ -querytimeout timeout_value ]  
```  
  
## <a name="arguments"></a>Argumentos  
 **-server** _instance_name_  
 Especifica la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] donde se ejecutará la operación. La instancia del servidor que se va a especificar depende de la operación de trasvase de registros indicada. Para **-backup**, *instance_name* debe ser el nombre del servidor principal en una configuración de trasvase de registros. Para **-copy** o **-restore**, *instance_name* debe ser el nombre de un servidor secundario en una configuración de trasvase de registros.  
  
 **-backup** _primary_id_  
 Realiza una operación de copia de seguridad de la base de datos principal cuyo identificador principal se especifica en *primary_id*. Para obtener este identificador, selecciónelo en la tabla del sistema [log_shipping_primary_databases](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md) o use el procedimiento almacenado [sp_help_log_shipping_primary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md) .  
  
 La operación de copia de seguridad crea la copia de seguridad del registro en el directorio de copia de seguridad. A continuación, la aplicación **sqllogship** limpia los archivos de copia de seguridad antiguos, basándose en el período de retención de archivos. Más tarde, la aplicación registra el historial de la operación de copia de seguridad en el servidor principal y en el servidor de supervisión. Por último, la aplicación ejecuta [sp_cleanup_log_shipping_history](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md), que limpia la información del historial antigua, basándose en el período de retención.  
  
 **-copy** _secondary_id_  
 Realiza una operación de copia para copiar copias de seguridad desde el servidor secundario especificado de la base de datos o bases de datos secundarias cuyo identificador secundario se indica en *secondary_id*. Para obtener este identificador, selecciónelo en la tabla del sistema [log_shipping_secondary](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md) o use el procedimiento almacenado [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) .  
  
 La operación copia los archivos de copia de seguridad desde el directorio de copia de seguridad al directorio de destino. A continuación, la aplicación **sqllogship** registra el historial de la operación de copia en el servidor secundario y en el servidor de supervisión.  
  
 **-restore** _secondary_id_  
 Realiza una operación de restauración en el servidor secundario especificado de la base de datos o bases de datos secundarias cuyo identificador secundario se indica en *secondary_id*. Para obtener este identificador, use el procedimiento almacenado **sp_help_log_shipping_secondary_database** .  
  
 Los archivos de copia de seguridad del directorio de destino creados después del punto de restauración más reciente se restauran en la base de datos o bases de datos secundarias. A continuación, la aplicación **sqllogship** limpia los archivos de copia de seguridad antiguos, basándose en el período de retención de archivos. Más tarde, la aplicación registra el historial de la operación de restauración en el servidor secundario y en el servidor de supervisión. Por último, la aplicación ejecuta **sp_cleanup_log_shipping_history**, que limpia la información del historial antigua, basándose en el período de retención.  
  
 **-verboselevel** _level_  
 Especifica el nivel de mensajes agregados al historial de trasvase de registros. *level* es uno de los siguientes enteros:  
  
|Nivel|Descripción|  
|-----------|-----------------|  
|0|No se obtienen mensajes de depuración ni de seguimiento.|  
|1|Se obtienen mensajes de control de errores.|  
|2|Se obtienen mensajes de control de errores y advertencias.|  
|**3**|Se obtienen mensajes de control de errores, advertencias e informativos. Este es el valor predeterminado.|  
|4|Se obtienen todos los mensajes de depuración y traza.|  
  
 **-logintimeout** _timeout_value_  
 Especifica la cantidad de tiempo asignado al intento de iniciar sesión en la instancia del servidor antes de que se agote el tiempo de espera del intento. El valor predeterminado es 15 segundos. *timeout_value* es de tipo **int** _._  
  
 **-querytimeout** _timeout_value_  
 Especifica la cantidad de tiempo asignado para iniciar la operación especificada antes de que se agote el tiempo de espera. El valor predeterminado es sin tiempo de espera. *timeout_value* es de tipo **int** _._  
  
## <a name="remarks"></a>Observaciones  
 Se recomienda que use los trabajos de copia de seguridad, copia y restauración para realizar la copia de seguridad, copia y restauración cuando sea posible. Para iniciar estos trabajos desde una operación por lotes u otra aplicación, llame al procedimiento almacenado [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) .  
  
 El historial de trasvase de registros creado por **sqllogship** se combina con el historial creado por los trabajos de copia de seguridad, copia y restauración de trasvase de registros. Si tiene previsto usar **sqllogship** repetidamente para realizar operaciones de copia de seguridad, copia y restauración para una configuración de trasvase de registros, considere la posibilidad de deshabilitar los trabajos de trasvase de registros correspondientes. Para obtener más información, consulte [Disable or Enable a Job](../ssms/agent/disable-or-enable-a-job.md).  
  
 La aplicación **sqllogship** , SqlLogShip.exe, está instalada en el directorio x:\Archivos de programa\Microsoft SQL Server\130\Tools\Binn.  
  
## <a name="permissions"></a>Permisos  
 **sqllogship** usa la autenticación de Windows. La cuenta de la autenticación de Windows donde se ejecuta el comando requiere acceso al directorio de Windows y permisos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . El requisito depende de si el comando **sqllogship** especifica la opción **-backup**, **-copy** o **-restore** .  
  
|Opción|Acceso al directorio|Permisos|  
|------------|----------------------|-----------------|  
|**-backup**|Requiere acceso de lectura/escritura al directorio de copia de seguridad.|Requiere los mismos permisos que la instrucción BACKUP. Para obtener más información, vea [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md).|  
|**-copy**|Requiere acceso de lectura al directorio de copia de seguridad y acceso de escritura al directorio de copia.|Requiere los mismos permisos que el procedimiento almacenado [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) .|  
|**-restore**|Requiere acceso de lectura/escritura al directorio de copia.|Requiere los mismos permisos que la instrucción RESTORE. Para obtener más información, vea [RESTORE &#40;Transact-SQL&#41;](../t-sql/statements/restore-statements-transact-sql.md).|  
  
> [!NOTE]  
>  Para conocer las rutas de acceso de los directorios de copia de seguridad y copia, puede ejecutar el procedimiento almacenado **sp_help_log_shipping_secondary_database** o examinar la tabla **log_shipping_secondary** en **msdb**. Las rutas de acceso al directorio de copia de seguridad y al de destino se encuentran en las columnas **backup_source_directory** y **backup_destination_directory** respectivamente.  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)   
 [log_shipping_secondary &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [sp_start_job &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)  
  
  
