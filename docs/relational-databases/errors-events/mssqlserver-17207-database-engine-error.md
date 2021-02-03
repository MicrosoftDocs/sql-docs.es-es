---
description: MSSQLSERVER_17207
title: MSSQLSERVER_17207
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17204 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: fb3d8d885345c30e432f25f005969664ab514fa6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196707"
---
# <a name="mssqlserver_17207"></a>MSSQLSERVER_17207
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |
| :-------- | :---- |
|Nombre de producto|SQL Server|  
|Id. de evento|17207|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBLKIO_OS2DISKERROR|  
|Texto del mensaje|%ls: error del sistema operativo %ls al crear o abrir el archivo "%ls". Diagnostique y corrija el error del sistema operativo y vuelva a intentar realizar la operación.|  


## <a name="explanation"></a>Explicación  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no pudo abrir el archivo especificado debido al error del sistema operativo especificado.  

Es posible que vea el error 17207 en el evento de aplicaciones Windows o en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede abrir una base de datos o archivos de registro de transacciones. Este es un ejemplo de cómo podría ser el error.

``` 
Error: 17207, Severity: 16, State: 1.
FileMgr::StartSecondaryDataFiles: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'F:\MSSQL\DATA\MyDB_FG1_1.ndf'. Diagnose and correct the operating system error, and retry the operation.
```

Puede ver estos errores durante el proceso de inicio de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o cualquier operación de base de datos que intente iniciar la base de datos (por ejemplo, ALTER DATABASE). En algunos escenarios, puede ver los errores 17207 y 17204 y, en otras ocasiones, puede que solo vea uno de ellos.

Si una base de datos de usuario se ejecuta con estos errores, se quedará en el estado RECOVERY_PENDING y las aplicaciones no podrán acceder a la base de datos. Si una base de datos del sistema encuentra estos errores, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se iniciará y no podrá conectarse a esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esto también podría dar lugar a que un recurso de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se quede sin conexión.

Si el problema está relacionado con el grupo de archivos de secuencia de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], observará que solo aparece la ruta de acceso completa al directorio en lugar de un nombre de archivo. A continuación se muestra un ejemplo: 
```
Error: 17207, Severity: 16, State: 1.
STREAMFCB::Startup: Operating system error 2(The system cannot find the file specified.) occurred while creating or opening file 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\bpa_files_test_fs_1\bpa_files_test_fs_1'. Diagnose and correct the operating system error, and retry the operation.
```

## <a name="cause"></a>Causa
Antes de que se pueda usar cualquier base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], es necesario iniciar la base de datos. El proceso de inicio de la base de datos implica inicializar diversas estructuras de datos que representan la base de datos y los archivos de base de datos, abrir todos los archivos que pertenecen a la base de datos y, por último, ejecutar la recuperación en la base de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza la función de la API de Windows [CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea) para abrir los archivos que pertenecen a una base de datos.
 
