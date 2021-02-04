---
description: Configuración de conexiones TLS en un servidor de informes en modo nativo
title: Configuración de conexiones TLS en un servidor de informes en modo nativo | Microsoft Docs
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Secure Sockets Layer (SSL)
ms.assetid: 212f2042-456a-4c0a-8d76-480b18f02431
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0532c42a5bd080b7f686c1663c6f88d8066bc9ff
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076267"
---
# <a name="configure-tls-connections-on-a-native-mode-report-server"></a>Configuración de conexiones TLS en un servidor de informes en modo nativo

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] El modo nativo usa el servicio HTTP SSL (Capa de sockets seguros) para establecer conexiones cifradas con un servidor de informes. La Seguridad de la capa de transporte (TLS) se conocía antes como Capa de sockets seguros (SSL). Si tiene el archivo de certificado (.cer) instalado en un almacén de certificados local en el equipo del servidor de informes, puede enlazar el certificado a una reserva de direcciones URL de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para admitir las conexiones con el servidor de informes en un canal cifrado.  
  
> [!TIP]  
>  Si usa el modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vea la documentación de SharePoint para obtener más información. Por ejemplo, la entrada del blog [Habilitación de TLS en una aplicación web de SharePoint 2010](/archive/blogs/sowmyancs/how-to-enable-ssl-on-a-sharepoint-2010-web-application).  
  
 Como Internet Information Services (IIS) también usa HTTP SSL, hay importantes problemas de interoperabilidad que es necesario tener en cuenta si IIS y [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se ejecutan en el mismo equipo. Asegúrese de revisar la sección Problemas de interoperabilidad con IIS para obtener información sobre cómo resolver estos problemas.  
  
## <a name="server-certificate-requirements"></a>Requisitos de certificado de servidor  
 Debe tener un certificado de servidor instalado en el equipo (no se admiten los certificados de cliente). Reporting Services no proporciona funciones para solicitar, generar, descargar o instalar un certificado. Windows Server 2012 y las versiones posteriores proporcionan un complemento Certificados que se puede usar para pedir un certificado de una entidad de certificación de confianza.  
  
 En esta prueba, puede generar un certificado localmente. Si usa la utilidad **MakeCert** y el comando de ejemplo como una plantilla, asegúrese de especificar el nombre del servidor como host y quite todos los saltos de línea antes de ejecutar el comando. Si ejecuta el comando en una ventana DOS, puede que tenga que aumentar el tamaño de búfer de la ventana para que pueda contener todo el comando.  
  
 Si ejecuta IIS y [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] conjuntamente en el mismo equipo, puede usar la aplicación de consola Administrador de IIS para instalar el certificado en el equipo. En Administrador de IIS se incluyen opciones para crear y empaquetar un archivo de solicitud de certificado (.crt) para su posterior procesamiento por parte una entidad de certificación de confianza. La entidad de certificación que use generará un archivo de certificado (.cer) y se lo enviará de vuelta. Puede utilizar la Consola de administración de IIS para instalar el archivo de certificado en el almacén local. Para obtener más información, vea [Using SSL to Encrypt Confidential Data](/previous-versions/windows/it-pro/windows-server-2003/cc738495(v=ws.10)) en Technet.  
  
## <a name="interoperability-issues-with-iis"></a>Problemas de interoperabilidad con IIS  
 La presencia de IIS en el mismo equipo que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] afectará significativamente a las conexiones TLS con un servidor de informes:  
  
-   Si IIS está instalado, el servicio World Wide Web (W3SVC) siempre debe estar ejecutándose. El servicio HTTP SSL establecerá una dependencia con IIS si detecta que IIS se está ejecutando. Esto significa que el servicio World Wide Web (W3SVC) tiene que ejecutarse siempre que IIS y [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] estén instalados en el mismo equipo y se configuren direcciones URL de servidor de informes para las conexiones TLS.  
  
-   Si desinstala IIS, puede interrumpir temporalmente el servicio para una dirección URL de servidor de informes enlazada a TLS. Por esta razón, se recomienda reiniciar el equipo después de desinstalar IIS.  
  
     Es necesario reiniciar el equipo para borrar todas las sesiones TLS de la memoria caché. Algunos sistemas operativos almacenan en memoria caché las sesiones TLS hasta diez horas, con lo que una dirección URL https:// continúa funcionando incluso después de quitar el enlace TLS de la reserva de direcciones URL en HTTP.SYS. Al reiniciar el equipo, se cierra cualquier conexión abierta que utilice el canal.  
  
## <a name="bind-tls-to-a-reporting-services-url-reservation"></a>Enlace de TLS a una reserva de direcciones URL de Reporting Services  
 Los pasos siguientes no incluyen instrucciones para solicitar, generar, descargar o instalar un certificado. Debe tener un certificado instalado y disponible para usarlo. Puede elegir libremente las propiedades del certificado que especifique, la entidad de certificación de la que lo obtenga y las herramientas y utilidades que use para solicitar e instalar el certificado.  
  
 Puede utilizar la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para enlazar el certificado. Si el certificado está instalado correctamente en el almacén del equipo local, la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] lo detectará y lo mostrará en la lista **Certificados SSL** en las páginas **Dirección URL del servicio web** y **Dirección URL del Portal web**.  
  
