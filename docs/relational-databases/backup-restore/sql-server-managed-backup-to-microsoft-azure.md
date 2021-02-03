---
title: Copia de seguridad administrada de SQL Server en Microsoft Azure | Microsoft Docs
description: Copia de seguridad administrada de SQL Server para Microsoft Azure administra y automatiza las copias de seguridad de SQL Server en Microsoft Azure Blob Storage.
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: afa01165-39e0-4efe-ac0e-664edb8599fd
author: cawrites
ms.author: chadam
ms.openlocfilehash: c925c4a8361b54ff563b6a1443e392f0ca392df9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237268"
---
# <a name="sql-server-managed-backup-to-microsoft-azure"></a>Copia de seguridad administrada en Microsoft Azure para SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] administra y automatiza las copias de seguridad de SQL Server en el servicio Almacenamiento de blobs de Microsoft Azure. Puede dejar que SQL Server determine la programación copia de seguridad según la carga de trabajo de transacciones de la base de datos. Como alternativa, puede usar las opciones avanzadas para definir una programación. La configuración de retención determina durante cuánto tiempo se almacenan las copias de seguridad en Almacenamiento de blobs de Azure. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] admite la restauración a un momento dado para el período de retención especificado.  
  
 A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], los procedimientos y el comportamiento subyacente de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ha cambiado. Para obtener más información, consulte [Migración de la configuración de copia de seguridad administrada de SQL Server 2014 a SQL Server 2016](../../relational-databases/backup-restore/migrate-sql-server-2014-managed-backup-settings-to-sql-server-2016.md).  
  
> [!TIP]  
>  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] se recomienda para las instancias de SQL Server que se ejecutan en máquinas virtuales de Microsoft Azure.  
  
## <a name="benefits"></a>Ventajas  
 Actualmente, para automatizar las copias de seguridad de varias bases de datos, se requiere desarrollar una estrategia de copia de seguridad, escribir código personalizado y programar copias de seguridad. Con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], puede crear un plan de copia de seguridad especificando solo el periodo de retención y la ubicación de almacenamiento. Aunque la configuración avanzada está disponible, no es necesaria. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] programa, realiza y mantiene las copias de seguridad.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] puede configurarse en el nivel de base de datos o de la instancia de SQL Server. Al ajustar la configuración en el nivel de instancia, también se realiza automáticamente una copia de seguridad de todas las bases de datos nuevas. La configuración en el nivel de base de datos puede utilizarse para reemplazar los valores predeterminados de nivel de instancia en un caso individual.  
  
 También puede cifrar las copias de seguridad para obtener seguridad adicional y configurar una programación personalizada con el fin de controlar cuándo se realizan las copias de seguridad. Para obtener más información sobre las ventajas de usar el servicio de almacenamiento de blobs de Microsoft Azure para las copias de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Copia de seguridad y restauración de SQL Server con el servicio de Almacenamiento de blobs de Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
##  <a name="prerequisites"></a><a name="Prereqs"></a> Requisitos previos  
 Almacenamiento de Microsoft Azure se usa [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] para almacenar los archivos de copia de seguridad. Se necesitan los siguientes requisitos previos:  
  
