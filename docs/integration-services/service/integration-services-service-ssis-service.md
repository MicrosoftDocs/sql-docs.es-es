---
description: Servicio Integration Services (servicio SSIS)
title: Servicio Integration Services (servicio SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssiseditserverregistration.connectionproperties.f1
- sql13.swb.connecttodts.connectionproperties.f1
- sql13.swb.connection.login.dtsserver.f1
- sql13.swb.connecttodts.login.f1
- sql13.swb.connecttodtsserver.login.f1
helpviewer_keywords:
- Integration Services service, about Integration Services service
- SQL Server Integration Services service
- service [Integration Services],about Integration Services service
- service [Integration Services]
- SQL Server Integration Services, service
ms.assetid: 2c785b3b-4a0c-4df7-b5cd-23756dc87842
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 30d1635e0d58af970cae609fb2e3a6a7f2335791
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237743"
---
# <a name="integration-services-service-ssis-service"></a>Servicio Integration Services (servicio SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Los temas de esta sección describen el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , un servicio de Windows para administrar paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . No se requiere este servicio para crear, guardar y ejecutar los paquetes de Integration Services. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] admite el servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para mantener la compatibilidad con versiones anteriores de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] almacena objetos, valores y datos operativos en la base de datos **SSISDB** para los proyectos que se han implementado en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] mediante el modelo de implementación de proyectos. El servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , que es una instancia del motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , hospeda la base de datos. Para obtener más información sobre la base de datos, vea [Catálogo de SSIS](../../integration-services/catalog/ssis-catalog.md). Para obtener más información sobre la implementación de proyectos en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vea [Deploy Integration Services (SSIS) Projects and Packages](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md) (Implementación de proyectos y paquetes de Integration Services [SSIS]).  
  
## <a name="management-capabilities"></a>Funcionalidades de administración  
 El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] es un servicio de Windows para administrar paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] solo está disponible en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Ejecutar el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] proporciona las siguientes capacidades de administración:  
  
-   Iniciar paquetes almacenados en ubicaciones locales y remotas  
  
-   Detener paquetes que se ejecutan en ubicaciones locales y remotas  
  
-   Supervisar paquetes que se ejecutan en ubicaciones locales y remotas  
  
-   Importar y exportar paquetes  
  
-   Administrar el almacenamiento de paquetes  
  
-   Personalizar carpetas de almacenamiento  
  
-   Detener paquetes que se están ejecutando cuando se detiene el servicio  
  
-   Ver el registro de eventos de Windows  
  
-   Conectar con varios servidores de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  
  
## <a name="startup-type"></a>cuadro de tipo de inicio,
 El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se instala al instalar el componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. De forma predeterminada, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se inicia y el tipo de inicio del servicio se establece en automático. El servicio se debe ejecutar para poder supervisar los paquetes almacenados en el Almacén de paquetes [!INCLUDE[ssIS](../../includes/ssis-md.md)] . El Almacén de paquetes [!INCLUDE[ssIS](../../includes/ssis-md.md)] puede ser la base de datos msdb en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o las carpetas designadas en el sistema de archivos.  
  
 El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] no es necesario si únicamente desea diseñar y ejecutar paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Sin embargo, sí se necesita para ver la lista de paquetes y supervisarlos con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

## <a name="manage-the-service"></a>Administración del servicio
  
 Al instalar el componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se instala también el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . De forma predeterminada, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se inicia y el tipo de inicio del servicio se establece en automático. Sin embargo, también debe instalar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para usar el servicio y administrar los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] almacenados y en ejecución.  
  
