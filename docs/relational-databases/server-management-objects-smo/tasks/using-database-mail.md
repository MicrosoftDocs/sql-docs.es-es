---
description: Utilizar el correo electrónico de base de datos
title: Usar Correo electrónico de base de datos | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- e-mail [SMO]
- Database Mail [SMO]
- mail [SMO]
ms.assetid: 7605390f-b485-48cc-8d97-e364a066067b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 983524f042465efff6a39522466022e34849b74e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475406"
---
# <a name="using-database-mail"></a>Utilizar el correo electrónico de base de datos
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  En SMO, el objeto <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> referenciado por la propiedad <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> representa el subsistema del correo electrónico de base de datos. Mediante el objeto <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> de SMO, puede configurar el subsistema del correo electrónico de base de datos y administrar los perfiles y cuentas de correo. El <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> objeto SMO pertenece al objeto **Server** , lo que significa que el ámbito de las cuentas de correo está en el nivel de servidor.  
  
## <a name="examples"></a>Ejemplos  
 Para utilizar cualquier ejemplo de código que se proporcione, deberá elegir el entorno de programación, la plantilla de programación y el lenguaje de programación con los que crear su aplicación. Para obtener más información, vea [crear un proyecto de Visual C&#35; SMO en Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
 Para los programas que utilizan el Correo electrónico de base de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , debe incluir la instrucción **Imports** para calificar el espacio de nombres Mail. Inserte la instrucción después de las demás instrucciones **Imports** , antes de cualquier declaración de la aplicación, como:  
  
 `Imports Microsoft.SqlServer.Management.Smo`  
  
 `Imports Microsoft.SqlServer.Management.Common`  
  
 `Imports Microsoft.SqlServer.Management.Smo.Mail`  
  
## <a name="creating-a-database-mail-account-by-using-visual-basic"></a>Crear una cuenta de correo electrónico de base de datos utilizando Visual Basic  
 En este ejemplo de código se muestra cómo crear una cuenta de correo electrónico en SMO. El Correo electrónico de base de datos está representado por el objeto <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> y está referenciado por la propiedad <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> del objeto <xref:Microsoft.SqlServer.Management.Smo.Server>. SMO puede utilizarse para configurar mediante programación el Correo electrónico de base de datos, pero no puede usarse para enviar o administrar el correo electrónico recibido.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server()
'Define the Database Mail service with a SqlMail object variable and reference it using the Server Mail property.
Dim sm As SqlMail
sm = srv.Mail
'Define and create a mail account by supplying the Database Mail service, name, description, display name, and email address arguments in the constructor.
Dim a As MailAccount
a = New MailAccount(sm, "AdventureWorks Administrator", "AdventureWorks Automated Mailer", "Mail account for administrative e-mail.", "dba@Adventure-Works.com")
a.Create()
```
  
## <a name="creating-a-database-mail-account-by-using-visual-c"></a>Crear una cuenta de correo electrónico de base de datos utilizando Visual C#  
 En este ejemplo de código se muestra cómo crear una cuenta de correo electrónico en SMO. El Correo electrónico de base de datos está representado por el objeto <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> y está referenciado por la propiedad <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> del objeto <xref:Microsoft.SqlServer.Management.Smo.Server>. SMO puede utilizarse para configurar mediante programación el Correo electrónico de base de datos, pero no puede usarse para enviar o administrar el correo electrónico recibido.  
  
```csharp  
{  
         //Connect to the local, default instance of SQL Server.  
         Server srv = default(Server);   
           srv = new Server();   
           //Define the Database Mail service with a SqlMail object variable   
           //and reference it using the Server Mail property.   
           SqlMail sm;   
           sm = srv.Mail;   
           //Define and create a mail account by supplying the Database Mail  
           //service, name, description, display name, and email address  
           //arguments in the constructor.   
           MailAccount a = default(MailAccount);   
           a = new MailAccount(sm, "AdventureWorks2012 Administrator", "AdventureWorks2012 Automated Mailer", "Mail account for administrative e-mail.", "dba@Adventure-Works.com");   
           a.Create();    
}  
```  
  
## <a name="creating-a-database-mail-account-by-using-powershell"></a>Crear una cuenta de correo electrónico de base de datos utilizando PowerShell  
 En este ejemplo de código se muestra cómo crear una cuenta de correo electrónico en SMO. El Correo electrónico de base de datos está representado por el objeto <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> y está referenciado por la propiedad <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> del objeto <xref:Microsoft.SqlServer.Management.Smo.Server>. SMO puede utilizarse para configurar mediante programación el Correo electrónico de base de datos, pero no puede usarse para enviar o administrar el correo electrónico recibido.  
  
  
  
```powershell  
#Connect to the local, default instance of SQL Server.  
  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#Define the Database Mail; reference it using the Server Mail property.  
$sm = $srv.Mail  
  
#Define and create a mail account by supplying the Database Mail service,  
#name, description, display name, and email address arguments in the constructor.  
$a = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Mail.MailAccount -argumentlist $sm, `  
"Adventure Works Administrator", "Adventure Works Automated Mailer",`  
 "Mail account for administrative e-mail.", "dba@Adventure-Works.com"  
$a.Create()  
```  
  
  
