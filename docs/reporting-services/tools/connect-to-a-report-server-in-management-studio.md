---
title: Conectar con un servidor de informes en Management Studio | Microsoft Docs
description: Obtenga información sobre cómo conectarse a cualquier servidor de la familia de SQL Server y examinar de forma gráfica su contenido mediante el Explorador de objetos de SQL Server Management Studio.
ms.date: 05/07/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttors.connectionproperties.f1
- sql13.swb.connecttors.login.f1
- sql13.swb.connection.login.reportserver.f1
helpviewer_keywords:
- report servers [Reporting Services], connections
- connections [Reporting Services], report server
- registering report servers
- report servers [Reporting Services], registering
- Connect to Server dialog box, Reporting Services
ms.assetid: c875ff87-ee7d-443a-a702-bdb4b6c27c6e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 735c7805d1deba9861a6e51249acf9928d97db7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076816"
---
# <a name="connect-to-a-report-server-in-management-studio"></a>Conectar con un servidor de informes en Management Studio

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] proporciona el Explorador de objetos, que permite conectarse a cualquier servidor de la familia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y examinar su contenido de forma gráfica. Para Reporting Services, puede usar el Explorador de objetos para hacer las tareas siguientes:

- Habilitar las características del servidor de informes.

- Establecer los valores predeterminados de servidor y configurar las definiciones de roles.

- Administrar trabajos que se están ejecutando.

- Administrar las programaciones de los trabajos.

 Puede conectarse a un servidor de informes en modo nativo o a un servidor de informes que se ejecuta en el modo integrado con SharePoint. La sintaxis de conexión y los tipos de operaciones que puede realizar dependen del modo de servidor del servidor de informes y de sus permisos. Si no se puede conectar al servidor de informes o tiene problemas al realizar tareas específicas, probablemente no tiene permisos suficientes o ha especificado el nombre del servidor de informes incorrectamente. Para obtener más información sobre los permisos y la sintaxis de conexión, vea la tabla que se encuentra al final de este artículo.

 No puede usar el Explorador de objetos para ver o administrar el contenido del servidor de informes. La administración de contenido se lleva a cabo en el portal web si el servidor de informes se ejecuta en modo nativo o a través de un sitio de SharePoint si el servidor de informes se ejecuta en el modo integrado de SharePoint.

 El Explorador de objetos le permite abrir conexiones en varias instancias del servidor en la misma área de trabajo siempre y cuando los servidores se registren en el mismo grupo de servidores. Para poderse conectar a una instancia del servidor de informes en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], se debe registrar el servidor. Si el servidor de informes ya está registrado, puede omitir este paso. Al final de este artículo, encontrará instrucciones para registrar servidores de informes.

### <a name="to-connect-to-a-native-mode-report-server"></a>Para conectarse a un servidor de informes en modo nativo

1. Si el Explorador de objetos no está abierto todavía, selecciónelo en el menú **Ver**.

2. Haga clic en **Conectar** para ver la lista de tipos de servidor y, a continuación, seleccione **Reporting Services**.

3. En el cuadro de diálogo **Conectar al servidor** , escriba el nombre de la instancia del servidor de informes. Los nombres de instancia del servidor de informes se basan en nombres de instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . De manera predeterminada, el nombre de una instancia del servidor de informes local es únicamente el nombre del equipo. Si ha instalado el servidor de informes como una instancia con nombre, use esta sintaxis para especificarlo: *\<servername>[\\<nombreDeInstancia\>]* .

4. Seleccione el **tipo de autenticación**. Si está usando la autenticación de Windows, conéctese con las credenciales. Si selecciona la autenticación básica o la autenticación de formularios, escriba la cuenta y la contraseña.  
  
5. Seleccione **Conectar**. El servidor de informes se muestra en el Explorador de objetos.  

6. Haga clic con el botón secundario en el nodo del servidor para establecer propiedades del sistema y valores predeterminados del servidor. Para más información, vea [Establecer las propiedades del servidor de informes &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).

### <a name="to-connect-to-a-sharepoint-integrated-mode-report-server"></a>Para conectarse a un servidor de informes en el modo integrado con SharePoint  

1. Si el Explorador de objetos no está abierto todavía, selecciónelo en el menú **Ver**.

2. Haga clic en **Conectar** para ver la lista de tipos de servidor y, a continuación, seleccione **Reporting Services**.

3. En el cuadro de diálogo **Conectar al servidor** , escriba una dirección URL para un sitio de SharePoint. En el ejemplo siguiente se ilustra la sintaxis: `https://<web server>/sites/<site>`.

4. Seleccione el **tipo de autenticación**. Si está usando la autenticación de Windows, debe conectarse usando sus credenciales. Si selecciona la autenticación básica o la autenticación de formularios, escriba la cuenta y la contraseña.

5. Seleccione **Conectar**. El servidor de informes se muestra en el Explorador de objetos.

6. Haga clic con el botón secundario en el nodo del servidor para establecer propiedades del sistema y valores predeterminados del servidor. Para más información, vea [Establecer las propiedades del servidor de informes &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).

### <a name="to-register-a-report-server"></a>Para registrar un servidor de informes

