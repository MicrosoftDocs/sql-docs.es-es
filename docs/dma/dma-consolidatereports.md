---
title: Evalúe una empresa y consolide informes de evaluación con Data Migration Assistant
description: Aprenda a usar DMA para evaluar una empresa y consolidar los informes de evaluación antes de actualizar SQL Server o migrar a Azure SQL Database.
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: c2804bbc185a3aa17d7f50d85b9ef6c2cc84d002
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044652"
---
# <a name="assess-an-enterprise-and-consolidate-assessment-reports-with-dma"></a>Evaluación de una empresa y consolidación de informes de evaluación con DMA.

Las siguientes instrucciones paso a paso le ayudarán a usar la Data Migration Assistant para realizar una evaluación de escalado correcta para actualizar SQL Server locales o SQL Server que se ejecutan en máquinas virtuales de Azure o para migrar a Azure SQL Database.

## <a name="prerequisites"></a>Prerrequisitos

- Designe un equipo de herramientas en la red desde el que se iniciará DMA. Asegúrese de que este equipo tiene conectividad con los destinos de SQL Server.
- Descargue e instale:
  - [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v 3.6 o posterior.
  - [PowerShell](https://aka.ms/wmf5download) v 5.0 o versiones posteriores.
  - [.NET Framework](https://www.microsoft.com/download/details.aspx?id=30653) v 4.5 o superior.
  - [SSMS](../ssms/download-sql-server-management-studio-ssms.md) 17,0 o superior.
  - [Escritorio Power BI](/power-bi/fundamentals/desktop-get-the-desktop).
  - [Módulos de Azure PowerShell](/powershell/azure/install-az-ps)
- Descargar y extraer:
  - La [plantilla de Power BI de informes DMA](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/4/PowerBI-Reports.zip).
  - El [script LoadWarehouse](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/3/LoadWarehouse1.zip).

## <a name="loading-the-powershell-modules"></a>Carga de los módulos de PowerShell

Guardar los módulos de PowerShell en el directorio de módulos de PowerShell le permite llamar a los módulos sin necesidad de cargarlos explícitamente antes de usarlos.

Para cargar los módulos, realice los pasos siguientes:

1. Vaya a C:\Archivos de Files\WindowsPowerShell\Modules y, a continuación, cree una carpeta denominada **DataMigrationAssistant**.
2. Abra los [módulos de PowerShell](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/1/PowerShell-Modules2.zip)y, a continuación, guárdelos en la carpeta que creó.

      ![Módulos de PowerShell](../dma/media//dma-consolidatereports/dma-powershell-modules.png)

    Cada carpeta contiene el archivo psm1 asociado, tal y como se muestra en el siguiente gráfico:

   ![Archivos psm1 de módulos de PowerShell](../dma/media//dma-consolidatereports/dma-powershell-modules-psm1-files.png)

   > [!NOTE]
   > La carpeta y el archivo psm1 que contiene deben tener el mismo nombre.

   > [!IMPORTANT]
   > Es posible que tenga que desbloquear los archivos de PowerShell después de guardarlos en el directorio de WindowsPowerShell para asegurarse de que los módulos se cargan correctamente. Para desbloquear un archivo de PowerShell, haga clic con el botón derecho en el archivo, seleccione **propiedades**, seleccione el cuadro de texto **desbloquear** y, después, seleccione **Aceptar**.

   ![propiedades del archivo psm1](../dma/media//dma-consolidatereports/dma-psm1-file-properties.png)

    PowerShell debe cargar ahora estos módulos automáticamente cuando se inicia una nueva sesión de PowerShell.

## <a name="create-an-inventory-of-sql-servers"></a><a name="create-inventory"></a> Crear un inventario de servidores SQL Server

Antes de ejecutar el script de PowerShell para evaluar los servidores SQL Server, debe crear un inventario de los servidores SQL Server que desea evaluar.

Este inventario puede estar en una de estas dos formas:

- Archivo CSV de Excel
- Tabla de SQL Server

### <a name="if-using-a-csv-file"></a>Si se usa un archivo CSV

> [!IMPORTANT]
> Asegúrese de que el archivo de inventario se guarda como un archivo separado por comas (CSV).
>
> En el caso de las instancias predeterminadas, establezca el nombre de instancia en MSSQLServer.

Al usar un archivo CSV para importar los datos, asegúrese de que hay solo dos columnas de **nombre de instancia** de datos y **nombre de base** de datos, y que las columnas no tienen filas de encabezado.

 ![contenido del archivo CSV](../dma/media//dma-consolidatereports/dma-csv-file-contents.png)

### <a name="if-using-a-sql-server-table"></a>Si se usa una tabla de SQL Server

> [!IMPORTANT]
> En el caso de las instancias predeterminadas, establezca el nombre de instancia en MSSQLServer.

Cree una base de datos denominada **EstateInventory** y una tabla denominada **DatabaseInventory**. La tabla que contiene estos datos de inventario puede tener cualquier número de columnas, siempre que existan las cuatro columnas siguientes:

- nombreDeServidor
- InstanceName
- DatabaseName
- AssessmentFlag

![Captura de pantalla de un SQL Server contenido de la tabla si utiliza una tabla de SQL Server.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-database-inventory.png)

Si esta base de datos no se encuentra en el equipo de herramientas, asegúrese de que el equipo de herramientas tenga conectividad de red a esta instancia de SQL Server.

La ventaja de usar una tabla de SQL Server a través de un archivo CSV es que puede usar la columna de marca de evaluación para controlar la instancia o base de datos que se recoge para la evaluación, lo que facilita la separación de las evaluaciones en fragmentos más pequeños.  Después, puede abarcar varias evaluaciones (consulte la sección sobre la ejecución de una evaluación más adelante en este artículo), que es más fácil que mantener varios archivos CSV.

Tenga en cuenta que, en función del número de objetos y su complejidad, una evaluación puede tardar un tiempo excepcionalmente largo (horas +), por lo que es prudente separar la evaluación en fragmentos manejables.

### <a name="if-using-an-instance-inventory"></a>Si se usa un inventario de instancia

Cree una base de datos denominada **EstateInventory** y una tabla denominada **InstanceInventory**. La tabla que contiene estos datos de inventario puede tener cualquier número de columnas, siempre que existan las cuatro columnas siguientes:

- nombreDeServidor
- InstanceName
- Port
- AssessmentFlag

![Captura de pantalla de un SQL Server contenido de la tabla si se usa un inventario de instancia.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-instance-inventory.png)

## <a name="running-a-scaled-assessment"></a>Ejecución de una evaluación escalada

Después de cargar los módulos de PowerShell en el directorio modules y crear un inventario, debe ejecutar una evaluación escalada abriendo PowerShell y ejecutando la función dmaDataCollector.

  ![lista de funciones de dmaDataCollector](../dma/media//dma-consolidatereports/dma-dmaDataCollector-function-listing.png)

Los parámetros asociados a la función dmaDataCollector se describen en la tabla siguiente.

|Parámetro  |Descripción |
|---------|---------|
|**getServerListFrom** | El inventario. Los valores posibles son **SQLServer** y **CSV**.<br/>Para obtener más información, vea [crear un inventario de servidores SQL Server](#create-inventory). |
|**csvPath** | La ruta de acceso al archivo de inventario de CSV.  Solo se usa cuando **getServerListFrom** está establecido en  **CSV**. |
|**serverName** | El nombre de la instancia de SQL Server del inventario cuando se usa **SQLServer** en el parámetro **getServerListFrom** . |
|**databaseName** | La base de datos que hospeda la tabla de inventario. |
|**useInstancesOnly** | Marca de bits para especificar si se va a usar una lista de instancias para la evaluación o no.  Si se establece en 0, se usará la tabla DatabaseInventory para compilar la lista de destino de la evaluación. |
|**AssessmentName** | Nombre de la evaluación de DMA. |
|**TargetPlatform** | El tipo de destino de la evaluación que desea realizar.  Los valores posibles son **AzureSQLDatabase**, **ManagedSqlServer**, **SQLServer2012**, **SQLServer2014**, **SQLServer2016**, **SQLServerLinux2017**, **SQLServerWindows2017**,  **SqlServerWindows2019** y **SqlServerLinux2019**.  |
|**AuthenticationMethod** | El método de autenticación para conectarse a los destinos SQL Server que desea evaluar. Los valores posibles son **SQLAuth** y **WindowsAuth**. |
|**OutputLocation** | Directorio en el que se va a almacenar el archivo de salida de evaluación de JSON. Dependiendo del número de bases de datos que se estén evaluando y del número de objetos dentro de las bases de datos, las evaluaciones pueden tardar un tiempo excepcionalmente largo. El archivo se escribirá una vez completadas todas las evaluaciones. |

Si hay un error inesperado, se terminará la ventana de comandos que se inicia con este proceso.  Revise el registro de errores para determinar por qué se produjo el error.

  ![Ubicación de registro de errores](../dma/media//dma-consolidatereports/dma-error-log-file-location.png)

## <a name="consuming-the-assessment-json-file"></a>Consumo del archivo JSON de evaluación

Una vez finalizada la evaluación, ya está listo para importar los datos en SQL Server para su análisis. Para consumir el archivo JSON de evaluación, abra PowerShell y ejecute la función dmaProcessor.

  ![lista de funciones de dmaProcessor](../dma/media//dma-consolidatereports/dma-dmaProcessor-function-listing.png)

Los parámetros asociados a la función dmaProcessor se describen en la tabla siguiente.

|Parámetro  |Descripción |
|---------|---------|
|**proceso** | Ubicación en la que se procesará el archivo JSON. Los valores posibles son **SQLServer** y **AzureSQLDatabase**. |
|**serverName** | La instancia de SQL Server a la que se procesarán los datos.  Si especifica **AzureSQLDatabase** para el parámetro **processto** , incluya solo el nombre del SQL Server (no incluya. Database.Windows.net). Se le pedirán dos inicios de sesión cuando se destinen Azure SQL Database; el primero es su credencial de inquilino de Azure, mientras que el segundo es el inicio de sesión de administrador para Azure SQL Server. |
|**CreateDMAReporting** | La base de datos provisional que se va a crear para procesar el archivo JSON.  Si la base de datos especificada ya existe y se establece este parámetro en uno, los objetos no se crean.  Este parámetro es útil para volver a crear un solo objeto que se ha quitado. |
|**CreateDataWarehouse** | Crea el almacenamiento de datos que va a usar el informe de Power BI. |
|**databaseName** | El nombre de la base de datos DMAReporting. |
|**warehouseName** | El nombre de la base de datos de almacenamiento de datos. |
|**jsonDirectory** | Directorio que contiene el archivo de evaluación JSON.  Si hay varios archivos JSON en el directorio, se procesan uno a uno. |

La función dmaProcessor solo debe tardar unos segundos en procesar un único archivo.

## <a name="loading-the-data-warehouse"></a>Cargar el almacenamiento de datos

Después de que dmaProcessor haya finalizado el procesamiento de los archivos de evaluación, los datos se cargarán en la base de datos DMAReporting de la tabla ReportData. Llegados a este punto, debe cargar el almacenamiento de datos.

1. Use el script LoadWarehouse para rellenar los valores que faltan en las dimensiones.

    El script toma los datos de la tabla ReportData en la base de datos DMAReporting y los carga en el almacén.  Si se producen errores durante este proceso de carga, es probable que se deba a que faltan entradas en las tablas de dimensiones.

2. Cargue el almacenamiento de datos.

  ![Contenido de LoadWarehouse cargado](../dma/media//dma-consolidatereports/dma-load-warehouse-loaded.png)

## <a name="set-your-database-owners"></a>Establecer los propietarios de la base de datos

Aunque no es obligatorio, para obtener el máximo valor de los informes, se recomienda establecer los propietarios de la base de datos en la dimensión **dimDBOwner** y, a continuación, actualizar **DBOwnerKey** en la tabla **FactAssessment** .  Seguir este proceso permitirá segmentar y filtrar el informe de Power BI basado en propietarios de base de datos específicos.

También puede usar el script LoadWarehouse para proporcionar las instrucciones de TSQL básicas para establecer los propietarios de la base de datos.

  ![Propietarios de configuración de LoadWarehouse](../dma/media//dma-consolidatereports/dma-load-warehouse-set-owners.png)

## <a name="dma-reports"></a>Informes DMA

1. Abra la plantilla Power BI informes DMA en el Power BI Desktop.
2. Escriba los detalles del servidor que apuntan a la base de datos **DMAWarehouse** y, a continuación, seleccione **cargar**.

   ![Informes de DMA Power BI plantilla cargada](../dma/media//dma-consolidatereports/dma-reports-powerbi-template-loaded.png)

   Una vez que el informe haya actualizado los datos de la base de datos **DMAWarehouse** , se le presentará un informe similar al siguiente.

   ![Vista de informe DMAWarehouse](../dma/media//dma-consolidatereports/dma-DMAWarehouse-report1.png)

   > [!TIP]
   > Si no ve los datos que esperaba, intente cambiar el marcador activo.  Para obtener más información, vea los detalles en la sección siguiente.

## <a name="working-with-dma-reports"></a>Trabajar con informes DMA

Para trabajar con informes DMA, use marcadores y segmentaciones para filtrar por:

- Tipos de evaluación (Azure SQL Database, Azure SQL Instancia administrada, SQL Server) 
- Nombre de instancia
- Nombre de la base de datos
- Nombre de equipo

Para tener acceso a la hoja marcadores y filtros, seleccione el marcador filtros en la Página principal del informe:

![Filtros y marcadores de informe DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks-filters.png)

Al seleccionar el marcador filtros, se habilita la siguiente hoja:

![Hoja vistas de informes DMA](../dma/media//dma-consolidatereports/dma-report-views-blade.png)

Puede usar marcadores para cambiar el contexto de informes entre:

- Azure SQL Database evaluaciones en la nube
- Evaluaciones de la nube de Azure SQL Instancia administrada
- Evaluaciones locales

![Marcadores de vistas de informes DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks1.png)

Para ocultar la hoja filtros, haga clic en el botón atrás:

![Botón atrás de vistas de informe DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks-back.png)

Hay un mensaje en la parte inferior izquierda de la página del informe para mostrar si un filtro se aplica actualmente a cualquiera de los siguientes elementos:

- FactAssessment: nombreDeInstancia
- FactAssessment – DatabaseName
- dimDBOwner-DBOwner

![Solicitud de filtro aplicado](../dma/media//dma-consolidatereports/dma-filter-applied-prompt.png)

> [!NOTE]
> Si solo realiza una evaluación de Azure SQL Database, solo se rellenan los informes en la nube. Por el contrario, si solo realiza una evaluación local, solo se rellenan los informes locales. Sin embargo, si realiza una evaluación de Azure y de forma local y, después, carga ambas evaluaciones en el almacén, puede cambiar entre informes en la nube e informes locales haciendo clic en el icono asociado.

## <a name="reports-visuals"></a>Informes visuales

Los detalles que se muestran en el Power BI informes se muestran en las secciones siguientes.

### <a name="readiness-"></a>Preparación

  ![Porcentaje de preparación de DMA](../dma/media//dma-consolidatereports/dma-readiness-percentage.png)

Este visual se actualiza en función del contexto de selección (todo, instancia, base de datos [múltiplos de]).

### <a name="readiness-count"></a>Recuento de disponibilidad

  ![Recuento de preparación de DMA](../dma/media//dma-consolidatereports/dma-readiness-count.png)

Este visual muestra el recuento de bases de datos que están listas para migrar el recuento de bases de datos que aún no están listas para migrar.

### <a name="readiness-bucket"></a>Depósito de preparación

  ![Depósito de preparación de DMA](../dma/media//dma-consolidatereports/dma-readiness-bucket.png)

Este visual muestra un desglose de las bases de datos por los siguientes cubos de preparación:

- 100% LISTO
- 75-99% LISTO
- 50-75% LISTO
- NO ESTÁ LISTO

### <a name="issues-word-cloud"></a>Problemas de la nube de Word

  ![Problemas de DMA WordCloud](../dma/media//dma-consolidatereports/dma-issues-word-cloud.png)

Este visual muestra los problemas que se producen actualmente en el contexto de selección (todo, instancia, base de datos [múltiplos de]). Cuanto mayor sea la palabra en pantalla, mayor será el número de problemas de esa categoría. Al mantener el puntero del mouse sobre una palabra se muestra el número de problemas que se producen en esa categoría.

### <a name="database-readiness"></a>Preparación de la base de datos

  ![Informe de preparación de la base de datos DMA](../dma/media//dma-consolidatereports/dma-database-readiness-report.png)

Esta sección es la parte principal del informe, que muestra la preparación de una instancia de Database. Este informe tiene una jerarquía de obtención de detalles de:

- InstanceDatabase
- ChangeCategory
- Title
- ObjectType
- ImpactedObjectName

 ![Obtención de detalles del informe de preparación de base de datos DMA](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown.png)

Este informe también sirve como punto de filtro para crear el informe del plan de corrección.

Para profundizar en el informe del plan de corrección, haga clic con el botón derecho en un punto de datos de este gráfico, elija **obtención de detalles** y, a continuación, seleccione **planes de corrección**.

Esta tarea filtra el informe del plan de corrección hasta el nivel de la jerarquía actual en función del punto en el que se selecciona la opción de obtención de detalles.

  ![Detalle del informe de preparación de base de datos DMA filtrado](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown-filtered.png)

  ![Informe del plan de corrección de DMA](../dma/media//dma-consolidatereports/dma-remediation-plan-report.png)

También puede usar el informe del plan de corrección por su cuenta para crear un plan de corrección personalizado mediante los filtros de la hoja **filtros de visualización** .

  ![Opciones de filtro de informe del plan de corrección DMA](../dma/media//dma-consolidatereports/dma-remediation-plan-report-filter-options.png)

### <a name="script-disclaimer"></a>Renuncia de script

*Los scripts de ejemplo que se proporcionan en este artículo no se admiten en ningún servicio o programa de soporte técnico estándar de Microsoft. Todos los scripts se proporcionan tal cual sin garantía de ningún tipo. Microsoft renuncia aún más a todas las garantías implícitas, incluidas, sin limitación, las garantías implícitas de comerciabilidad o de idoneidad para un propósito específico. El riesgo completo derivado del uso o el rendimiento de los scripts y la documentación de ejemplo se mantiene con usted. En ningún caso, Microsoft, sus autores o cualquier otra persona implicada en la creación, la producción o entrega de los scripts puede ser responsable de cualquier daño (incluido, sin limitación, los daños en la pérdida de beneficios empresariales, la interrupción del negocio, la pérdida de información empresarial u otra pérdida pecuniaria) que se derive del uso o de la imposibilidad de usar los scripts o la documentación de ejemplo, incluso si Microsoft se ha informado de la posibilidad de dichos daños.  Permiso de búsqueda antes de republicar estos scripts en otros sitios, repositorios y blogs.*