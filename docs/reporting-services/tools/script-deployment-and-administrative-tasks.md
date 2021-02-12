---
title: Script para tareas administrativas y de implementación | Microsoft Docs
description: Obtenga información sobre las herramientas, tecnologías y estrategias para automatizar las tareas rutinarias de instalación, implementación y administración de un servidor de informes.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- scripts [Reporting Services]
- moving reports
- report servers [Reporting Services], duplicating settings
- deploying [Reporting Services], scripts
- copying report server settings
- administrative tasks [Reporting Services]
- duplicating report server environment
- migrating reports [Reporting Services]
- scripts [Reporting Services], deployments
- transferrng reports
- reports [Reporting Services], migrating
ms.assetid: d0416c9e-e3f9-456d-9870-2cfd2c49039b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 883acc6af22c69b6deda8606c2361e33e55aeb1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077926"
---
# <a name="script-deployment-and-administrative-tasks"></a>Script para tareas administrativas y de implementación

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] admite el uso de scripts para automatizar las tareas rutinarias de instalación, implementación y administración. La implementación de un servidor de informes es un proceso de varios pasos en el que deben usarse varias herramientas y procedimientos de configuración: no existe ningún programa o método que automatice todas las tareas.  
  
 Tampoco se recomienda automatizar siempre todos los pasos. A veces, la manera más sencilla y eficaz de realizar un paso es hacerlo manualmente o a través de una herramienta gráfica. Por ejemplo, si se desea implementar muchos informes y modelos, es mejor copiar las bases de datos del servidor de informes que escribir código para volver a crear el entorno del servidor de informes.  
  
 Algunos pasos requieren código personalizado. Por ejemplo, la configuración de las direcciones URL para el servicio web y el Administrador de informes se puede automatizar, pero solo si se escribe código personalizado que realice llamadas al proveedor WMI (Instrumental de administración de Windows) del servidor de informes. Si no se desea escribir código, deberá usarse la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para realizar este paso.  
  
 Para ejecutar el script que configura un servidor de informes, debe ser un administrador local en el equipo que está configurando. Para obtener más información, vea [Configurar un servidor de informes para la administración remota](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).  
  
 En este tema se describen los enfoques recomendados para automatizar algunos pasos específicos. Se mencionan varios programas e interfaces de programación; podrá ver descripciones de cada uno de ellos más adelante en este tema.  
  
## <a name="deployment-tasks-and-how-to-automate-them"></a>Tareas de implementación y procedimientos para automatizarlas  
 En la tabla siguiente se resumen todas las tareas de instalación y configuración que son necesarias para implementar un servidor de informes. Quizás le sea de utilidad para relacionar una tarea específica con un método que le permita automatizarla o realizarla de manera desatendida.  
  