1. Si no puede conectarse a un servidor de informes, entonces no tiene permiso para acceder a él o el servidor no está registrado. Para registrar el servidor, seleccione el menú **Ver** > **Servidores registrados**.

2. Seleccione el icono de **Reporting Services**.

3. Haga clic con el botón derecho en **Reporting Services** > **Nuevo** > **Registro del servidor**. Se muestra el cuadro de diálogo **Nuevo registro de servidor** .

4. Para **Nombre del servidor**, escriba un valor. Especifique el valor según el modo de servidor:

    - Para un servidor de informes en modo nativo, escriba el nombre de la instancia del servidor de informes. Los nombres de instancia del servidor de informes se basan en nombres de instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . De manera predeterminada, el nombre de una instancia del servidor de informes local es únicamente el nombre del equipo. Si ha instalado el servidor de informes como una instancia con nombre, use esta sintaxis para especificarlo: *\<servername>[\\<nombreDeInstancia\>]* .

    - Para un servidor de informes que se ejecuta en el modo integrado con SharePoint, el servidor al que se va a conectar es el sitio de SharePoint con el que el servidor de informes está conectado. Conéctese al sitio de SharePoint para poder ver los niveles de permisos. Los permisos controlan el acceso a las operaciones y el contenido del servidor de informes. Puede especificar cualquier sitio de la colección de sitios. En el ejemplo siguiente se ilustra la sintaxis: `https://mysharepointsite`.

5. Para **Autenticación**, seleccione el modo de autenticación que ya usa el servidor de informes.

   - Si utiliza la seguridad predeterminada, elija **Autenticación de Windows**.
   - Si ha instalado e implementado una extensión de seguridad personalizada, elija **Autenticación de formularios**.
   - Si configurara el servidor de informes para usar la autenticación básica, elija **Autenticación básica**.
   - Si el servidor de informes se configura para el modo integrado con SharePoint, elija **Autenticación de Windows**.

6. Seleccione **Probar** para verificar la conexión.

7. Cuando se le solicite, seleccione **Aceptar** y, después, **Guardar**.

## <a name="connection-syntax-and-permissions"></a>Permisos y sintaxis de conexión

 En la tabla siguiente se resume la sintaxis de conexión, los pasos, las operaciones y los permisos requeridos para realizar tareas concretas.

 Al especificar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] como el tipo de servidor en el cuadro de diálogo **Conectar al servidor** , puede especificar un nombre de servidor de informes o un extremo para el servicio web.

|Conectar a|   Tareas   |   Permisos   |
|----------|-----------|-----------------|  
|Servidor de informes en modo nativo, conectado como la instancia con nombre o predeterminada:<br /><br /> \<server name>\<_instance><br /><br /> La conexión al servidor de informes se realiza a través del proveedor WMI del servidor de informes.|Ver y establecer las propiedades del servidor y los valores predeterminados.<br /><br /> Ver y cancelar trabajos.<br /><br /> Crear y administrar programaciones compartidas.<br /><br /> Crear, modificar o eliminar definiciones de roles.|Se asigna al rol Administrador del sistema.|  
|Servidor de informes en modo nativo, conectado como la instancia con nombre o predeterminada, a través del extremo al servicio web del servidor de informes:<br /><br /> `https://<servername>/reportserver`<br /><br /> La especificación de una dirección URL al servidor de informes ofrece una manera alternativa de conectarse al servidor de informes.|Ver y establecer las propiedades del servidor y los valores predeterminados.<br /><br /> Ver y cancelar trabajos.<br /><br /> Crear y administrar programaciones compartidas.<br /><br /> Crear, modificar o eliminar definiciones de roles.|Se asigna al rol Administrador del sistema.|  
|Servidor de informes en el modo integrado con SharePoint, conectado a través del sitio de SharePoint:<br /><br /> `https://<webserver>/<SharePointSite>`|Ver y establecer las propiedades del servidor y los valores predeterminados.<br /><br /> Ver y cancelar trabajos.<br /><br /> Crear y administrar programaciones compartidas definidas para el sitio al que está conectado.<br /><br /> Ver los niveles de permisos definidos para el sitio al que está conectado.|Nivel de permiso de Control total en el sitio de SharePoint al que está conectado.|
|Servidor de informes en el modo con SharePoint, conectado a través del nombre de la instancia del servidor de informes:<br /><br /> \<server name>\<_instance>|Ver y establecer las propiedades del servidor y los valores predeterminados.<br /><br /> Ver y cancelar trabajos.|El nivel de permiso Control total en el sitio de SharePoint que está integrado con el servidor de informes.<br /><br /> Tenga en cuenta que, cuando se conecta al servidor de informes en lugar de al sitio de SharePoint, se reduce el número de tareas que puede realizar. Esto se debe a que el servidor de informes solamente puede devolver datos de aplicación almacenados o administrados en la base de datos del servidor de informes, y no en la configuración de SharePoint y en las bases de datos de contenido.|

## <a name="see-also"></a>Consulte también

 [Configurar una conexión de base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 [Reporting Services en SQL Server Management Studio &#40;SSRS&#41;](../../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md)