### <a name="to-configure-a-report-server-url-for-tls"></a>Para configurar una dirección URL del servidor de informes para TLS  
  
1.  Inicie la herramienta Configuración de Reporting Services y conéctese al servidor de informes.  
  
2.  Seleccione **Dirección URL del servicio web**.  
  
3.  Expanda la lista de certificados TLS/SSL. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] detecta los certificados de autenticación del servidor en el almacén local. Si instaló un certificado y no lo ve en la lista, puede que tenga que reiniciar el servicio. Puede utilizar los botones **Detener** e **Iniciar** en la página **Estado del servidor de informes** en la herramienta Configuración de Reporting Services para reiniciar el servicio (página superior).  
  
4.  Seleccione el certificado.  
  
5.  Haga clic en **Aplicar**.  
  
6.  Haga clic en la dirección URL para comprobar que funciona.  
  
 La configuración de la base de datos del servidor de informes es un requisito para probar la dirección URL. Si aún no ha creado la base de datos del servidor de informes, hágalo antes de probar la dirección URL.  
  
 Las reservas de direcciones URL de la dirección URL del portal web y la dirección URL de los servicios web del servidor de informes se configuran de forma independiente. Si también desea configurar el acceso del portal web a través de un canal cifrado con TLS, continúe con los pasos siguientes:  
  
1.  Acceda a **Dirección URL del Portal web**.
  
2.  Seleccione **Advanced** (Avanzadas).  
  
3.  En **Varias identidades HTTPS para la característica actual de Reporting Services** , seleccione **Agregar**.  
  
4.  Seleccione el certificado, luego **Aceptar** y, por último, **Aplicar**.  
  
5.  Pruebe la dirección URL para verificar que funciona.  
  
## <a name="how-certificate-bindings-are-stored"></a>Cómo se almacenan los enlaces de certificados  
 Los enlaces de certificados se almacenarán en HTTP.SYS. Una representación de los enlaces que definió también se almacenará en la sección **URLReservations** del archivo RSReportServer.config. Los valores del archivo de configuración solo son una representación de los valores reales que se especifican en otro lugar. No modifique directamente los valores en el archivo de configuración. La configuración solo aparecerá en el archivo después de utilizar la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o el proveedor de Instrumental de administración de Windows (WMI) del servidor de informes para enlazar un certificado.  
  
> [!NOTE]  
>  Si configura un enlace con un certificado TLS/SSL en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y posteriormente desea quitar el certificado del equipo, asegúrese de quitar el enlace de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] antes de quitar el certificado del equipo. De lo contrario, no podrá quitar el enlace mediante la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o WMI y recibirá el error "Parámetro no válido". Si ya ha quitado el certificado del equipo, puede utilizar la herramienta Httpcfg.exe para quitar el enlace de HTTP.SYS. Para obtener más información sobre Httpcfg.exe, consulte la documentación del producto de Windows.  
  
 Los enlaces TLS son un recurso compartido en Microsoft Windows. Los cambios realizados por el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] u otras herramientas como el Administrador de IIS pueden afectar a otras aplicaciones ubicadas en el mismo equipo. Se recomienda usar la misma herramienta para editar los enlaces que usó para crearlos.  Por ejemplo, si creó enlaces TLS mediante Configuration Manager, se recomienda usar este administrador para configurar el ciclo de vida de los enlaces. Si utiliza el administrador de IIS para crear enlaces, se recomienda usar este administrador para administrar el ciclo de vida de los enlaces. Si IIS estaba instalado en el equipo antes de que se instalara [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], es recomendable que revise la configuración de TLS en IIS antes de configurar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Si quita enlaces TLS de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con el Administrador de configuración del servidor de informes, puede que los enlaces TLS ya no funcionen para los sitios web en un servidor que ejecute Internet Information Services (IIS) o en otro servidor HTTP.SYS. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Configuration Manager quita la siguiente clave del Registro: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443** Cuando se quita esta clave del Registro, también se quita el enlace TLS para IIS. Sin este enlace, TLS no se proporciona para el protocolo HTTPS. Para diagnosticar este problema, utilice el Administrador de IIS o la utilidad de línea de comandos HTTPCFG.exe. Para resolver el problema, restaure el enlace TLS para los sitios web mediante el Administrador de IIS. Para evitar este problema en el futuro, use el Administrador de IIS para quitar los enlaces TLS y luego vuelva a emplearlo para restaurar el enlace para los sitios web que quiera. Para más información, consulte el artículo de la base de conocimiento [SSL no funciona después de quitar un enlace SSL (https://support.microsoft.com/kb/956209/n)](https://web.archive.org/web/20150215042139/http://support.microsoft.com:80/kb/956209).  
  
## <a name="see-also"></a>Consulte también  
 [Autenticación con el servidor de informes](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Configurar y administrar un servidor de informes &#40;modo nativo de SSRS&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [El archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Configurar las direcciones URL del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
  