> [!NOTE]
> Para conectar directamente con una instancia del servicio Integration Services heredado, tendrá que usar la versión de SQL Server Management Studio (SSMS) alineada con la versión de SQL Server en la que se ejecuta el servicio Integration Services. Por ejemplo, para conectar con el servicio Integration Services heredado que se ejecuta en una instancia de SQL Server 2016, debe usar la versión de SSMS publicada para SQL Server 2016. [Descargue SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>   En el cuadro de diálogo **Conectar con el servidor** de SSMS, no se puede escribir el nombre de un servidor en el que se está ejecutando una versión anterior del servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . En cambio, para administrar paquetes almacenados en un servidor remoto, no tiene que conectarse a la instancia del servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en ese servidor remoto. En su lugar, modifique el archivo de configuración para el servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de manera que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] muestre los paquetes almacenados en el servidor remoto.   
  
 Solo puede instalar una única instancia del servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en un equipo. El servicio no es específico de una instancia determinada de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Para realizar la conexión con el servicio se utiliza el nombre del equipo en el que se ejecuta.  
  
 Puede administrar el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] mediante uno de los siguientes complementos de Microsoft Management Console (MMC): los servicios o el Administrador de configuración de SQL Server. Antes de que pueda administrar paquetes en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], asegúrese de que se ha iniciado el servicio.  
  
 De forma predeterminada, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se configura para administrar los paquetes de la base de datos msdb de la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] que se instala al mismo tiempo que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si no se instala al mismo tiempo una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se configura para administrar paquetes de la base de datos msdb de la instancia local predeterminada del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Para administrar paquetes que están almacenados en una instancia con nombre o remota de [!INCLUDE[ssDE](../../includes/ssde-md.md)], o en varias instancias de [!INCLUDE[ssDE](../../includes/ssde-md.md)], tiene que modificar el archivo de configuración para el servicio.
  
 De manera predeterminada, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] está configurado para dejar de ejecutar paquetes cuando se detiene el servicio. Sin embargo, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] no espera a que los paquetes se detengan y es posible que algunos paquetes sigan ejecutándose después de detener el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Si se detiene el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puede seguir ejecutando paquetes con el Asistente para importación y exportación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , la Utilidad de ejecución de paquetes y la utilidad del símbolo del sistema **dtexec** (dtexec.exe). Sin embargo, no podrá supervisar los paquetes en ejecución.  
  
 De manera predeterminada, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se ejecuta en el contexto de la cuenta Servicio de red. Se recomienda ejecutar el servicio SQL Server Integration Services con una cuenta que tenga permisos limitados, como la cuenta de servicio de red. La ejecución del servicio SQL Server Integration Services en una cuenta con privilegios elevados representa un riesgo de seguridad potencial. 
  
 El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] escribe en el registro de eventos de Windows. Puede ver los eventos del servicio en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. También puede ver los eventos del servicio mediante el Visor de eventos de Windows.  
  
## <a name="set-the-properties-of-the-service"></a>Establecimiento de las propiedades del servicio
  
 El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] administra y supervisa los paquetes de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]por primera vez, se inicia el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y el tipo de inicio del servicio se establece como automático.  
  
 Una vez instalado el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puede establecer sus propiedades mediante el Administrador de configuración de SQL Server o el complemento Servicios de componentes de MMC.  
  
 Para configurar otras características importantes del servicio, incluidas las ubicaciones donde almacena y administra los paquetes, debe modificar el archivo de configuración del servicio.
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-sql-server-configuration-manager"></a>Para establecer las propiedades del servicio Integration Services con el Administrador de configuración de SQL Server  
  
1.  En el menú **Inicio** , seleccione **Todos los programas**, **Microsoft SQL Server**, **Herramientas de configuración** y haga clic en **Administrador de configuración de SQL Server**.  
  
2.  En el complemento **Administrador de configuración de SQL Server** , busque **SQL Server Integration Services** en la lista de servicios, haga clic con el botón derecho en **SQL Server Integration Services** y, después, haga clic en **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades de SQL Server Integration Services** , puede hacer lo siguiente:  
  
    -   Haga clic en la pestaña **Iniciar sesión** para ver la información de inicio de sesión, como el nombre de cuenta.  
  
    -   Haga clic en la pestaña **Servicio** para ver información sobre el servicio, como el nombre del equipo host y para especificar el modo de inicio del servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
        > [!NOTE]  
        >  La pestaña **Avanzadas** no contiene información para el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