|Tarea|Enfoque|  
|----------|--------------|  
|Instalar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Puede ejecutar el programa de instalación desde la línea de comandos para realizar una instalación desatendida.<br /><br /> Puede usar el programa de instalación para instalar y configurar un servidor de informes, pero solo si especifica la opción de configuración predeterminada y el sistema cumple todos los requisitos de este tipo de instalación. Si no puede instalar la configuración predeterminada, deberá realizar una instalación de solo archivos.|  
|Configurar la cuenta de servicio.|La cuenta de servicio se configura inicialmente a través de la instalación. Para automatizar los cambios a la cuenta de servicio como una tarea posterior a la instalación, debe escribir código personalizado que realice llamadas al proveedor WMI del servidor de informes. No hay ninguna utilidad de símbolo del sistema o plantillas de script para configurar la cuenta de servicio mediante programación.<br /><br /> Si los requisitos de codificación evitan que automatice este paso, puede configurar con facilidad la cuenta manualmente ejecutando la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para más información, vea [Configurar una cuenta de servicio &#40;Administrador de configuración del servidor de informes&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).|  
|Configurar el servicio web del servidor de informes y las direcciones URL del Administrador de informes.|Debe escribir código personalizado que realice llamadas al proveedor WMI del servidor de informes. No hay ninguna utilidad de línea de comandos o plantillas de script para configurar las direcciones URL.<br /><br /> Si desea evitar escribir código, puede configurar las direcciones URL manualmente ejecutando la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para más información, vea [Configurar una dirección URL &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).|  
|Crear la base de datos del servidor de informes.|Debe escribir código personalizado que realice llamadas al proveedor WMI del servidor de informes. No hay ninguna utilidad de símbolo del sistema o plantillas de script para crear las bases de datos del servidor de informes y RSExecRole.<br /><br /> Si desea evitar escribir código, puede crear la base de datos manualmente ejecutando la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para más información, vea [Crear una base de datos del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).|  
|Configurar la conexión a la base de datos del servidor de informes.|Si va a cambiar la cadena de conexión, la cuenta o la contraseña o el tipo de autenticación, ejecute la utilidad **rsconfig** para configurar la conexión. Para más información, vea [Configurar una conexión a la base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md) y [rsconfig (utilidad) &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md).<br /><br /> No puede utilizar rsconfig.exe para crear o actualizar la base de datos. La base de datos y RSExecRole deben existir ya.|  
|Configurar una implementación escalada|Para automatizar una implementación escalada, elija uno de los métodos siguientes:<br /><br /> -   Ejecute la utilidad rskeymgmt.exe para unir instancias del servidor de informes a una instalación existente. Para más información, vea [Agregar y quitar claves de cifrado para implementaciones escaladas &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md).<br />-   Escriba código personalizado que se ejecute en el proveedor WMI del servidor de informes.|  
|Realizar una copia de seguridad de las claves de cifrado|Para automatizar el procedimiento de copia de seguridad de las claves de cifrado, elija uno de los métodos siguientes:<br /><br /> -   Ejecute la utilidad rskeymgmt.exe para realizar la copia de seguridad de las claves. Para obtener más información, vea [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).<br />-   Escriba código personalizado que se ejecute en el proveedor WMI del servidor de informes.|  
|Configurar el correo electrónico del servidor de informes|Escriba código personalizado que se ejecute en el proveedor WMI de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . El proveedor admite un subconjunto de los valores de configuración de correo electrónico.<br /><br /> Aunque el archivo RSReportServer.config incluye toda la configuración, no utilice el archivo de forma automatizada. En concreto, no utilice un archivo por lotes para copiar el archivo en otro servidor de informes. Cada archivo de configuración incluye valores específicos de la instancia actual. Dichos valores no serán válidos en otras instancias del servidor de informes.<br /><br /> Para más información sobre la configuración, consulte [Configuración de correo electrónico: modo nativo de Reporting Services (Administrador de configuración)](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md).|  
|Configurar la cuenta de ejecución desatendida|Para automatizar la configuración de la cuenta de procesamiento desatendido, elija uno de los métodos siguientes:<br /><br /> -   Ejecute la utilidad rsconfig.exe para configurar la cuenta. Para más información, vea [Configurar la cuenta de ejecución desatendida &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).<br />-   Escriba código personalizado que realice llamadas al proveedor WMI del servidor de informes.|  
|Implementar contenido existente en otro servidor de informes, incluyendo la jerarquía de carpetas, las asignaciones de roles, los informes, las suscripciones, las programaciones, los orígenes de datos y los recursos.|La mejor forma de volver a crear un entorno de servidor de informes existente es copiar la base de datos del servidor de informes en una nueva instancia del servidor de informes.<br /><br /> Un método alternativo es escribir código personalizado que vuelva a crear el contenido del servidor de informes existente mediante programación. Sin embargo, tenga en cuenta que las suscripciones, las instantáneas del informe y el historial del informe no se pueden volver a crear mediante programación.<br /><br /> Algunas implementaciones pueden beneficiarse de ambas técnicas, es decir, restaurar una base de datos del servidor de informes y, después, ejecutar código personalizado con el fin de modificar la base de datos del servidor de informes para una instalación específica.<br /><br /> Para consultar un ejemplo detallado, vea [Script rs.exe de ejemplo de Reporting Services para copiar contenido entre servidores de informes](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).<br /><br /> Para más información sobre cómo modificar la ubicación de una base de datos del servidor de informes, vea [Mover las bases de datos del servidor de informes a otro equipo &#40;Modo nativo de SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md). Para obtener más información acerca de la creación del entorno del servidor de informes mediante programación, vea la sección "Usar script para migrar las carpetas y el contenido del servidor de informes" en este tema.|  
  
## <a name="tools-and-technologies-for-automating-server-deployment"></a>Herramientas y tecnologías para automatizar la implementación del servidor  
 En la lista siguiente se resumen los programas y las interfaces que se pueden utilizar para automatizar las tareas de implementación y mantenimiento:  
  
-   El programa de instalación se puede ejecutar en modo desatendido para instalar y, en algunos casos, configurar los componentes del servidor de informes. Debe usar la opción de instalación de solo archivos para que el programa de instalación configure una instancia de servidor de informes.  
  
-   Se pueden usar el proveedor WMI de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y las utilidades de línea de comandos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para la configuración local y remota del servidor.  
  
     El proveedor WMI de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] expone las clases, propiedades y métodos que permiten al usuario configurar todos los aspectos de una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , lo que incluye la especificación de cuentas de servicio, la configuración de direcciones URL, la creación y configuración de la base de datos del servidor de informes o la configuración de un servidor de informes para la entrega por correo electrónico. Para utilizar el proveedor WMI, es necesario escribir script o código personalizados. Para obtener más información, vea [Obtener acceso al proveedor WMI de Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
     Las utilidades de la línea de comandos (rsconfig.exe y rskeymgmt.exe) son una alternativa a escribir código. Se pueden escribir archivos por lotes que ejecuten las utilidades. Estas utilidades permiten automatizar algunas tareas de configuración, pero no todas.  
  
-   La herramienta de host del script del servidor de informes (rs.exe) puede ejecutar código personalizado de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] que se haya escrito para volver a crear o mover el contenido de un servidor de informes a otro. Con este método, se escribe script en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], se guarda como un archivo .rss y, después, se usa rs.exe para ejecutarlo en el servidor de informes de destino. El script escrito puede llamar a la interfaz SOAP para el servicio web del servidor de informes. Los scripts de implementación se escriben siguiendo este método porque es posible volver a crear un espacio de nombres de carpetas del servidor de informes y su contenido, así como volver a crear la seguridad basada en roles.  
  
