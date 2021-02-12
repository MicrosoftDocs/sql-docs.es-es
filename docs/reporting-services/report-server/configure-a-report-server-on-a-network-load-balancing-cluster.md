---
title: Configurar un servidor de informes en un clúster con equilibrio de carga de red | Microsoft Docs
description: Aprenda a configurar la escalabilidad horizontal de un servidor de informes para ejecutarlo en NLB. Implemente una solución de clúster de equilibrio de carga de red para admitir una implementación escalada de Reporting Services.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 9e7a7b0dce01b21fcb746fdfdcbd5adfa7c7483d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067211"
---
# <a name="configure-a-report-server-on-a-network-load-balancing-cluster"></a>Configurar un servidor de informes en un clúster con equilibrio de carga de red

  Si va a configurar una ampliación escalada de un servidor de informes para ejecutarse en un clúster con equilibrio de carga de red (NLB), debe hacer lo siguiente:  
  
- Asegúrese de que el clúster NLB es accesible a través de un nombre de servidor virtual que se asigna a la dirección IP del servidor virtual. Un nombre de servidor virtual es necesario para poder configurar un único punto de entrada al clúster NLB. Cuando configure una dirección URL para cada instancia del servidor de informes, especificará el nombre del servidor virtual como host.  
  
- Configure la validación del estado de la vista para admitir la vista de los informes interactivos. Los informes interactivos se suelen representar varias veces durante una sesión de un solo usuario para visualizar datos nuevos o diferentes en respuesta a las acciones del usuario. Configurando la validación del estado de la vista, se preserva la continuidad dentro de la sesión de usuario independientemente del servidor de informes que atienda la solicitud real.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no proporciona funcionalidad para equilibrar la carga en una implementación escalada o para definir un punto único de acceso a través de una dirección URL compartida. Debe implementar una solución de clúster NLB de hardware o software independiente para admitir una implementación escalada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Puede instalar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en los nodos que ya formen parte de un clúster NLB, o puede configurar una implementación escalada primero e instalar el software del clúster después.  
  
## <a name="steps-for-report-server-deployment-on-an-nlb-cluster"></a>Pasos para la implementación del servidor de informes en un clúster NLB

 Siga estas instrucciones para instalar y configurar la implementación:  
  
