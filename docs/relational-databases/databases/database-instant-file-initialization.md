---
title: Inicialización instantánea de archivos de la base de datos
description: Obtenga información sobre la inicialización instantánea de archivos y cómo habilitar esta opción en la base de datos de SQL Server.
ms.custom: contperf-fy20q4
ms.date: 07/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- initializing files [SQL Server]
- instant file initialization [SQL Server]
- fast file initialization [SQL Server]
- file initialization [SQL Server]
- IFI [SQL Server]
- database instant file initialization [SQL Server]
ms.assetid: 1ad468f5-4f75-480b-aac6-0b01b048bd67
author: stevestein
ms.author: sstein
ms.openlocfilehash: 529949c2bf37526eba6d9042322a658f89880a9d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237368"
---
# <a name="database-instant-file-initialization"></a>Inicialización instantánea de archivos de la base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
En este artículo, obtendrá información sobre la inicialización instantánea de archivos y cómo habilitar esta opción para acelerar el crecimiento de los archivos de base de datos de SQL Server.  

De forma predeterminada, los archivos de datos y registro se inicializan para sobrescribir los datos existentes que los archivos eliminados anteriormente hayan dejado en el disco. Los archivos de datos y registro se inicializan por primera vez llenando los archivos con ceros al realizar las operaciones siguientes:  
  
- Crear una base de datos.  
- Agregue archivos de registro o datos a una base de datos existente.  
- Aumentar el tamaño de un archivo existente (incluidas las operaciones de crecimiento automático).  
- Restaurar una base de datos o un grupo de archivos.  

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la inicialización instantánea de archivos (IFI) permite una ejecución más rápida de las operaciones de archivo mencionadas anteriormente, ya que recupera el espacio en disco usado sin llenar el espacio con ceros. En lugar de eso, el contenido del disco se sobrescribe al escribir nuevos datos en los archivos. Los archivos de registro no se pueden inicializar de forma instantánea.


## <a name="enable-instant-file-initialization"></a>Habilitación de la inicialización instantánea de archivos

La inicialización instantánea de archivos solo está disponible si se ha concedido *SE_MANAGE_VOLUME_NAME* a la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los miembros del grupo de administradores de Windows tienen este derecho y pueden concederlo a otros usuarios añadiéndolos a la directiva de seguridad **Realizar tareas de mantenimiento del volumen** .  
> [!IMPORTANT]
> Ciertos usos de la característica, como el [Cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md), pueden evitar la inicialización instantánea de archivos.  

> [!NOTE]
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], este permiso se puede conceder a la cuenta de servicio durante la instalación. <br><br>Si usa la [instalación de línea de comandos](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md), agregue el argumento /SQLSVCINSTANTFILEINIT o active la casilla *Conceder el privilegio de realización de tareas de mantenimiento de volumen al servicio del motor de la base de datos de SQL Server* en el [asistente para la instalación](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).
  
Para conceder a una cuenta el permiso `Perform volume maintenance tasks` :  
  
1.  En el equipo en el que se creará el archivo de datos, abra la aplicación **Directiva de seguridad local** (`secpol.msc`).  
  
1.  En el panel izquierdo, expanda **Directivas locales** y, a continuación, haga clic en **Asignación de derechos de usuario**.  
  
1.  En el panel derecho, haga doble clic en **Realizar tareas de mantenimiento del volumen**.  
  
1.  Haga clic en **Agregar usuario o grupo** y agregue la cuenta que ejecuta el servicio SQL Server.  
  
1.  Haga clic en **Aplicar** y, a continuación, cierre todos los cuadros de diálogo de **Directiva de seguridad local** .  

1. Reinicie el servicio SQL Server.

1. Compruebe el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el inicio.
   
  
    **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores).
    1. Si la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene concedido *SE_MANAGE_VOLUME_NAME*, se registra un mensaje informativo que es similar al siguiente:

        `Database Instant File Initialization: enabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`

    1. Si la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **no** tiene concedido *SE_MANAGE_VOLUME_NAME*, se registra un mensaje informativo que es similar al siguiente:

        `Database Instant File Initialization: disabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`
    > [!NOTE]
    > También se puede usar la columna *instant_file_initialization_enabled* en el DMV [sys.dm_server_services](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md) para identificar si la inicialización instantánea de archivos está habilitada.

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Se recomienda habilitar la inicialización instantánea de archivos, ya que las ventajas pueden compensar el riesgo de seguridad.

