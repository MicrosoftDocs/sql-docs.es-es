---
description: Registrar un servidor conectado (SQL Server Management Studio)
title: Registrar un servidor conectado
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.registerserver.f1
helpviewer_keywords:
- Registered Servers [SQL Server], register connected servers
- connected server registrations [SQL Server]
ms.assetid: 77deb5f5-0f80-484f-8b8b-29afa67ec18f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/28/2016
ms.openlocfilehash: 9f34d85e6b26a8674ed0c05788299eb568e7080a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250410"
---
# <a name="register-a-connected-server-sql-server-management-studio"></a>Registrar un servidor conectado (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo registrar un servidor conectado en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS). Mediante el registro del servidor, puede guardar la información de conexión correspondiente a los servidores a los que accede con frecuencia. Un servidor puede registrarse antes de conectarlo o cuando se conecta desde el Explorador de objetos.  Puede ver los servidores registrados en SSMS si va a **Ver**\\**Servidores registrados** en el menú.
  
 **En este tema**  
  
-   **Para registrar un servidor, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-register-a-connected-server"></a>Para registrar un servidor conectado  
  
En el Explorador de objetos, haga clic con el botón derecho en un servidor al que ya esté conectado y haga clic en **Registrar**.
  
**Nombre del servidor**  
Este campo se establece de manera predeterminada en el nombre del servidor al que se conectó.  De manera opcional, puede escribir un nombre de servidor o elegir uno en la lista desplegable.

**Autenticación**  
Están disponibles dos modos de autenticación cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

-    **Autenticación de Windows**  
El modo de autenticación de Windows permite al usuario conectarse mediante una cuenta de usuario de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. 

-    **Autenticación de SQL Server**   
Cuando un usuario se conecta con un nombre y una contraseña de inicio de sesión específicos desde una conexión que no es de confianza, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la autenticación y comprueba si se ha configurado una cuenta de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y si la contraseña especificada coincide con la que se ha almacenado anteriormente. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no tiene configurada una cuenta de inicio de sesión, la autenticación no se realizará correctamente y el usuario recibirá un mensaje de error.

     > [!IMPORTANT]  
     > [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)] Para obtener más información, vea [Elegir un modo de autenticación](../../relational-databases/security/choose-an-authentication-mode.md).  

     -    **Nombre de usuario**  
Muestra el nombre de usuario actual con el que se conecta. Esta opción de solo lectura únicamente está disponible si ha seleccionado la autenticación de Windows para conectarse. Para cambiar **Nombres de usuario**, inicie una sesión en el equipo como un usuario distinto. 

     -    **Inicio de sesión**  
Escriba el inicio de sesión con el que va a conectarse. Esta opción solo está disponible si ha seleccionado la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse.  

     -    **Contraseña**  
Escriba la contraseña del inicio de sesión. Esta opción solo puede editarse si ha seleccionado la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse. 

     -    **Recordar contraseña**  
Seleccione esta opción para que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cifre y guarde la contraseña que ha escrito. Esta opción solo aparece si ha seleccionado la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse.  

          > [!NOTE]  
          > Si ha guardado la contraseña y ya no quiere conservarla, desactive esta casilla y, luego, haga clic en **Guardar**.  

**Nombre del servidor registrado**  
El nombre que desea que aparezca en Servidores registrados. Este nombre no tiene que coincidir con el cuadro **Nombre del servidor** .  
  
**Descripción del servidor registrado**  
Escriba una descripción opcional del servidor.  
  
**Test**  
Haga clic para probar la conexión al servidor seleccionado en **Nombre del servidor**.  
  
**Guardar**  
Haga clic aquí para guardar la configuración del servidor registrado. 

## <a name="see-also"></a>Consulte también

[Crear un servidor registrado (SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md)