4.  Haga clic en **OK**.  
  
5.  En el menú **Archivo** , haga clic en **Salir** para cerrar el complemento **Administrador de configuración de SQL Server** .  
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-services"></a>Para establecer las propiedades del servicio Integration Services con el complemento Servicios  
  
1.  En el **Panel de control**, si utiliza la Vista clásica, haga clic en **Herramientas administrativas** o bien, si utiliza la Vista por categorías, haga clic en **Rendimiento y mantenimiento** y, a continuación, en **Herramientas administrativas**.  
  
2.  Haga clic en **Servicios**.  
  
3.  En el complemento **Servicios** , localice **SQL Server Integration Services** en la lista de servicios, haga clic con el botón derecho en **SQL Server Integration Services** y, después, haga clic en **Propiedades**.  
  
4.  En el cuadro de diálogo **Propiedades de SQL Server Integration Services** , puede hacer lo siguiente:  
  
    -   Haga clic en la pestaña **General**. Para habilitar el servicio, seleccione el tipo de inicio manual o automático. Para deshabilitar el servicio, seleccione Deshabilitar en el cuadro **Tipo de inicio** . Si el servicio se está ejecutando, no se detendrá al seleccionar Deshabilitar.  
  
         Si el servicio ya está habilitado, puede hacer clic en **Detener** para detenerlo o en **Iniciar** para iniciarlo.  
  
    -   Haga clic en la pestaña **Iniciar sesión** para ver o editar la información de inicio de sesión.  
  
    -   Haga clic en la pestaña **Recuperación** para ver las respuestas predeterminadas del equipo a un error en el servicio. Puede modificar estas opciones para adaptarlas a su entorno.  
  
    -   Haga clic en la pestaña **Dependencias** para ver una lista de los servicios dependientes. El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] no tiene dependencias.  
  
5.  Haga clic en **OK**.  
  
6.  Opcionalmente, si el tipo de inicio es Manual o Automático, puede hacer clic con el botón derecho en **SQL Server Integration Services** y hacer clic en **Iniciar, Detener o Reiniciar**.  
  
7.  En el menú **Archivo** , haga clic en **Salir** para cerrar el complemento **Servicios** .  

## <a name="grant-permissions-to-the-service"></a>Concesión de permisos al servicio
  En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cuando se instalaba [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] todos los usuarios del grupo Usuarios tenía acceso al servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de forma predeterminada. Cuando instala la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los usuarios no tienen acceso al servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . El servicio es seguro de forma predeterminada. Una vez instalado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el administrador debe conceder acceso al servicio.  
  
### <a name="to-grant-access-to-the-integration-services-service"></a>Para conceder acceso al servicio Integration Services  
  
1.  Ejecute Dcomcnfg.exe. Dcomcnfg.exe proporciona una interfaz de usuario para modificar algunos valores de configuración del Registro.  
  
2.  En el cuadro de diálogo **Servicios de componente**, expanda el nodo Servicios de componente > Equipos > Mi PC > Configuración DCOM.  
  
3.  Haga clic con el botón derecho en **Microsoft SQL Server Integration Services 13.0** y, después, haga clic en **Propiedades**.  
  
4.  En la pestaña **Seguridad** , haga clic en **Editar** en la sección **Permisos de inicio y activación** .  
  
5.  Agregue usuarios y asigne los permisos adecuados y, a continuación, haga clic en Aceptar.  
  
6.  Repita los pasos 4 a 5 para los permisos de acceso.  
  
7.  Reinicie SQL Server Management Studio.  
  
8.  Reinicie el Servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  

### <a name="event-logged-when-permissions-are-missing"></a>Evento que se registra cuando faltan los permisos

