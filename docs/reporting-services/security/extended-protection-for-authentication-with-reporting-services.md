---
description: Protección ampliada para la autenticación con Reporting Services
title: Protección ampliada para la autenticación con Reporting Services | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
ms.assetid: eb5c6f4a-3ed5-430b-a712-d5ed4b6b9b2b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b13ca53aad69647aee5158d86de2cfb16ac04710
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076107"
---
# <a name="extended-protection-for-authentication-with-reporting-services"></a>Protección ampliada para la autenticación con Reporting Services

  En versiones recientes del sistema operativo [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, la protección ampliada es un conjunto de mejoras. La protección ampliada mejora cómo protegen las aplicaciones las credenciales y la autenticación. La propia característica no proporciona directamente protección contra ataques específicos como el reenvío de credenciales, sino que proporciona una infraestructura para aplicaciones como [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con el fin de aplicar la protección ampliada para la autenticación.  
  
 Las principales mejoras de autenticación que forman parte de la protección ampliada son los enlaces de servicio y de canal. El enlace de canal usa un token de enlace de canal (CBT) para comprobar que no se ha puesto en peligro el canal establecido entre dos extremos. El enlace de servicio usa nombres principales de servicio (SPN) para validar el destino previsto de los tokens de autenticación. Para obtener información general sobre la protección ampliada, vea [Autenticación de Windows integrada con protección ampliada](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90)).  
  
SQL Server Reporting Services (SSRS) admite y aplica la protección ampliada que se ha habilitado en el sistema operativo y se ha configurado en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. De forma predeterminada, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] acepta solicitudes que especifican la autenticación Negotiate o NTLM y, por tanto, pueden beneficiarse de la compatibilidad de la protección ampliada del sistema operativo y de las características de la protección ampliada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  De forma predeterminada, Windows no habilita la protección ampliada. Para obtener información sobre cómo habilitar la protección ampliada de Windows, vea [Protección ampliada para la autenticación](https://support.microsoft.com/topic/microsoft-security-advisory-extended-protection-for-authentication-7dd2ee6d-c2e9-3484-2d8e-466261d3f0c7). Tanto el sistema operativo como la pila de autenticación de cliente deben ser compatibles con la protección ampliada para la autenticación se realice correctamente. En sistemas operativos más antiguos, quizá deba instalar varias actualizaciones para disponer de un equipo totalmente preparado para la protección ampliada. Para obtener información sobre los desarrollos recientes con la protección ampliada, vea [Actualizar información con protección ampliada](/previous-versions/sql/sql-server-2008/dd146365(v=sql.100)).  

## <a name="reporting-services-extended-protection-overview"></a>Información general sobre la protección ampliada de Reporting Services

SSRS admite y aplica la protección ampliada que se ha habilitado en el sistema operativo. Si el sistema operativo no admite la protección ampliada o no se ha habilitado la característica en él, se producirá un error de autenticación de la característica de protección ampliada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] La protección ampliada también requiere un certificado TLS/SSL. Para más información, vea [Configurar conexiones TLS en un servidor de informes en modo nativo](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
> [!IMPORTANT]  
>  De forma predeterminada, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no habilita la protección ampliada. La característica se puede habilitar si se modifica el archivo de configuración **rsreportserver.config** o mediante las API de WMI para actualizar el archivo de configuración. SSRS no proporciona una interfaz de usuario para modificar o ver la configuración de protección ampliada. Para obtener más información, vea la sección de [configuración](#ConfigurationSettings) de este tema.  
  
 Los problemas comunes que se producen a causa de los cambios de la configuración de la protección ampliada o de una configuración incorrecta no se muestran mediante mensajes ni ventanas de cuadro de diálogo de error obvios. Los problemas relacionados con la configuración y la compatibilidad de la protección ampliada tienen como resultado errores de autenticación y errores en los registros de seguimiento de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
> [!IMPORTANT]
>  Algunas tecnologías de acceso a datos pueden no admitir la protección ampliada. Para conectar los orígenes de datos de SQL Server y la base de datos del catálogo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se usa una tecnología de acceso a datos. El hecho de que una tecnología de acceso a datos no admite la protección ampliada afecta a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de las maneras siguientes:  
> 
>  -   El servidor SQL Server que ejecuta la base de datos del catálogo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no puede tener habilitada la protección ampliada; de lo contrario se producirá un error de conexión entre el servidor de informes y la base de datos del catálogo, y se devolverán errores de autenticación.  
> -   Los servidores SQL Server que se usan como orígenes de datos de los informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no pueden tener habilitada la protección ampliada; de lo contrario, los intentos de conexión del servidor de informes con el origen de datos del informe provocarán errores y devolverán errores de autenticación.  
> 
>  La documentación de una tecnología de acceso a datos debe tener información sobre la compatibilidad con la protección ampliada.  
  
### <a name="upgrade"></a>Actualizar  
  
-   La actualización de un servidor de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a SQL Server 2016 agrega opciones de configuración con valores predeterminados al archivo **rsreportserver.config**. Si la configuración ya se ha realizado, la instalación de SQL Server 2016 la conservará en el archivo **rsreportserver.config**.  
  
-   Cuando se agregan parámetros de configuración al archivo de configuración **rsreportserver.config** , el comportamiento predeterminado es desactivar la característica de protección ampliada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y el usuario debe habilitarla de la forma descrita en este tema. Para obtener más información, vea la sección de [configuración](#ConfigurationSettings) de este tema.  
  
-   El valor predeterminado del parámetro **RSWindowsExtendedProtectionLevel** es **Off**.  
  
-   El valor predeterminado del parámetro **RSWindowsExtendedProtectionScenario** es **Proxy**.  
  
-   no comprueba si el sistema operativo ni la instalación actual de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] tienen habilitada la protección ampliada.  
  
### <a name="what-reporting-services-extended-protection-does-not-cover"></a>Lo que no cubre la protección ampliada de Reporting Services  
 La característica de protección ampliada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no es compatible con las siguientes áreas y escenarios de características:  
  
-   Los autores de las extensiones de seguridad personalizada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] deben agregar compatibilidad para la protección ampliada a su extensión de seguridad personalizada.  
  
-   Los componentes de terceros agregados o usados en la instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] los deben actualizar dichos proveedores para que admitan la protección ampliada. Para obtener más información, póngase en contacto con el otro proveedor.  
  
