---
title: SQL Server Management Studio (SSMS)
description: Obtenga información sobre SQL Server Management Studio (SSMS) y lo que puede hacer SMMS, incluso cómo administrar soluciones de Analysis Services.
ms.prod: sql
ms.technology: ssms
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: ''
f1_keywords:
- sql13.ssms.viewhelp.f1
helpviewer_keywords:
- SQL Server Management Studio
- SQL Server Management Studio for Integration Services
- SQL Server Management Studio for Reporting Services
- SQL Server Management Studio for Analysis Services
ms.custom: seo-lt-2019
ms.date: 09/11/2019
ms.openlocfilehash: 49c6f8c104ea71f2826d40446fa3e6a110b6aeb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340706"
---
# <a name="what-is-sql-server-management-studio-ssms"></a>¿Qué es SQL Server Management Studio (SSMS)?

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) es un entorno integrado para administrar cualquier infraestructura de SQL. Use SSMS para acceder, configurar, administrar y desarrollar todos los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], Azure SQL Database y Azure Synapse Analytics. SSMS ofrece una única utilidad integral que combina un amplio grupo de herramientas gráficas con una serie de editores de script enriquecidos que permiten a desarrolladores y administradores de bases de datos de todos los niveles acceder a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- [**Descarga de SQL Server Management Studio (SSMS)**](download-sql-server-management-studio-ssms.md)
- [**Descarga de SQL Server Developer**](https://my.visualstudio.com/Downloads?q=SQL%20Server%20Developer)
- [**Descarga de Visual Studio**](https://www.visualstudio.com/downloads/)

![Captura de pantalla del SQL Server Management Studio.](media/sql-server-management-studio-ssms/ssms.png)

## <a name="sql-server-management-studio-components"></a>Componentes de SQL Server Management Studio  
  
|Descripción|Componente|  
|---------------|---------|  
|Uso del **Explorador de objetos** para ver y administrar todos los objetos de una o más instancias de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|[Explorador de objetos](../ssms/object/object-explorer.md)|  
|Cómo usar el **Explorador de plantillas** para compilar y administrar archivos de texto reutilizable que se pueden usar para acelerar el desarrollo de consultas y scripts.|[Explorador de plantillas](../ssms/template/template-explorer.md)|  
|Cómo usar el **Explorador de soluciones** desusado para compilar proyectos que se emplean para administrar elementos de administración como scripts y consultas.|[Explorador de soluciones](../ssms/solution/solution-explorer.md)|  
|Cómo usar las herramientas de diseño visual incluidas en [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].|[Visual Database Tools](../ssms/visual-db-tools/visual-database-tools.md)|  
|Cómo usar los editores de lenguaje de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] para compilar y depurar interactivamente consultas y scripts.|[Editores de texto y consultas](./f1-help/database-engine-query-editor-sql-server-management-studio.md)

## <a name="sql-server-management-studio-for-business-intelligence"></a>SQL Server Management Studio para Business Intelligence

Para obtener acceso, configurar y administrar [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)], [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] y [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], utilice [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. Aunque las tres tecnologías de Business Intelligence se basan en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], las tareas administrativas asociadas a cada una de ellas son ligeramente diferentes.

> [!NOTE]
> Para crear y modificar soluciones de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]e [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , utilice [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)], no [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)] es un entorno de desarrollo basado en [!INCLUDE[msCoName](../includes/msconame_md.md)][!INCLUDE[vsprvs](../includes/vsprvs-md.md)].

### <a name="managing-analysis-services-solutions-using-sql-server-management-studio"></a>Administrar las soluciones de Analysis Services con SQL Server Management Studio

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] permite administrar los objetos de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , por ejemplo, realizar copias de seguridad y procesar los objetos.

[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] proporciona un proyecto de Script de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] en el que se desarrollan y guardan los scripts escritos en expresiones multidimensionales (MDX), Extensiones de minería de Datos (DMX) y XML for Analysis (XMLA). Los proyectos de Scripts de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] se usan para realizar tareas de administración o para volver a crear objetos, como bases de datos y cubos, en instancias de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] . Por ejemplo, puede desarrollar un script XMLA en un proyecto de Script de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] que cree directamente los objetos nuevos en una instancia de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] existente. Los proyectos de Scripts de [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] se pueden guardar como parte de una solución e integrarlos con un control de código fuente.
  