Si la cuenta de servicio del Agente SQL Server no tiene el DCOM de Integration Services **[Permisos de inicio y activación]**, se agrega el evento siguiente a los registros de eventos del sistema cuando el Agente SQL Server ejecuta los trabajos del paquete SSIS:

```
Log Name: System
Source: **Microsoft-Windows-DistributedCOM**
Date: 1/9/2019 5:42:13 PM
Event ID: **10016**
Task Category: None
Level: Error
Keywords: Classic
User: NT SERVICE\SQLSERVERAGENT
Computer: testmachine
Description:
The application-specific permission settings do not grant Local Activation permission for the COM Server application with CLSID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
and APPID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
to the user NT SERVICE\SQLSERVERAGENT SID (S-1-5-80-344959196-2060754871-2302487193-2804545603-1466107430) from address LocalHost (Using LRPC) running in the application container Unavailable SID (Unavailable). This security permission can be modified using the Component Services administrative tool.
```

## <a name="configure-the-service"></a>Configuración del servicio
 
Cuando se instala [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], el proceso de instalación crea e instala el archivo de configuración para el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Este archivo de configuración contiene los siguientes valores:  
  
-   Si se envía a los paquetes un comando de detención cuando se detenga el servicio.  
  
-   Las carpetas raíz que deben mostrarse para [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el Explorador de objetos de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] son MSDB y File System.  
  
-   Los paquetes del sistema de archivos administrados por el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se encuentran en la carpeta %Archivos de programa%\Microsoft SQL Server\130\DTS\Packages.  
  
 Este archivo de configuración también especifica qué base de datos msdb contiene los paquetes que el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] administrará. De forma predeterminada, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se configura para administrar los paquetes de la base de datos msdb de la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] que se instala al mismo tiempo que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si no se instala al mismo tiempo una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se configura para administrar paquetes de la base de datos msdb de la instancia local predeterminada del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
### <a name="default-configuration-file-example"></a>Ejemplo de archivo de configuración predeterminado  
 En el ejemplo siguiente se muestra un archivo de configuración predeterminado que especifica los valores siguientes:  
  
-   Los paquetes dejan de ejecutarse cuando se detiene el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Las carpetas raíz donde se almacenan los paquetes en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] son MSDB y Sistema de archivos.  
  
-   El servicio administra paquetes que están almacenados en la base de datos msdb de la instancia local y predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   El servicio administra los paquetes que están almacenados en el sistema de archivos en la carpeta Paquetes.  
  
 **Ejemplo de archivo de configuración predeterminado**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>.</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file"></a>Modifique el archivo de configuración  
 Puede modificar el archivo de configuración para permitir que los paquetes se sigan ejecutando si se detiene el servicio, para mostrar carpetas raíz adicionales en el Explorador de objetos, o para especificar una carpeta distinta o carpetas adicionales del sistema de archivos que deban ser administradas por el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Por ejemplo, puede crear carpetas raíz adicionales de tipo, **SqlServerFolder**, para administrar paquetes en las bases de datos msdb de instancias adicionales de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
