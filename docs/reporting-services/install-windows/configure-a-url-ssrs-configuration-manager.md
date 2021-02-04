---
description: Configuración de una URL (Administrador de configuración del servidor de informes)
title: Configuración de una dirección URL (Configuration Manager) | Microsoft Docs
ms.date: 05/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- URL access [Reporting Services], syntax
ms.assetid: 851e163a-ad2a-491e-bc1e-4df92327092f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7a58a11e71c471bf02ca68ddaf4a416c3d9c554b
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075707"
---
# <a name="configure-a-url--report-server-configuration-manager"></a>Configuración de una URL (Administrador de configuración del servidor de informes)
  Para poder usar el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] o el servicio web del servidor de informes, debe configurar al menos una dirección URL para cada aplicación. Configurar las direcciones URL es obligatorio si ha instalado [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en el modo de "solo archivos" (es decir, al seleccionar la opción **Install but do not configure the server (Instalar pero no configurar el servidor)** en la página Opciones de instalación del servidor de informes del Asistente para la instalación). Si instaló [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en la configuración predeterminada, las direcciones URL ya están configuradas para cada aplicación.  
  
 Utilice la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para configurar las direcciones URL. Todas las partes de la dirección URL se definen en esta herramienta. A diferencia de las versiones anteriores, los sitios web de Internet Information Services (IIS) ya no proporcionan acceso a las aplicaciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] proporciona los valores predeterminados que mejor funcionan en la mayoría de escenarios de implementación, incluso en las implementaciones en paralelo con otros servicios web y aplicaciones. Las direcciones URL predeterminadas incorporan nombres de instancia, con lo que se reduce el riesgo de que se produzcan conflictos de direcciones URL si ejecuta varias instancias del servidor de informes en el mismo equipo.  
  
 Este tema proporciona instrucciones para las tareas siguientes:  
  
-   Crear una dirección URL para el servicio web del servidor de informes.  
  
-   Crear una dirección URL para el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)].  
  
-   Establecer propiedades avanzadas de las direcciones URL para definir más direcciones URL.  
  
 Para más información sobre cómo se almacenan y mantienen las direcciones URL o sobre problemas de interoperabilidad, vea [Acerca de las reservas y el registro de reservas de URL &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md) e [Instalar Reporting Services e Internet Information Services en paralelo &#40;modo nativo de SSRS&#41;](../../reporting-services/install-windows/install-reporting-and-internet-information-services-side-by-side.md). Para revisar ejemplos de direcciones URL que suelen usarse en una instalación de Reporting Services, vea [Ejemplos de direcciones URL](#URLExamples) en este tema.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Antes de crear o modificar una dirección URL, recuerde los puntos siguientes:  
  
-   Debe ser miembro del grupo local de administradores en el equipo del servidor de informes.  
  
-   Si IIS está instalado en el mismo equipo, compruebe los nombres de los directorios virtuales de cualquier sitio web que use el puerto 80. Si ve algún directorio virtual que utilice los nombres de directorios virtuales predeterminados de Reporting Services (es decir, "Reports" y "ReportServer"), elija otros nombres de directorios virtuales para las direcciones URL de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que configure.  
  
-   Debe utilizar la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para configurar la dirección URL. No utilice una utilidad de sistema. No modifique nunca directamente las reservas de direcciones URL en la sección **URLReservations** del archivo RSReportServer.config. El uso de la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] es necesario para actualizar la reserva de direcciones URL subyacente que está almacenada internamente y para sincronizar la configuración de direcciones URL que se almacena en el archivo RSReportServer.config.  
  
-   Elija una hora en la que haya poca actividad con los informes. Cada vez que la reserva de direcciones URL cambie, puede esperar que los dominios de aplicación para el servicio web del servidor de informes y el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] se puedan reciclar.  
  
-   Para obtener información general de la construcción de la dirección URL y el uso de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Configurar las direcciones URL del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md).  
  
### <a name="to-configure-a-url-for-the-report-server-web-service"></a>Para configurar una dirección URL para el servicio web del servidor de informes  
  
1.  Inicie la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y conéctese a una instancia local del servidor de informes  
  
2.  Haga clic en **Dirección URL del servicio web**.  
  