## <a name="deployment-scenarios-and-recommendations"></a>Escenarios y recomendaciones para la implementación  
 En los siguientes escenarios se ilustran las distintas implementaciones y topologías, así como la configuración recomendada para protegerlas con la protección ampliada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
### <a name="direct"></a>Directo  
 Este escenario describe la conexión directa con un servidor de informes, por ejemplo, el entorno de una intranet.  
  
|Escenario|Diagrama del escenario|Protección|  
|--------------|----------------------|-------------------|  
|Comunicación de TLS directa.<br /><br /> El servidor de informes aplicará el cliente en el enlace de canal del servidor de informes.|![RS_ExtendedProtection_DirectSSL](../../reporting-services/security/media/rs-extendedprotection-directssl.gif "RS_ExtendedProtection_DirectSSL")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes|Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Direct**.<br /><br /> <br /><br /> -El enlace de servicio no es necesario, porque el enlace de canal usará el canal TLS.|  
|Comunicación HTTP directa. El servidor de informes aplicará el cliente en el enlace de servicio del servidor de informes.|![RS_ExtendedProtection_Direct](../../reporting-services/security/media/rs-extendedprotection-direct.gif "RS_ExtendedProtection_Direct")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes|Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Any**.<br /><br /> <br /><br /> -No existe ningún canal TLS y, por tanto, no es posible la aplicación del enlace de canal.<br /><br /> -El enlace de servicio se puede validar. Sin embargo, no es una defensa completa sin el enlace de canal y el enlace de servicio por sí mismo solo protegerá contra amenazas básicas.|  
  
### <a name="proxy-and-network-load-balancing"></a>Equilibrio de carga del proxy y de la red  
 Las aplicaciones cliente se conectan a un dispositivo o software que realiza TLS y pasa por las credenciales al servidor para la autenticación, por ejemplo, una extranet, Internet o una intranet segura. El cliente se conecta a un proxy o todos los clientes usan un proxy.  
  
 Ocurre lo mismo cuando usa un dispositivo de equilibrio de carga de red (NLB).  
  
