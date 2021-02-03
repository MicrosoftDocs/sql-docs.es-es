---
description: MSSQLSERVER_5120
title: MSSQLSERVER_5120
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5120 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: 927134b385b3164416bca61c776778081b683d33
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185562"
---
# <a name="mssqlserver_5120"></a>MSSQLSERVER_5120
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|5120|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DSK_FCB_FAILURE|  
|Texto del mensaje|Error de tabla: No se puede abrir el archivo físico "%.*ls". Error del sistema operativo %d: "%ls".|  
  
## <a name="explanation"></a>Explicación  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no pudo abrir un archivo de base de datos.  El error del sistema operativo proporcionado en el mensaje apunta a razones subyacentes más específicas para el error. Este error suele aparecer junto con otros errores como [17204](mssqlserver-17204-database-engine-error.md) o [17207](mssqlserver-17207-database-engine-error.md).
  
## <a name="user-action"></a>Acción del usuario  
  
  Diagnostique y corrija el error del sistema operativo y, después, vuelva a intentar realizar la operación. Hay varios estados que pueden ayudar a Microsoft a reducir el área del producto donde se está produciendo el error. 
  
### <a name="access-is-denied"></a>Acceso denegado 
Si obtiene el error `Access is Denied` del sistema operativo = 5, tenga en cuenta estos métodos:
   -  Compruebe los permisos que se establecen en el archivo examinando las propiedades del archivo en el Explorador de Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza grupos de Windows para aprovisionar Access Control en los distintos recursos de archivo. Asegúrese de que el grupo adecuado [con nombres como SQLServerMSSQLUser$ComputerName$MSSQLSERVER o SQLServerMSSQLUser$ComputerName$InstanceName] tiene los permisos necesarios en el archivo de base de datos mencionado en el mensaje de error. Vea [Configuración de permisos del sistema de archivos para el acceso al motor de base de datos](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true) para obtener más información. Asegúrese de que el grupo de Windows incluye realmente la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el identificador de seguridad del servicio.
   -  Revise la cuenta de usuario en la que se ejecuta actualmente el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede usar el administrador de tareas de Windows para obtener esta información. Busque el valor "Nombre de usuario" para el archivo ejecutable "sqlservr.exe". Además, si ha cambiado recientemente la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe saber que la manera admitida para realizar esta operación es usar la utilidad [Administrador de configuración de SQL Server](../sql-server-configuration-manager.md). 
   -  En función del tipo de operación (abrir bases de datos durante el inicio del servidor, adjuntar una base de datos, restaurarla, etc.) la cuenta que se usa para la suplantación y el acceso al archivo de base de datos puede variar. Revise el tema [Proteger archivos de datos y de registro](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) para comprender qué operación establece qué permiso y para qué cuentas. Use una herramienta como [Process Monitor](/sysinternals/downloads/procmon) de Windows SysInternals para saber si el acceso al archivo se produce en el contexto de seguridad de la cuenta de inicio del servicio de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o el identificador de seguridad del servicio, o una cuenta suplantada.

      Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suplanta las credenciales de usuario del inicio de sesión que ejecuta la operación ALTER DATABASE o CREATE DATABASE, verá la siguiente información en la herramienta Process Monitor (un ejemplo).
      
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
  
  
### <a name="attaching-files-that-reside-on-a-network-attached-storage"></a>Asociación de archivos que residen en almacenamiento conectado a la red  
Si no puede volver a adjuntar una base de datos que reside en almacenamiento conectado a la red, es posible que se registre un mensaje similar al siguiente en el registro de aplicaciones.

`Msg 5120, Level 16, State 101, Line 1 Unable to open the physical file "\\servername\sharename\filename.mdf". Operating system error 5: (Access is denied.).`

Este problema se produce porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restablece los permisos de archivo al desasociar la base de datos. Al intentar volver a adjuntar la base de datos, se produce un error debido a los permisos limitados de recursos compartidos.

Para solucionarlo, siga estos pasos:
1. Use la opción de inicio -T para iniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Use esta opción de inicio para activar la marca de seguimiento 1802 en el [Administrador de configuración de SQL Server](../sql-server-configuration-manager.md) (vea [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md) para obtener información sobre 1802). Para obtener más información sobre cómo cambiar los parámetros de inicio, vea [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md).

2. Use el comando siguiente para volver a adjuntar la base de datos.
   ```sql
    exec sp_detach_db DatabaseName
    go 
   ```

3. Use el siguiente comando para volver a adjuntar la base de datos.
   ```sql
   exec sp_attach_db DatabaseName, '\\Network-attached storage_Path\DatabaseMDFFile.mdf', '\\Network-attached storage_Path\DatabaseLDFFile.ldf'
   go
   ```