3.  Especifique el directorio virtual. El nombre del directorio virtual identifica qué aplicación recibe la solicitud. Dado que varias aplicaciones pueden compartir una dirección IP y el puerto, el nombre del directorio virtual especifica qué aplicación recibe la solicitud.  
  
     Este valor debe ser único para asegurarse de que la solicitud alcanza el destino pretendido. Este valor es necesario. No distingue entre mayúsculas y minúsculas. Hay una correspondencia uno a uno entre un nombre de directorio virtual y una instancia de una aplicación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si crea varias direcciones URL para la misma instancia de aplicación, debe utilizar el mismo nombre de directorio virtual en todas las direcciones URL que defina para esta instancia de aplicación.  
  
     Para el servicio web del servidor de informes, el nombre del directorio virtual predeterminado es **ReportServer**.  
  
4.  Especifique la dirección IP que identifique de forma exclusiva el equipo del servidor de informes en la red. Si desea especificar un encabezado de host o definir más direcciones URL para la misma instancia de aplicación, debe hacer clic en **Avanzadas**. Para obtener instrucciones de cómo establecer las propiedades avanzadas de la dirección URL, consúltelas posteriormente en este tema. De lo contrario, utilice la página **Dirección URL del servicio web** para seleccionar entre los valores siguientes:  
  
    -   **Todas asignadas** especifica que cualquiera de las direcciones IP que están asignadas al equipo se puede utilizar en una dirección URL que señale a una aplicación de servidor de informes. Este valor también abarca nombres de host descriptivos (como nombres de equipo) que un servidor de nombres de dominio puede resolver en una dirección IP que se asigna al equipo. Éste es el valor predeterminado de una dirección URL de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
    -   **Todas sin asignar** especifica que el servidor de informes recibirá cualquier solicitud no administrada por otra aplicación. Recomendamos que evite esta opción. Si selecciona esta opción, es posible que otra aplicación que tenga una reserva de direcciones URL más fuertes intercepte las solicitudes destinadas al servidor de informes.  
  
    -   **127.0.0.1** es la dirección IPv4 que se usa para tener acceso al host local. Admite la administración local en el equipo del servidor de informes. Si selecciona solo este valor, solo los usuarios que hayan iniciado sesión localmente en el equipo servidor de informes tendrá acceso a la aplicación.  
  
    -   **::1** es la dirección de retorno con el formato IPv6.  
  
    -   Las direcciones IP concretas también aparecen en esta lista. Las direcciones IP pueden estar en los formatos IPv4 e IPv6. *Nnn.nnn.nnn.nnn* es la dirección IPv4 de 32 bits de una tarjeta adaptadora de red del equipo. Las direcciones IPv6 son de 128 bits, con ocho campos de 4 bytes separados por dos puntos: \<prefix>:*nnnn:nnnn:nnnn:nnnn:nnnn:nnnn*  
  
         Si tiene varias tarjetas o si la tarjeta de red admite tanto direcciones IPv4 como direcciones IPv6, verá varias direcciones IP. Si selecciona solo una dirección IP, limitará el acceso de la aplicación únicamente a la dirección IP (y a cualquier nombre de host que un servidor de nombres de dominio asigne a esa dirección). No puede utilizar el host local para tener acceso a un servidor de informes y no puede utilizar las direcciones IP de otras tarjetas de adaptadores de red que estén instalados en el equipo del servidor de informes. Normalmente, si selecciona este valor, es porque está configurando varias reservas de direcciones URL que también especifican direcciones IP explícitas o nombres de host (por ejemplo, uno para una tarjeta de un adaptador de red que se use para las conexiones de intranet y un segundo que se use para las conexiones de extranet).  
  
