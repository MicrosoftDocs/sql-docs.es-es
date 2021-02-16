---
description: Actualizar paquetes de Integration Services
title: Actualizar paquetes de Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, migrating
- migrating packages [Integration Services]
ms.assetid: 68dbdf81-032c-4a73-99f6-41420e053980
author: MikeRayMSFT
ms.author: mikeray
manager: erikre
ms.openlocfilehash: f064880f44b25a96ab5f93e15ee27c361182ba81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352038"
---
# <a name="upgrade-integration-services-packages"></a>Actualizar paquetes de Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cuando se actualiza una instancia de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los paquetes de [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] existentes no se actualizan automáticamente al formato de paquete que la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utiliza. Tendrá que seleccionar un método de actualización y actualizar manualmente los paquetes.  
  
 Para obtener información sobre la actualización de paquetes al convertir un proyecto al modelo de implementación de proyecto, vea [Implementación de proyectos y paquetes de Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).
  
## <a name="selecting-an-upgrade-method"></a>Seleccionar un método de actualización  
 Puede utilizar distintos métodos para actualizar los paquetes de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] . En algunos de estos métodos, la actualización solo es temporal. En otros, es definitiva. La tabla siguiente describe cada uno de estos métodos y si la actualización es temporal o definitiva.  
  
> [!NOTE]  
>  Cuando se ejecuta un paquete de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] mediante la utilidad **dtexec** (dtexec.exe) que se instala con la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la actualización temporal del paquete incrementa el tiempo de ejecución. La proporción de incremento de tiempo de ejecución del paquete depende del tamaño del mismo. Para evitar un incremento del tiempo de ejecución, se recomienda actualizar el paquete antes de ejecutarlo.  
  
|Método de actualización|Tipo de actualización|  
|--------------------|---------------------|  
|Use la utilidad **dtexec** (dtexec.exe) que se instala con la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar un paquete de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] .<br /><br /> Para obtener más información, consulte [utilidad dtexec](../../integration-services/packages/dtexec-utility.md).|La actualización del paquete es temporal.<br /><br /> No se pueden guardar los cambios.|  
|Abra un archivo de paquete de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|La actualización del paquete es definitiva si guarda el paquete; de lo contrario, es temporal.|  
|Agregue un paquete de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a un proyecto existente en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|La actualización del paquete es definitiva.|  
|Abra un archivo de proyecto de [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o posterior en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]y luego use el Asistente para actualizar paquetes [!INCLUDE[ssIS](../../includes/ssis-md.md)] para actualizar varios paquetes en el proyecto.<br /><br /> Para obtener más información, vea [Actualizar paquetes de Integration Services mediante el Asistente para actualizar paquetes SSIS](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md) y [Ayuda F1 del Asistente para actualización del paquete SSIS](../../integration-services/ssis-package-upgrade-wizard-f1-help.md).|La actualización del paquete es definitiva.|  
|Use la utilidad <xref:Microsoft.SqlServer.Dts.Runtime.Application.Upgrade%2A> para actualizar uno o más paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|La actualización del paquete es definitiva.|  
  
## <a name="custom-applications-and-custom-components"></a>Aplicaciones y componentes personalizados  
 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] no funcionarán con la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Puede utilizar la versión actual de las herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para ejecutar y administrar paquetes que incluyen componentes personalizados de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssIS](../../includes/ssis-md.md)] . Se han agregado cuatro reglas de redirección de enlace a los archivos siguientes para ayudar a redirigir los ensamblados en tiempo de ejecución de las versiones 10.0.0.0 ([!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]), 11.0.0.0 ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) o 12.0.0.0 ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) a la versión 15.0.0.0 ([!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]).  
  
-   DTExec.exe.config  
  
-   dtshost.exe.config  
  
-   DTSWizard.exe.config  
  
-   DTUtil.exe.config  
  
-   DTExecUI.exe.config  
  
 Si quiere usar [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] para diseñar paquetes que incluyan componentes personalizados de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], debe modificar el archivo devenv.exe.config que se encuentra en *\<drive>* :\Archivos de programa\Microsoft Visual Studio 10.0\Common7\IDE.  
  
 Para usar estos paquetes con aplicaciones cliente compiladas con el motor de ejecución para [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)], incluya reglas de redirección en la sección de configuración del archivo *.exe.config para el ejecutable. Las reglas redirigirán los ensamblados en tiempo de ejecución a la versión 15.0.0.0 ([!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]). Para obtener más información sobre la redirección de la versión de ensamblado, vea [Elemento \<assemblyBinding> para \<runtime>](/dotnet/framework/configure-apps/file-schema/runtime/assemblybinding-element-for-runtime).  
  
### <a name="locating-the-assemblies"></a>Buscar los ensamblados  
 En [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)], los ensamblados de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se actualizaron a .NET 4.0. Hay una caché global de ensamblados diferente para .NET 4, que se encuentra en *\<drive>* :\Windows\Microsoft.NET\assembly. Puede buscar todos los ensamblados de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] bajo esta ruta de acceso, normalmente en la carpeta GAC_MSIL.  
  
 Como ocurre en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los archivos básicos de extensibilidad .dll de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] también se encuentran en *\<drive>* :\Archivos de programa\Microsoft SQL Server\130\SDK\Assemblies.  
  
## <a name="understanding-sql-server-package-upgrade-results"></a>Descripción de los resultados de la actualización del paquete SQL Server  
 Durante el proceso de actualización del paquete, la mayoría de los componentes y las características de los paquetes de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] se convierten sin problema en sus homólogos de la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin embargo, hay varios componentes y características que no se actualizarán u obtendrán resultados en la actualización que habría que tener en cuenta. La tabla siguiente identifica estos componentes y características.  
  
> [!NOTE]  
>  Para identificar qué paquetes experimentan los problemas enumerados en esta tabla, ejecute el Asesor de actualizaciones.  
  
|Componente o característica|Resultados de la actualización|  
|--------------------------|---------------------|  
|Cadenas de conexión|En el caso de paquetes de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , los nombres de ciertos proveedores han cambiado y requieren valores diferentes en las cadenas de conexión. Para actualizar las cadenas de conexión, utilice uno de los procedimientos siguientes:<br /><br /> Use el Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] para actualizar el paquete y seleccione la opción **Actualizar las cadenas de conexión para reflejar los nuevos nombres de proveedor** .<br /><br /> En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], en la página General del cuadro de diálogo Opciones, seleccione la opción **Actualizar las cadenas de conexión para reflejar los nuevos nombres de proveedor** . Para obtener más información acerca de esta opción, vea Página General.<br /><br /> En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el paquete y cambie manualmente el texto de la propiedad ConnectionString.<br /><br /> Nota: No puede usar los procedimientos anteriores para actualizar una cadena de conexión cuando esta se almacena en un archivo de configuración o en un archivo de origen de datos ni cuando una expresión establece la propiedad **ConnectionString** . Para actualizar la cadena de conexión en estos casos, debe actualizar manualmente el archivo o la expresión.<br /><br /> Para obtener más información sobre los orígenes de datos, vea [Orígenes de datos](../../integration-services/connection-manager/data-sources.md).|  
  
### <a name="scripts-that-depend-on-adodbdll"></a>Scripts que dependen de ADODB.dll  
 Es posible que los scripts Tarea de script y Componente de script que hacen referencia de forma explícita a ADODB.dll no se actualicen o ejecuten en equipos sin [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] instalado. Para actualizar los scripts Tarea de script o Componente de script, se recomienda que quite la dependencia de ADODB.dll.  Ado.Net es la alternativa recomendada para el código administrado como scripts VB y C#.  
  
