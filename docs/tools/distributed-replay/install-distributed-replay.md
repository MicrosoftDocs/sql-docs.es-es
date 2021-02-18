---
title: Instalar Distributed Replay
titleSuffix: SQL Server Distributed Replay
description: 'En este artículo se describen las formas en que se puede instalar Distributed Replay: con el Asistente para instalación, en una ventana del símbolo del sistema o por medio de un archivo de configuración.'
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ea1171da-f50e-4f16-bedc-5e468a46477f
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: fc4c971d62ac4a54a786b426ea8f7855882fa0c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345923"
---
# <a name="install-distributed-replay"></a>Instalar Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Puede instalar Distributed Replay en una de estas tres maneras:  
  
-   [Instalar Distributed Replay desde el Asistente para instalación](#bkmk_wizard)  
  
-   [Instalar Distributed Replay desde el símbolo del sistema](#bkmk_command_prompt)  
  
-   [Instalar Distributed Replay utilizando un archivo de configuración](#bkmk_configuration_file)  
  
##  <a name="install-distributed-replay-from-the-installation-wizard"></a><a name="bkmk_wizard"></a> Instalar Distributed Replay desde el Asistente para instalación  
 Instalar las características de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay con el Asistente para instalación de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Cuando planee dónde desea instalar las características, tenga en cuenta lo siguiente:  
  
-   Puede instalar la herramienta de administración en el mismo equipo que el controlador de Distributed Replay, o en equipos diferentes.  
  
-   Solo puede haber un controlador en cada entorno de Distributed Replay.  
  
-   Puede instalar el servicio de cliente en 16 equipos (físicos o virtuales) como máximo.  
  
-   Solo una instancia del servicio de cliente se puede instalar en el equipo del controlador de Distributed Replay. Si el entorno de Distributed Replay tiene más de un cliente, no se recomienda instalar el servicio de cliente en el mismo equipo que el controlador. De hacerlo, puede reducirse la velocidad total de la Distributed Replay.  
  
-   En escenarios de pruebas de rendimiento, no se recomienda instalar la herramienta de administración, el servicio Distributed Replay Controller o el servicio de cliente en la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La instalación de todas estas características en el servidor de destino debe limitarse a las pruebas funcionales para la compatibilidad de aplicaciones.  
  
-   Después de la instalación, el servicio de controlador, controlador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay, debe ejecutarse antes de iniciarse el servicio de cliente de Distributed Replay en los clientes.  
  
> [!NOTE]  
>  Para quitar o cambiar las características de Distributed Replay, utilice la ventana **Programas y características** de Windows en el **Panel de control**. Seleccione [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en la ventana **Desinstalar o cambiar un programa** y, a continuación, haga clic en **Quitar** para abrir el Asistente para la instalación de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . En la página **Seleccionar características** , compruebe las características de Distributed Replay que desea quitar.  
  
 **Requisitos previos:**  
  
-   Asegúrese de que los equipos que desea utilizar cumplan los requisitos que se describen en el tema [Distributed Replay Requirements](../../tools/distributed-replay/distributed-replay-requirements.md).  
  
-   Antes de empezar este procedimiento, cree las cuentas de usuario de dominio en las que se ejecutarán los servicios de controlador y de cliente. Se recomienda que estas cuentas no sean miembros del grupo Administradores de Windows. Para obtener más información, vea la sección sobre cuentas de usuario y de servicio en el tema [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md) .  
  
    > [!NOTE]  
    >  Puede utilizar cuentas de usuario local si está ejecutando la herramienta de administración, el servicio de controlador y el servicio de cliente en el mismo equipo.  
  
 **Ubicaciones de instalación:**  
  
 Suponiendo que utilice las ubicaciones predeterminadas de archivos y una instalación estándar, el directorio base se encuentra en C:\Archivos de programa\Microsoft SQL Server. Dentro de ese directorio, los archivos binarios y los ensamblados se instalan en las siguientes ubicaciones:  
  
-   En un sistema de 32 bits:  
  
     [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Herramientas  
  
     \- O BIEN -  
  
     \<Share Feature Directory>\Tools\\ (directorio alternativo de características compartidas proporcionado por el usuario)  
  
-   En un sistema de 64 bits:  
  
     C:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (x86)\130\Tools  
  
     \- O BIEN -  
  
     \<Share Feature Directory (x86)>\Tools\\ (directorio alternativo de características compartidas (x86) proporcionado por el usuario)  
  
#### <a name="to-install-distributed-replay-features"></a>Para instalar las características de Distributed Replay  
  
1.  Para iniciar la instalación de cualquiera de las características de Distributed Replay, inicie el Asistente para la instalación de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
2.  La página **Reglas auxiliares del programa de instalación** identifica los problemas que pueden producirse en la instalación de los archivos auxiliares del programa de instalación de SQL Server. Debe corregir los errores debidos a esos problemas antes de continuar con la instalación.  
  
3.  En la página **Clave del producto** , seleccione un botón de opción para indicar si está instalando una edición gratuita de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o una versión de producción del producto con una clave de PID. Para obtener más información, vea [Ediciones y componentes de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
4.  En la página **Términos de licencia** , lea el contrato de licencia y active la casilla para aceptar los términos y condiciones de la licencia. Para ayudar a mejorar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], también puede habilitar la opción de uso de características y enviar informes a [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
5.  En la página **Archivos auxiliares del programa de instalación** , haga clic en **Instalar** para instalar o actualizar los archivos auxiliares del programa de instalación para [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
6.  En la página **Rol de instalación**, seleccione **Instalación de características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** y, después, haga clic en **Siguiente** para continuar en la página **Selección de características**.  
  
7.  En la página **Selección de características** , configure las características que desea instalar.  
  
    -   Para instalar la herramienta de administración, seleccione **Herramientas de administración - Básica**.  
  
    -   Para instalar el servicio de controlador, seleccione **Controlador de Distributed Replay**.  
  
    -   Para instalar el servicio de cliente, seleccione **Distributed Replay Client**.  
  
     **Importante**: Al configurar Distributed Replay Controller, puede especificar una o más cuentas de usuario que se usarán para ejecutar los servicios de cliente de Distributed Replay. La lista siguiente es una relación de las cuentas admitidas:  
  
    -   Cuenta de usuario de dominio  
  
    -   Cuenta de usuario local creada por el usuario  
  
    -   Administrador  
  
    -   Cuenta virtual y MSA (cuenta de servicio administrada)  
  
    -   Network Services, servicios locales y sistema  
  
     No se aceptan cuentas de grupo (locales o de dominio) y otras cuentas integradas (como Todos).  
  
8.  Opcionalmente, haga clic en el botón de puntos suspensivos (…) para cambiar la ruta de acceso al directorio de características compartidas.  
  
    1.  En equipos de 32 bits, la ruta de instalación predeterminada es **C:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
    2.  En equipos de 64 bits, la ruta de instalación predeterminada es **C:\Archivos de programa (x86)\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
9. Cuando haya terminado, haga clic en **Siguiente**.  
  
10. En la página **Reglas de instalación** , el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valida la configuración del equipo. Una vez completado el proceso de validación, haga clic en **Siguiente**.  
  
11. La página **Requisitos de espacio en disco** calcula el espacio en disco necesario para las características que haya especificado. A continuación, compara el espacio necesario con el espacio en disco disponible.  
  
12. En la página **Informes de errores** , especifique la información que desee enviar a [!INCLUDE[msCoName](../../includes/msconame-md.md)] para ayudar a mejorar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. De forma predeterminada, se habilita la opción de informe de errores.  
  
13. En la página **Reglas de configuración de instalación** , el Comprobador de configuración del sistema ejecutará uno o varios conjuntos de reglas para validar la configuración del equipo con las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que haya especificado.  
  
14. En la página **Preparado para instalar el programa** , haga clic en **Instalar**.  
  
    > [!IMPORTANT]  
    >  Después de instalar Distributed Replay, debe crear las reglas de firewall en los equipos cliente y de controlador, y conceder a cada equipo cliente permisos en el servidor de destino. Para obtener más información, vea [Completar los pasos posteriores a la instalación](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
### <a name="net-framework-security"></a>Seguridad de .NET Framework  
 Debe disponer de permisos administrativos para instalar cualquiera de las características de Distributed Replay. Solo un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con permisos sysadmin puede agregar las cuentas del servicio de cliente al rol de servidor sysadmin del servidor de prueba. Para obtener más información sobre las consideraciones de seguridad de Distributed Replay, vea [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
##  <a name="install-distributed-replay-from-the-command-prompt"></a><a name="bkmk_command_prompt"></a> Instalar Distributed Replay desde el símbolo del sistema  
 La instalación de una instancia nueva de Distributed Replay en el símbolo del sistema permite especificar las características que se instalarán y cómo se deben configurar. La instalación en el símbolo del sistema admite la instalación, la reparación, la actualización y la desinstalación de los componentes de Distributed Replay. Al realizar la instalación a través del símbolo del sistema, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el modo totalmente silencioso mediante el uso del parámetro /Q.  
  
> [!NOTE]  
>  En instalaciones locales, debe ejecutar el programa de instalación como administrador. Si instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un recurso compartido remoto, deberá usar una cuenta de dominio que tenga permisos de lectura y ejecución para dicho recurso.  
  
### <a name="installation-parameters"></a>Parámetros de instalación  
 La lista de las características de nivel superior incluye [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]y Herramientas. La característica Herramientas instalará las herramientas de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]y otros componentes compartidos. Para instalar los componentes de Distributed Replay, especifique los siguientes parámetros:  
  
|Componente|Parámetro|  
|---------------|---------------|  
|Controlador de Distributed Replay|**DREPLAY_CTLR**|  
|Servicio cliente de Distributed Replay|**DREPLAY_CLT**|  
|Herramienta de administración|**Herramientas**|  
  
> [!IMPORTANT]  
>  Después de instalar Distributed Replay, debe crear las reglas de firewall en los equipos cliente y de controlador, y conceder a cada equipo cliente permisos en el servidor de destino. Para obtener más información, vea [Completar los pasos posteriores a la instalación](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
 Use los parámetros de la tabla siguiente para desarrollar scripts de la línea de comandos para la instalación.  
  
|Parámetro|Descripción|Valores admitidos|  
|---------------|-----------------|----------------------|  
|/CTLRSVCACCOUNT<br /><br /> **Opcional**|Cuenta de servicio para el servicio Distributed Replay Controller.|Comprueba la cuenta y la contraseña|  
|/CTLRSVCPASSWORD<br /><br /> **Opcional**|Contraseña de la cuenta de servicio de Distributed Replay Controller.|Comprueba la cuenta y la contraseña|  
|/CTLRSTARTUPTYPE<br /><br /> **Opcional**|Tipo de inicio para el servicio Distributed Replay Controller.|Automático<br /><br /> Disabled<br /><br /> Manual|  
|/CTLRUSERS<br /><br /> **Opcional**|Especifica los usuarios que tienen permisos para el servicio de Distributed Replay Controller.|Conjunto de cadenas de la cuenta de usuario que usan " " (espacio) para el delimitador<br /><br /> **Importante**: al configurar el servicio Distributed Replay Controller, puede especificar una o más cuentas de usuario que se usarán para ejecutar los servicios Distributed Replay Client. La lista siguiente es una relación de las cuentas admitidas:<br /><br /> Cuenta de usuario de dominio<br /><br /> Cuenta de usuario local creada por el usuario<br /><br /> Administrador<br /><br /> Administrador<br /><br /> Cuenta virtual y MSA (cuenta de servicio administrada)<br /><br /> Network Services, servicios locales y sistema<br /><br /> <br /><br /> Nota: No se aceptan cuentas de grupo (locales o de dominio) y otras cuentas integradas (como Todos).|  
|/CLTSVCACCOUNT<br /><br /> **Opcional**|Cuenta de servicio para el servicio de cliente de Distributed Replay.|Comprueba la cuenta y la contraseña|  
|/CLTSVCPASSWORD<br /><br /> **Opcional**|Contraseña de la cuenta de servicio de Distributed Replay Client.|Comprueba la cuenta y la contraseña|  
|/CLTSTARTUPTYPE<br /><br /> **Opcional**|Tipo de inicio para el servicio de cliente de Distributed Replay.|Automático<br /><br /> Disabled<br /><br /> Manual|  
|/CLTCTLRNAME<br /><br /> **Opcional**|Nombre del equipo con el que se comunica el cliente para el servicio de Distributed Replay Controller.||  
|/CLTWORKINGDIR<br /><br /> **Opcional**|Directorio de trabajo para el servicio de Distributed Replay Client.|Ruta de acceso válida|  
|/CLTRESULTDIR<br /><br /> **Opcional**|Directorio de resultados para el servicio de Distributed Replay Client.|Ruta de acceso válida|  
  
### <a name="sample-syntax"></a>Sintaxis de ejemplo:  
 **Para instalar el componente Distributed Replay Controller**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CTLR /IAcceptSQLServerLicenseTerms /CTLRUSERS="domain\user1" "domain\user2" /CTLRSVCACCOUNT="domain\svcuser" /CTLRSVCPASSWORD="password" /CTLRSTARTUPTYPE=Automatic  
```  
  
 **Para instalar el componente Distributed Replay Client**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CLT /IAcceptSQLServerLicenseTerms /CLTSVCACCOUNT="domain\svcuser" /CLTSVCPASSWORD="password" /CLTSTARTUPTYPE=Automatic /CLTCTLRNAME=ControllerMachineName /CLTWORKINGDIR="C:\WorkingDir" /CLTRESULTDIR="C:\ResultDir  
```  
  
##  <a name="install-distributed-replay-using-a-configuration-file"></a><a name="bkmk_configuration_file"></a> Instalar Distributed Replay utilizando un archivo de configuración  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite generar un archivo de configuración basado en la entrada de usuario y en la configuración predeterminada del sistema. Si especifica que desea que se instalen las herramientas de administración, puede utilizar el archivo de configuración para implementar los tres componentes de Distributed Replay (herramienta de administración, controlador de Distributed Replay y cliente Distributed Replay). Admite la instalación, reparación y desinstalación de los componentes de Distributed Replay.  
  
 El programa de instalación admite el uso del archivo de configuración solamente a través de la línea de comandos. A continuación se indica el orden de procesamiento de los parámetros cuando se usa el archivo de configuración:  
  
-   El archivo de configuración sobrescribe los valores predeterminados de un paquete.  
  
-   Los valores de línea de comandos sobrescriben los valores del archivo de configuración.  
  
 Para obtener más información sobre cómo usar un archivo de configuración, vea [Instalar SQL Server 2016 usando un archivo de configuración](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
  
> [!IMPORTANT]  
>  Después de instalar Distributed Replay, debe crear las reglas de firewall en los equipos cliente y de controlador, y conceder a cada equipo cliente permisos en el servidor de destino. Para obtener más información, vea [Completar los pasos posteriores a la instalación](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
#### <a name="to-generate-a-configuration-file"></a>Para generar un archivo de configuración  
  
1.  Siga los pasos del asistente para instalación hasta llegar a la página **Listo para instalar** . La ruta de acceso al archivo de configuración se especifica en la sección que así lo indica en la página **Listo para instalar** .  
  
2.  Cancele la instalación sin completarla realmente para generar el archivo INI.  
  
#### <a name="to-install-distributed-replay-using-the-configuration-file"></a>Para instalar Distributed Replay utilizando el archivo de configuración  
  
-   Realice la instalación utilizando el símbolo del sistema y proporcione el archivo ConfigurationFile.ini mediante el parámetro ConfigurationFile.  
  
 **Sintaxis de ejemplo**  
  
 A continuación figura un ejemplo sobre cómo especificar el archivo de configuración en el símbolo del sistema:  
  
```
Setup.exe /CTLRSVCPASSWORD="ctlrsvcpswd" /CLTSVCPASSWORD="cltsvcpswd" / ConfigurationFile=ConfigurationFile.INI\  
```

> [!NOTE]
> Debe especificar ambas contraseñas en la línea de comandos porque no puede configurarlas en el archivo de configuración.  

## <a name="see-also"></a>Consulte también

- [Características admitidas por las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)
- [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Requisitos de Distributed Replay](../../tools/distributed-replay/distributed-replay-requirements.md)
- [Opciones de línea de comandos de la herramienta de administración &#40;utilidad Distributed Replay&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)
- [Configurar Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)