---
title: Descargar SQL Server Data Tools (SSDT)
description: Obtenga información sobre SQL Server Data Tools (SSDT). Vea cómo instalar este conjunto de herramientas de desarrollo de bases de datos con Visual Studio 2019 y Visual Studio 2017.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
keywords: instalar ssdt, descargar ssdt, ssdt más reciente
ms.assetid: b0fc4987-d260-4d0a-9dd1-98099835b361
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 02/20/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: cf088c407a95d32f3cfc7413b55ec41661126e78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077246"
---
# <a name="download-sql-server-data-tools-ssdt-for-visual-studio"></a>Descarga de SQL Server Data Tools (SSDT) para Visual Studio

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

**SQL Server Data Tools (SSDT)** es una herramienta de desarrollo moderna para crear bases de datos relacionales de SQL Server, bases de datos de Azure SQL, modelos de datos de Analysis Services (AS), paquetes de Integration Services (IS) e informes de Reporting Services (RS). Gracias a SSDT, puede diseñar e implementar cualquier tipo de contenido de SQL Server con la misma facilidad con la que desarrollaría una aplicación en Visual Studio.

## <a name="ssdt-for-visual-studio-2019"></a>SSDT para Visual Studio 2019

### <a name="changes-in-ssdt-for-visual-studio-2019"></a>Cambios en SSDT para Visual Studio 2019

La funcionalidad principal de SSDT para crear proyectos de base de datos ha permanecido sin cambios en Visual Studio.

Con Visual Studio 2019, la funcionalidad necesaria para habilitar Analysis Services, Integration Services y los proyectos de Reporting Services solo se ha trasladado a las respectivas extensiones de Visual Studio (VSIX).

> [!NOTE]
> No hay ningún instalador independiente de SSDT para Visual Studio 2019.

### <a name="install-ssdt-with-visual-studio-2019"></a>Instalación de SSDT con Visual Studio 2019

Si [Visual Studio 2019](/visualstudio/install/install-visual-studio?preserve-view=true&view=vs-2019) ya está instalado, puede modificar la lista de cargas de trabajo para incluir SSDT. Si no tiene Visual Studio 2019 instalado, puede descargar e instalar [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/).

Para modificar las cargas de trabajo de Visual Studio instaladas para incluir SSDT, use el Instalador de Visual Studio.

1. Inicie el Instalador de Visual Studio. En el menú Inicio de Windows, puede buscar "instalador".

   ![Instalador de Visual Studio en el menú Inicio de Windows para 2019](../ssdt/media/visual-studio-installer.png)

2. En el instalador, seleccione la edición de Visual Studio a la que quiere agregar SSDT y, luego, elija **Modificar**.

3. Seleccione **SQL Server Data Tools** en **Almacenamiento y procesamiento de datos** en la lista de cargas de trabajo.

   ![Carga de trabajo de procesamiento y almacenamiento de datos 2019](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2019.png)

