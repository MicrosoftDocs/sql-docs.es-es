---
description: Configurar la autenticación de Windows en el servidor de informes
title: Configurar la autenticación de Windows en el servidor de informes | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [Reporting Services]
- Reporting Services, configuration
ms.assetid: 4de9c3dd-0ee7-49b3-88bb-209465ca9d86
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 22a9134ed3caf220dbe895d7e4de8b9d52290e08
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536336"
---
# <a name="configure-windows-authentication-on-the-report-server"></a>Configurar la autenticación de Windows en el servidor de informes
  De forma predeterminada, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] acepta solicitudes que especifican la autenticación NTLM o Negotiate. Si la implementación incluye aplicaciones cliente y exploradores que utilizan estos proveedores de seguridad, puede utilizar los valores predeterminados sin necesidad de ninguna configuración adicional. Si desea utilizar un proveedor de seguridad diferente para la seguridad integrada de Windows (por ejemplo, si desea utilizar directamente Kerberos) o si modificó los valores predeterminados y prefiere restaurar los originales, puede utilizar la información de este tema para especificar los valores de autenticación en el servidor de informes.  
  
 Para utilizar la seguridad integrada de Windows, cada usuario que requiera acceso a un servidor de informes debe tener una cuenta de usuario de dominio o local de Windows válida, o ser miembro de una cuenta de grupo de dominio o local de Windows. Puede incluir cuentas de otros dominios siempre que sean de confianza. Las cuentas deben tener acceso al equipo del servidor de informes y deben asignarse posteriormente a roles con el fin de lograr acceso a operaciones específicas del servidor de informes.  
  
 Además, se deben cumplir los requisitos adicionales siguientes:  
  