|Requisito previo|Descripción|  
|------------------|-----------------|  
|**Cuenta de Microsoft Azure**|Puede comenzar a trabajar con Azure con un [prueba gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de explorar [opciones de compra](https://azure.microsoft.com/pricing/purchase-options/).|  
|**Cuenta de Azure Storage**|Las copias de seguridad se almacenan en el servicio Almacenamiento de blobs de Microsoft Azure asociado con una cuenta de Almacenamiento de Azure. Para obtener instrucciones paso a paso sobre cómo crear una cuenta de almacenamiento, vea [Acerca de las cuentas de almacenamiento de Azure](/azure/storage/common/storage-account-create).|  
|**Contenedor de blobs**|Los blobs se organizan en contenedores. Hay que especificar el contenedor de destino para los archivos de copia de seguridad. Puede crear un contenedor en el [Portal de administración de Microsoft Azure](https://manage.windowsazure.com/), o bien usar el comando **New-AzureStorageContainer** de [Azure PowerShell](/powershell/azure/).|  
|**Firma de acceso compartido (SAS)**|El acceso al contenedor de destino se controla mediante una firma de acceso compartido (SAS). Para más información, vea [Firmas de acceso compartido, Parte 1: Descripción del modelo SAS](/azure/storage/common/storage-sas-overview). Puede crear un token SAS en el código o con el comando de PowerShell **New-AzureStorageContainerSASToken** . Para ver un script de PowerShell que simplifica este proceso, vea [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell)(Simplificación de la creación de credenciales de SQL con tokens de firmas de acceso compartido (SAS) en Almacenamiento de Azure con PowerShell). El token SAS se puede almacenar en una **credencial SQL** para emplearla con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|**Agente SQL Server**|El Agente SQL Server debe estar en ejecución para que [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] funcione. Considere la posibilidad de establecer la opción de inicio automático.|  
  
## <a name="components"></a>Componentes  
 Transact-SQL es la interfaz principal para interactuar con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Los procedimientos almacenados del sistema se utilizan para habilitar, configurar y supervisar [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Las funciones del sistema se utilizan para recuperar la configuración, los valores de parámetros y la información del archivo de copia de seguridad existentes. Los eventos extendidos se utilizan para exponer los errores y advertencias. Los mecanismos de alerta se habilitan mediante los trabajos del Agente SQL y la administración basada en directivas de SQL Server. La siguiente es una lista de los objetos y una descripción de su funcionalidad en relación con [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Los cmdlets de PowerShell también están disponibles para configurar [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. SQL Server Management Studio permite restaurar las copias de seguridad creadas por [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] mediante la tarea **Restaurar base de datos**  
  
|Objeto del sistema|Descripción|  
|-|-|  
|**MSDB**|Almacena los metadatos y el historial de copias de seguridad de todas las copias de seguridad creadas por [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)|Habilita [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_advanced &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)|Configura las opciones de configuración avanzada de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], como el cifrado.|  
|[managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)|Crea una programación personalizada de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_ backup_master_switch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-master-switch-transact-sql.md)|Pone en pausa y reanuda [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_set_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql.md)|Habilita y configura la supervisión de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Ejemplos: habilitar eventos extendidos, configuración de correo para las notificaciones.|  
|[managed_backup.sp_backup_on_demand &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-on-demand-transact-sql.md)|Realiza una copia de seguridad ad hoc de una base de datos que está habilitada para usa [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sin interrumpir la cadena de registros.|  
|[managed_backup.fn_backup_db_config &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-backup-db-config-transact-sql.md)|Devuelve los valores de configuración y el estado de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] actuales de una base de datos o de todas las de la instancia.|  
|[managed_backup.fn_is_master_switch_on &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-is-master-switch-on-transact-sql.md)|Devuelve el estado del modificador principal.|  
|[managed_backup.sp_get_backup_diagnostics &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql.md)|Devuelve los eventos registrados mediante los eventos extendidos.|  
|[managed_backup.fn_get_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-parameter-transact-sql.md)|Devuelve los valores actuales de la configuración del sistema de copia de seguridad como la supervisión y la configuración del correo de las alertas.|  
|[managed_backup.fn_available_backups &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-available-backups-transact-sql.md)|Recupera las copias de seguridad disponibles de una base de datos especificada o de todas las de una instancia.|  
|[managed_backup.fn_get_current_xevent_settings &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-current-xevent-settings-transact-sql.md)|Devuelve la configuración actual de los eventos extendidos.|  
|[managed_backup.fn_get_health_status &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql.md)|Devuelve los recuentos agregados de los errores registrados mediante los eventos extendidos durante un periodo específico.|  
  
## <a name="backup-strategy"></a>Estrategia de copia de seguridad  
  
### <a name="backup-scheduling"></a>Programación de copias de seguridad  
 Puede especificar una programación de copia de seguridad personalizada con el procedimiento almacenado del sistema [managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md). Si no especifica una programación personalizada, el tipo de copias de seguridad programadas y la frecuencia de copia de seguridad se determinarán en función de la carga de trabajo de la base de datos. La configuración del período de retención se utiliza para determinar el tiempo que un archivo de copia de seguridad debe conservarse en el almacenamiento y la capacidad de recuperar la base de datos hasta un momento dado dentro del período de retención.  
  
### <a name="backup-file-naming-conventions"></a>Convenciones de nomenclatura de los archivos de copia de seguridad  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] usa el contenedor que especifique, por lo que es el usuario quien se encarga de controlar el nombre del contenedor. Para los archivos de copia de seguridad, las bases de datos sin disponibilidad reciben un nombre de acuerdo a la convención siguiente: el nombre se crea con los primeros 40 caracteres del nombre de la base de datos, el GUID de la base de datos sin "-" y la marca de tiempo. El carácter de subrayado se inserta entre los segmentos como separadores. La extensión de archivo **.bak** se usa en el caso de que la copia de seguridad sea completa y **.log** se usa para las copias de seguridad de registros. En las bases de datos del grupo de disponibilidad, además de la convención de nomenclatura de archivos descrita anteriormente, se agrega el GUID de la base de datos del grupo de disponibilidad después de los 40 caracteres del nombre de la base de datos. El GUID de la base de datos del grupo de disponibilidad es el valor de group_database_id de sys.databases.  
  
### <a name="full-database-backup"></a>Copia de seguridad completa de base de datos  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] programa una copia de seguridad completa de la base de datos si se cumple alguna de las siguientes condiciones.  
  
-   Una base de datos se habilita para [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] por primera vez o cuando [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] se habilita con la configuración predeterminada en el nivel de instancia.  
  
-   El crecimiento del registro desde la última copia de seguridad completa de la base de datos es igual o mayor que 1 GB.  
  
-   Ha transcurrido el intervalo de tiempo máximo de una semana desde la última copia de seguridad completa de la base de datos.  
  
-   La cadena de registros se interrumpe. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] comprueba periódicamente si la cadena de registros está intacta comparando el primer y el último LSN de los archivos de copia de seguridad. Si se interrumpe la cadena de registros por cualquier motivo, [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] programa una copia de seguridad completa de la base de datos. La razón más común para la interrupción de la cadena de registros es, probablemente, un comando de copia de seguridad emitido con Transact-SQL o con la tarea de copia de seguridad en SQL Server Management Studio.  Otros escenarios comunes incluyen la eliminación accidental de los archivos de registro de copia de seguridad o la sobrescritura accidental de las copias de seguridad.  
  
### <a name="transaction-log-backup"></a>Copia de seguridad de registros de transacciones  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] programa una copia de seguridad de registro si se cumple una de las siguientes condiciones:  
  
-   No se encuentra el historial de copias de seguridad de registros. Normalmente esto es así cuando [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] se habilita por primera vez.  
  
-   El espacio del registro de transacciones utilizado es de 5 MB o más.  
  
-   Se alcanza el intervalo de tiempo máximo de 2 horas desde la última copia de seguridad de registros.  
  
-   En cualquier momento, la copia de seguridad del registro de transacciones se retrasa después de una copia de seguridad completa de la base de datos. El objetivo es mantener la cadena de registros por delante de la copia de seguridad completa.  
  
## <a name="retention-period-settings"></a>Configuración del período de retención  
 Cuando se habilita la copia de seguridad debe establecer el periodo de retención en días: el mínimo es 1 día y el máximo 30 días.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] según la configuración del período de retención evalúa la capacidad de recuperar a un momento dado en el tiempo especificado para determinar qué archivos de copia de seguridad mantener e identificar los que hay que eliminar. El backup_finish_date de la copia de seguridad se utiliza para determinar y hacer coincidir el tiempo especificado por la configuración del período de retención.  
  
## <a name="important-considerations"></a>Consideraciones importantes  
 Para las bases de datos, si hay un trabajo de copia de seguridad completa de la base de datos en ejecución, [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] espera a que el trabajo actual se complete antes de hacer otra copia de seguridad completa de la misma base de datos. Asimismo, solo una copia de seguridad del registro de transacciones se puede ejecutar en un momento dado. Sin embargo, una copia de seguridad completa y una copia de seguridad del registro de transacciones pueden ejecutarse simultáneamente. Los errores se registran como Eventos extendidos.  
  
 Si se programan más de 10 copias de seguridad completas simultáneas de la base de datos, se emitirá una advertencia a través del canal de depuración de Eventos extendidos. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] mantiene entonces una cola de prioridad para las bases de datos restantes que requieren una copia de seguridad hasta que se programen y completen todas.  

> [!NOTE]
> Los servidores proxy no admiten copias de seguridad administradas en SQL Server.
>
  
##  <a name="supportability"></a><a name="support_limits"></a> Compatibilidad  
 Las siguientes consideraciones y limitaciones de compatibilidad son específicas de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]:  
  