|Escenario|Diagrama del escenario|Protección|  
|--------------|----------------------|-------------------|  
|Comunicación HTTP. El servidor de informes aplicará el cliente en el enlace de servicio del servidor de informes.|![RS_ExtendedProtection_Indirect](../../reporting-services/security/media/rs-extendedprotection-indirect.gif "RS_ExtendedProtection_Indirect")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes<br /><br /> 3) Proxy|Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Any**.<br /><br /> <br /><br /> -No existe ningún canal TLS y, por tanto, no es posible la aplicación del enlace de canal.<br /><br /> -El servidor de informes se debe configurar para saber el nombre del servidor proxy con el fin de asegurarse de que se aplica el enlace de servicio correctamente.|  
|Comunicación HTTP.<br /><br /> El servidor de informes aplicará el cliente en el enlace de canal de proxy y en el enlace de servicio del servidor de informes.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes<br /><br /> 3) Proxy|Set <br />                    Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Proxy**.<br /><br /> <br /><br /> -El canal TLS al proxy está disponible. Por tanto, se puede aplicar el enlace de canal en el proxy.<br /><br /> -También se puede aplicar el enlace de servicio.<br /><br /> El servidor de informes debe saber el nombre del proxy y el administrador del servidor de informes debe crear una reserva de direcciones URL para este, con un encabezado de host o configurar el nombre de proxy en la entrada del Registro de Windows **BackConnectionHostNames**.|  
|Comunicación HTTPS indirecta con un proxy seguro. El servidor de informes aplicará el cliente en el enlace de canal de proxy y en el enlace de servicio del servidor de informes.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes<br /><br /> 3) Proxy|Set <br />                    Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Proxy**.<br /><br /> <br /><br /> -El canal TLS al proxy está disponible. Por tanto, se puede aplicar el enlace de canal en el proxy.<br /><br /> -También se puede aplicar el enlace de servicio.<br /><br /> El servidor de informes debe saber el nombre del proxy y el administrador del servidor de informes debe crear una reserva de direcciones URL para este, con un encabezado de host o configurar el nombre de proxy en la entrada del Registro de Windows **BackConnectionHostNames**.|  
  
### <a name="gateway"></a>Puerta de enlace  
 En este escenario se describen las aplicaciones cliente que se conectan a un dispositivo o software que realiza TLS y autentica al usuario. Después, el dispositivo o software suplanta el contexto del usuario o un contexto de usuario distinto antes de efectuar una solicitud al servidor de informes.  
  
|Escenario|Diagrama del escenario|Protección|  
|--------------|----------------------|-------------------|  
|Comunicación HTTP indirecta.<br /><br /> La puerta de enlace aplicará el cliente en un enlace de canal de puerta de enlace. Existe una puerta de enlace en el enlace de servicio del servidor de informes.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes<br /><br /> 3) Dispositivo de puerta de enlace|Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Any**.<br /><br /> <br /><br /> -El enlace de canal desde el cliente al servidor de informes no es posible ya que la puerta de enlace suplanta un contexto y, por tanto, crea un nuevo token NTLM.<br /><br /> -No existe ningún TLS desde la puerta de enlace al servidor de informes, por tanto, no se puede aplicar ningún enlace de canal.<br /><br /> -Se puede aplicar el enlace de servicio.<br /><br /> -El dispositivo de puerta de enlace lo debe configurar el administrador para aplicar el enlace de canal.|  
|Comunicación HTTPS indirecta con una puerta de enlace segura. La puerta de enlace aplicará el cliente en el enlace de canal de puerta de entrada y el servidor de informes aplicará la puerta de enlace en el enlace de canal del servidor de informes.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes<br /><br /> 3) Dispositivo de puerta de enlace|Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Direct**.<br /><br /> <br /><br /> -El enlace de canal desde el cliente al servidor de informes no es posible ya que la puerta de enlace suplanta un contexto y, por tanto, crea un nuevo token NTLM.<br /><br /> -TLS desde la puerta de enlace al servidor de informes, es decir, se puede aplicar enlace de canal.<br /><br /> -No se requiere enlace de servicio.<br /><br /> -El dispositivo de puerta de enlace lo debe configurar el administrador para aplicar el enlace de canal.|  
  
### <a name="combination"></a>Combinación  
 Este escenario describe los entornos de extranet o Internet en los que el cliente se conecta con un proxy. Se combina con un entorno de intranet donde un cliente se conecta con un servidor de informes.  
  