Para más información sobre cómo usar [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], consulte [Desarrollar e implementar usando SQL Server Management Studio](/analysis-services/instances/analysis-services-scripts-project-in-sql-server-management-studio).
  
### <a name="managing-integration-services-solutions-using-sql-server-management-studio"></a>Administrar las soluciones de Integration Services con SQL Server Management Studio

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] permite usar el servicio [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] para administrar paquetes y supervisar los paquetes en ejecución. Además, se puede utilizar [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] para organizar los paquetes en carpetas; ejecutar, importar y exportar paquetes; migrar los paquetes de Servicios de transformación de datos (DTS); y actualizar los paquetes de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .

### <a name="managing-reporting-services-projects-using-sql-server-management-studio"></a>Administrar proyectos de Reporting Services con SQL Server Management Studio

Utilice SQL Server Management Studio para habilitar las características de Reporting Services y administrar el servidor, las bases de datos, los roles y los trabajos.

Las programaciones compartidas se administran con la carpeta Programaciones compartidas y las bases de datos del servidor de informes (ReportServer, ReportServerTempdb) también se administran. También se crea un rol RSExecRole en la base de datos del sistema maestra cuando se mueve una base de datos del servidor de informes a un motor de base de datos de SQL Server nuevo o diferente ([!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde_md.md)]). Para más información sobre estas tareas, vea los artículos siguientes:  

- [Reporting Services en SSMS](../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md)
- [Administración de una base de datos del servidor de informes](../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)
- [Creación de RSExecRole](../reporting-services/security/create-the-rsexecrole.md)

También administra el servidor habilitando y configurando diversas características, estableciendo los servidores predeterminados y administrando los roles y los trabajos. Para más información sobre estas tareas, vea los artículos siguientes:

- [Establecimiento de las propiedades del servidor de informes](../reporting-services/tools/set-report-server-properties-management-studio.md)
- [Crear, eliminar o modificar un rol](../reporting-services/security/role-definitions-create-delete-or-modify.md)
- [Habilitar y deshabilitar la impresión del lado cliente para Reporting Services](../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md)

## <a name="non-english-language-versions-of-sql-server-management-studio-ssms"></a>Versiones de idioma de SQL Server Management Studio (SSMS) distintas del inglés

Se ha suprimido el bloque en la configuración de idiomas mixtos. Puede instalar SSMS en alemán en un Windows en francés. Si el idioma del sistema operativo no coincide con el idioma de SSMS, el usuario debe cambiar el idioma en Herramientas > Opciones > Configuración internacional. En caso contrario, SSMS mostrará la interfaz de usuario en inglés.

Para obtener más información sobre las distintas configuraciones regionales de versiones anteriores, vea [Instalación de versiones de idioma de SQL Server Management Studio (SSMS) distintas del inglés](install-other-languages.md).

## <a name="support-policy-for-ssms"></a>Directiva de compatibilidad de SSMS

- A partir de SSMS 17.0, el equipo de herramientas SQL ha adoptado la [directiva de ciclo de vida moderna de Microsoft](https://support.microsoft.com/help/30881/modern-lifecycle-policy).
- Lea el anuncio [de la directiva de ciclo de vida moderna](https://support.microsoft.com/help/447912/announcing-microsoft-modern-lifecycle-policy) original. Para obtener más información, vea [Preguntas más frecuentes sobre la directiva moderna de ciclo de vida](https://support.microsoft.com/help/30882/modern-lifecycle-policy-faq).
- Para obtener información sobre la recopilación de datos de diagnóstico y el uso de características, vea el [complemento de privacidad de SQL Server](../sql-server/sql-server-privacy.md).

## <a name="cross-platform-tool"></a>Herramienta multiplataforma

[!INCLUDE[ssms-azure-data-studio-mention](../includes/ssms-azure-data-studio-mention.md)]

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Instalación de versiones de idioma de SQL Server Management Studio (SSMS) distintas del inglés](install-other-languages.md)
- [Conexión a una instancia de SQL Server y realización de consultas](./quickstarts/ssms-connect-query-sql-server.md)
- [Escritura de instrucciones Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]