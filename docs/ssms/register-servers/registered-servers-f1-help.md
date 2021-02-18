---
description: Servidores registrados (Ayuda F1)
title: Servidores registrados (Ayuda F1)
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], Help
- Registered Servers [SQL Server], help
- SQL Server Management Studio Help [SQL Server], registered servers
ms.assetid: 59f76b28-ba78-4a1a-b5d5-8b581f30114d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0d4b3a2e75a00e3688ec53c5e64e6ab7727375a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341042"
---
# <a name="registered-servers-f1-help"></a>Servidores registrados (Ayuda F1)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Esta sección contiene la Ayuda F1 del componente Servidores registrados de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Describa las distintas opciones.
  
 Para información sobre Servidores registrados y obtener vínculos a todo lo que puede hacer con ellos, vaya al tema [Servidores registrados](./register-servers.md) . 
 

 Haga clic en esta opción para guardar la configuración de Servidores registrados. 
 
 ## <a name="reporting-services-new-or-edit-server-registration-general-tab"></a>Nuevo o Editar el registro de servidores (pestaña General) de Reporting Services 
  Utilice esta pestaña para especificar opciones cuando registre una instancia de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Para obtener acceso a esta página, en Servidores registrados, haga clic en **Reporting Services** , en la barra de herramientas de **Servidores registrados** haga clic con el botón derecho en cualquier grupo de servidores registrados como, por ejemplo, **Reporting Services**, seleccione **Nuevo** y haga clic en **Registro de servidor**.  
  
### <a name="options"></a>Opciones  
 **Tipo de servidor**  
 Cuando se registra un servidor desde Servidores registrados, el cuadro **Tipo de servidor** es de solo lectura y coincide con el tipo de servidor que se muestra en el panel **Servidores registrados** . Para registrar un tipo de servidor diferente, haga clic en el servidor que desee en la barra de herramientas de **Servidores registrados** antes de comenzar a registrar un nuevo servidor.  
  
 **Nombre del servidor**  
 Especifique la instancia de servidor de informes a la que se va a conectar. En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puede tener acceso al servidor de informes a través de su nombre de instancia. Puede tener una instancia de servidor de informes para cada instancia de SQL Server que instale. Si utiliza la instancia predeterminada, escriba el nombre de la instancia de SQL Server. Si utiliza una instancia con nombre, especifique la instancia con nombre para conectar al servidor de informes en el formato MSSQL$InstanceName.  
  
 **Autenticación**  
 La autenticación del servidor de informes se realiza a través de Internet Information Services (IIS). Seleccione uno de los siguientes modos de autenticación al conectarse a Reporting Services:  
  
 **Modo de autenticación de Windows (autenticación de Windows)**  
 Se conecta a una instancia de servidor de informes usando sus credenciales de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Autenticación básica**  
 Conéctese mediante la **Autenticación básica** si la instalación de Reporting Services está configurada para usar la Autenticación básica.  
  
 **Autenticación de formularios**  
 Conéctese mediante la **Autenticación de formularios** si la instalación de Reporting Services está configurada para usar una extensión de autenticación personalizada.  
  
 **Nombre de usuario**  
 Escriba el nombre de inicio de sesión que se va a usar en la conexión. Esta opción solo se encuentra disponible si ha seleccionado **Autenticación básica** o **Autenticación de formularios**.  
  
 **Contraseña**  
 Escriba la contraseña del nombre de usuario. Esta opción solo se puede editar si ha seleccionado **Autenticación básica** o **Autenticación de formularios**.  
  
 **Recordar contraseña**  
 Guarda la contraseña que ha escrito. Esta opción solo se encuentra disponible si ha seleccionado **Autenticación básica** o **Autenticación de formularios**.  
  