|Escenario|Diagrama del escenario|Protección|  
|--------------|----------------------|-------------------|  
|Acceso indirecto y directo desde el cliente al servicio del servidor de informes sin TLS en conexiones del cliente al proxy ni del cliente al servidor de informes.|1) Aplicación cliente<br /><br /> 2) Servidor de informes<br /><br /> 3) Proxy<br /><br /> 4) Aplicación cliente|Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Any**.<br /><br /> <br /><br /> -Se puede aplicar el enlace de servicio desde el cliente al servidor de informes.<br /><br /> El servidor de informes debe saber el nombre del proxy y el administrador del servidor de informes debe crear una reserva de direcciones URL para este, con un encabezado de host o configurar el nombre de proxy en la entrada del Registro de Windows **BackConnectionHostNames**.|  
|Acceso indirecto y directo desde el cliente al servidor de informes donde el cliente establece una conexión TLS al proxy o al servidor de informes.|![RS_ExtendedProtection_CombinationSSL](../../reporting-services/security/media/rs-extendedprotection-combinationssl.gif "RS_ExtendedProtection_CombinationSSL")<br /><br /> 1) Aplicación cliente<br /><br /> 2) Servidor de informes<br /><br /> 3) Proxy<br /><br /> 4) Aplicación cliente|Establezca **RSWindowsExtendedProtectionLevel** en **Allow** o **Require**.<br /><br /> Establezca **RSWindowsExtendedProtectionScenario** en **Proxy**.<br /><br /> <br /><br /> -Se puede usar el enlace de canal<br /><br /> El servidor de informes debe saber el nombre del proxy y el administrador del servidor de informes debe crear una reserva de direcciones URL para el proxy, con un encabezado de host o configurar el nombre de proxy en la entrada del Registro de Windows **BackConnectionHostNames**.|  
  
## <a name="configuring-reporting-services-extended-protection"></a>Configurar la protección ampliada de Reporting Services  
 El archivo **rsreportserver.config** contiene los valores de configuración que controlan el comportamiento de la protección ampliada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Para más información sobre el uso y la edición del archivo **rsreportserver.config** , vea [El archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md). La configuración de la protección ampliada también se puede cambiar e inspeccionar mediante las API WMI. Para más información, vea [Método SetExtendedProtectionSettings &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md).  
  
 Cuando se produce un error de autenticación de la configuración, se deshabilitan los tipos de autenticación **RSWindowsNTLM**, **RSWindowsKerberos** y **RSWindowsNegotiate** en el servidor de informes.  
  
###  <a name="configuration-settings-for-reporting-services-extended-protection"></a><a name="ConfigurationSettings"></a> Configuración de la protección ampliada de los servicios de informes  
 La tabla siguiente proporciona información sobre los valores de configuración que aparecen en **rsreportserver.config** para la protección ampliada.  
  
|Configuración|Descripción|  
|-------------|-----------------|  
|**RSWindowsExtendedProtectionLevel**|Especifica el grado de aplicación de la protección ampliada. Los valores válidos son:<br /><br /> **Off**: Predeterminada. Especifica que no existe comprobación de enlace de canal ni de enlace de servicio<br /><br /> El valor **Allow** admite la protección ampliada, pero no la necesita.  Especifica lo siguiente:<br /><br /> -La protección ampliada se aplicará en las aplicaciones cliente que se ejecuten en sistemas operativos que la admitan. La forma de aplicar la protección la determina la opción **RsWindowsExtendedProtectionScenario**.<br /><br /> -Se permitirá la autenticación en aplicaciones que se ejecuten en sistemas operativos que no sean compatibles con la protección ampliada.<br /><br /> **Require** especifica lo siguiente:<br /><br /> -La protección ampliada se aplicará en las aplicaciones cliente que se ejecuten en sistemas operativos que la admitan.<br /><br /> **No** se permitirá la autenticación en aplicaciones que se ejecuten en sistemas operativos que no sean compatibles con la protección ampliada.|  
|**RsWindowsExtendedProtectionScenario**|Especifica qué formas de protección ampliada se validan: Enlace de canales, enlace de servicios o ambos. Los valores válidos son:<br /><br /> **Proxy**: Predeterminada. Especifica lo siguiente:<br /><br /> -Si está presente, la autenticación de Windows NTLM, Kerberos y Negotiate cuando está presente un token de enlace de canal.<br /><br /> -Se aplica el enlace de servicio.<br /><br /> **Any** Especifica lo siguiente:<br /><br /> -No se necesitan la autenticación de Windows NTLM, Kerberos ni Negotiate, ni ningún enlace de canal.<br /><br /> -Se aplica el enlace de servicio.<br /><br /> **Direct** Especifica lo siguiente:<br /><br /> -La autenticación de Windows NTLM, Kerberos y Negotiate cuando están presentes un CBT y una conexión TLS al servicio actual, y el CBT para que la conexión TLS coincida con el CBT del token de NTLM, Kerberos o Negotiate.<br /><br /> -No se aplica el enlace de servicio.<br /><br /> <br /><br /> Nota: Se ignora la configuración **RsWindowsExtendedProtectionScenario** si **RsWindowsExtendedProtectionLevel** está establecido en **OFF**.|  
  
 Entradas del ejemplo en el archivo de configuración **rsreportserver.config** :  
  