> [!NOTE]  
>  Algunos caracteres no son válidos en los nombres de carpeta. Los caracteres válidos para los nombres de carpeta se determinan mediante la clase [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] System.IO.Path **de** y el campo **GetInvalidFilenameChars** . El campo **GetInvalidFilenameChars** proporciona una matriz específica de la plataforma de caracteres que no se pueden especificar en los argumentos de la cadena de ruta pasada a los miembros de la clase **Path** . El juego de caracteres no válidos puede variar en función del sistema de archivos. Normalmente, los caracteres no válidos son las comillas ("), el carácter mayor que (<) y la barra vertical (|).  
  
 Sin embargo, tendrá que modificar el archivo de configuración para administrar paquetes que estén almacenados en una instancia con nombre o una instancia remota de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Si no actualiza el archivo de configuración, no puede usar el **Explorador de objetos** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ver los paquetes que estén almacenados en la base de datos msdb en la instancia con nombre o en la instancia remota. Si intenta utilizar el **Explorador de objetos** para ver estos paquetes, aparece el mensaje de error siguiente:  
  
 `Failed to retrieve data for this request. (Microsoft.SqlServer.SmoEnum)`  
  
 `The SQL Server specified in Integration Services service configuration is not present or is not available. This might occur when there is no default instance of SQL Server on the computer. For more information, see the topic "Configuring the Integration Services Service" in SQL Server 2008 Books Online.`  
  
 `Login Timeout Expired`  
  
 `An error has occurred while establishing a connection to the server. When connecting to SQL Server 2008, this failure may be caused by the fact that under the default settings SQL Server does not allow remote connections.`  
  
 `Named Pipes Provider: Could not open a connection to SQL Server [2]. (MsDtsSvr).`  
  
 Para modificar el archivo de configuración para el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , se ha de utilizar un editor de texto.  
  
> [!IMPORTANT]  
>  Después de modificar el archivo de configuración del servicio, deberá reiniciar el servicio para usar la configuración del servicio actualizada.  
  
### <a name="modified-configuration-file-example"></a>Ejemplo de archivo de configuración modificado  
 El ejemplo siguiente muestra un archivo de configuración modificado para [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Este archivo es para una instancia con nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominada `InstanceName` en un servidor denominado `ServerName`.  
  
 **Ejemplo de un archivo de configuración modificado para una instancia con nombre de SQL Server**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>ServerName\InstanceName</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file-location"></a>Modificación de la ubicación del archivo de configuración  
 La clave del Registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\ServiceConfigFile** especifica la ubicación y el nombre del archivo de configuración que usa el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. El valor predeterminado de la clave del Registro es **C:\Archivos de programa\Microsoft SQL Server\130\DTS\Binn\MsDtsSrvr.ini.xml**. Puede actualizar el valor de la clave del Registro para utilizar un nombre y una ubicación diferentes para el archivo de configuración. Tenga en cuenta que el número de versión en la ruta de acceso (120 para [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)], 130 para [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], etc.) varía según la versión de SQL Server.
  
> [!CAUTION]  
>  Editar el Registro de forma incorrecta puede originar problemas graves que requieran volver a instalar el sistema operativo. [!INCLUDE[msCoName](../../includes/msconame-md.md)] no puede garantizar la resolución de dichos problemas. Haga una copia de seguridad de los datos importantes antes de modificar el Registro. Para obtener información sobre cómo hacer una copia de seguridad, restaurar y modificar el Registro, vea el artículo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base, [Definición del Registro de Microsoft Windows](https://support.microsoft.com/kb/256986).  
  
 El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] carga el archivo de configuración cuando se inicia el servicio. Si se cambia la entrada del Registro, es preciso reiniciar el servicio.  

## <a name="connect-to-the-local-service"></a>Conexión con el servicio local
  Antes de conectar con el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , el administrador debe concederle acceso al servicio. 
  
### <a name="to-connect-to-the-integration-services-service"></a>Conexión con el servicio Integration Services  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Haga clic en **Explorador de objetos** , en el menú **Ver** .  
  
3.  En la barra de herramientas del Explorador de objetos, haga clic en **Conectar** y, a continuación, en **Integration Services**.  
  
4.  En el cuadro de diálogo **Conectar al servidor** , indique el nombre del servidor. Puede usar un punto (.), (local) o **localhost** para indicar el servidor local.  
  
5.  Haga clic en **Conectar**.  

## <a name="connect-to-a-remote-ssis-server"></a>Conexión a un servidor SSIS remoto
  
 Para conectarse a una instancia de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en un servidor remoto, desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] u otra aplicación de administración, los usuarios de la aplicación deben tener un conjunto específico de derechos para el servidor.  
  
