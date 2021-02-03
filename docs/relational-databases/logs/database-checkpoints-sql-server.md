---
title: Puntos de comprobación de base de datos (SQL Server)| Microsoft Docs
description: Obtenga información sobre los puntos de control, los puntos conocidos desde los que el Motor de base de datos de SQL Server puede empezar a aplicar los cambios incluidos en el registro durante la recuperación.
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- automatic checkpoints
- transaction logs [SQL Server], checkpoints
- logs [SQL Server], active
- pages [SQL Server], dirty
- MinLSN
- checkpoints [SQL Server]
- pages [SQL Server], flushing
- dirty pages
- transaction logs [SQL Server], active logs
- recovery interval option [SQL Server]
- buffer cache [SQL Server]
- logs [SQL Server], checkpoints
- Minimum Recovery LSN
- flushing pages
- active logs
ms.assetid: 98a80238-7409-4708-8a7d-5defd9957185
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 259e58dcf9ef713a9bb8787a63ac8cbf3a349c88
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237730"
---
# <a name="database-checkpoints-sql-server"></a>Puntos de comprobación de base de datos (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
 Un *punto de comprobación* crea un buen punto conocido desde donde [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] puede empezar a aplicar cambios incluidos en el registro durante la recuperación después de un cierre inesperado o un bloqueo del sistema.

##  <a name="overview"></a><a name="Overview"></a> Información general   
Por motivos de rendimiento, [!INCLUDE[ssDE](../../includes/ssde-md.md)] realiza modificaciones en las páginas de la base de datos en memoria, en la memoria caché de búfer, y no escribe estas páginas en el disco después de cada cambio. En su lugar, [!INCLUDE[ssDE](../../includes/ssde-md.md)] emite periódicamente un punto de comprobación en cada base de datos. Un *punto de comprobación* escribe las páginas modificadas en memoria actuales (denominadas *páginas desfasadas*) y la información del registro de transacciones de la memoria en el disco y, además, registra la información del registro de transacciones.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] admite varios tipos de puntos de comprobación: automáticos, indirectos, manuales e internos. En la tabla siguiente se resumen los tipos de **puntos de comprobación**:
  
