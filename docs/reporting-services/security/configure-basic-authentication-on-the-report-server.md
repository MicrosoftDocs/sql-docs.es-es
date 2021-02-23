---
description: Configurar la autenticación básica en el servidor de informes
title: Configurar la autenticación básica en el servidor de informes | Microsoft Docs
ms.date: 02/10/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- Basic authentication
ms.assetid: 8faf2938-b71b-4e61-a172-46da2209ff55
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d3b875eafb10e4234df2cde35bb61bbecfebad94
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489329"
---
# <a name="configure-basic-authentication-on-the-report-server"></a>Configurar la autenticación básica en el servidor de informes
  De forma predeterminada, Reporting Services acepta solicitudes que especifican la autenticación NTLM o Negotiate. Si su implementación incluye aplicaciones cliente o exploradores que utilizan la autenticación básica, debe agregar esta autenticación a la lista de tipos admitidos. Además, si desea utilizar el Generador de informes, debe permitir el acceso anónimo a los archivos del Generador de informes.  
  
 Para configurar la autenticación básica en el servidor de informes, modifique los valores y elementos XML en el archivo RSReportServer.config. Puede copiar y pegar los ejemplos de este tema para reemplazar los valores predeterminados.  
  
 Antes de habilitar la autenticación básica, compruebe que la infraestructura de seguridad la admite. Con la autenticación básica, el servicio web del servidor de informes pasará las credenciales a la entidad de seguridad local. Si las credenciales especifican una cuenta de usuario local, la entidad de seguridad local autentica al usuario en el equipo del servidor de informes y el usuario obtendrá un token de seguridad válido para los recursos locales. Las credenciales para las cuentas de usuario de dominio se reenvían a un controlador de dominio que las autentica. El vale resultante es válido para los recursos de red.  
  
 Si desea mitigar el riesgo de que se intercepten las credenciales mientras se dirigen a un controlador de dominio de la red, se requiere cifrado en el canal, como la Seguridad de la capa de transporte (TLS), anteriormente conocida como Capa de sockets seguros (SSL). Por sí sola, la autenticación básica transmite el nombre de usuario en texto sin cifrar y la contraseña en codificación en base 64. Cuando se agrega cifrado al canal, el paquete es ilegible. Para más información, vea [Configurar conexiones TLS en un servidor de informes en modo nativo](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
 Después de habilitar la autenticación básica, tenga en cuenta que los usuarios no pueden seleccionar la opción **Seguridad integrada de Windows** al establecer las propiedades de conexión en un origen de datos externo que proporciona los datos para un informe. La opción estará deshabilitada en las páginas de propiedades del origen de datos.  
  
> [!NOTE]  
>  Las instrucciones siguientes están pensadas para un servidor de informes en modo nativo. Si el servidor de informes se implementa en modo integrado de SharePoint, se deben utilizar los valores de autenticación predeterminados que especifican la seguridad integrada de Windows. El servidor de informes utiliza las características internas de la extensión de autenticación de Windows predeterminada para admitir el servidor de informes en modo integrado de SharePoint.  
  
### <a name="to-configure-a-report-server-to-use-basic-authentication"></a>Para configurar un servidor de informes de modo que use la autenticación básica  
  
1. Abra RSReportServer.config en un editor de texto.  
  
     Para localizar el archivo de configuración, consulte la sección [Ubicación del archivo](../report-server/rsreportserver-config-configuration-file.md#bkmk_file_location) del artículo "Archivo de configuración RSReportServer.config".
  
2. Busque \<**Authentication**>.  
  
3. De las estructuras XML siguientes, copie la que mejor se ajuste a sus necesidades. La primera estructura XML proporciona marcadores de posición para especificar todos los elementos, que se describen en la sección siguiente:  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

    ```xml
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsBasic>  
                       <LogonMethod>3</LogonMethod>  
                       <Realm></Realm>  
                       <DefaultDomain></DefaultDomain>  
                 </RSWindowsBasic>  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
    Si usa los valores predeterminados, puede copiar la estructura de elementos mínima:  
  
    ```xml
          <AuthenticationTypes>  
                 <RSWindowsBasic/>  
          </AuthenticationTypes>  
    ```  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE [ssrs-appliesto-pbirs](../../includes/ssrs-appliesto-pbirs.md)]

    ```xml
      <Authentication>
          <AuthenticationTypes>
                      <RSWindowsBasic/>
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
      <RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
      <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>
      </Authentication>
    ```

4. Péguela sobre las entradas existentes para \<**Authentication**>.  
  
     Si usa varios tipos de autenticación, basta con que agregue el elemento **RSWindowsBasic** , pero no elimine las entradas correspondientes a **RSWindowsNegotiate**, **RSWindowsNTLM** o **RSWindowsKerberos**.  
  
     Observe que no puede utilizar **Custom** con otros tipos de autenticación.  
  
5. Reemplace los valores vacíos de \<**Realm**> o \<**DefaultDomain**> por valores que sean válidos para su entorno.  
  
6. Guarde el archivo.  
  
7. Si configuró una implementación escalada, repita estos pasos con los demás servidores de informes de la implementación.  
  
8. Reinicie el servidor de informes para borrar las sesiones que estén abiertas en ese momento.  
  
## <a name="rswindowsbasic-reference"></a>Referencia de RSWindowsBasic  
 Se pueden especificar los elementos siguientes al configurar la autenticación básica.  
  
|Elemento|Obligatorio|Valores válidos|  
|-------------|--------------|------------------|  
|LogonMethod|Sí<br /><br /> Si no especifica un valor, se usará 3.|**2** = inicio de sesión en red; diseñado para servidores de alto rendimiento para autenticar las contraseñas de texto simple.<br /><br /> **3** : inicio de sesión con texto no cifrado, que conserva las credenciales de inicio de sesión en el paquete de autenticación que se envía con cada solicitud HTTP. Esto permite que el servidor suplante al usuario a la hora de establecer la conexión con otros servidores de la red. (Es el valor predeterminado).<br /><br /> Nota: Los valores 0 (para el inicio de sesión interactivo) y 1 (para el inicio de sesión por lotes) **NO** se admiten en [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)].|  
|Dominio|Opcional|Especifica una partición de recurso que incluye características de autorización y de autenticación que se utilizan para controlar el acceso a los recursos protegidos de una organización.|  
|DominioPredeterminado|Opcional|Especifica el dominio que utiliza el servidor para autenticar al usuario. Este valor es opcional, pero si lo omite, el servidor de informes utilizará el nombre de equipo como dominio. Si el equipo es miembro de dominio, ese dominio es el predeterminado. Si instaló el servidor de informes en un controlador de dominio, el dominio que se utilizará será el controlado por el equipo .|  
  
## <a name="see-also"></a>Consulte también  
 [Dominios de aplicación para las aplicaciones del servidor de informes](../../reporting-services/report-server/application-domains-for-report-server-applications.md)   
 [Seguridad y protección de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)  
  
  