-   Se admite la copia de seguridad de bases de datos del sistema de tipo **master**, **model** y **msdb** . No se admite la copia de seguridad de **tempdb** . 
  
-   En [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], se admiten todos los modelos de recuperación (Completa, Registro masivo y Simple).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] solo es compatible con las copias de seguridad de registros y completas de bases de datos. La automatización de la copia de seguridad de archivos no se admite.  
  
-   El servicio Microsoft Azure Blob Storage es la única opción de almacenamiento de copias de seguridad que se admite. Las copias de seguridad en disco o cinta no se admiten.  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] utiliza la característica de copia de seguridad de blobs en bloques. El tamaño máximo de un blob en bloques es de 200 GB. Sin embargo, gracias a la creación de bandas, el tamaño máximo de una copia de seguridad individual puede ser de hasta 12 TB. Si necesita realizar copias de seguridad de mayor tamaño, considere la posibilidad de usar la compresión y probar el tamaño del archivo de copia de seguridad antes de configurar [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Puede hacer la prueba realizando la copia de seguridad en un disco local o manualmente en Almacenamiento de Microsoft Azure con la instrucción Transact-SQL **BACKUP TO URL** . Para más información, consulte [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] puede tener algunas limitaciones cuando se configura con otras tecnologías que admiten la copia de seguridad, la alta disponibilidad o la recuperación de desastres.  
  
## <a name="see-also"></a>Consulte también  
- [Habilitación de la copia de seguridad administrada de SQL Server en Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)   
- [Configuración de las opciones avanzadas de copia de seguridad administrada de SQL Server en Microsoft Azure](../../relational-databases/backup-restore/configure-advanced-options-for-sql-server-managed-backup-to-microsoft-azure.md)   
- [Deshabilitación de la copia de seguridad administrada de SQL Server en Microsoft Azure](../../relational-databases/backup-restore/disable-sql-server-managed-backup-to-microsoft-azure.md)
- [Realizar copias de seguridad y restaurar bases de datos del sistema](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [Copias de seguridad y restauración de bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
  
