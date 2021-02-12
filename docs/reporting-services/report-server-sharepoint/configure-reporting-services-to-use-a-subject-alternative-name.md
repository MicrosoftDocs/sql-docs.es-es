---
title: Configuración de Reporting Services para utilizar un nombre alternativo del firmante (SAN) | Microsoft Docs
description: Aprenda a configurar SQL Server Reporting Services y Power BI Report Server para usar un SAN mediante la modificación del archivo rsreportserver.config y el uso de la herramienta Netsh.exe.
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 25e5fce47d86ea4d544b43f297fcb793d8167a66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065170"
---
# <a name="configure-reporting-services-to-use-a-subject-alternative-name-san"></a>Configuración de Reporting Services para utilizar un nombre alternativo del firmante (SAN)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

En este tema se explica cómo configurar Reporting Services (SSRS) y Power BI Report Server para usar un nombre alternativo del firmante (SAN) mediante la modificación del archivo rsreportserver.config y el uso de la herramienta Netsh.exe.

Las instrucciones se aplican a la dirección URL del servicio web y a la del portal web en la herramienta Configuration Manager de Report Server.

Para utilizar el SAN, el certificado TLS/SSL debe estar registrado en el servidor y firmado y tener la clave privada. No puede utilizar un certificado autofirmado.

Las direcciones URL de Reporting Services y Power BI Report Server se pueden configurar para usar un certificado TLS/SSL. Normalmente, un certificado solo tiene un nombre de asunto, que permite solo una dirección URL para una sesión de Seguridad de la capa de transporte (TLS), conocida anteriormente como Capa de sockets seguros (SSL). El SAN es un campo adicional del certificado que permite a un servicio TLS escuchar varias direcciones URL y compartir el puerto TLS con otras aplicaciones. Por ejemplo, un SAN podría tener un aspecto similar a `www.myreports.com`.

Para más información sobre la configuración de TLS para Reporting Services, vea [Configuración de conexiones TLS en un servidor de informes en modo nativo](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
## <a name="configure-to-use-a-subject-alternative-name-for-web-service-url"></a>Configuración para usar un nombre alternativo del firmante para la dirección URL del servicio web
  
1.  Inicie la herramienta Configuration Manager de Report Server.  
  
     Para más información, vea [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  En la página **Dirección URL del servicio web**, seleccione un puerto TLS/SSL y un certificado TLS/SSL.  
  
     ![Administrador de configuración del servidor de informes](../../reporting-services/report-server-sharepoint/media/reportingservices-configurationmanager.png "Administrador de configuración del servidor de informes")  
  
     El administrador de configuración registra el certificado TLS/SSL para el puerto.  
  
3.  Abra el archivo rsreportserver.config.  
  
     En el caso del modo nativo de SSRS 2016, el archivo se encuentra de manera predeterminada en la carpeta siguiente:  
  
    ```  
    \Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
    ```  
  
     En el caso del modo nativo de SSRS 2017 y versiones posteriores, el archivo se encuentra de manera predeterminada en la carpeta siguiente:  
  
    ```  
    \Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer  
    ```  
    
     En el caso de Power BI Report Server, el archivo se encuentra de manera predeterminada en la carpeta siguiente:  
  
    ```  
    \Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer  
    ```  
  
4.  Copie la sección de la URL correspondiente a la aplicación **ReportServerWebService**.
  
     Por ejemplo, la siguiente sección de dirección URL es:  
  
    ```  
        <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
     La sección de dirección URL modificada siguiente es:
  
    ```  
    <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
        <URL>  
         <UrlString>https://www.myreports.com:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051/AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
  > [!TIP]  
>  * Para SSRS 2017 y versiones posteriores, el valor de `AccountSid` es `S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301`, y el de `AccountName`, `NT SERVICE\SQLServerReportingServices`.
>  * En el caso de Power BI Report Server, el valor de `AccountSid` es `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663`, y el de `AccountName`, `NT SERVICE\PowerBIReportServer`.
  
5.  Guarde el archivo rsreportserver.config.  
  
6.  Inicie un símbolo del sistema con **Ejecutar como administrador** y, luego, ejecute la herramienta Netsh.exe.  
  
    ```  
    C:\windows\system32\netsh  
    ```  
  
7.  Cambie al contexto http escribiendo lo siguiente.  
  
    ```  
    Netsh>http  
    ```  
  
8.  Muestre las urlacl existentes escribiendo lo siguiente:
  
    ```  
    Netsh http>show urlacl  
    ```  
  
     Aparece una entrada como la siguiente.  
  
    ```  
    Reserved URL            : https://+:443/ReportServer/  
        User: NT SERVICE\ReportServer  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
    ```  
  
     Una urlacl es una DACL (lista de control de acceso discrecional) para una dirección URL reservada.  
  
9. Cree una entrada para el nombre alternativo del firmante, con el mismo usuario y SDDL que la entrada existente, escribiendo lo siguiente:  
  
    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/ReportServer    
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
  
10. En la **Dirección URL del portal web**, cree una entrada para el nombre alternativo del firmante escribiendo lo siguiente:

    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/Reports  
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
> [!TIP]  
>  * Para SSRS 2017 y versiones posteriores, el valor de `user` es `NT SERVICE\SQLServerReportingServices`, y el de `sddl`, `D:(A;;GX;;;S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301)`.
>  * En el caso de Power BI Report Server, el valor de `user` es `NT SERVICE\PowerBIReportServer`, y el de `sddl`, `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663`.

> [!NOTE]  
> En Power BI Report Server, debe crear dos entradas adicionales para el nombre alternativo del firmante escribiendo lo siguiente:
>  * `add urlacl url=https://www.myreports.com:443/PowerBI user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`
>  * `add urlacl url=https://www.myreports.com:443/wopi user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`

11. En la página **Estado del servidor de informes** de la herramienta Configuration Manager de Report Server, haga clic en **Detener** y, luego, en **Iniciar** para reiniciar el servidor de informes.  
  
## <a name="see-also"></a>Consulte también

 [Archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Administrador de configuración del servidor de informes](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Modificar un archivo de configuración de Reporting Services](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Configurar las direcciones URL del servidor de informes](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