5.  Especifique el puerto. El puerto 80 es el valor predeterminado porque se puede compartir con otras aplicaciones. Si desea utilizar un número de puerto personalizado, recuerde que tendrá que especificarlo siempre en la dirección URL que se usa para tener acceso al servidor de informes. Puede utilizar las técnicas siguientes para buscar un puerto disponible:  
  
    -   Desde un símbolo del sistema, escriba el comando siguiente para devolver una lista de los puertos TCP que se estén utilizando:  
  
         `netstat -anp tcp`  
  
    -   Revise el artículo de soporte técnico de Microsoft, [Información acerca de las asignaciones de puertos TCP/IP](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/174904), para leer sobre las asignaciones de puertos TCP y las diferencias entre los puertos conocidos (0 a 1023), los puertos registrados (1024 a 49151) y los puertos dinámicos o los privados (49152 a 65535).  
  
    -   Si usa Firewall de Windows, debe abierto el puerto. Para obtener instrucciones, consulte [Configure a Firewall for Report Server Access](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
6.  Si aún no lo ha hecho, compruebe que IIS (si está instalado) no tiene un directorio virtual con el mismo nombre que piensa utilizar.  
  
7.  Si instaló un certificado TLS/SSL, puede seleccionarlo ahora para enlazar la dirección URL al certificado TLS/SSL que esté instalado en el equipo.  
  
8.  O bien, si selecciona un certificado TLS/SSL, puede especificar un puerto personalizado. El valor predeterminado es 443, pero puede utilizar cualquier puerto que esté disponible.  
  
9. Haga clic en **Aplicar** para crear la dirección URL.  
  
10. Pruebe la dirección URL haciendo clic en el vínculo en la sección **Direcciones URL** de la página. Observe que la base de datos del servidor de informes debe crearse y configurarse para poder probar la dirección URL. Para más información, vea [Crear una base de datos del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

> [!NOTE]
>  Si tiene enlaces TLS y reservas de direcciones URL existentes y desea cambiar el enlace TLS para, por ejemplo, usar otro certificado o encabezado de host, se recomienda que complete los pasos siguientes por orden:  
> 
>  1.  Quite primero todas las reservas de direcciones URL.  
> 2.  A continuación, quite todos los enlaces TLS.  
> 3.  Después, vuelva a crear las direcciones URL y los enlaces TLS.  
> 
>  El procedimiento anterior se puede realizar mediante el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
> 
>  Microsoft Windows admite un enlace para cada combinación de dirección IP y puerto. Si configura un servidor de informes de forma que use un valor de encabezado de host específico y el certificado de la combinación de puerto-dirección IP se envía también a un valor de encabezado de host diferente, aparecerá en el explorador una advertencia en la que se indica que el certificado no coincide con la dirección URL que se está usando.  
> 
>  Para corregir el problema, elimine todos los enlaces y, a continuación, cree nuevos enlaces con una configuración única, o configure los registros de direcciones URL de Reporting Services con caracteres comodín.
  
### <a name="to-create-a-url-reservation-for-the-ssrswebportal"></a>Para crear una reserva de dirección URL para el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]  
  
1.  Inicie la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y conéctese a la instancia del servidor de informes.  
  
2.  Haga clic en **Dirección URL del Portal web**.  
  
3.  Especifique el directorio virtual. El [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] escucha en la misma dirección IP y puerto que el servicio web del servidor de informes. Si ha configurado el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] para señalar a un servicio web del servidor de informes diferente, debe modificar la configuración de la dirección URL del [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] en el archivo RSReportServer.config.  
  
4.  Si ha instalado un certificado TLS/SSL, puede seleccionarlo para requerir que todas las solicitudes para [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] se enruten a través de HTTPS.  
  
     O bien, si selecciona un certificado TLS/SSL, puede especificar un puerto personalizado. El valor predeterminado es 443, pero puede utilizar cualquier puerto que esté disponible.  
  
5.  Haga clic en **Aplicar** para crear la dirección URL.  
  
6.  Pruebe la dirección URL haciendo clic en el vínculo en la sección **Direcciones URL** de la página.  
  
## <a name="setting-advanced-properties-to-specify-additional-urls"></a>Establecer las propiedades avanzadas para especificar direcciones URL adicionales  
 Puede reservar varias direcciones URL para el servicio web del servidor de informes o el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] al especificar puertos diferentes o nombres de host (una dirección IP o un nombre de encabezado de host que un servidor de nombres de dominio pueda resolver como una dirección IP asignada al equipo). Si se crean varias direcciones URL, puede establecer rutas de acceso diferentes a la misma instancia del servidor de informes. Por ejemplo, para permitir el acceso desde la intranet y la extranet a un servidor de informes, podría utilizar la dirección URL predeterminada para el acceso a través de la intranet y un nombre de host completo adicional para el acceso desde la extranet:  
  
-   `https://myserver01/reportserver`  
  
