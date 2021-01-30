---
description: sp_notify_operator (Transact-SQL)
title: sp_notify_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_notify_operator_TSQL
- sp_notify_operator
dev_langs:
- TSQL
helpviewer_keywords:
- sp_notify_operator
ms.assetid: c440f5c9-9884-4196-b07c-55d87afb17c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 954a314760a22d9b00996dc460062fcc75af16aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192002"
---
# <a name="sp_notify_operator-transact-sql"></a>sp_notify_operator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Envía un mensaje de correo electrónico a un operador mediante el Correo electrónico de base de datos.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_notify_operator  
    [ @profile_name = ] 'profilename' ,  
    [ @id = ] id ,  
    [ @name = ] 'name' ,  
    [ @subject = ] 'subject' ,  
    [ @body = ] 'message' ,  
    [ @file_attachments = ] 'attachment'  
    [ @mail_database = ] 'mail_host_database'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @profile_name = ] 'profilename'` Nombre del perfil de Correo electrónico de base de datos que se va a utilizar para enviar el mensaje. *ProfileName* es **nvarchar (128)**. Si no se especifica *ProfileName* , se utiliza el perfil de correo electrónico de base de datos predeterminado.  
  
`[ @id = ] id` Identificador del operador al que se va a enviar el mensaje. *ID* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar uno de los *identificadores* o *nombres* .  
  
`[ @name = ] 'name'` Nombre del operador al que se va a enviar el mensaje. *Name* es de tipo **nvarchar (128)** y su valor predeterminado es NULL. Se debe especificar uno de los *identificadores* o *nombres* .  
  
> **Nota:** Debe definirse una dirección de correo electrónico para el operador antes de que pueda recibir mensajes.  
  
`[ @subject = ] 'subject'` Asunto del mensaje de correo electrónico. el *asunto* es **nvarchar (256)** y no tiene ningún valor predeterminado.  
  
`[ @body = ] 'message'` El cuerpo del mensaje de correo electrónico. *Message* es de tipo **nvarchar (Max)** y no tiene ningún valor predeterminado.  
  
`[ @file_attachments = ] 'attachment'` Nombre de un archivo que se va a adjuntar al mensaje de correo electrónico. *Attachment* es **nvarchar (512)** y no tiene ningún valor predeterminado.  
  
`[ @mail_database = ] 'mail_host_database'` Especifica el nombre de la base de datos host de correo. *mail_host_database* es **nvarchar (128)**. Si no se especifica ningún *mail_host_database* , se utiliza de forma predeterminada la base de datos **msdb** .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Envía el mensaje especificado a la dirección de correo electrónico del operador especificado. Si el operador no tiene configurada una dirección de correo electrónico, generará un error.  
  
 El Correo electrónico de base de datos y la base de datos host de correo deben configurarse antes de que se pueda enviar una notificación al operador.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestra cómo enviar una notificación de correo electrónico al operador `François Ajenstat` mediante el perfil de base de datos de correo electrónico de `AdventureWorks Administrator`. El asunto del mensaje de correo electrónico es `Test Notification`. El mensaje de correo electrónico indica que se trata de una notificación de prueba por correo electrónico.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_notify_operator  
   @profile_name = N'AdventureWorks Administrator',  
   @name = N'François Ajenstat',  
   @subject = N'Test Notification',  
   @body = N'This is a test of notification via e-mail.' ;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_help_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_delete_operator &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)  
  
  