|Nombre|[!INCLUDE[tsql](../../includes/tsql-md.md)] Interfaz|Descripción|  
|----------|----------------------------------|-----------------|  
|Automático|EXEC sp_configure **'** recovery interval **','** _seconds_ **'**|Se emite automáticamente en segundo plano para cumplir el límite de tiempo superior que sugiere la opción de configuración de servidor **recovery interval** . Los puntos de comprobación automáticos se ejecutan hasta completarse.  Los puntos de comprobación automáticos están limitados según el número de operaciones de escritura pendientes y en función de si [!INCLUDE[ssDE](../../includes/ssde-md.md)] detecta un aumento de la latencia de escritura superior a 50 milisegundos.<br /><br /> Para más información, consulte [Configure the recovery interval Server Configuration Option](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).|  
|Indirecto|ALTER DATABASE ... SET TARGET_RECOVERY_TIME **=** _target\_recovery\_time_ { SECONDS &#124; MINUTES }|Se emiten en segundo plano para cumplir un tiempo de recuperación de destino especificado por el usuario para una determinada base de datos. Empezando por [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)], el valor predeterminado es 1 minuto. El valor predeterminado es 0 para versiones antiguas, que indica que la base de datos utilizará puntos de comprobación automáticos, cuya frecuencia depende del valor de intervalo de recuperación de la instancia de servidor.<br /><br /> Para obtener más información, vea [Cambiar el tiempo de recuperación de destino de una base de datos &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).|  
|Manual|CHECKPOINT [*duración_del_punto_de_comprobación*]|Se emite cuando se ejecuta un comando CHECKPOINT de [!INCLUDE[tsql](../../includes/tsql-md.md)] . El punto de comprobación manual se produce en la base de datos actual para la conexión. De forma predeterminada, los puntos de comprobación manuales se ejecutan hasta completarse. La limitación funciona de la misma forma que para los puntos de comprobación automáticos.  Opcionalmente, el parámetro *checkpoint_duration* especifica un periodo de tiempo solicitado, en segundos, para que se complete el punto de comprobación.<br /><br /> Para más información, consulte [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md).|  
|Interno|Ninguno.|Se emite por varias operaciones de servidor, como la copia de seguridad y la creación de instantánea de base de datos, para garantizar que las imágenes de disco coinciden con el estado actual del registro.|  
  
> [!NOTE]
> La opción de configuración avanzada **-k** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite que un administrador de bases de datos limite el comportamiento de E/S de los puntos de comprobación según el rendimiento del subsistema de E/S de algunos tipos de puntos de comprobación. La opción de configuración **-k** es válida para los puntos de comprobación y para cualquier punto de comprobación manual e interno sin limitar.  
  
 Para los puntos de comprobación automáticos, manuales e internos, solo las modificaciones efectuadas después del último punto de comprobación se deben poner al día durante la recuperación de la base de datos. De este modo se reduce el tiempo necesario para recuperar una base de datos.  
  
> [!IMPORTANT]
> Las transacciones no confirmadas de larga ejecución aumentan el tiempo de recuperación para todos los tipos de puntos de comprobación.   
  
##  <a name="interaction-of-the-target_recovery_time-and-recovery-interval-options"></a><a name="InteractionBwnSettings"></a> Interacción de las opciones TARGET_RECOVERY_TIME y "recovery interval"  
 En la tabla siguiente se resume la interacción entre la configuración de **sp_configure "** recovery interval **"** que afecta a todo el servidor y la configuración específica `ALTER DATABASE ... TARGET_RECOVERY_TIME`.  
  
|target_recovery_time|"recovery interval"|Tipo de punto de comprobación usado|  
|----------------------------|-------------------------|-----------------------------|  
|0|0|Puntos de comprobación automáticos cuyo intervalo de recuperación de destino es 1 minuto.|  
|0|>0|Los puntos de comprobación automáticos cuyo intervalo de recuperación de destino lo especifica la configuración definida por el usuario de la opción **"intervalo de recuperación" sp_configure**.|  
|>0|No aplicable.|Puntos de comprobación indirectos cuyo tiempo de recuperación de destino lo determina la configuración TARGET_RECOVERY_TIME, expresado en segundos.|  
  
##  <a name="automatic-checkpoints"></a><a name="AutomaticChkpt"></a> Puntos de comprobación automáticos  
Un punto de comprobación automático se produce cada vez que el número de entradas de registro alcanza el número que [!INCLUDE[ssDE](../../includes/ssde-md.md)] calcula que puede procesar durante el tiempo especificado en la opción de configuración del servidor **recovery interval** . Para más información, consulte [Configure the recovery interval Server Configuration Option](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).
 
En cada base de datos sin un tiempo de recuperación de destino definido por el usuario, [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera puntos de comprobación automáticos. La frecuencia de los puntos de comprobación automáticos depende de la opción de configuración de servidor avanzada **recovery interval** , que especifica el tiempo máximo que una determinada instancia de servidor debe usar para recuperar una base de datos durante un reinicio del sistema. [!INCLUDE[ssDE](../../includes/ssde-md.md)] calcula el número máximo de entradas de registro que puede procesar durante el intervalo de recuperación. Cuando una base de datos que usa puntos de comprobación automáticos alcanza el número máximo de entradas de registro, [!INCLUDE[ssDE](../../includes/ssde-md.md)] emite un punto de comprobación en la base de datos. 
 
El intervalo de tiempo entre puntos de comprobación puede ser **muy** variable. Una base de datos con una carga de trabajo de transacciones considerable tendrá más puntos de comprobación frecuentes que otra que se usa principalmente para operaciones de solo lectura. Según el modelo de recuperación simple, un punto de comprobación automático también se pone en cola si el registro se llena al 70 %.  
  
Según el modelo de recuperación simple, a menos que algún factor retrase el truncamiento del registro, un punto de comprobación automático trunca la sección no usada del registro de transacciones. Por el contrario, según los modelos de recuperación completa y de registro masivo, después de que se haya establecido una cadena de copia de seguridad, los puntos de comprobación automáticos no provocan el truncamiento del registro. Para más información, consulte [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
Después de un bloqueo del sistema, el periodo de tiempo necesario para recuperar una determinada base de datos depende en gran medida de la cantidad de E/S necesaria para rehacer las páginas que estaban desfasadas en el momento del bloqueo. Esto significa que la configuración **recovery interval** no es confiable. No puede determinar una duración de recuperación precisa. Además, cuando punto de comprobación automático está en curso, la actividad de E/S general de los datos aumenta considerablemente y es bastante impredecible.  
   
###  <a name="impact-of-recovery-interval-on-recovery-performance"></a><a name="PerformanceImpact"></a> Repercusión del intervalo de recuperación en el rendimiento de recuperación  
En un sistema de procesamiento de transacciones en línea (OLTP) con transacciones cortas, la opción **recovery interval** es el factor principal que determina la duración de la recuperación. Pero la opción **recovery interval** no afecta al tiempo necesario para deshacer una transacción de ejecución prolongada. La recuperación de una base de datos con una transacción de ejecución prolongada puede tardar mucho más que lo especificado en el valor de **intervalo de recuperación**. 
 
Por ejemplo, si una transacción de ejecución prolongada tardó dos horas en realizar actualizaciones antes de que se deshabilitara el servidor, la recuperación real tarda un tiempo considerablemente superior al valor de **recovery interval** para recuperar la transacción de ejecución prolongada. Para obtener más información sobre la repercusión de una transacción de ejecución prolongada en el tiempo de recuperación, vea [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md). Para más información sobre el proceso de recuperación, vea [Información general sobre restauración y recuperación (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).
  
Normalmente, los valores predeterminados proporciona un rendimiento de recuperación óptimo. No obstante, el cambio del intervalo de recuperación podría mejorar el rendimiento en las siguientes circunstancias:  
  
-   Si la recuperación suele tardar considerablemente más de 1 minuto cuando no se están revirtiendo transacciones de ejecución prolongada.  
  
-   Si observa que los puntos de comprobación frecuentes afectan negativamente al rendimiento de una base de datos.  
  
Si decide aumentar la configuración **recovery interval** , es recomendable que lo haga gradualmente en pequeños incrementos y evaluando el efecto de cada aumento incremental en el rendimiento de recuperación. Este planteamiento es importante ya que a medida que aumenta la configuración **recovery interval** , se multiplica el tiempo que tarda en completarse la recuperación de la base de datos. Por ejemplo, si cambia **intervalo de recuperación** a 10 minutos, la recuperación tarda aproximadamente 10 veces más en completarse que cuando **intervalo de recuperación** se establece en 1 minuto.  
  
##  <a name="indirect-checkpoints"></a><a name="IndirectChkpt"></a> Puntos de comprobación indirectos
Los puntos de control indirectos, presentados en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], proporcionan una alternativa de nivel de base de datos configurable a los puntos de comprobación automáticos. Esto se puede configurar mediante la especificación de la opción de configuración de la base de datos **Tiempo de recuperación de destino**. Para obtener más información, vea [Cambiar el tiempo de recuperación de destino de una base de datos &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).
Si se produce un bloqueo del sistema, los puntos de comprobación indirectos proporcionan un tiempo de recuperación más rápido y predecible que los puntos de comprobación automáticos. Los puntos de comprobación indirectos proporcionan las siguientes ventajas:  
  
-   Una carga de trabajo transaccional en línea en una base de datos que esté configurada para puntos de comprobación indirectos puede experimentar un deterioro del rendimiento. Los puntos de comprobación indirectos garantizan que el número de páginas desfasadas se encuentra por debajo de un umbral determinado para que la recuperación de la base de datos finalice en el tiempo de recuperación de destino. 

  La opción de configuración **intervalo de recuperación** usa el número de transacciones para determinar el tiempo de recuperación, a diferencia de los **puntos de comprobación indirectos**, que usan el número de páginas desfasadas. Cuando se habilitan los puntos de comprobación indirectos en una base de datos que recibe un gran número de operaciones de DML, el escritor en segundo plano puede iniciar agresivamente el vaciado de búferes desfasados en el disco para asegurarse de que el tiempo necesario para realizar la recuperación se encuentra dentro del tiempo de recuperación de destino establecido de la base de datos. Esto puede provocar actividad de E/S adicional en determinados sistemas que pueden contribuir a un cuello de botella de rendimiento si el subsistema del disco está funcionando por encima del umbral de E/S o cerca de él.  
  
-   Los puntos de comprobación indirectos permiten controlar el tiempo de recuperación de base de datos al tener en cuenta el costo de las E/S aleatorias durante REDO. Eso permite que una instancia de servidor permanezca dentro de un límite superior en los tiempos de recuperación de una determinada base de datos (excepto cuando una transacción de ejecución prolongada provoca tiempos de UNDO excesivos).  
  
-   Los puntos de comprobación indirectos reducen los picos de E/S relacionados con los puntos de comprobación al escribir continuamente las páginas desfasadas en disco en segundo plano.  
  
Pero una carga de trabajo transaccional en línea en una base de datos que esté configurada para puntos de comprobación indirectos puede experimentar un deterioro del rendimiento. Esto se debe a que el escritor en segundo plano que usa el punto de comprobación indirecto en ocasiones aumenta la carga de escritura total para una instancia de servidor.  
 
> [!IMPORTANT]
> El punto de comprobación indirecto es el comportamiento predeterminado para nuevas bases de datos creadas en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], incluidas las bases de datos Model y TempDB.          
> Las bases de datos que se actualizan in situ o se restauraron a partir de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usarán el comportamiento de punto de comprobación automático anterior, a menos que explícitamente se modifique para usar el punto de comprobación indirecto.       

### <a name="improved-indirect-checkpoint-scalability"></a><a name="ctp23"></a> Escalabilidad mejorada de puntos de control indirectos
Antes de [!INCLUDE[ssNoVersion](../../includes/sssql19-md.md)], es posible que experimente errores de programador que no rinde cuando hay una base de datos que genera un gran número de páginas desfasadas, como `tempdb`. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]se presenta una mejor escalabilidad para los puntos de control indirectos, lo que debería evitar estos errores en las bases de datos con una gran carga de trabajo `UPDATE`/`INSERT`.
  
##  <a name="internal-checkpoints"></a><a name="EventsCausingChkpt"></a> Puntos de comprobación internos  
Hay distintos componentes del servidor que generan puntos de comprobación internos para garantizar que las imágenes de disco coinciden con el estado actual del registro. Los puntos de comprobación internos se generan en respuesta a los siguientes eventos:  
  
-   Se han agregado o eliminado archivos de base de datos mediante ALTER DATABASE.  
  
-   Se realiza una copia de seguridad de la base de datos.  
  
-   Se crea una instantánea de base de datos, tanto explícita como internamente para DBCC CHECKDB.  
  
-   Se realiza una actividad que requiere cerrar la base de datos. Por ejemplo, el valor de AUTO_CLOSE es ON y se ha cerrado la última conexión de usuario a la base de datos, o bien se realiza una modificación de una opción de la base de datos que requiere reiniciarla.  
  
-   Se detiene una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deteniendo el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER). Las dos acciones insertan un punto de comprobación en cada base de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Poner fuera de línea una instancia de clúster de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .      
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
 **Para cambiar el intervalo de recuperación en una instancia de servidor**  
  
-   [Establecer la opción de configuración del servidor Intervalo de recuperación](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)  
  
 **Para configurar puntos de comprobación indirectos en una base de datos**  
  
-   [Cambiar el tiempo de recuperación de destino de una base de datos &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)  
  
 **Para emitir un punto de comprobación manual en una base de datos**  
  
-   [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)  

## <a name="see-also"></a>Consulte también  
[El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)            
[Guía de arquitectura y administración de registros de transacciones de SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)      
 
