---
description: sysmail_update_account_sp (Transact-SQL)
title: sysmail_update_account_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_account_sp
- sysmail_update_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_account_sp
ms.assetid: ba2fdccc-5ed4-40ef-a479-79497b4d61aa
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b441dfe7aa92bd78bb87ef05b4b7f9cdd6108919
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181886"
---
# <a name="sysmail_update_account_sp-transact-sql"></a>sysmail_update_account_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia la información de una cuenta existente del Correo electrónico de base de datos.  
 
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_update_account_sp [ [ @account_id = ] account_id ] [ , ] [ [ @account_name = ] 'account_name' ] ,  
    [ @email_address = ] 'email_address' ,   
    [ @display_name = ] 'display_name' ,   
    [ @replyto_address = ] 'replyto_address' ,  
    [ @description = ] 'description' ,   
    [ @mailserver_name = ] 'server_name' ,   
    [ @mailserver_type = ] 'server_type' ,   
    [ @port = ] port_number ,   
    [ @timeout = ] 'timeout' ,  
    [ @username = ] 'username' ,  
    [ @password = ] 'password' ,  
    [ @use_default_credentials = ] use_default_credentials ,  
    [ @enable_ssl = ] enable_ssl   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @account_id = ] account_id` IDENTIFICADOR de la cuenta que se va a actualizar. *ACCOUNT_ID* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar al menos una de *ACCOUNT_ID* o *account_name* . Si se especifican los dos, el procedimiento cambia el nombre de la cuenta.  
  
`[ @account_name = ] 'account_name'` Nombre de la cuenta que se va a actualizar. *account_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar al menos una de *ACCOUNT_ID* o *account_name* . Si se especifican los dos, el procedimiento cambia el nombre de la cuenta.  
  
`[ @email_address = ] 'email_address'` La nueva dirección de correo electrónico de la que se va a enviar el mensaje. Esta dirección debe ser una dirección de correo electrónico de Internet. El nombre de servidor de la dirección es el servidor que Database Mail utiliza para enviar correo de esta cuenta. *EMAIL_ADDRESS* es de tipo **nvarchar (128)** y su valor predeterminado es NULL.  
  
`[ @display_name = ] 'display_name'` Nuevo nombre para mostrar que se va a usar en los mensajes de correo electrónico de esta cuenta. *display_name* es de tipo **nvarchar (128)** y no tiene ningún valor predeterminado.  
  
`[ @replyto_address = ] 'replyto_address'` La nueva dirección que se va a usar en el encabezado Reply-To de los mensajes de correo electrónico de esta cuenta. *replyto_address* es de tipo **nvarchar (128)** y no tiene ningún valor predeterminado.  
  
`[ @description = ] 'description'` La nueva descripción de la cuenta. la *Descripción* es de tipo **nvarchar (256)** y su valor predeterminado es NULL.  
  
`[ @mailserver_name = ] 'server_name'` El nuevo nombre del servidor de correo SMTP que se va a usar para esta cuenta. El equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ser capaz de resolver la *SERVER_NAME* en una dirección IP. *SERVER_NAME* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @mailserver_type = ] 'server_type'` El nuevo tipo del servidor de correo. *server_type* es de **tipo sysname** y no tiene ningún valor predeterminado. Solo se admite un valor de **' SMTP '** .  
  
`[ @port = ] port_number` El nuevo número de puerto del servidor de correo. *port_number* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @timeout = ] 'timeout'` Parámetro timeout para SmtpClient. Send de un único mensaje de correo electrónico. El *tiempo de espera* es de **tipo int** en segundos y no tiene ningún valor predeterminado.  
  
`[ @username = ] 'username'` El nuevo nombre de usuario que se utilizará para iniciar sesión en el servidor de correo. *El nombre de usuario* es **sysname** y no tiene ningún valor predeterminado.  
  
`[ @password = ] 'password'` Nueva contraseña que se va a usar para iniciar sesión en el servidor de correo. *password* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @use_default_credentials = ] use_default_credentials` Especifica si se debe enviar el correo al servidor SMTP con las credenciales del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] servicio. **use_default_credentials** es de bits y no tiene ningún valor predeterminado. Si el valor de este parámetro es 1, el Correo electrónico de base de datos usa las credenciales de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cuando este parámetro es 0, Correo electrónico de base de datos usa el **\@ nombre de usuario** y la **\@ contraseña** para la autenticación en el servidor SMTP. Si el **\@ nombre de usuario** y la **\@ contraseña** son NULL, se usará la autenticación anónima. Consulte con el administrador de SMTP antes de especificar este parámetro.  
  
`[ @enable_ssl = ] enable_ssl` Especifica si Correo electrónico de base de datos cifra la comunicación mediante la seguridad de la capa de transporte (TLS), conocida anteriormente como Capa de sockets seguros (SSL). Utilice esta opción si se requiere TLS en el servidor SMTP. **enable_ssl** es de bits y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Si se especifican el nombre y el Id. de cuenta, el procedimiento almacenado cambia el nombre de la cuenta además de actualizar su información. Cambiar el nombre de la cuenta puede ser útil para corregir errores en el nombre.  
  
 El procedimiento almacenado **sysmail_update_account_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-changing-the-information-for-an-account"></a>A. Cambio de la información de una cuenta  
 En el ejemplo siguiente se actualiza la cuenta `AdventureWorks Administrator` en la base de datos **msdb** . La información de la cuenta se establece con los valores proporcionados.  
  
```  
EXECUTE msdb.dbo.sysmail_update_account_sp  
     @account_name = 'AdventureWorks Administrator'  
    ,@description = 'Mail account for administrative e-mail.'  
    ,@email_address = 'dba@Adventure-Works.com'  
    ,@display_name = 'AdventureWorks Automated Mailer'  
    ,@replyto_address = NULL  
    ,@mailserver_name = 'smtp.Adventure-Works.com'  
    ,@mailserver_type = 'SMTP'  
    ,@port = 25  
    ,@timeout = 60  
    ,@username = NULL  
    ,@password = NULL  
    ,@use_default_credentials = 0  
    ,@enable_ssl = 0;  
```  
  
### <a name="b-changing-the-name-of-an-account-and-the-information-for-an-account"></a>B. Cambio del nombre de una cuenta y de la información de una cuenta  
 En el ejemplo siguiente se cambia el nombre y se actualiza la información de la cuenta con el Id. de cuenta `125`. El nuevo nombre de la cuenta es `Backup Mail Server`.  
  
```  
EXECUTE msdb.dbo.sysmail_update_account_sp  
     @account_id = 125  
    ,@account_name = 'Backup Mail Server'  
    ,@description = 'Mail account for administrative e-mail.'  
    ,@email_address = 'dba@Adventure-Works.com'  
    ,@display_name = 'AdventureWorks Automated Mailer'  
    ,@replyto_address = NULL  
    ,@mailserver_name = 'smtp-backup.Adventure-Works.com'  
    ,@mailserver_type = 'SMTP'  
    ,@port = 25  
    ,@timeout = 60  
    ,@username = NULL  
    ,@password = NULL  
    ,@use_default_credentials = 0  
    ,@enable_ssl = 0;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Creación de una cuenta de Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
