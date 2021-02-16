---
description: Errores comunes del Correo electrónico de base de datos
title: Errores comunes del Correo electrónico de base de datos | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0df7b437ff897966502291975a99d56a64003f08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272366"
---
# <a name="common-errors-with-database-mail"></a>Errores comunes del Correo electrónico de base de datos 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

En este artículo se describen algunos errores comunes detectados en el Correo electrónico de base de datos y las formas de resolverlos.

## <a name="could-not-find-stored-procedure-sp_send_dbmail"></a>No se encontró el procedimiento almacenado 'sp_send_dbmail'
El procedimiento almacenado [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md) está instalado en la base de datos msdb. Debe ejecutar **sp_send_dbmail** desde la base de datos msdb, o bien especificar un nombre de tres partes para el procedimiento almacenado.

Ejemplo:
```sql
EXEC msdb.dbo.sp_send_dbmail ...
```

O:

```sql
USE msdb;
GO
EXEC dbo.sp_send_dbmail ...
```

Utilice el [Asistente para configuración de Correo electrónico de base de datos](configure-database-mail.md) para habilitar y configurar el Correo electrónico de base de datos.

## <a name="profile-not-valid"></a>Perfil no válido
Este mensaje tiene dos causas posibles: El perfil especificado no existe o el usuario que ejecuta [sp_send_dbmail (Transact-SQL)](../system-stored-procedures/sp-send-dbmail-transact-sql.md) no tiene permiso para tener acceso al perfil.

Para comprobar los permisos de un perfil, ejecute el procedimiento almacenado [sysmail_help_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) con el nombre del perfil. Use el procedimiento almacenado [sysmail_add_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) o el [Asistente para configuración de Correo electrónico de base de datos](configure-database-mail.md) para conceder permiso a un usuario o grupo de msdb para tener acceso a un perfil.

## <a name="permission-denied-on-sp_send_dbmail"></a>Permiso denegado para sp_send_dbmail

En este tema se describe cómo solucionar un mensaje de error en el que se indica que el usuario que intenta enviar mensajes del Correo electrónico de base de datos no tiene permiso para ejecutar sp_send_dbmail.

El texto del mensaje de error es el siguiente:

```
EXECUTE permission denied on object 'sp_send_dbmail', 
database 'msdb', schema 'dbo'.
```

Para enviar mensajes del Correo electrónico de base de datos, se debe ser usuario de la base de datos msdb y miembro del rol de base de datos DatabaseMailUserRole en la base de datos msdb. Para agregar usuarios o grupos de msdb a este rol, utilice SQL Server Management Studio o ejecute la instrucción que se indica a continuación para el usuario o el rol que necesita enviar mensajes del Correo electrónico de base de datos.

```sql
EXEC msdb.dbo.sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<user or role name>';
GO
```
Para obtener más información, vea [sp_addrolemember](../system-stored-procedures/sp-addrolemember-transact-sql.md) y [sp_droprolemember](../system-stored-procedures/sp-droprolemember-transact-sql.md).

## <a name="database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log"></a>Correo electrónico de base de datos en cola, sin entradas en sysmail_event_log o en el registro de eventos de aplicación Windows 

El Correo electrónico de base de datos depende de Service Broker para poner en cola los mensajes de correo electrónico. Si el Correo electrónico de base de datos se detiene o si la entrega de mensajes de Service Broker no está activada en la base de datos **msdb**, los mensajes se ponen en cola, pero no se pueden entregar. En ese caso, los mensajes de Service Broker permanecen en la cola del correo de Service Broker. Service Broker no activa el programa externo, por lo que no hay entradas de registro en **sysmail_event_log** ni actualizaciones del estado del elemento en **sysmail_allitems**.

Ejecute la siguiente instrucción para comprobar si Service Broker está habilitado en la base de datos **msdb**:

```sql
SELECT is_broker_enabled FROM sys.databases WHERE name = 'msdb';
```

El valor 0 indica que la entrega de mensajes de Service Broker no está activada en la base de datos msdb. Para corregir el problema, active Service Broker en la base de datos con el siguiente comando de Transact-SQL:

```sql
USE master ;
GO

ALTER DATABASE msdb SET ENABLE_BROKER ;
GO
``` 

El Correo electrónico de base de datos se basa en una serie de procedimientos almacenados internos. Para reducir el área expuesta, los procedimientos almacenados están deshabilitados en una nueva instalación de SQL Server. Para habilitar estos procedimientos almacenados, use la [opción Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) del procedimiento almacenado del sistema **sp_configure**, como en el siguiente ejemplo:

```sql
EXEC sp_configure 'show advanced options', 1;  
RECONFIGURE;
EXEC sp_configure 'Database Mail XPs', 1;  
RECONFIGURE  
GO  

Database Mail may be stopped in the **msdb** database. To check status of Database Mail, execute the following statement:

```sql
EXECUTE dbo.sysmail_help_status_sp;
```

Para iniciar el Correo electrónico de base de datos en una base de datos host de correo, ejecute el siguiente comando en la base de datos msdb:

```sql
EXECUTE dbo.sysmail_start_sp;
```

Service Broker examina la duración del diálogo de los mensajes al activarse; por tanto, los mensajes que hayan estado en la cola de transmisión de Service Broker durante más tiempo que el especificado para la duración configurada del diálogo producen un error inmediato. El Correo electrónico de base de datos actualiza el estado de los mensajes con error en [sysmail_allitems](../system-catalog-views/sysmail-allitems-transact-sql.md) y las vistas relacionadas. Debe decidir si los mensajes se deben enviar de nuevo. Para obtener más información sobre cómo configurar la duración del diálogo que el Correo electrónico de base de datos utiliza, vea [sysmail_configure_sp (Transact-SQL)](../system-stored-procedures/sysmail-configure-sp-transact-sql.md).



##  <a name="see-also"></a><a name="RelatedContent"></a> Vea también
  
-  [Correo electrónico de base de datos](database-mail.md)
-  [Crear un perfil de Correo electrónico de base de datos](create-a-database-mail-profile.md)
  
  
