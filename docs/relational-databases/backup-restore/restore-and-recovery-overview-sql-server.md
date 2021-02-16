---
title: Información general sobre restauración y recuperación (SQL Server) | Microsoft Docs
description: Este artículo sirve de introducción a las operaciones que se deben realizar para recuperar una base de datos de SQL Server de un error por medio de la restauración de un conjunto de copias de seguridad de SQL Server en secuencia.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring tables [SQL Server]
- backups [SQL Server], restore scenarios
- database backups [SQL Server], restore scenarios
- database restores [SQL Server]
- restoring [SQL Server]
- restores [SQL Server]
- table restores [SQL Server]
- restoring databases [SQL Server], about restoring databases
- database restores [SQL Server], scenarios
- accelerated database recovery
ms.assetid: e985c9a6-4230-4087-9fdb-de8571ba5a5f
author: cawrites
ms.author: chadam
ms.openlocfilehash: edc2a92b260e85c2de1ff5b4fc1758748643d174
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346762"
---
# <a name="restore-and-recovery-overview-sql-server"></a>Información general sobre restauración y recuperación (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Para recuperar de un error una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un administrador de bases de datos tiene que restaurar un conjunto de copias de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una secuencia de restauración correcta y significativa de forma lógica. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite restaurar los datos de las copias de seguridad de toda una base de datos, un archivo de datos o una página de datos, tal y como se describe a continuación:  
  
-   La base de datos (una *restauración de la base de datos completa*)  
  
     Se restaura y recupera toda la base de datos, que permanece sin conexión durante las operaciones de restauración y recuperación.  
  
-   El archivo de datos (una *restauración de archivos*)  
  
     Se restaura y recupera un archivo de datos o conjunto de archivos. Durante la restauración de un archivo, los grupos de archivo que incluyen los archivos se dejan sin conexión de forma automática mientras dure el proceso de restauración. Cualquier intento de obtener acceso a un grupo de archivos sin conexión genera un error.  
  