> [!IMPORTANT]
> Para conectar directamente con una instancia del servicio Integration Services heredado, tendrá que usar la versión de SQL Server Management Studio (SSMS) alineada con la versión de SQL Server en la que se ejecuta el servicio Integration Services. Por ejemplo, para conectar con el servicio Integration Services heredado que se ejecuta en una instancia de SQL Server 2016, debe usar la versión de SSMS publicada para SQL Server 2016. [Descargue SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>  Para administrar paquetes almacenados en un servidor remoto, no tiene que conectarse a la instancia del servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en ese servidor remoto. En su lugar, modifique el archivo de configuración para el servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de manera que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] muestre los paquetes almacenados en el servidor remoto.
  
### <a name="connecting-to-integration-services-on-a-remote-server"></a>Conectarse a Integration Services en un servidor remoto  
  
#### <a name="to-connect-to-integration-services-on-a-remote-server"></a>Para conectarse a Integration Services en un servidor remoto  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Seleccione **Archivo**, **Conectar Explorador de objetos** para mostrar el cuadro de diálogo **Conectar al servidor** .  
  
3.  Seleccione **Integration Services** en la lista **Tipo de servidor** .  
  
4.  Escriba el nombre de un servidor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el cuadro de texto **Nombre del servidor**.  
  
    > [!NOTE]  
    >  El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] no es específico de la instancia. La conexión al servicio se realiza utilizando el nombre del equipo en el que el servicio de Integration Services se está ejecutando.  
  
5.  Haga clic en **Conectar**.  
  
> [!NOTE]  
>  En el cuadro de diálogo **Buscar servidores** no se muestran las instancias remotas de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Además, las opciones disponibles en la pestaña **Opciones de conexión** del cuadro de diálogo **Conectar al servidor** , que se muestra al hacer clic en el botón **Opciones** , no se aplican a las conexiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="eliminating-the-access-is-denied-error"></a>Eliminación del error de acceso denegado  
 Cuando un usuario que no tiene los derechos necesarios intenta conectarse a una instancia de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en un servidor remoto, el servidor responde con el mensaje de error "Acceso denegado". Se puede evitar ese mensaje de error asegurándose de que los usuarios tengan los permisos DCOM necesarios.  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-server-2003-or-windows-xp"></a>Para configurar derechos para usuarios remotos en Windows Server 2003 o Windows XP  
  
1.  Si el usuario no es miembro del grupo de administradores local, agregue el usuario al grupo de usuarios de COM distribuido. Puede hacerlo en el complemento Administración de equipos de MMC, en el menú **Herramientas administrativas**.  
  
2.  Abra el Panel de control, haga doble clic en **Herramientas administrativas** y en **Servicios de componente** para iniciar el complemento Servicios de componentes de MMC.  
  
3.  Expanda el nodo **Servicios de componente** en el panel izquierdo de la consola. Expanda los nodos **Equipos** y **Mi PC** y, a continuación, haga clic en el nodo **Configuración DCOM**.  
  
4.  Seleccione el nodo **Configuración DCOM** y, a continuación, seleccione SQL Server Integration Services 11.0 en la lista de aplicaciones que pueden configurarse.  
  
5.  Haga clic con el botón derecho en SQL Server Integration Services 11.0 y seleccione **Propiedades**.  
  
6.  En el cuadro de diálogo **Propiedades de SQL Server Integration Services 11.0** , seleccione la pestaña **Seguridad** .  
  
7.  En **Permisos de inicio y activación**, active **Personalizar** y haga clic en **Editar** para abrir el cuadro de diálogo **Permisos de inicio** .  
  
8.  En el cuadro de diálogo **Permisos de inicio** , agregue o elimine usuarios y asigne los permisos adecuados a los usuarios y grupos correspondientes. Los permisos disponibles son Ejecución local, Ejecución remota, Activación local y Activación remota. Los derechos de ejecución conceden o deniegan el permiso para iniciar y detener el servicio; los derechos de activación conceden o deniegan el permiso para conectarse al servicio.  
  