En el caso de proyectos de Analysis Services, Integration Services o Reporting Services, puede instalar las [extensiones](/visualstudio/ide/finding-and-using-visual-studio-extensions) adecuadas desde Visual Studio en **Extensiones** > **Administrar extensiones** o en el [marketplace](https://marketplace.visualstudio.com/search?term=services&target=VS&category=All%20categories&vsVersion=&sortBy=Relevance).

* [Analysis Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftAnalysisServicesModelingProjects)
* [Servicio de integración](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Reporting Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftReportProjectsforVisualStudio)

## <a name="ssdt-for-visual-studio-2017"></a>SSDT para Visual Studio 2017

### <a name="changes-in-ssdt-for-visual-studio-2017"></a>Cambios en SSDT para Visual Studio 2017

A partir de Visual Studio 2017, se ha integrado la funcionalidad de creación de proyectos de base de datos en la instalación de Visual Studio. No hay ninguna necesidad de instalar al instalador independiente de SSDT para la experiencia SSDT básica.

Ahora, para crear proyectos de Integration Services, Analysis Services o Reporting Services, necesita igualmente el instalador independiente de SSDT.

### <a name="install-ssdt-with-visual-studio-2017"></a>Instalar SSDT con Visual Studio 2017

Para instalar SSDT durante la [instalación de Visual Studio](/visualstudio/install/install-visual-studio), seleccione la carga de trabajo **Procesamiento y almacenamiento de datos** y, después, seleccione **SQL Server Data Tools**.

Si Visual Studio ya está instalado, use el Instalador de Visual Studio para modificar las cargas de trabajo instaladas para incluir SSDT.

1. Inicie el Instalador de Visual Studio. En el menú Inicio de Windows, puede buscar "instalador".

   ![Instalador de Visual Studio en el menú Inicio de Windows para 2017](../ssdt/media/visual-studio-installer.png)

2. En el instalador, seleccione la edición de Visual Studio a la que quiere agregar SSDT y, luego, elija **Modificar**.

3. Seleccione **SQL Server Data Tools** en **Almacenamiento y procesamiento de datos** en la lista de cargas de trabajo.

   ![Carga de trabajo de procesamiento y almacenamiento de datos 2017](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2017.png)

### <a name="install-analysis-services-integration-services-and-reporting-services-tools"></a>Instalar las herramientas de Analysis Services, Integration Services y Reporting Services

Para instalar la compatibilidad con proyectos de Integration Services, Analysis Services o Reporting Services, ejecute el [instalador independiente de SSDT](#ssdt-for-vs-2017-standalone-installer).

El instalador enumera las instancias disponibles de Visual Studio a las que se van a agregar las herramientas de SSDT. Si Visual Studio todavía no está instalado, al seleccionar **Instalar una nueva instancia de SQL Server Data Tools** se instala SSDT con una versión mínima de Visual Studio, pero para una mejor experiencia se recomienda usar SSDT con [la versión más reciente de Visual Studio](https://www.visualstudio.com/downloads).

![Selección de AS, IS, RS](../ssdt/media/download-sql-server-data-tools-ssdt/select-services.png)

## <a name="ssdt-for-vs-2017-standalone-installer"></a>SSDT para VS 2017 (instalador independiente)

:::image type="icon" source="media/download.png" border="false"::: **[Descarga de SSDT para Visual Studio 2017 (15.9.7)](https://go.microsoft.com/fwlink/?linkid=2151759)**

> [!IMPORTANT]
> * Antes de instalar SSDT para Visual Studio 2017 (15.9.7), desinstale las extensiones *Proyectos de Analysis Services* y *Proyectos de Reporting Services*, si están instaladas, y cierre todas las instancias de VS. 
> * Se quitó el componente de bandeja de entrada Origen de Power Query para SQL Server 2017. Ahora anunciamos el Origen de Power Query de SQL Server 2017 y 2019 como componente estándar, que se puede descargar [aquí](https://www.microsoft.com/download/details.aspx?id=100619).
> * Para diseñar paquetes mediante conectores de Oracle y Teradata y tener como destino una versión de SQL Server anterior a SQL 2019, además del [conector de Oracle para SQL 2019](https://www.microsoft.com/download/details.aspx?id=58228) y el [conector de Teradata para SQL 2019](https://www.microsoft.com/download/details.aspx?id=100599) de Microsoft, también debe instalar la versión correspondiente de estos conectores de Attunity.
>    * [Microsoft Connector versión 5.0 para Oracle y Teradata de Attunity dirigido a SQL Server 2017](https://www.microsoft.com/download/details.aspx?id=55179)
>    * [Microsoft Connector versión 4.0 para Oracle y Teradata de Attunity dirigido a SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=52950)
>    * [Microsoft Connector versión 3.0 para Oracle y Teradata de Attunity dirigido a SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=44582)
>    * [Microsoft Connector versión 2.0 para Oracle y Teradata de Attunity dirigido a SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29283)

### <a name="release-notes"></a>Notas de la versión

Para obtener una lista completa de los cambios, vea [Notas de la versión de SQL Server Data Tools (SSDT)](release-notes-ssdt.md).

### <a name="system-requirements"></a>Requisitos del sistema

SSDT para Visual Studio 2017 tiene los mismos [requisitos del sistema](/visualstudio/productinfo/vs2017-system-requirements-vs) que Visual Studio.

### <a name="available-languages---ssdt-for-vs-2017"></a>Idiomas disponibles: SSDT para VS 2017

Esta versión de **SSDT para VS 2017** puede instalarse en los idiomas siguientes:

* [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x804)
* [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x404)
* [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x409)
* [Francés](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40c)
* [Alemán](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x407)
* [Italiano](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x410)
* [Japonés](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x411)
* [Coreano](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x412)
* [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x416)
* [Ruso](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x419)
* [Español](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40a)

### <a name="considerations-and-limitations"></a>Consideraciones y limitaciones

* No es posible instalar sin conexión la versión de comunidad.

* Para actualizar SSDT, debe seguir la misma ruta de acceso que se usa para instalar SSDT. Por ejemplo, si ha agregado SSDT con las extensiones VSIX, debe actualizar a través de las extensiones VSIX. Si instaló SSDT a través de una instalación independiente, debe actualizarlo con ese método.

## <a name="offline-install"></a>Instalación sin conexión

Para instalar SSDT cuando no esté conectado a Internet, siga los pasos de esta sección. Para más información, consulte [Creación de una instalación de red de Visual Studio 2017](/visualstudio/install/create-a-network-installation-of-visual-studio).

En primer lugar, complete los siguientes pasos **en línea**:

1. [Descargue el instalador independiente de SSDT](#ssdt-for-vs-2017-standalone-installer).

2. [Descargue vs_sql.exe](https://aka.ms/vs/15/release/vs_sql.exe).

3. Mientras sigue en línea, ejecute uno de los siguientes comandos para descargar todos los archivos necesarios para la instalación sin conexión. Con la opción `--layout` como clave, descarga los archivos reales para la instalación sin conexión. Reemplace `<filepath>` por el diseño en trazado real para guardar los archivos.
   1. Para un idioma específico, pase la configuración regional: `vs_sql.exe --layout c:\<filepath> --lang en-us` (un solo lenguaje es ~ 1 GB).
   1. Para todos los idiomas, omita el argumento `--lang`: `vs_sql.exe --layout c:\<filepath>` (todos los idiomas son ~3,9 GB).

Después de completar los pasos anteriores, se pueden realizar los pasos siguientes mientras se está sin **conexión**:

1. Ejecute `vs_setup.exe --NoWeb` para instalar el shell de VS2017 y un proyecto de datos de SQL Server.

2. Desde la carpeta de diseños, ejecute `SSDT-Setup-ENU.exe /install` y seleccione SSIS/SSRS/SSAS.
   a. Para una instalación desatendida, ejecute `SSDT-Setup-ENU.exe /INSTALLALL[:vsinstances] /passive`.

Para ver las opciones disponibles, ejecute `SSDT-Setup-ENU.exe /help`.

> [!NOTE]
> Si usa una versión completa de Visual Studio 2017, cree una carpeta sin conexión exclusiva para SSDT y ejecute `SSDT-Setup-ENU.exe` desde la carpeta recién creada (no agregue SSDT a otro diseño sin conexión de Visual Studio 2017). Si agrega el diseño SSDT a un diseño sin conexión de Visual Studio existente, no se crearán los componentes de tiempo de ejecución (.exe) necesarios.

## <a name="supported-sql-versions"></a>Versiones de SQL admitidas

|Plantillas de proyecto|Plataformas SQL compatibles|
|-------------------|--------------------|
|Bases de datos relacionales| SQL Server 2005\* - SQL Server 2017<br> (use SSDT 17.x o SSDT para Visual Studio 2017 para conectarse a [SQL Server en Linux](../linux/sql-server-linux-overview.md))<br /><br />Azure SQL Database<br /><br />Azure Synapse Analytics (solo admite consultas, todavía no se admiten proyectos de base de datos)<br /><br /> \* Ya no se ofrece soporte técnico para SQL Server 2005.<br /><br /> Cambie a una versión de SQL oficialmente compatible.|
|Modelos de Analysis Services<br /><br />Informes de Reporting Services | SQL Server 2008 - SQL Server 2017|
|paquetes de Integration Services| SQL Server 2012 - SQL Server 2019 |

## <a name="dacfx"></a>DacFx

En SSDT para Visual Studio 2015 y 2017 se usa DacFx 17.4.1: [Descargar Data-Tier Application Framework (DacFx) 17.4.1](https://www.microsoft.com/download/details.aspx?id=56508).

## <a name="previous-versions"></a>Versiones anteriores

Para descargar e instalar SSDT para Visual Studio 2015 o una versión anterior de SSDT, vea [Versiones anteriores de SQL Server Data Tools (SSDT y SSDT-BI)](previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md).

## <a name="see-also"></a>Consulte también

* [Foro MSDN de SSDT](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt) 

* [Blog del equipo de SSDT](/archive/blogs/ssdt/)

* [Referencia de la API de DACFx](/previous-versions/sql/sql-server-2014/dn645454(v=sql.120))

* [Descarga de SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)

## <a name="next-steps"></a>Pasos siguientes

Después de instalar SSDT, siga estos tutoriales para aprender a crear bases de datos, paquetes, modelos de datos e informes mediante SSDT.

* [Desarrollo de bases de datos sin conexión orientado a proyectos](project-oriented-offline-database-development.md)

* [Tutorial de SSIS: Creación de un paquete ETL sencillo](../integration-services/ssis-how-to-create-an-etl-package.md)

* [Tutoriales de Analysis Services (SSAS)](/analysis-services/analysis-services-tutorials-ssas)

* [Crear un informe de tabla básico (Tutorial de SSRS)](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