Al usar la inicialización instantánea de archivos, el contenido del disco eliminado solo se sobrescribe cuando se escriben datos nuevos en los archivos. Por este motivo, una entidad de seguridad no autorizada puede acceder al contenido eliminado hasta que se escriban otros datos en una área específica del archivo de datos.

Mientras el archivo de la base de datos se adjunte a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este riesgo de divulgación de la información se reduce mediante la lista de control de acceso discrecional (DACL) del archivo. Esta DACL permite acceder al archivo solo a la cuenta de servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y al administrador local. Sin embargo, cuando el archivo está separado, un usuario o servicio que no tenga *SE_MANAGE_VOLUME_NAME* puede acceder a él.

Existen consideraciones similares cuando:

* *Se realiza una copia de seguridad de la base de datos*. Si el archivo de copia de seguridad no está protegido con una DACL adecuada, el contenido eliminado puede estar disponible para un usuario o servicio no autorizado.  

* *Un archivo se aumenta con IFI*. Un administrador de SQL Server podría acceder a los contenidos de la página sin procesar y ver el contenido que se ha eliminado anteriormente.

* *Los archivos de la base de datos se hospedan en una red de área de almacenamiento*. También es posible que esta red de área de almacenamiento presente siempre nuevas páginas como inicializadas previamente y que configurar el sistema operativo para que vuelva a inicializar las páginas cree una sobrecarga innecesaria.

Si le preocupa la posibilidad de que se divulgue contenido eliminado, realice una de las acciones siguientes o ambas:  
  
- Asegúrese siempre de que los archivos separados y los archivos de copia de seguridad tienen DACL restrictivas.  
- Deshabilite la inicialización instantánea de archivos para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    Para ello, revoque *SE_MANAGE_VOLUME_NAME* de la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
    
    > [!NOTE]
    > Si se deshabilita, aumentará el tiempo de asignación de los archivos de datos y solo afecta a los archivos que se han creado o se ha aumentado su tamaño después de que el derecho del usuario se revocara.
  
### <a name="se_manage_volume_name-user-right"></a>Derecho de usuario SE_MANAGE_VOLUME_NAME

El privilegio de usuario *SE_MANAGE_VOLUME_NAME* se puede asignar en el applet **Directiva de seguridad local** de **Herramientas administrativas de Windows**. En **Directivas locales**, seleccione **Asignación de derechos de usuario** y modifique la propiedad **Realizar tareas de mantenimiento del volumen**.

## <a name="performance-considerations"></a>Consideraciones de rendimiento

El proceso de inicialización de archivos de base de datos escribe ceros en las nuevas regiones del archivo durante la inicialización. La duración de este proceso depende del tamaño de la parte del archivo que se inicialice y del tiempo de respuesta y la capacidad del sistema de almacenamiento. Si la inicialización tarda mucho, es posible que vea que se registran los mensajes siguientes en el registro de errores de SQL Server y en el registro de aplicaciones.

```
Msg 5144
Autogrow of file '%.*ls' in database '%.*ls' was cancelled by user or timed out after %d milliseconds.  Use ALTER DATABASE to set a smaller FILEGROWTH value for this file or to explicitly set a new file size.
```

```
Msg 5145
Autogrow of file '%.*ls' in database '%.*ls' took %d milliseconds.  Consider using ALTER DATABASE to set a smaller FILEGROWTH for this file.
```

Un crecimiento automático prolongado de un archivo de base de datos o de registro de transacciones puede producir problemas de rendimiento de las consultas. Esto se debe a que una operación en la que se necesita el crecimiento automático de un archivo conservará recursos como bloqueos o bloqueos temporales mientras dure la operación de crecimiento del archivo. Es posible que vea esperas largas en bloqueos temporales para las páginas de asignación. La operación que necesita el crecimiento automático prolongado mostrará un tipo de espera de PREEMPTIVE_OS_WRITEFILEGATHER.





## <a name="see-also"></a>Consulte también  
 [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md)