```  
<Authentication>  
         <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
         <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionLevel>  
</Authentication>  
```  
  
## <a name="service-binding-and-included-spns"></a>Enlace de servicio y SPN incluidos  
 El enlace de servicio usa nombres principales de servicio (SPN) para validar el destino previsto de los tokens de autenticación. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa la información de reserva de dirección URL para compilar una lista de los SPN que se consideran válidos. Si se usa la información de reservas de direcciones URL para la validación tanto de SPN como de reservas de direcciones URL, los administraciones del sistema pueden administrarlos desde una sola ubicación.  
  
 La lista de los SPN válidos se actualiza cuando se inicia el servidor de informes, se cambia la configuración de la protección ampliada o se recicla el dominio de aplicación.  
  
 La lista válida de los SPN es específica de cada aplicación. Por ejemplo, el Administrador de informes y el servidor de informes tendrán cada uno una lista distinta de los SPN válidos calculados.  
  
 La lista de los SPN válidos calculados para una aplicación la determinan los siguientes factores:  
  
-   Cada reserva de direcciones URL.  
  
-   Cada SPN recuperado desde el controlador de dominios para la cuenta de servicio de los servicios de informes.  
  
-   Si una reserva de direcciones URL incluye caracteres comodín ('*' o '+'), el servidor de informes agregará cada entrada desde la colección de hosts.  
  
### <a name="hosts-collection-sources"></a>Orígenes de la colección de hosts.  
 En la tabla siguiente se citan los posible orígenes para la colección de hosts.  
  
|Tipo de origen de datos|Descripción|  
|--------------------|-----------------|  
|ComputerNameDnsDomain|El nombre del dominio DNS asignado al equipo local. Si el equipo local es un nodo de un clúster, se usa el nombre de dominio DNS del servidor virtual de clústeres.|  
|ComputerNameDnsFullyQualified|El nombre DNS completo que identifica exclusivamente el equipo local. Este nombre es una combinación del nombre del host DNS y el nombre del dominio DNS, con el formato *nombreDeHost*.*nombreDeDominio*. Si el equipo local es un nodo de un clúster, se usa el nombre DNS completo del servidor virtual de clústeres.|  
|ComputerNameDnsHostname|El nombre del host DNS del equipo local. Si el equipo local es un nodo de un clúster, se usa el nombre del host DNS del servidor virtual de clústeres.|  
|ComputerNameNetBIOS|El nombre NetBIOS del equipo local. Si el equipo local es un nodo de un clúster, se usa el nombre NetBIOS del servidor virtual de clústeres.|  
|ComputerNamePhysicalDnsDomain|El nombre del dominio DNS asignado al equipo local. Si el equipo local es un nodo de un clúster, se usa el nombre de dominio DNS del equipo local, no el nombre del servidor virtual de clústeres.|  
|ComputerNamePhysicalDnsFullyQualified|El nombre DNS completo que identifica exclusivamente el equipo. Si el equipo local es un nodo de un clúster, se usa el nombre DNS completo del equipo local, no el nombre del servidor virtual de clústeres.<br /><br /> El nombre DNS completo es una combinación del nombre del host DNS y del nombre del dominio DNS, con el formato *nombreDeHost*.*nombreDeDominio*.|  
|ComputerNamePhysicalDnsHostname|El nombre del host DNS del equipo local. Si el equipo local es un nodo de un clúster, se usa el nombre del host DNS del equipo local, no el nombre del servidor virtual de clústeres.|  
|ComputerNamePhysicalNetBIOS|El nombre NetBIOS del equipo local. Si el equipo local es un nodo de un clúster, se usa el nombre NetBIOS del equipo local, no el nombre del servidor virtual de clústeres.|  
  
Para más información, vea [Registrar un nombre principal de servicio &#40;SPN&#41; para un servidor de informes](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md) y [Acerca de las reservas y el registro de reservas de URL &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md).  
  
## <a name="next-steps"></a>Pasos siguientes

[Conectar al motor de base de datos con protección ampliada](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)   
[Introducción a la protección ampliada para la autenticación](/previous-versions/dotnet/netframework-3.5/dd767318(v=vs.90))   
[Autenticación de Windows integrada con protección ampliada](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90))   
[Microsoft Security Advisory: Protección ampliada para la autenticación](https://go.microsoft.com/fwlink/?LinkId=179923)   
[Registro de seguimiento del servicio del servidor de informes](../../reporting-services/report-server/report-server-service-trace-log.md)   
[El archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Método SetExtendedProtectionSettings &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