-   Los archivos RSReportServer.config deben tener establecido **AuthenticationType** en **RSWindowsNegotiate**, **RSWindowsKerberos** o **RSWindowsNTLM**. De forma predeterminada, el archivo RSReportServer.config incluye el valor **RSWindowsNegotiate** si la cuenta de servicio del servidor de informes es NetworkService o LocalSystem; de lo contrario, se usa el valor **RSWindowsNTLM** . Puede agregar **RSWindowsKerberos** si tiene aplicaciones que solo utilizan la autenticación Kerberos.  
  
    > [!IMPORTANT]  
    >  Al usar **RSWindowsNegotiate** , se producirá un error de autenticación Kerberos si ha configurado el servicio del servidor de informes para ejecutarse en una cuenta de usuario de dominio y no ha registrado un nombre de entidad de seguridad de servicio (SPN) para la cuenta. Para obtener más información, vea [Resolver los errores de autenticación Kerberos al conectarse a un servidor de informes](#proxyfirewallRSWindowsNegotiate) en este tema.  
  
-   [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] se debe configurar para la autenticación de Windows. De forma predeterminada, en los archivos Web.config del servicio web del servidor de informes se incluye el valor \<authentication mode="Windows">. Si lo cambia a \<authentication mode="Forms">, se producirá un error en la autenticación de Windows para [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   Los archivos Web.config para el servicio web del servidor de informes deben tener \<identity impersonate= "true" />.  
  
-   La aplicación cliente o el explorador deben admitir la seguridad integrada de Windows.  

- El [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] no requiere configuración adicional.
  
 Para cambiar la configuración de autenticación del servidor de informes, modifique los valores y los elementos XML en el archivo RSReportServer.config. Puede copiar y pegar los ejemplos de este tema para implementar combinaciones concretas.  
  
 La configuración predeterminada funciona mejor si todos los equipos cliente y servidor se encuentran en el mismo dominio o en un dominio de confianza, y el servidor de informes se implementa para el acceso a la intranet detrás de un firewall corporativo. Los dominios únicos y de confianza son un requisito necesario para pasar credenciales de Windows. Las credenciales se pueden pasar más de una vez si se habilita la versión 5 del protocolo Kerberos para los servidores. De lo contrario, las credenciales se pueden pasar solo una vez antes de que expiren. Para más información sobre cómo configurar credenciales para conexiones a varios equipos, vea [Especificar información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
 Las instrucciones siguientes están pensadas para un servidor de informes en modo nativo. Si el servidor de informes se implementa en modo integrado de SharePoint, se deben utilizar los valores de autenticación predeterminados que especifican la seguridad integrada de Windows. El servidor de informes utiliza las características internas de la extensión de autenticación de Windows predeterminada para admitir los servidores de informes en modo integrado de SharePoint.  
  
## <a name="extended-protection-for-authentication"></a>Protección ampliada para la autenticación  
 A partir de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], se admite la protección ampliada para autenticación. La característica de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el uso del enlace de canal y del enlace de servicio para mejorar la protección de la autenticación. Las características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] tienen que usarse con un sistema operativo que admita la protección ampliada. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para la protección extendida viene determinada por la configuración del archivo RSReportServer.config. El archivo puede actualizarse modificando el archivo o usando las API WMI. Para más información, consulte [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md).  
  
### <a name="to-configure-a-report-server-to-use-windows-integrated-security"></a>Para configurar un servidor de informes para usar la seguridad integrada de Windows  
  
1.  Abra RSReportServer.config en un editor de texto.  
  
2.  Busque \<**Authentication**>.  
  
3.  De las estructuras XML siguientes, copie la que mejor se ajuste a sus necesidades. Puede especificar **RSWindowsNegotiate**, **RSWindowsNTLM** y **RSWindowsKerberos** en cualquier orden. Debe habilitar la persistencia de autenticación si desea autenticar la conexión en lugar de cada solicitud individual. Con la persistencia de autenticación, todas las solicitudes que requieran autenticación se permitirán mientras dure la conexión.  
  
     La primera estructura XML es la configuración predeterminada cuando la cuenta de servicio del servidor de informes es NetworkService o LocalSystem:  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
     La segunda estructura XML es la configuración predeterminada cuando la cuenta de servicio del servidor de informes no es NetworkService o LocalSystem:  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    ```  
  
     \</Authentication>  
  
     La tercera estructura XML especifica todos los paquetes de seguridad que se utilizan en la seguridad integrada de Windows:  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
                 <RSWindowsKerberos />  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
     La cuarta estructura XML solo especifica NTLM para las implementaciones que no admiten Kerberos o para solucionar los errores de autenticación Kerberos:  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
4.  Péguela sobre las entradas existentes para \<**Authentication**>.  
  
     Observe que no puede utilizar **Custom** con los tipos **RSWindows** .  
  
5.  Modifique los valores para la protección ampliada según corresponda. La protección ampliada está deshabilitada de forma predeterminada.  Si estas entradas no están presentes, el equipo actual puede no estar ejecutando ninguna versión de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que admita la protección ampliada. Para obtener más información, vea [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md).  
  
    ```  
          <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
          <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
    ```  
  
6.  Guarde el archivo.  
  
7.  Si configuró una implementación escalada, repita estos pasos con los demás servidores de informes de la implementación.  
  
8.  Reinicie el servidor de informes para borrar las sesiones que estén abiertas en ese momento.  
  
##  <a name="resolving-kerberos-authentication-errors-when-connecting-to-a-report-server"></a><a name="proxyfirewallRSWindowsNegotiate"></a> Resolver errores de autenticación Kerberos al conectarse a un servidor de informes  
 En un servidor de informes que esté configurado para usar la autenticación Kerberos o Negotiate, se producirá un error en una conexión de cliente con el servidor de informes si hay un error de autenticación Kerberos. Se producen errores de autenticación Kerberos cuando:  
  
-   El servicio del servidor de informes se ejecuta como una cuenta de usuario de dominio de Windows y no se registró un nombre de la entidad de seguridad del servicio (SPN) para la cuenta.  
  
-   El servidor de informes se configura con el valor **RSWindowsNegotiate** .  
  
-   El explorador elige Kerberos sobre NTLM en el encabezado de autenticación en la solicitud que envía al servidor de informes.  
  
 Puede detectar el error si habilitó el registro de Kerberos. Otro síntoma del error es que se solicitan varias veces las credenciales y, a continuación, aparece una ventana del explorador vacía.  
  
 Para confirmar si hay un error de autenticación Kerberos, quite < **RSWindowsNegotiate** /> del archivo de configuración y vuelva a intentar establecer la conexión.  
  
 Después de confirmar el problema, puede abordarlo de las maneras siguientes:  
  
-   Registre un SPN para el servicio del servidor de informes en la cuenta de usuario de dominio. Para más información, vea [Registrar un nombre de entidad de seguridad de servicio &#40;SPN&#41; para un servidor de informes](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).  
  
-   Cambie la cuenta de servicio para que se ejecute en una cuenta integrada como servicio de red. Las cuentas integradas asignan el SPN HTTP al SPN de host, que se define al unir un equipo a la red. Para más información, vea [Configurar una cuenta de servicio &#40;Administrador de configuración del servidor de informes&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).
  
-   Use NTLM. Generalmente, NTLM funcionará en los casos en que no lo haga la autenticación Kerberos. Para utilizar NTLM, quite **RSWindowsNegotiate** del archivo RSReportServer.config y compruebe que solo se especifica **RSWindowsNTLM** . Si elige esta solución, puede continuar utilizando una cuenta de usuario de dominio para el servicio del servidor de informes aunque no defina un SPN para él.  
  
#### <a name="logging-information"></a>Registrar información  
 Hay varios orígenes de información de registro que pueden servir de ayuda para resolver problemas relacionados con Kerberos.  
  
##### <a name="user-account-control-attribute"></a>Atributo User-Account-Control  
 Determine si la cuenta de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] tiene el atributo suficiente establecido en Active Directory. Revise el archivo de registro del seguimiento del servicio de informe de errores para encontrar el valor registrado para el atributo UserAccountControl. El valor registrado está en formato decimal. Necesita convertir el valor decimal al formato hexadecimal y, a continuación, buscar ese valor en el tema MSDN que describe el atributo User-Account-Control.  
  
-   La entrada del registro de seguimiento del servicio de informe de errores será similar a la siguiente:  
  
    ```  
    appdomainmanager!DefaultDomain!8f8!01/14/2010-14:42:28:: i INFO: The UserAccountControl value for the service account is 590336  
    ```  
  
-   Una opción para convertir el valor decimal al formato hexadecimal es usar la Calculadora de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. La Calculadora de Windows admite varios modos que muestran las opciones 'Dec' y 'Hexadecimal'. Seleccione la opción 'Dec', pegue o escriba en ella el valor decimal que encuentre en el archivo de registro y, a continuación, seleccione la opción  'Hex'.  
  
-   Después, vea el tema [User-Account-Control attribute](/windows/win32/adschema/a-useraccountcontrol) (Atributo User-Account-Control) para derivar el atributo para la cuenta de servicio.  
  
##### <a name="spns-configured-in-active-directory-for-the-reporting-services-service-account"></a>SPN configurados en Active Directory para la cuenta de servicio de Reporting Services.  
 Para registrar los SPN en el archivo de registro de seguimiento del servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puede habilitar temporalmente la característica Protección ampliada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Modifique el archivo de configuración de **rsreportserver.config** estableciendo lo siguiente:  
  
    ```  
    <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>   
    <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>  
    ```  
  
-   Reinicie el servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .

 Si no quiere seguir usando la protección ampliada, establezca de nuevo los valores de configuración predeterminados y reinicie la cuenta de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
```  
<RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>  
<RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
```  
  
 Para obtener más información, vea [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
#### <a name="how-the-browser-chooses-negotiated-kerberos-or-negotiated-ntlm"></a>Cómo el explorador elige Kerberos negociado o NTLM negociado  
 Cuando se utiliza Internet Explorer para conectarse al servidor de informes, se especifica Kerberos negociado o NTLM negociado en el encabezado de autenticación. NTLM se utiliza en lugar de Kerberos cuando:  
  
-   La solicitud se envía a un servidor de informes local.  
  
-   La solicitud se envía a una dirección IP del equipo del servidor de informes en lugar de a un nombre de servidor o encabezado de host.  
  
-   El software de firewall bloquea los puertos que se usan para la autenticación Kerberos.  
  
-   El sistema operativo de un servidor determinado no tiene habilitado Kerberos.  
  
-   El dominio incluye versiones antiguas de los sistemas operativos Windows de servidor y de cliente que no admiten la característica de autenticación Kerberos integrada en las versiones más recientes del sistema operativo.  
  
 Además, Internet Explorer puede elegir Kerberos negociado o NTLM negociado, dependiendo de cómo se hayan configurado los valores de dirección URL, LAN y proxy.  
  
###### <a name="report-server-url"></a>Dirección URL del servidor de informes  
 Si la dirección URL incluye un nombre de dominio completo, Internet Explorer selecciona NTLM. Si la dirección URL especifica el host local, Internet Explorer selecciona NTLM. Si la dirección URL especifica el nombre de red del equipo, Internet Explorer selecciona Negotiate; esto tendrá éxito o no dependiendo de que exista un SPN para la cuenta de servicio del servidor de informes.  
  
###### <a name="lan-and-proxy-settings-on-the-client"></a>Configuración de LAN y proxy en el cliente  
 La configuración de LAN y proxy que se establece en Internet Explorer puede determinar si se elige NTLM antes que Kerberos. Sin embargo, dado que la configuración de LAN y proxy varía a través de las organizaciones, no es posible determinar con precisión los valores exactos que contribuyen a los errores de autenticación Kerberos. Por ejemplo, una organización puede exigir una configuración de proxy que transforme las direcciones URL de una intranet en direcciones URL de nombre de dominio completo que se resuelven a través de las conexiones a Internet. Si se usan proveedores de autenticación diferentes para tipos distintos de direcciones URL, puede suceder que algunas conexiones tengan éxito cuando se esperaba lo contrario.  
  
 Si se producen errores de conexión que considera que se deben a errores de autenticación, puede probar combinaciones diferentes de configuración de LAN y proxy para aislar el problema. En Internet Explorer, la configuración de LAN y proxy se encuentra en el cuadro de diálogo **Configuración de la red de área local (LAN)** , que se abre al hacer clic en **Configuración de LAN** en la pestaña **Conexión** de **Opciones de Internet**.  
  
## <a name="external-resources"></a>Recursos externos  
  
-   Para obtener información adicional con respecto a Kerberos y los servidores de informes, vea [Implementar una solución de Business Intelligence con SharePoint, Reporting Services y servidor de supervisión de PerformancePoint con Kerberos.](https://www.kasperonbi.com/deploying-a-business-intelligence-solution-using-sharepoint-reporting-services-and-performancepoint-monitoring-server-with-kerberos/)  
  
## <a name="see-also"></a>Consulte también  
 [Autenticación con el servidor de informes](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Conceder permisos en un servidor de informes en modo nativo](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [El archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Configurar la autenticación básica en el servidor de informes](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
 [Configurar la autenticación de formularios o personalizada en el servidor de informes](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)   
 [Protección ampliada para la autenticación con Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