9. Haga clic en Aceptar para cerrar el cuadro de diálogo.  
  
10. En **Permisos de acceso**, repita los pasos 7 y 8 con el fin de asignar los permisos pertinentes a los correspondientes usuarios y grupos.  
  
11. Cierre el complemento MMC.  
  
12. Reinicie el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-2000-with-the-latest-service-packs"></a>Para configurar derechos para usuarios remotos en Windows 2000 con los Service Pack más recientes  
  
1.  Ejecute **dcomcnfg.exe** en el símbolo del sistema.  
  
2.  En la página **Aplicaciones** del cuadro de diálogo **Propiedades de Configuración de COM distribuido** , seleccione SQL Server Integration Services 11.0 y haga clic en **Propiedades**.  
  
3.  Seleccione la página **Seguridad** .  
  
4.  Utilice los dos cuadros de diálogo independientes para configurar **Permisos de acceso** y **Permisos de inicio**. No se puede distinguir entre el acceso remoto y el acceso local; los permisos de acceso incluyen el acceso local y el remoto, y los permisos de inicio incluyen la ejecución local y la remota.  
  
5.  Cierre los cuadros de diálogo y **dcomcnfg.exe**.  
  
6.  Reinicie el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="connecting-by-using-a-local-account"></a>Conectarse con una cuenta local

Si se trabaja en una cuenta local de Windows en un equipo cliente, solo es posible conectarse al servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en un equipo remoto si en el equipo remoto existe una cuenta local con el mismo nombre y contraseña, y con los derechos adecuados.  
  
### <a name="ssis-windows-service-doesnt-support-delegation"></a>Delegación no admitida por el servicio de Windows SSIS

SSIS no admite la delegación de credenciales, a veces denominada como salto doble. En este escenario, se trabaja con un equipo cliente, SSIS está instalado en un segundo equipo y SQL Server está instalado en un tercer equipo. Aunque SSMS pase correctamente las credenciales del usuario del equipo cliente al segundo equipo (en el que SSIS se ejecuta), SSIS no puede delegar esas credenciales del segundo equipo al tercer equipo (en el que se ejecuta SQL Server).

## <a name="configure-the-firewall"></a>Configuración del firewall
  
 El sistema Firewall de Windows impide el acceso no autorizado a los recursos de los equipos de una conexión de red. Para obtener acceso a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] mediante este firewall, debe configurarlo para permitir el acceso.  
  