Los mensajes 17207 y 17204 indican que se ha producido un error mientras [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intentaba abrir los archivos de base de datos durante el proceso de inicio.
 
Estos mensajes de error contienen la siguiente información:
1. Nombre de la función de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que intenta abrir el archivo. El nombre de la función que se observa normalmente en estos mensajes de error es uno de los siguientes:
   - FCB::Open: el archivo ha detectado un error cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta abrirlo.
   - FileMgr::StartPrimaryDataFiles: un archivo de datos principal o un archivo perteneciente al grupo de archivos principal.
   - FileMgr::StartSecondaryDataFiles: un archivo perteneciente a un grupo de archivos secundario.
   - FileMgr::StartLogFiles: un archivo de registro de transacciones.
   - STREAMFCB::Startup: contenedor de SQL FileStream.
   - FCB::RemoveAlternateStreams
  
      
1. La información de estado distingue varias ubicaciones dentro de una función que puede generar este mensaje de error.
1. La ruta de acceso física completa del archivo.
1. El id. de archivo correspondiente al archivo.
1. El código de error del sistema operativo y la descripción del error. En algunos casos, solo verá el código de error.
 
La información de error del sistema operativo que se imprime en estos mensajes de error es la causa principal del error 17204. Las causas comunes de estos mensajes de error son un problema de permisos o una ruta de acceso incorrecta al archivo.


## <a name="user-action"></a>Acción del usuario  
1. La resolución del error 17207 implica la comprensión del código de error del sistema operativo asociado y el diagnóstico del error. Una vez que se resuelva la condición de error del sistema operativo, puede intentar reiniciar la base de datos (con ALTER DATABASE SET ONLINE, por ejemplo) o la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para poner en línea la base de datos afectada. En algunos casos, es posible que no pueda resolver el error del sistema operativo, por lo que tendrá que tomar medidas correctivas específicas. Trataremos estas acciones en esta sección.
1. Si el mensaje de error 17207 solo contiene un código de error y no una descripción del error, puede intentar resolver el código de error mediante el comando desde un shell del sistema operativo: net helpmsg <error code>. Si obtiene un código de estado de 8 dígitos como código de error, puede hacer referencia a las fuentes de información como [¿Cómo convertir un valor HRESULT en un código de error de Win32?](https://devblogs.microsoft.com/oldnewthing/20061103-07/?p=29133) para descodificar los códigos de estado en errores del sistema operativo.
1. Si obtiene el error ```Access is Denied``` del sistema operativo = 5, tenga en cuenta estos métodos:
   -  Para comprobar los permisos que se establecen en el archivo, examine las propiedades del archivo en el Explorador de Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa grupos de Windows para aprovisionar el control de acceso en los distintos recursos de archivo. Asegúrese de que el grupo adecuado (con nombres como SQLServerMSSQLUser$ComputerName$MSSQLSERVER o SQLServerMSSQLUser$ComputerName$InstanceName) tiene los permisos necesarios en el archivo de base de datos mencionado en el mensaje de error. Vea [Configurar permisos del sistema de archivos para el acceso al motor de base de datos](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true) para obtener más información. Asegúrese de que el grupo de Windows incluye realmente la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el identificador de seguridad del servicio.
   -  Revise la cuenta de usuario en la que se ejecuta actualmente el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede usar el administrador de tareas de Windows para obtener esta información. Busque el valor "Nombre de usuario" para el archivo ejecutable "sqlservr.exe". Además, si ha cambiado recientemente la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe saber que la manera admitida para realizar esta operación es usar la utilidad Administrador de configuración de SQL Server. Hay más información disponible en [Administrador de configuración de SQL Server](../sql-server-configuration-manager.md). 
   -  En función del tipo de operación (abrir bases de datos durante el inicio del servidor, adjuntar una base de datos, restaurarla, etc.) la cuenta que se usa para la suplantación y el acceso al archivo de base de datos puede variar. Revise el tema [Proteger archivos de datos y de registro](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) para comprender qué operación establece qué permiso y para qué cuentas. Use una herramienta como [Process Monitor](/sysinternals/downloads/procmon) de Windows SysInternals para saber si el acceso al archivo se produce en el contexto de seguridad de la cuenta de inicio del servicio de la instancia de SQL Server (o el identificador de seguridad del servicio) o una cuenta suplantada.

      Si SQL Server suplanta las credenciales de usuario del inicio de sesión que ejecuta la operación ALTER DATABASE o CREATE DATABASE, verá la siguiente información en la herramienta Process Monitor (ejemplo).

        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
1. Si obtiene `The system cannot find the file specified` error del sistema operativo = 3:
   - Revise la ruta de acceso completa del mensaje de error.
   - Asegúrese de que la unidad de disco y la ruta de acceso de la carpeta son visibles y accesibles desde el Explorador de Windows.
   - Revise el registro de eventos de Windows para determinar si existe algún problema con esta unidad de disco.
   - Si la ruta de acceso es incorrecta y la base de datos ya existe en el sistema, puede cambiar las rutas de acceso de los archivos de base de datos mediante los métodos que se explican en el artículo [Movimiento de archivos de base de datos](../databases/move-database-files.md). Es posible que tenga que usar este procedimiento, especialmente para los archivos de base de datos del sistema que encuentran 17204 o 17207, y trabaja en un escenario de recuperación ante desastres en el que las unidades de disco especificadas no están disponibles. En este tema también se explica cómo puede identificar la ubicación actual de las distintas bases de datos del sistema [master, model, tempdb, msdb y mssqlsystemresource].
   - Si ve este error porque faltan los archivos de base de datos, tendrá que restaurar la base de datos a partir de una copia de seguridad válida:
     - Si el archivo de base de datos asociado al error pertenece a un grupo de archivos secundario, tiene la opción de marcar ese grupo de archivos como sin conexión, poner la base de datos en línea y, después, realizar una restauración solo de ese grupo de archivos. Para obtener más información, consulte la sección OFFLINE del tema [Opciones File y Filegroup de ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).
     - Si el archivo que ha generado el error es un archivo de registro de transacciones, revise la información de las secciones "FOR ATTACH" y "FOR ATTACH_REBUILD_LOG" del tema [CREATE DATABASE (Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md) para entender cómo puede volver a crear los archivos de registro de transacciones que faltan.
   - Asegúrese de que el disco o la ubicación de red [por ejemplo, la unidad iSCSI] está disponible antes de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intente acceder a los archivos de base de datos en estas ubicaciones. Si es necesario, cree las dependencias necesarias en el Administrador de clústeres o el Administrador de control de servicios.

1. Si recibe el error del sistema operativo = 32 `The process cannot access the file because it is being used by another process`:
   - Use una herramienta como [Process Explorer](/sysinternals/downloads/process-explorer) o [Handle](/sysinternals/downloads/handle) de Windows Sysinternals para determinar si otro proceso o servicio ha adquirido un bloqueo exclusivo en este archivo de base de datos.
   - Detenga el acceso de ese proceso a los archivos de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Algunos ejemplos comunes son los programas antivirus (vea las instrucciones sobre exclusiones de archivos en el siguiente [artículo de KB](https://support.microsoft.com/help/309422/choosing-antivirus-software-for-computers-that-run-sql-server)).
   - En un entorno de clústeres, asegúrese de que el proceso sqlservr.exe del nodo propietario anterior haya liberado realmente los identificadores de los archivos de base de datos. Normalmente esto no ocurre, pero las configuraciones incorrectas del clúster o las rutas de acceso de E/S pueden generar estos problemas.