-   `https://www.adventure-works.com/reportserver`  
  
 No puede establecer varios nombres de directorios virtuales para la misma instancia de aplicación. A cada instancia de aplicación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se le asigna un único nombre de directorio virtual. Si tiene varias instancias de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en el mismo equipo, el nombre del directorio virtual para una aplicación debería incluir el nombre de instancia para asegurarse de que cada solicitud llega a su destino pretendido.  
 
  **Encabezado de host**  
 Si ya tiene definido un encabezado de host en un servidor de nombres de dominio que se resuelva como su equipo, puede especificar ese encabezado de host en una dirección URL que configure para el acceso al servidor de informes.  
  
 Un encabezado de host es un nombre único que permite que varios sitios web compartan una única dirección IP y puerto. Los nombres de encabezado de host son más fáciles de recordar y escribir que los números de dirección IP y puerto. Un ejemplo de nombre de encabezado de host podría ser www.adventure-works.com.  
  
 **Puerto SSL**  
 Especifica el puerto para las conexiones TLS/SSL. El puerto predeterminado para TLS es 443.  
  
 **Certificado SSL**  
 Especifica el nombre del certificado de un certificado TLS/SSL que instaló en este equipo. Si el certificado se asigna a un carácter comodín, puede utilizarlo para una conexión de servidor de informes.  
  
 Especifica el nombre completo del equipo para el que se registra el certificado. El nombre que se especifica debe ser idéntico al nombre para el que se registra el certificado.  
  
 Para utilizar esta opción debe tener instalado un certificado. También debe modificar la opción de configuración UrlRoot del archivo RSReportServer.config de manera que especifique el nombre completo del equipo para el que se ha registrado el certificado. Para más información, vea [Configurar conexiones TLS en un servidor de informes en modo nativo](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
### <a name="to-set-advanced-properties-on-a-url"></a>Para establecer propiedades avanzadas en una dirección URL  
  
1.  En la página **Dirección URL del servicio web** o **Dirección URL del Portal web** , haga clic en **Avanzadas**.  
  
2.  Haga clic en **Agregar**.  
  
3.  Haga clic en la dirección IP o en el nombre de encabezado de host. Si especifica un encabezado de host, asegúrese de especificar un nombre que el servicio DNS pueda resolver. Si está especificando el nombre de dominio disponible públicamente, incluya la dirección URL completa, incluido `https://www`.  
  
4.  Especifique el puerto. Si especifica un puerto personalizado, la dirección URL de la aplicación siempre debe incluir el número de puerto.  
  
5.  Haga clic en **OK**.  
  
6.  Para probar la dirección URL, abra una ventana del explorador y escriba la dirección URL.  
  
## <a name="urls-for-multiple-report-server-instances-on-the-same-computer"></a>Direcciones URL para varias instancias del servidor de informes en el mismo equipo  
 Si está reservando direcciones URL para varias instancias de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], debería seguir las convenciones de nomenclatura para poder evitar conflictos de nombres. Para más información, vea [Reservas de direcciones URL para implementaciones del servidor de informes de varias instancias &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/url-reservations-for-multi-instance-report-server-deployments.md).  
  
##  <a name="examples-of-url-configurations"></a><a name="URLExamples"></a> Ejemplos de configuraciones de direcciones URL  
 En la lista siguiente se muestran algunos ejemplos de la apariencia que puede tener una dirección URL del servidor de informes:  
  
-   `https://localhost/reportserver`  
  
-   `https://localhost/reportserver_SQLEXPRESS`  
  
-   `https://sales01/reportserver`  
  
-   `https://sales01:8080/reportserver`  
  
-   `https://sales.adventure-works.com/reportserver`  
  
-   `https://www.adventure-works.com:8080/reportserver01`  
  
 Las direcciones URL que se usan para obtener acceso al [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] comparten un formato similar y suelen crearse en el mismo sitio web que hospeda al servidor de informes. La única diferencia es el nombre del directorio virtual (en este caso es **reports** , pero se puede configurar para que se use el nombre que se prefiera):  
  
-   `https://localhost/reports`  
  
-   `https://localhost/reports_SQLEXPRESS`  
  
-   `https://sales01/reports`  
  
-   `https://sales01:8080/reports`  
  
-   `https://sales.adventure-works.com/reports`  
  
-   `https://www.adventure-works.com:8080/reports`  
  
## <a name="see-also"></a>Consulte también

 [Configurar las direcciones URL del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
