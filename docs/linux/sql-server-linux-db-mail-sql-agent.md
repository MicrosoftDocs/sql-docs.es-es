---
title: Correo electrónico de BD y alertas por correo electrónico con el Agente SQL en Linux
description: Obtenga información sobre cómo usar el correo electrónico de BD y cómo configurar las alertas por correo electrónico con el Agente SQL Server (mssql-server-agent) en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: tbd
ms.openlocfilehash: 882ab5c8bff2acd7c93cc8cbeeba135152ce76ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348172"
---
# <a name="db-mail-and-email-alerts-with-sql-agent-on-linux"></a>Correo electrónico de BD y alertas por correo electrónico con el Agente SQL en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En los pasos siguientes se muestra cómo configurar el correo electrónico de BD y usarlo con el Agente SQL Server (**mssql-server-agent**) en Linux. 

## <a name="1-enable-db-mail"></a>1. Habilitación del correo electrónico de BD

```sql
USE master 
GO 
sp_configure 'show advanced options',1 
GO 
RECONFIGURE WITH OVERRIDE 
GO 
sp_configure 'Database Mail XPs', 1 
GO 
RECONFIGURE  
GO  
```

## <a name="2-create-a-new-account"></a>2. Creación de una nueva cuenta
```sql
EXECUTE msdb.dbo.sysmail_add_account_sp 
@account_name = 'SQLAlerts', 
@description = 'Account for Automated DBA Notifications', 
@email_address = 'sqlagenttest@gmail.com', 
@replyto_address = 'sqlagenttest@gmail.com', 
@display_name = 'SQL Agent', 
@mailserver_name = 'smtp.gmail.com', 
@port = 587, 
@enable_ssl = 1, 
@username = 'sqlagenttest@gmail.com', 
@password = '<password>' 
GO
```

## <a name="3-create-a-default-profile"></a>3. Creación de un perfil predeterminado

```sql
EXECUTE msdb.dbo.sysmail_add_profile_sp 
@profile_name = 'default', 
@description = 'Profile for sending Automated DBA Notifications' 
GO
```

## <a name="4-add-the-database-mail-account-to-a-database-mail-profile"></a>4. Adición de la cuenta de Correo electrónico de base de datos al perfil de Correo electrónico de base de datos
```sql
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp 
@profile_name = 'default', 
@principal_name = 'public', 
@is_default = 1 ; 
 ```
 
## <a name="5-add-account-to-profile"></a>5. Adición de la cuenta al perfil 
```sql
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp   
@profile_name = 'default',   
@account_name = 'SQLAlerts',   
@sequence_number = 1;  
 ```
 
## <a name="6-send-test-email"></a>6. Envío de un correo electrónico de prueba
> [!NOTE]
> Es posible que tenga que ir al cliente de correo electrónico y habilitar la opción "permitir que los clientes menos seguros envíen correo". No todos los clientes reconocen el correo electrónico de BD como demonio de correo electrónico.

```
EXECUTE msdb.dbo.sp_send_dbmail 
@profile_name = 'default', 
@recipients = 'recipient-email@gmail.com', 
@Subject = 'Testing DBMail', 
@Body = 'This message is a test for DBMail' 
GO
```

## <a name="7-set-db-mail-profile-using-mssql-conf-or-environment-variable"></a>7. Establecimiento del perfil de correo electrónico de BD mediante mssql-conf o la variable de entorno
Puede usar la utilidad mssql-conf o las variables de entorno para registrar el perfil de correo electrónico de BD. En este caso, llamaremos a nuestro perfil predeterminado.

```bash
# via mssql-conf
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile default
# via environment variable
MSSQL_AGENT_EMAIL_PROFILE=default
```

## <a name="8-set-up-an-operator-for-sqlagent-job-notifications"></a>8. Configuración de un operador para las notificaciones del trabajo SQLAgent 

```sql
EXEC msdb.dbo.sp_add_operator 
@name=N'JobAdmins',  
@enabled=1, 
@email_address=N'recipient-email@gmail.com',  
@category_name=N'[Uncategorized]' 
GO 
```

## <a name="9-send-email-when-agent-test-job-succeeds"></a>9. Envío de un correo electrónico cuando el "Trabajo de prueba del agente" se ejecute correctamente 

```
EXEC msdb.dbo.sp_update_job 
@job_name='Agent Test Job', 
@notify_level_email=1, 
@notify_email_operator_name=N'JobAdmins' 
GO
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre cómo usar el Agente SQL Server para crear, programar y ejecutar trabajos, vea [Ejecución de un trabajo del Agente SQL Server en Linux](sql-server-linux-run-sql-server-agent-job.md).