> [!IMPORTANT]  
>  Para administrar paquetes almacenados en un servidor remoto, no tiene que conectarse a la instancia del servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en ese servidor remoto. En su lugar, modifique el archivo de configuración para el servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de manera que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] muestre los paquetes almacenados en el servidor remoto.
  
 El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utiliza el protocolo DCOM.
  
 Existen varios sistemas de firewall. Si ejecuta un firewall distinto de Firewall de Windows, vea la documentación del firewall para obtener información específica del sistema que utiliza.  
  
 Si el firewall admite el filtrado de aplicaciones, puede utilizar la interfaz de usuario de Windows para especificar las excepciones permitidas en el firewall, como programas y servicios. Si no es el caso, debe configurar DCOM para utilizar un conjunto limitado de puertos TCP. El vínculo al sitio web de Microsoft mencionado anteriormente incluye información acerca de cómo especificar los puertos TCP que debe utilizar.  
  
 El servicio Integration Services utiliza el puerto 135 y no es posible cambiarlo. Debe abrir el puerto TCP 135 para obtener acceso al Administrador de control de servicios (SCM). Entre las tareas que realiza el SCM se encuentra el inicio y detención de servicios de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y la transmisión de solicitudes de control al servicio en ejecución.  
  
 La información que se incluye en la siguiente sección es específica de Firewall de Windows. Para configurar el sistema Firewall de Windows, debe ejecutar un comando desde el símbolo del sistema o establecer las propiedades en el cuadro de diálogo de Firewall de Windows.  
  
 Para obtener más información sobre la configuración predeterminada de Firewall de Windows y una descripción de los puertos TCP que afectan al motor de base de datos, Analysis Services, Reporting Services e Integration Services, vea [Configurar Firewall de Windows para permitir el acceso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="configuring-a-windows-firewall"></a>Configuración de Firewall de Windows  
 Puede utilizar los siguientes comandos para abrir el puerto TCP 135, agregar MsDtsSrvr.exe a la lista de excepciones y especificar el ámbito de desbloqueo del firewall.  
  
#### <a name="to-configure-a-windows-firewall-using-the-command-prompt-window"></a>Para configurar un firewall de Windows en la ventana del símbolo del sistema  
  
1.  Ejecute el siguiente comando:

    ```dos
    netsh firewall add portopening protocol=TCP port=135 name="RPC (TCP/135)" mode=ENABLE scope=SUBNET
    ```
  
2.  Ejecute el siguiente comando:

    ```dos
    netsh firewall add allowedprogram program="%ProgramFiles%\Microsoft SQL Server\100\DTS\Binn\MsDtsSrvr.exe" name="SSIS Service" scope=SUBNET
    ```
  
    > [!NOTE]  
    >  Para abrir el firewall en todos los equipos, además de los que se encuentran en Internet, reemplace el ámbito=SUBNET por el ámbito=ALL.  
  
 En el siguiente procedimiento se describe cómo utilizar la interfaz de usuario de Windows para abrir el puerto TCP 135, agregar MsDtsSrvr.exe a la lista de excepciones y especificar el ámbito de desbloqueo del firewall.  
  
#### <a name="to-configure-a-firewall-using-the-windows-firewall-dialog-box"></a>Para configurar un firewall mediante el cuadro de diálogo Firewall de Windows  
  
1.  En el Panel de control, haga doble clic en **Firewall de Windows**.  
  
2.  En el cuadro de diálogo **Firewall de Windows** , haga clic en la pestaña **Excepciones** y, a continuación, haga clic en **Agregar programa**.  
  
3.  En el cuadro de diálogo **Agregar un programa** , haga clic en **Examinar**, navegue a la carpeta Archivos de programa\Microsoft SQL Server\100\DTS\Binn, haga clic en MsDtsSrvr.exe y, después, en **Abrir**. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Agregar un programa** .  
  
4.  En la pestaña **Excepciones** , haga clic en **Agregar puerto**.  
  
5.  En el cuadro de diálogo **Agregar un puerto** , escriba **RPC(TCP/135)** u otro nombre descriptivo en el cuadro **Nombre**, escriba **135** en el cuadro **Número de puerto** y seleccione **TCP**.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utiliza siempre el puerto 135. No se puede especificar un puerto diferente.  
  
6.  En el cuadro de diálogo **Agregar un puerto** , puede hacer clic en **Cambiar ámbito** para modificar el ámbito predeterminado.  
  
7.  En el cuadro de diálogo **Cambiar ámbito** , seleccione **Mi red (solo subred)** o escriba una lista personalizada y haga clic en **Aceptar**.  
  
8.  Para cerrar el cuadro de diálogo **Agregar un puerto** , haga clic en **Aceptar**.  
  
9. Para cerrar el cuadro de diálogo **Firewall de Windows** , haga clic en **Aceptar**.  
  
    > [!NOTE]  
    >  Para configurar el Firewall de Windows, este procedimiento utiliza el elemento **Firewall de Windows** del Panel de control. El elemento **Firewall de Windows** solo configura el firewall para el perfil de la ubicación de red actual. Sin embargo, también puede configurar el Firewall de Windows mediante la herramienta de línea de comandos **netsh** o el complemento [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC) denominado Firewall de Windows con seguridad avanzada. Para obtener más información sobre estas herramientas, vea [Configurar Firewall de Windows para permitir el acceso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