-   La página de datos (una *restauración de páginas*)  
  
     Con el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, puede restaurar bases de datos individuales. Las restauraciones de páginas pueden utilizarse con cualquier base de datos, independientemente del número de grupos de archivos.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] funcionan en todos los sistemas operativos admitidos. Para obtener más información sobre los sistemas operativos admitidos, vea [Requisitos de hardware y software para instalar SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Para obtener más información sobre la compatibilidad con las copias de seguridad de versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea la sección "Soporte de compatibilidad" de [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
##  <a name="overview-of-restore-scenarios"></a><a name="RestoreScenariosOv"></a> Información general de los escenarios de restauración  
 Un *escenario de restauración* en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es el proceso de restaurar datos de una o más copias de seguridad y, a continuación, recuperar la base de datos. Los escenarios de restauración admitidos dependen del modelo de recuperación de la base de datos y de la edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La siguiente tabla presenta los posibles escenarios de restauración compatibles para diferentes modelos de recuperación.  
  
|escenario de restauración|Modelo de recuperación simple|Modelo de recuperación completa o modelo de recuperación optimizado para cargas masivas de registros|  
|----------------------|---------------------------------|----------------------------------------------|  
|restauración de la base de datos completa|Es la estrategia de restauración básica. Una restauración de base de datos completa puede implicar simplemente la restauración y recuperación de una copia de seguridad completa de base de datos. Por otra parte, una restauración de base de datos completa puede consistir en restaurar una copia de seguridad completa de base de datos y, luego, restaurar y recuperar una copia de seguridad diferencial.<br /><br /> Para obtener más información, vea [Restauraciones de base de datos completas &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md).|Es la estrategia de restauración básica. Una restauración completa de una base de datos supone restaurar una copia de seguridad completa de base de datos y, opcionalmente, una copia de seguridad diferencial (si existe), además de restaurar todas las copias de seguridad de registros posteriores (en orden secuencial). La restauración completa de base de datos finaliza al recuperar la última copia de seguridad de registros y restaurarla (RESTORE WITH RECOVERY).<br /><br /> Para obtener más información, vea [Restauraciones de base de datos completas &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|  
|File restore **\***|Restauración de uno o más archivos de solo lectura dañados, sin restaurar la base de datos completa. La restauración de archivos está disponible solo si la base de datos tiene como mínimo un grupo de archivos de solo lectura.|Restaura uno o más archivos, sin restaurar la base de datos completa. La restauración de archivos puede realizarse mientras la base de datos está sin conexión o, en algunas ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cuando está en línea. Durante la restauración de archivos, los grupos de archivos en los que se incluyen los archivos en cuestión permanecen siempre sin conexión.|  
|Restauración de página|No aplicable|Restaura una o más páginas dañadas. La restauración de páginas puede realizarse mientras la base de datos está sin conexión o, en algunas ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cuando está en línea. Durante la restauración de páginas, las páginas que se están restaurando permanecen siempre sin conexión.<br /><br /> Es preciso que haya disponible una cadena intacta de copias de seguridad de registros, hasta el archivo de registro actual, y deben aplicarse todas a fin de actualizar la página según el archivo de registro actual.<br /><br /> Para obtener más información, vea [Restaurar páginas &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).|  
|Restauración por etapas **\***|Restauración y recuperación de la base de datos por etapas a nivel de grupo de archivos, empezando por el grupo de archivos principal y todos los grupos de archivos secundarios de lectura/escritura.|Restauración y recuperación de la base de datos por etapas a nivel del grupo de archivos, empezando por el grupo de archivos principal.<br /><br /> Para obtener más información, vea [Restauraciones por etapas &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).|  
  
 **\*** La restauración en línea solo se admite en la edición Enterprise.  

### <a name="steps-to-restore-a-database"></a>Pasos para restaurar una base de datos
Para realizar una restauración de archivos, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] ejecuta dos pasos: 

-   Crea los archivos de base de datos que faltan.

-   Copia los datos de los dispositivos de copia de seguridad en los archivos de la base de datos.

Para realizar una restauración de la base de datos, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] ejecuta tres pasos: 

-   Crea los archivos de la base de datos y del registro de transacciones si aún no existen.

-   Copia todas las páginas de datos, registro e índice del medio de copia de seguridad de una base de datos en los archivos de base de datos. 

-   Aplica el registro de transacciones en lo que se conoce como [proceso de recuperación](#TlogAndRecovery).

 Independientemente de la forma de restauración de datos, antes de que una base de datos se pueda recuperar, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] garantiza la coherencia lógica de toda la base de datos. Por ejemplo, si restaura un archivo, no puede recuperarlo y conectarlo hasta que se haya puesto al día hasta un punto lo bastante avanzado de forma que sea coherente con la base de datos.  
  
### <a name="advantages-of-a-file-or-page-restore"></a>Ventajas de la restauración de archivos o páginas  
 La restauración y recuperación de archivos o páginas, en lugar de toda la base de datos, ofrece las siguientes ventajas:  
  
-   La restauración de menos datos reduce el tiempo necesario para copiarlos y recuperarlos.  
  
-   En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , es posible que la restauración de archivos o páginas permita que otros datos de la base de datos permanezcan en línea durante la operación de restauración.  

## <a name="recovery-and-the-transaction-log"></a><a name="TlogAndRecovery"></a> Recuperación y el registro de transacciones
Para la mayoría de los escenarios de restauración, es necesario aplicar una copia de seguridad del registro de transacciones y permitir que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ejecute el **proceso de recuperación** para que la base de datos esté en línea. La recuperación es el proceso que utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para que cada base de datos se inicie en un estado de transacción coherente o limpio.

En caso de una conmutación por error u otro apagado no limpio, las bases de datos pueden quedar en un estado en que algunas modificaciones no han llegado a escribirse desde la caché del búfer a los archivos de datos; estos pueden contener modificaciones como resultado de transacciones incompletas. Cuando se inicia una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se ejecuta una recuperación de cada base de datos, que consta de tres fases en función del último [punto de control de base de datos](../../relational-databases/logs/database-checkpoints-sql-server.md):

-   La **fase de análisis** analiza el registro de transacciones para determinar cuál es el último punto de control y crea la tabla de páginas desfasadas (DPT) y la tabla de transacciones activas (ATT). La DPT contiene los registros de las páginas desfasadas en el momento en que se cerró la base de datos. La ATT contiene los registros de las transacciones que estaban activas en el momento en que la base de datos no se cerró correctamente.

-   La **fase de puesta al día** reenvía todas las modificaciones registradas en el registro que es posible que no se hayan escrito en los archivos de datos en el momento en que se cerró la base de datos. El [número mínimo de secuencia de registro](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#minlsn) (minLSN) necesario para una recuperación correcta en toda la base de datos se encuentra en la DPT y marca el inicio de las operaciones de fase de puesta al día necesarias en todas las páginas desfasadas. En esta fase, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] escribe en el disco todas las páginas desfasadas que pertenecen a las transacciones confirmadas.

-   **Fase de reversión** revierte las transacciones incompletas encontradas en la ATT para asegurarse de que se conserva la integridad de la base de datos. Después de la reversión, la base de datos pasa a estar en línea y no se pueden aplicar más copias de seguridad del registro de transacciones a la base de datos.

La información sobre el progreso de cada fase de recuperación de base de datos se registra en el [registro de errores](../../tools/configuration-manager/viewing-the-sql-server-error-log.md) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. También se puede realizar el seguimiento del progreso de recuperación de la base de datos mediante eventos extendidos. Para obtener más información, vea la entrada de blog [Nuevos eventos extendidos para el progreso de recuperación de base de datos](/archive/blogs/sql_server_team/new-extended-events-for-database-recovery-progress).

> [!NOTE]
> En un escenario de restauración por etapas, si un grupo de archivos de solo lectura ha sido de solo lectura desde antes de la creación de la copia de seguridad de archivos, no es necesario aplicar las copias de seguridad de registros al grupo de archivos y estas se omiten en la restauración de archivos. 

<a name="FastRecovery"></a>
> [!NOTE]
> Para maximizar la disponibilidad de las bases de datos en un entorno de Enterprise, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition puede poner en línea una base de datos después de la fase de puesta al día mientras la fase de reversión se sigue ejecutando. Esto se conoce como "recuperación rápida".

##  <a name="recovery-models-and-supported-restore-operations"></a><a name="RMsAndSupportedRestoreOps"></a> Modelos de recuperación y operaciones de restauración admitidas  
 Las operaciones de restauración disponibles para una base de datos dependen de su modelo de recuperación. En la tabla siguiente se resumen todos los modelos de recuperación y las diferentes situaciones de restauración en las que funcionarían.  
  
|Operación de restauración|Modelo de recuperación completa|Modelo de recuperación optimizado para cargas masivas de registros|Modelo de recuperación simple|  
|-----------------------|-------------------------|---------------------------------|---------------------------|  
|Recuperación de datos|Recuperación completa (si el registro está disponible).|Existe el riesgo de perder algunos datos.|Se perderán los datos desde la última copia de seguridad completa o diferencial.|  
|Restauración a un momento dado|Cualquier momento cubierto por las copias de seguridad de registros.|No está permitida si la copia de seguridad de registros contiene algún cambio registrado de forma masiva.|No compatible.|  
|Restauración de archivos * *\** _|Totalmente compatible.|A veces._ *\*\** *|Solo está disponible para archivos secundarios de solo lectura.|  
|Restauración de página * *\** _|Totalmente compatible.|A veces._ *\*\** *|Ninguno.|  
|Restauración por etapas (nivel de grupos de archivos) * *\** _|Totalmente compatible.|A veces._ *\*\** *|Solo está disponible para archivos secundarios de solo lectura.|  
  
 **\*** Disponible solo en la edición Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 **\*\*** Para consultar las condiciones necesarias, vea [Restricciones de restauración con el modelo de recuperación simple](#RMsimpleScenarios), más adelante en este tema.  
  
> [!IMPORTANT]  
> Independientemente del modelo de recuperación de una base de datos, una copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede restaurar en una versión de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] anterior a la versión que creó la copia de seguridad.  
  
## <a name="restore-scenarios-under-the-simple-recovery-model"></a><a name="RMsimpleScenarios"></a> Escenarios de restauración con el modelo de recuperación simple  
 El modelo de recuperación simple impone las siguientes restricciones en las operaciones de restauración:  
  
-   La restauración de archivos y la restauración por etapas están disponibles solo para grupos de archivos secundarios de solo lectura. Para obtener más información sobre estos escenarios de restauración, vea [Restauraciones de archivos &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md) y [Restauraciones por etapas &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
-   No se permite la restauración de páginas.  
  
-   No se permite la restauración a un momento dado.  
  
 Si alguna de estas restricciones no es conveniente para las recuperaciones que usted necesita, considere la posibilidad de utilizar el modelo de recuperación completa. Para obtener más información, vea [Información general de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
> [!IMPORTANT]  
> Independientemente del modelo de recuperación de una base de datos, una copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede restaurar en una versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anterior a la versión que creó la copia de seguridad.  
  
##  <a name="restore-under-the-bulk-logged-recovery-model"></a><a name="RMblogRestore"></a> Restaurar con el modelo de recuperación optimizado para cargas masivas de registros  
 En esta sección se tratan las consideraciones de restauración que son exclusivas del modelo de recuperación optimizado para cargas masivas de registros, que está pensado únicamente como un complemento para el modelo de recuperación completa.  
  
> [!NOTE]  
> Para obtener una introducción al modelo de recuperación optimizado para cargas masivas de registros, vea [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 En general, el modelo de recuperación optimizado para cargas masivas de registros es parecido al modelo de recuperación completa y la información descrita para el modelo de recuperación completa también se aplica al otro modelo. Sin embargo, la recuperación a un momento dado y la restauración en línea se ven afectadas por el modelo de recuperación optimizado para cargas masivas de registros.  
  
### <a name="restrictions-for-point-in-time-recovery"></a>Restricciones de la recuperación a un momento dado  
 Si una copia de seguridad de registros en el modelo de recuperación optimizado para cargas masivas de registros contiene cambios registrados de forma masiva, no se admite la recuperación a un momento dado. Si se intenta realizar una recuperación a un momento dado en una copia de seguridad de registros que contiene cambios masivos, se producirán errores en la operación de restauración.  
  
### <a name="restrictions-for-online-restore"></a>Restricciones de la restauración en línea  
 Una secuencia de restauración en línea solo funciona si se cumplen las condiciones siguientes:  
  
-   Se han realizado todas las copias de seguridad de registros necesarias antes de iniciar la secuencia de restauración.  
  
-   Se debe realizar una copia de seguridad de los cambios masivos antes de iniciar la secuencia de restauración en línea.  
  
-   Si en la base de datos existen cambios masivos, todos los archivos deben estar en línea o [inactivos](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md). (esto significa que ya no forman parte de la base de datos).  
  
 Si no se cumplen estas condiciones, se producirán errores en la secuencia de restauración en línea.  
  
> [!NOTE]  
>  Se recomienda volver al modelo de recuperación completa antes de iniciar una restauración en línea. Para obtener más información, vea [Modelos de recuperación &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 Para obtener más información sobre cómo realizar una restauración con conexión, vea [Restauración con conexión &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
##  <a name="database-recovery-advisor-sql-server-management-studio"></a><a name="DRA"></a> Asesor para recuperación de base de datos (SQL Server Management Studio)  
El Asistente para recuperación de base de datos facilita la creación de planes de restauración que implementan secuencias de restauración correctas óptimas. Se ha dado respuesta a muchos problemas conocidos de restauración de base de datos y mejoras solicitados por los clientes. Entre las principales mejoras que ofrece el Asistente para recuperación de base de datos se incluyen las siguientes:  
  
-   **Algoritmo del plan de restauración:**  el algoritmo usado para crear planes de restauración se ha mejorado de forma considerable, especialmente en escenarios de restauraciones complejas. Muchos casos extremos, incluidos los escenarios de bifurcación en restauraciones a un momento dado, se tratan de manera más eficaz que en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **Restauraciones a un momento dado:**  el Asistente para recuperación de base de datos simplifica considerablemente la restauración de una base de datos a un momento dado en el tiempo. Una escala de tiempo visual de copia de seguridad mejora significativamente la compatibilidad con restauraciones a un momento dado. Esta escala de tiempo visual permite identificar un punto posible en el tiempo como punto de recuperación de destino para restaurar una base de datos. La escala de tiempo facilita el recorrido de una ruta de recuperación bifurcada (una que abarque varias bifurcaciones de recuperación). Un plan determinado de restauración a un momento dado incluye automáticamente las copias de seguridad que son pertinentes para la restauración a un momento dado de destino (fecha y hora). Para obtener más información, vea [Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
Para obtener más información sobre el Asistente para recuperación de base de datos, vea los siguientes blogs de Facilidad de uso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   [Asistente para la recuperación: Introducción](/archive/blogs/managingsql/recovery-advisor-an-introduction)  
  
-   [Asistente para la recuperación: uso de SSMS para crear o restaurar copias de seguridad de división](/archive/blogs/managingsql/recovery-advisor-using-ssms-to-createrestore-split-backups)  

## <a name="accelerated-database-recovery"></a><a name="adr"></a> Recuperación acelerada de bases de datos
La [recuperación acelerada de bases de datos](/azure/sql-database/sql-database-accelerated-database-recovery/) está disponible en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. La recuperación acelerada de bases de datos mejora considerablemente la disponibilidad de la base de datos, especialmente en presencia de transacciones de larga duración, al volver a diseñar el [proceso de recuperación](#TlogAndRecovery) de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Una base de datos para la que se ha habilitado la recuperación acelerada de base de datos completa el proceso de recuperación más rápidamente después de una conmutación por error u otro apagado no limpio. Cuando está habilitada, la recuperación acelerada de base de datos también completa la reversión de las transacciones de larga ejecución canceladas significativamente más rápido.

Se puede habilitar la recuperación acelerada de bases de datos en cada base de datos en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] mediante la sintaxis siguiente:

```sql
ALTER DATABASE <db_name> SET ACCELERATED_DATABASE_RECOVERY = ON;
```

> [!NOTE]
> La recuperación acelerada de base de datos está habilitada de forma predeterminada en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

## <a name="see-also"></a><a name="RelatedContent"></a> Consulte también  
 [Información general de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)      
 [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)     
 [Guía de arquitectura y administración de registros de transacciones de SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)     
 [Realizar copias de seguridad y restaurar bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)     
 [Aplicar copias de seguridad de registros de transacción (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)