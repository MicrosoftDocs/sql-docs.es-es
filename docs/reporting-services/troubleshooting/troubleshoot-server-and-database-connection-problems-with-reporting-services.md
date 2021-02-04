---
title: Solución de problemas de conexión de base de datos y servidor
description: En este artículo, diagnosticará y solucionará los problemas que experimente al conectarse a un servidor de informes. También obtendrá información sobre los mensajes "Error inesperado".
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
ms.assetid: 8bbb88df-72fd-4c27-91b7-b255afedd345
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 74bf6f0da0daa0df248243ffbce505c75c55db5e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076327"
---
# <a name="troubleshoot-server--database-connection-problems-with-reporting-services"></a>Solución de problemas de conexión de base de datos y servidor con Reporting Services
Utilice este tema para solucionar los problemas que surjan durante la conexión a un servidor de informes. Este tema también proporciona información sobre los mensajes de "Error inesperado". Para más información sobre la configuración del origen de datos y cómo configurar la información de conexión del servidor de informes, vea [Especificar información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md) y [Configurar una conexión a la base de datos del servidor de informes (Administrador de configuración del servidor de informes)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="cannot-create-a-connection-to-data-source-datasourcename-rserroropeningconnection"></a>No se puede crear una conexión al origen de datos 'nombreDelOrigenDeDatos'. (rsErrorOpeningConnection)  
Se trata de un error genérico que se produce cuando el servidor de informes no puede abrir una conexión a un origen de datos externo que proporciona datos al informe. Este error se muestra junto con un segundo mensaje de error que indica la causa subyacente. Pueden aparecer los siguientes errores adicionales con **rsErrorOpeningConnection**.  
  
### <a name="login-failed-for-user-username"></a>Error de inicio de sesión del usuario 'nombreDeUsuario'  
El usuario no tiene permiso de acceso al origen de datos. Si utiliza una base de datos de SQL Server, compruebe que el usuario tenga un inicio de sesión de usuario de base de datos válido. Para obtener más información sobre cómo crear un usuario de base de datos o un inicio de sesión de SQL Server, consulte [Crear un usuario de base de datos](../../relational-databases/security/authentication-access/create-a-database-user.md) y [Crear un inicio de sesión](../../relational-databases/security/authentication-access/create-a-login.md).  
  
### <a name="login-failed-for-user-nt-authorityanonymous-logon"></a>Error de inicio de sesión del usuario 'NT AUTHORITY\ANONYMOUS LOGON'  
Este error se produce cuando se envían credenciales a través de varias conexiones de equipo. Si utiliza la autenticación de Windows y el protocolo Kerberos versión 5 no está habilitado, este error se producirá cuando se envíen las credenciales a través de más de una conexión de equipo. Para solucionar este error, considere la posibilidad de utilizar credenciales almacenadas o solicitadas. Para obtener más información sobre cómo solucionar este problema, consulte [Especificar información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
### <a name="an-error-has-occurred-while-establishing-a-connection-to-the-server"></a>Error al establecer una conexión al servidor.  
La causa del problema en la conexión a SQL Server puede deberse a que SQL Server no permite conexiones remotas en su configuración predeterminada. (proveedor: Proveedor de canalizaciones con nombre; error: 40 - No se pudo abrir una conexión a SQL Server). La instancia del motor de base de datos que hospeda la base de datos del servidor de informes devuelve este error. En la mayoría de los casos, este error se produce porque se detiene el servicio SQL Server. O, si utiliza SQL Server Express con Advanced Services o una instancia con nombre, este error se producirá si la dirección URL del servidor de informes o la cadena de conexión para la base de datos del servidor de informes no son correctas. Para solucionar estos problemas, haga lo siguiente:  
  
* Compruebe si el servicio SQL Server (**MSSQLSERVER**) se ha iniciado. En el equipo que hospeda la instancia del motor de base de datos, haga clic en Inicio, Herramientas administrativas, Servicios y desplácese hasta SQL Server (**MSSQLSERVER**). Si no se ha iniciado, haga clic con el botón derecho en el servicio, seleccione Propiedades, en Tipo de inicio seleccione Automático; y haga clic en Aplicar, en Iniciar y, después, en Aceptar.   
* Compruebe que la dirección URL del servidor de informes y la cadena de conexión para la base de datos del servidor de informes son correctas. Si Reporting Services o el motor de base de datos se han instalado como una instancia con nombre, la cadena de conexión predeterminada que se cree durante la instalación incluirá el nombre de la instancia. Por ejemplo, si instala una instancia predeterminada de SQL Server Express con Advanced Services en un servidor denominado DEVSRV01, la dirección URL del portal web es DEVSRV01\Reports$SQLEXPRESS. Además, el nombre del servidor de bases de datos en la cadena de conexión se parecerá a DEVSRV01\SQLEXPRESS. Para obtener más información sobre las direcciones URL y cadenas de conexión de origen de datos para SQL Server Express, consulte [Reporting Services en SQL Server Express con Advanced Services](/previous-versions/sql/sql-server-2008-r2/ms365166(v=sql.105)). Si desea comprobar la cadena de conexión para la base de datos del servidor de informes, inicie la herramienta de configuración de Reporting Services y vea la página Instalación de base de datos.  
  
### <a name="a-connection-cannot-be-made-ensure-that-the-server-is-running"></a>No se puede establecer una conexión. Asegúrese de que el servidor se está ejecutando.  
Se trata de un error devuelto por el proveedor de ADOMD.NET. Existen varias razones por las que puede producirse este error. Si ha especificado el servidor como "localhost", pruebe a especificar el nombre de servidor en su lugar. Este error también puede producirse si no puede asignarse memoria a la nueva conexión. Para obtener más información, consulte [Knowledge Base Article 912017 - Error message when you connect to an instance of SQL Server 2005 Analysis Services:](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/912017)(Artículo 912017 de Knowledge Base: Mensaje de error al conectar una instancia de SQL Server 2005 Analysis Services).  
  
Si el error también incluye "Host desconocido", indica que el servidor de Analysis Services no está disponible o está rechazando la conexión. Si el servidor de Analysis Services está instalado como una instancia con nombre en un equipo remoto, es probable que tenga que ejecutar el servicio Explorador de SQL Server para obtener el número de puerto utilizado por dicha instancia.  
  
### <a name="report-services-soap-proxy-source"></a>(Origen de proxy SOAP de Reporting Services)  
Si recibe este error durante la generación de modelos de informe y la sección de información adicional indica que el servidor SQL Server no existe o se deniega el acceso, podrían darse las circunstancias siguientes:  
* La cadena de conexión para el origen de datos incluye "localhost".  
* TCP/IP está deshabilitado para el servicio de SQL Server.  
  
Para solucionar este error, puede modificar la cadena de conexión para que utilice el nombre del servidor o habilitar el protocolo TCP/IP para el servicio. Siga estos pasos para habilitar TCP/IP:  
  
1. Inicie el Administrador de configuración de SQL Server.  
2. Expanda la opción **Configuración de red de SQL Server**.  
3. Seleccione **Protocolos de MSSQLSERVER**.  
4. Haga clic con el botón derecho en **TCP/IP** y seleccione **Habilitar**.  
5. Seleccione **Servicios de SQL Server**.  
6. Haga clic con el botón derecho en **SQL Server (MSSQLSERVER)** y seleccione **Reiniciar**.  
  
## <a name="wmi-error-when-connecting-to-a-report-server-in-management-studio"></a>Error de WMI al conectar con un servidor de informes en Management Studio  
De manera predeterminada, Management Studio utiliza el proveedor del Instrumental de administración de Windows (WMI) de Reporting Services para establecer una conexión con el servidor de informes. Si el proveedor de WMI no se instala correctamente, al intentar conectarse al servidor de informes obtendrá el error siguiente:  
  
No se puede conectar a \<your server name>. El proveedor de WMI de Reporting Services no está instalado o no está configurado correctamente (Microsoft.SqlServer.Management.UI.RSClient).  
  
Para resolver este error, debe reinstalar el software. En todos los demás casos, como solución temporal, puede conectarse al servidor de informes a través del extremo SOAP:  
  
* En el cuadro de diálogo **Conectar con el servidor** de Management Studio, en **Nombre del servidor**, escriba la dirección URL del servidor de informes. De forma predeterminada, es `https://<your server name>/reportserver`. Si utiliza SQL Server 2008 Express con Advanced Services, es `https://<your server name>/reportserver$sqlexpress`.  
  
Para resolver el error de forma que pueda conectarse mediante el proveedor de WMI, debe ejecutar el programa de instalación para reparar Reporting Services o volver a instalar Reporting Services.  
  
## <a name="connection-error-where-login-failed-due-to-unknown-user-name-or-bad-password"></a>Error de conexión, donde no se logró iniciar sesión debido a un nombre de usuario desconocido o una contraseña incorrecta  
Se puede producir un error **rsReportServerDatabaseLogonFailed** si está usando una cuenta de dominio para la conexión del servidor de informes a la base de datos del servidor de informes y ha cambiado la contraseña de la cuenta de dominio.   
  
El texto completo del error es: "El servidor de informes no puede abrir una conexión a la base de datos del servidor de informes. No se pudo iniciar sesión. (**rsReportServerDatabaseLogonFailed**). Error de inicio de sesión: nombre de usuario desconocido o contraseña incorrecta".  
  
Si restablece la contraseña, debe actualizar la conexión. Para más información, vea [Configurar una conexión a la base de datos del servidor de informes (Administrador de configuración del servidor de informes)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="the-report-server-cannot-open-a-connection-to-the-report-server-database-rsreportserverdatabaseunavailable"></a>El servidor de informes no puede abrir una conexión a la base de datos del servidor de informes. (rsReportServerDatabaseUnavailable).  
Mensaje completo: El servidor de informes no puede abrir una conexión a la base de datos del servidor de informes. Se necesita una conexión a la base de datos para todas las solicitudes y procesos" (rsReportServerDatabaseUnavailable)  
Este error se produce cuando el servidor de informes no puede conectarse a la base de datos relacional de SQL Server que proporciona almacenamiento interno al servidor. La conexión a la base de datos del servidor de informes se administra a través de la herramienta de configuración de Reporting Services. Puede ejecutar la herramienta, ir a la página Instalación de base de datos y corregir la información de conexión. Se recomienda utilizar la herramienta para actualizar la información de conexión; esta herramienta garantiza la actualización de los valores dependientes y el reinicio de los servicios. Para obtener más información, consulte [Configurar una conexión a la base de datos del servidor de informes (Administrador de configuración de SSRS)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md) y [Configurar la cuenta de servicio del servidor de informes (Administrador de configuración de SSRS)](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
Este error también puede producirse si la instancia del motor de base de datos donde se hospeda la base de datos del servidor de informes no está configurada para las conexiones remotas. La conexión remota está habilitada de manera predeterminada en algunas ediciones de SQL Server. Para comprobar si está habilitada en la instancia de motor de base de datos de SQL Server que esté usando, ejecute la herramienta Administrador de configuración de SQL Server. Debe habilitar TCP/IP y canalizaciones con nombre. Un servidor de informes utiliza ambos protocolos. Para obtener instrucciones sobre la forma de habilitar conexiones remotas, consulte la sección "Configurar las conexiones remotas a la base de datos del servidor de informes" del artículo [Configurar un servidor de informes para la administración remota](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).  
  
Si el error incluye el siguiente texto adicional, significa que la contraseña ha expirado en la cuenta utilizada para ejecutar la instancia del motor de base de datos: "An error has occurred while establishing a connection to the server (Se ha producido un error al establecer una conexión al servidor). When connecting to SQL Server, this failure may be caused by the fact that under the default settings SQL Server does not permit remote connections (La causa del problema en la conexión a SQL Server puede deberse a que SQL Server no permite conexiones remotas en su configuración predeterminada). (**proveedor: Interfaces de red SQL Server, error: 26 - Error al buscar el servidor/instancia especificados)** ". Para resolver este error, restablezca la contraseña.   
  
## <a name="rpc-server-is-not-listening"></a>"El servidor RPC no está en línea"  
El servicio del servidor de informes usa el servidor de llamadas a procedimiento remoto (RPC) para algunas operaciones. Si obtiene el error "El servidor RPC no está en línea", compruebe que el servicio del servidor de informes se está ejecutando.  
  
## <a name="unexpected-error-general-network-error"></a>Error inesperado (Error general de red)  
Indica un error de conexión a origen de datos. Debe comprobar la cadena de conexión y asegurarse de que tiene permiso de acceso al origen de datos. Si utiliza la autenticación de Windows para tener acceso a un origen de datos, debe poseer permiso de acceso al equipo que lo hospeda.  
  
## <a name="unable-to-grant-database-access-in-sharepoint-central-administration"></a>No se puede conceder acceso a bases de datos en Administración central de SharePoint  
Cuando se ha configurado Reporting Services para integrarse con un producto o tecnología de SharePoint en Windows Vista o Windows Server 2008, se puede recibir el mensaje de error siguiente al intentar conceder acceso en la página **Conceder acceso a la base de datos** de Administración central de SharePoint: "No se puede establecer conexión con el equipo".  
  
Esto sucede porque Control de cuentas de usuario (UAC) de Windows Vista o Windows Server 2008 requiere la aceptación explícita de un administrador para elevar y utilizar el token de administrador al realizar tareas que requieren permisos de administrador. Sin embargo, en este caso, el servicio de Administración de Windows SharePoint Services no se puede elevar para conceder a la cuenta o cuentas de servicio de Reporting Services acceso a las bases de datos de contenido y configuración de SharePoint.  
  
En SQL Server 2008 Reporting Services, solo la cuenta de servicio del servidor de informes requiere acceso a bases de datos; en SQL Server 2005 Reporting Services SP2, tanto la cuenta de servicio Windows del servidor de informes como la cuenta de servicio web del servidor de informes requieren acceso a bases de datos. Para obtener más información sobre la cuenta de servicio del servidor de informes en SQL Server 2008, consulte Cuenta de servicio (Configuración de Reporting Services).  
  
Hay dos soluciones para este problema.   
1.  En la primera, puede desactivar temporalmente UAC y usar Administración central de SharePoint para conceder acceso.  
> [!IMPORTANT]  
> Tenga precaución si desactiva UAC para solucionar este problema y activa UAC inmediatamente después de conceder acceso a bases de datos en Administración central de SharePoint. Si no desea desactivar UAC, utilice la segunda solución que se proporciona en esta sección. Para obtener información sobre UAC, vea la documentación del producto de Windows.  
2. En la segunda, puede conceder a la cuenta o cuentas de servicio de Reporting Services acceso a bases de datos manualmente. Puede utilizar el procedimiento siguiente para conceder acceso agregando la cuenta o cuentas de servicio de Reporting Services a los roles de base de datos y el grupo de Windows correcto. Este procedimiento se aplica a la cuenta de servicio del servidor de informes en SQL Server 2008 Reporting Services; si se ejecuta SQL Server 2005 Reporting Services, se ha de realizar el procedimiento para la cuenta de servicio Windows del servidor de informes y la cuenta de servicio web del servidor de informes.   
  
### <a name="to-manually-grant-database-access"></a>Para conceder acceso a bases de datos manualmente  
  
1. Agregue la cuenta de servicio del servidor de informes al grupo de Windows WSS_WPG en el equipo de Reporting Services.  
2. Establezca conexión con la instancia de base de datos que hospeda las bases de datos de contenido y configuración de SharePoint, y cree un inicio de sesión de base de datos SQL para la cuenta de servicio del servidor de informes.  
3. Agregue el inicio de sesión de base de datos SQL a los roles de base de datos siguientes:  
  
* Rol db_owner en la base de datos WSS_Content  
* Rol WSS_Content_Application_Pools en la base de datos SharePoint_Config  
  
## <a name="unable-to-connect-to-the-reports-and-reportserver-directories-when-the-report-server-databases-are-created-on-a-virtual-sql-server-that-runs-in-a-microsoft-cluster-services-mscs-cluster"></a>No se puede conectar con los directorios /reports y /reportserver cuando las bases de datos del servidor de informes se crean en un servidor SQL Server virtual que se ejecuta en un clúster de Servicios de Cluster Server de Microsoft (MSCS).  
Al crear las bases de datos del servidor de informes, **ReportServer** y **ReportServerTempDB**, en un servidor SQL Server virtual que se ejecuta en un clúster de MSCS, es posible que el nombre remoto con formato `<domain>\<computer_name>$` no se registre en SQL Server como inicio de sesión. Si ha configurado la cuenta de servicio del servidor de informes como una cuenta que requiere este nombre remoto para las conexiones, los usuarios no pueden conectar con los directorios /reports y /reportserver en Reporting Services. Por ejemplo, la cuenta de Windows integrada NetworkService requiere este nombre remoto. Para evitar este problema, utilice una cuenta de dominio explícita o un inicio de sesión de SQL Server para conectar con las bases de datos del servidor de informes.  
    
  ## <a name="see-also"></a>Consulte también  
[Compatibilidad del explorador de Reporting Services y Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[Errores y eventos (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
[Solución de problemas de recuperación de datos de informes de Reporting Services](../../reporting-services/troubleshooting/troubleshoot-data-retrieval-issues-with-reporting-services-reports.md)  
[Solución de problemas de suscripciones y entrega de Reporting Services](../../reporting-services/troubleshooting/troubleshoot-reporting-services-subscriptions-and-delivery.md)  
  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect-md.md)]