-   En la versión SQL Server 2012 se introdujeron los cmdlets de PowerShell para el modo integrado de SharePoint. Puede utilizar PowerShell para configurar y administrar la integración de SharePoint.  Para obtener más información, vea [Cmdlets de PowerShell para el modo de SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  
  
## <a name="use-scripts-to-migrate-report-server-content-and-folders"></a>Usar scripts para migrar contenido y carpetas del servidor de informes  
 Se pueden escribir scripts que dupliquen un entorno de servidor de informes en otra instancia de servidor de informes. Los scripts de implementación suelen escribirse en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] y, a continuación, se procesan mediante la utilidad del host de script del servidor de informes.  
  
 Para consultar un ejemplo detallado, vea [Script rs.exe de ejemplo de Reporting Services para copiar contenido entre servidores de informes](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
 Utilice scripts para copiar carpetas, orígenes de datos compartidos, recursos, informes, asignaciones de roles y parámetros de un servidor a otro. Puede escribir un script para una instancia de servidor de informes y, a continuación, ejecutarlo en otro servidor para volver a crear el espacio de nombres del servidor de informes. Si existen varios servidores de informes en la implementación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puede ejecutar el script en cada servidor individualmente para configurar todos los servidores de la misma manera.  
  
 En la siguiente lista, se describen los pasos necesarios para migrar informes de un servidor a otro.  
  
1.  Defina la variable de script con la dirección URL del servidor de informes de origen.  
  
2.  Use los métodos <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A> y <xref:ReportService2010.ReportingService2010.GetProperties%2A> para recuperar la definición de informe y las propiedades del informe.  
  
3.  Establezca la dirección URL para que señale al servidor de destino.  
  
4.  Use el método <xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A> , pasando las propiedades devueltas desde <xref:ReportService2010.ReportingService2010.GetProperties%2A> y la definición del informe devuelta por <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A>.  
  
 Combinando métodos de obtención y creación, puede realizar pasos similares que le permitan migrar parámetros, carpetas, orígenes de datos compartidos y recursos. Para más información sobre los métodos disponibles, vea [Referencia técnica &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md).  
  
> [!NOTE]  
>  Los scripts se ejecutan con las credenciales de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows del usuario que ejecuta el script, salvo que las credenciales se establezcan explícitamente.  
  
 Para obtener más información sobre cómo ejecutar y dar formato a un archivo de script, vea [Script con la utilidad rs.exe y el servicio Web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md).  
  
## <a name="using-scripts-to-set-server-properties"></a>Usar scripts para establecer propiedades del servidor  
 Puede escribir scripts que establezcan propiedades del sistema en el servidor de informes. El script de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET siguiente muestra una manera de establecer las propiedades. Este ejemplo deshabilita el control ActiveX RSClientPrint, aunque es posible sustituir **EnableClientPrinting** y **False** por cualquier nombre y valor de propiedad válidos. Para obtener una lista completa de propiedades de servidor, vea [Report Server System Properties](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).  
  
 Para utilizar el script, guárdelo en un archivo con la extensión .rss y, a continuación, utilice la utilidad de símbolo del sistema rs.exe para ejecutar el archivo en el servidor de informes. El script no está compilado, por lo que no es necesario tener instalado [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. En este ejemplo se da por supuesto que tiene permisos en el equipo local que hospeda el servidor de informes. Si no ha iniciado una sesión con una cuenta con permisos, debe especificar la información de la cuenta mediante argumentos adicionales de la línea de comandos. Para más información, vea [Utilidad RS.exe &#40;SSRS&#41;](../../reporting-services/tools/rs-exe-utility-ssrs.md).  
  
> [!TIP]  
>  Para consultar un ejemplo detallado, vea [Script rs.exe de ejemplo de Reporting Services para copiar contenido entre servidores de informes](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
```  
Public Sub Main()  
        Dim props(0) As [Property]  
        Dim setProp As New [Property]  
        setProp.Name = "EnableClientPrinting"  
        setProp.Value = "False"   
        props(0) = setProp  
        Try  
            rs.SetSystemProperties(props)  
        Catch ex As System.Web.Services.Protocols.SoapException  
            Console.Write(ex.Detail.InnerXml)  
        Catch e as Exception  
            Console.Write(e.Message)  
        End Try  
End Sub  
```

## <a name="next-steps"></a>Pasos siguientes

[Método GenerateDatabaseCreationScript &#40;MSReportServer_ConfigurationSetting de WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabasecreationscript.md)   
[Método GenerateDatabaseRightsScript &#40;MSReportServer_ConfigurationSetting de WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaserightsscript.md)   
[Método GenerateDatabaseUpgradeScript &#40;MSReportServer_ConfigurationSetting de WMI&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)   
[Instalar SQL Server desde el símbolo del sistema](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
[Instalar el servidor de informes en modo nativo de Reporting Services](~/reporting-services/install-windows/install-reporting-services-native-mode-report-server.md)   
[Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
[Utilidades del símbolo del sistema del servidor de informes &#40;SSRS&#41;](../../reporting-services/tools/report-server-command-prompt-utilities-ssrs.md)   
[Compatibilidad del explorador de Reporting Services y Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)   
[Herramientas de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