> **NOTA:** Si almacenó la contraseña y ya no quiere conservarla, desactive esta casilla y, luego, haga clic en **Guardar**.  
  
 **Nombre del servidor registrado**  
 El nombre que desea que aparezca en Servidores registrados. Este nombre no tiene que coincidir con el cuadro **Nombre del servidor** .  
  
 **Descripción del servidor registrado**  
 Escriba una descripción opcional del servidor.  
  
 **Test**  
 Haga clic para probar la conexión al servidor seleccionado en **Nombre del servidor**.  
  
 
 ## <a name="analysis-services---multidimensional-data-new-or-edit-server-registration-general-tab"></a>Nuevo o Editar el registro de servidores (pestaña General) de Analysis Services - Datos multidimensionales
 
  Utilice esta pestaña para especificar opciones cuando registre una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Para obtener acceso a esta página, en Servidores registrados, haga clic en **Analysis Services** , en la barra de herramientas de Servidores registrados haga clic con el botón derecho en cualquier grupo de servidores registrados como, por ejemplo, **Analysis Services**, seleccione **Nuevo** y haga clic en **Registro de servidor**.  
  
### <a name="options"></a>Opciones  
 **Tipo de servidor**  
 Cuando se registra un servidor desde Servidores registrados, el cuadro **Tipo de servidor** es de solo lectura y coincide con el tipo de servidor que se muestra en el panel Servidores registrados. Para registrar un tipo de servidor diferente, haga clic en el servidor que desee en la barra de herramientas de **Servidores registrados** antes de comenzar a registrar un nuevo servidor.  
  
 **Nombre del servidor**  
 Seleccione la instancia de servidor a la que va a conectarse. De forma predeterminada, aparecerá la instancia de servidor a la que se ha conectado por última vez.  
  
 **Autenticación**  
 La autenticación de Windows permite a un usuario conectarse mediante sus credenciales de [!INCLUDE[msCoName](../../includes/msconame-md.md)] , como un usuario de Windows o como miembro de un grupo de Windows.  
  
 **Nombre de usuario**  
 Esta opción no está disponible en esta versión.  
  
 **Contraseña**  
 Esta opción no está disponible en esta versión.  
  
 **Recordar contraseña**  
 Esta opción no está disponible en esta versión.  
  
 **Nombre del servidor registrado**  
 El nombre que desea que aparezca en Servidores registrados. Este nombre no tiene que coincidir con el cuadro **Nombre del servidor** .  
  
 **Descripción del servidor registrado**  
 Escriba una descripción opcional del servidor.  
  
 **Test**  
 Haga clic para probar la conexión al servidor seleccionado en **Nombre del servidor**. 
 
 ## <a name="ssis-new-or-edit-server-registration-general-tab"></a>Nuevo o Editar el registro de servidor (pestaña General) de SSIS 
 
 Utilice esta pestaña para especificar las opciones cuando se registra [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Para obtener acceso a esta página, en Servidores registrados, haga clic en **Integration Services** en la barra de herramientas de **Servidores registrados** , haga clic con el botón derecho en cualquier grupo de servidores registrados, seleccione **Nuevo** y, luego, haga clic en **Registro de servidor**.  
  
### <a name="options"></a>Opciones  
 **Tipo de servidor**  
 Cuando registra un servidor en Servidores registrados, el cuadro **Tipo de servidor** es de solo lectura y corresponde al tipo de servidor que se muestra en Servidores registrados. Para registrar un tipo diferente de servidor, haga clic en **Motor de base de datos**, **Analysis Server**, **Reporting Services**, **SQL Server Compact** **Edition** o **Integration Services** en la barra de herramientas de **Servidores registrados** antes de iniciar el registro de un nuevo servidor.  
  
 **Nombre del servidor**  
 Seleccione el servidor con el que se va a establecer conexión. De forma predeterminada, aparecerá el servidor con el que se realizó la última conexión.  
  
 **Autenticación**  
 El modo de autenticación de Windows permite al usuario conectarse mediante una cuenta de usuario de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Nombre de usuario**  
 Esta opción no está disponible en esta versión.  
  
 **Contraseña**  
 Esta opción no está disponible en esta versión.  
  
 **Recordar contraseña**  
 Esta opción no está disponible en esta versión.  
  
 **Nombre del servidor registrado**  
 El nombre que quiere que aparezca en **Servidores registrados**. Este nombre no tiene que coincidir con el cuadro **Nombre del servidor** .  
  
 **Descripción del servidor registrado**  
 Escriba una descripción opcional del servidor.  
  
 **Test**  
 Haga clic para probar la conexión al servidor seleccionado en **Nombre del servidor**. 
  

 
 