|Paso|Descripción|Más información|  
|----------|-----------------|----------------------|  
|1|Antes de instalar Reporting Services en los nodos de servidor en un clúster NLB, compruebe los requisitos de la implementación escalada.|[Configurar una implementación escalada horizontalmente del servidor de informes en modo nativo](../install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)|  
|2|Configure el clúster NLB y compruebe si funciona correctamente.<br /><br /> Asegúrese de asignar un nombre de encabezado de host a la dirección IP del servidor virtual del clúster NLB. El nombre de encabezado de host se utiliza en la dirección URL del servidor de informes y es más fácil de recordar y escribir que una dirección IP.|Para obtener más información, consulte la documentación de producto de Windows Server correspondiente a la versión del sistema operativo Windows que se ejecute.|  
|3|Agregue NetBIOS y el nombre de dominio completo (FQDN) del encabezado de host a la lista de **BackConnectionHostNames** almacenada en el Registro de Windows.<br /><br /> Por ejemplo, si el nombre de encabezado de host \<MyServer> es un nombre virtual para el nombre de equipo Windows de "contoso", probablemente pueda hacer referencia al formato FQDN como "contoso.domain.com". Deberá agregar tanto el nombre de hostheader (MyServer) como el nombre FQDN (contoso.domain.com) a la lista de **BackConnectionHostNames**.  <br /><br /> Después, reinicie el equipo para asegurarse de que los cambios surten efecto.|Se requiere este paso si el entorno de servidor incluye la autenticación NTLM en el equipo local, creando una conexión de bucle invertido.<br /><br /> Si es así, notará que las solicitudes entre el Administrador de informes y el servidor de informes dan el error 401 (no autorizado).|  
|4|Instale [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en el modo de solo archivos en los nodos que ya formen parte de un clúster NLB y configure las instancias del servidor de informes para la implementación escalada.<br /><br /> La implementación escalada que configure podría no responder a las solicitudes que se dirijan a la dirección IP del servidor virtual. La configuración de la implementación escalada para utilizar la dirección IP del servidor virtual se produce en un paso posterior, después de configurar la validación del estado de la vista.|[Configuración de una implementación escalada horizontalmente del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)|  
|5|Configure la validación del estado de la vista.<br /><br /> Para obtener los mejores resultados, realice este paso después de configurar la implementación escalada y antes de configurar las instancias del servidor de informes para utilizar la dirección IP del servidor virtual. Al configurar la validación del estado de la vista primero, puede evitar las excepciones de la validación del estado con errores que se producen cuando los usuarios intentan tener acceso a informes interactivos.|[Cómo configurar la validación del estado de la vista](#ViewState) en este tema.|  
|6|Configure **Hostname** y **UrlRoot** para que usen la dirección IP del servidor virtual del clúster NLB.|[Cómo configurar Hostname y UrlRoot](#SpecifyingVirtualServerName) en este tema.|  
|7|Compruebe que los servidores son accesibles a través del nombre de host que especificó.|[Comprobar el acceso del servidor de informes](#Verify) en este tema.|  
  
## <a name="how-to-configure-view-state-validation"></a><a name="ViewState"></a> Cómo configurar la validación del estado de la vista

::: moniker range="=sql-server-2016"
Para ejecutar una implementación escalada en un clúster NLB, debe configurar la validación del estado de la vista para que los usuarios puedan ver los informes HTML interactivos.  Debe hacer esto para el servicio web del servidor de informes.
::: moniker-end

::: moniker range=">=sql-server-2017"
Para ejecutar una implementación escalada en un clúster NLB, debe configurar la validación del estado de la vista para que los usuarios puedan ver los informes HTML interactivos.
::: moniker-end
  
 ASP.NET controla la validación del estado de la vista. De forma predeterminada, la validación del estado de la vista está habilitada y utiliza la identidad del servicio web para realizarse. Sin embargo, en un escenario con clústeres NLB hay varias instancias de servicios e identidades de servicios web que se ejecutan en equipos diferentes. Dado que la identidad del servicio varía para cada nodo, no puede confiar en una única identidad del proceso para realizar la validación.  
  
 Para evitar este problema, puede generar una clave de validación arbitraria que admita la validación del estado de la vista y, después, configurar manualmente cada nodo del servidor de informes para que utilice la misma clave. Puede utilizar cualquier secuencia hexadecimal generada de forma aleatoria. El algoritmo de validación (como SHA1) determina la longitud que debe tener la secuencia hexadecimal.  

::: moniker range="=sql-server-2016"

1. Genere una clave de validación y una clave de descifrado utilizando la funcionalidad de generación automática que proporciona [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Al final, debe tener una única entrada <`machineKey`> que puede pegar en el archivo Web.config para cada instancia del servidor de informes de la implementación de escala horizontal.  
  
    En el ejemplo siguiente se ilustra el valor que se debe obtener. No copie el ejemplo en sus archivos de configuración; los valores de las claves no son válidos.  
  
    ```xml
    <machineKey ValidationKey="123455555" DecryptionKey="678999999" Validation="SHA1" Decryption="AES"/>  
    ```  
  
2. Abra el archivo Web.config para el servidor de informes y, en la sección <`system.web`>, pegue el elemento <`machineKey`> que ha generado. De forma predeterminada, el archivo Web.config del Administrador de informes se encuentra en \Archivos de programa\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\Reportserver\Web.config.  
  
3. Guarde el archivo.  
  
4. Repita el paso anterior en cada servidor de informes de la implementación escalada.  
  
5. Compruebe que todos los archivos Web.Config de las carpetas \Reporting Services\Reportserver contienen elementos <`machineKey`> idénticos en la sección <`system.web`>.  

::: moniker-end

::: moniker range=">=sql-server-2017"

1. Genere una clave de validación y una clave de descifrado utilizando la funcionalidad de generación automática que proporciona [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Al final, debe tener una única entrada \<**MachineKey**> que podrá pegar en el archivo RSReportServer.config para cada instancia del servidor de informes de la implementación escalada.

    En el ejemplo siguiente se ilustra el valor que se debe obtener. No copie el ejemplo en sus archivos de configuración; los valores de las claves no son válidos. El servidor de informes requiere el uso correcto de mayúsculas y minúsculas.

    ```xml
    <MachineKey ValidationKey="123455555" DecryptionKey="678999999" Validation="SHA1" Decryption="AES"/>
    ```

2. Guarde el archivo.

3. Repita el paso anterior en cada servidor de informes de la implementación escalada.  

4. Compruebe que todos los archivos RSReportServer.config de las carpetas que forman parte de \Reporting Services\Report Server contengan elementos \<**MachineKey**> idénticos.

::: moniker-end

## <a name="how-to-configure-hostname-and-urlroot"></a><a name="SpecifyingVirtualServerName"></a> Cómo configurar Hostname y UrlRoot

 Para configurar una implementación escalada del servidor de informes en un clúster NLB, debe definir un nombre único del servidor virtual que proporcione un solo punto de acceso al clúster de servidores. A continuación, registre este nombre de servidor virtual con el Servidor de nombres de dominio (DNS) del entorno.  
  
 Después de definir el nombre del servidor virtual, puede configurar las propiedades **Hostname** y **UrlRoot** en el archivo RSReportServer.config para incluir el nombre del servidor virtual en la dirección URL del servidor de informes.  
  
 Configure la propiedad **Hostname** cuando esté utilizando las reservas de direcciones URL comodín en el entorno de informes. Al especificar la propiedad **Hostname** como el nombre de servidor virtual del servidor NLB, el tráfico de red para el entorno de informe se dirige al servidor NLB. A continuación, el NLB distribuye las solicitudes entre los nodos del servidor de informes.  
  
 Además, configure la propiedad **UrlRoot** para que los vínculos de informe funcionen en los informes que se hayan exportado a informes estáticos, como en formato de Excel o PDF, o en informes que generen las suscripciones, como las de correo electrónico.  
  
 Si integra [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] 3.0 u [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007, u hospeda los informes en una aplicación web personalizada, es posible que únicamente tenga que configurar la propiedad **UrlRoot** . En este caso, configure la propiedad **UrlRoot** para que sea la dirección URL del sitio de SharePoint o aplicación web. Esto dirigirá el tráfico de red para el entorno de informe a la aplicación que administra los informes en lugar de al servidor de informes o al clúster NLB.  
  
 No modifique **ReportServerUrl**. Si modifica esta dirección URL, añadirá un viaje de ida y vuelta adicional a través del servidor virtual cada vez que se administre una solicitud interna. Para más información, vea [Direcciones URL en archivos de configuración &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md). Para más información sobre cómo editar el archivo de configuración, vea [Modificar un archivo de configuración de Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
1. Abra RSReportServer.config en un editor de texto.  
  
2. Busque la sección **\<Service>** y agregue la información siguiente al archivo de configuración; reemplace el valor **Hostname** por el nombre de servidor virtual del servidor NLB:  
  
    ```xml
    <Hostname>virtual_server</Hostname>  
    ```  
  
3. Busque **UrlRoot**. El elemento no está especificado en el archivo de configuración, pero el valor predeterminado que se usa es una dirección URL con este formato: https:// o `https://<computername>/<reportserver>`, donde \<*reportserver*> es el nombre del directorio virtual del servicio web del servidor de informes.  
  
4. Escriba un valor para **UrlRoot** que incluya el nombre virtual del clúster con este formato: https:// o `https://<virtual_server>/<reportserver>`.  
  
5. Guarde el archivo.  
  
6. Repita estos pasos en cada archivo RSReportServer.config de cada servidor de informes de la implementación escalada.  
  
## <a name="verify-report-server-access"></a><a name="Verify"></a> Comprobar el acceso del servidor de informes

 Compruebe que puede acceder a la implementación escalada a través del nombre de servidor virtual (por ejemplo, `https://MyVirtualServerName/reportserver` y `https://MyVirtualServerName/reports`).  
  
 Para comprobar qué nodo procesa los informes en realidad, consulte los archivos de registro del servidor de informes o el registro de ejecución de RS (la tabla del registro de ejecución contiene una columna denominada **InstanceName** que muestra qué instancia ha procesado una solicitud concreta). Para más información, vea [Archivos de registro y orígenes de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
 Si no puede conectarse al servidor de informes, compruebe el NLB para asegurarse de que las solicitudes se envían al servidor de informes y vea el registro HTTP del servidor de informes para asegurarse de que el servidor está recibiendo las solicitudes.  
  
### <a name="troubleshooting-failed-requests"></a>Solucionar problemas de solicitudes con errores

 Si las solicitudes no llegan a las instancias del servidor de informes, revise el archivo RSReportServer.config para comprobar que el nombre del servidor virtual se especifica como nombre de host para las direcciones URL del servidor de informes:  
  
1. Abra el archivo RSReportServer.config en un editor de texto.  
  
2. Busque \<**Hostname**>, \<**ReportServerUrl**> y \<**UrlRoot**>, y compruebe el nombre de host para cada configuración. Si el valor no es el nombre de host que espera, reemplácelo por el correcto.  
  
 Si se inicia la herramienta Configuración de Reporting Services después de efectuar estos cambios, es posible que cambie el valor de \<**ReportServerUrl**> por el predeterminado. Mantenga siempre una copia de seguridad de los archivos de configuración por si necesita sustituirlos por la versión que contiene la configuración que desee utilizar.  
  
## <a name="see-also"></a>Consulte también

 [Configuración de una URL &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
 [Configuración de una implementación escalada horizontalmente del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)   
 [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Administración de un servidor de informes en modo nativo de Reporting Services](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)
