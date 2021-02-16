---
title: Implementar y ejecutar paquetes SSIS en Azure | Microsoft Docs
description: Obtenga información sobre cómo se pueden mover los proyectos, paquetes y cargas de trabajo de SQL Server Integration Services (SSIS) a la nube de Microsoft Azure.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 12b4e6c71698845314844e4cf0bcd5b371209641
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352007"
---
# <a name="lift-and-shift-sql-server-integration-services-workloads-to-the-cloud"></a>Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Ahora los proyectos, paquetes y cargas de trabajo de SQL Server Integration Services (SSIS) se pueden mover a la nube de Azure. Implemente, ejecute y administre proyectos y paquetes de SSIS del catálogo de SSIS (SSISDB) en Azure SQL Database o Azure SQL Managed Instance con herramientas conocidas, como SQL Server Management Studio (SSMS).

## <a name="benefits"></a>Ventajas
El movimiento de las cargas de trabajo de SSIS local a Azure presenta las siguientes ventajas potenciales:
-   **Reduce los costos operativos** y disminuye la carga de administrar la infraestructura que tiene cuando ejecuta SSIS localmente en máquinas virtuales de Azure.
-   **Aumenta la alta disponibilidad** con la capacidad de especificar varios nodos por clúster, así como las características de alta disponibilidad de Azure y de Azure SQL Database.
-   **Aumenta la escalabilidad** con la capacidad de especificar varios núcleos por nodo (escalabilidad vertical) y varios nodos por clúster (escalabilidad horizontal).

## <a name="architecture-of-ssis-on-azure"></a>Arquitectura de SSIS en Azure
En la tabla siguiente se resaltan las diferencias entre SSIS local y SSIS en Azure.

La diferencia más importante es la separación del almacenamiento y el tiempo de ejecución. Azure Data Factory hospeda el motor en tiempo de ejecución para paquetes SSIS en Azure. El motor en tiempo de ejecución se denomina Azure SSIS Integration Runtime (Azure SSIS IR). Para más información, vea [Azure SSIS Integration Runtime](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime).

| Location | Storage | Tiempo de ejecución | Escalabilidad |
|---|---|---|---|
| En el entorno local | SQL Server | Tiempo de ejecución de SSIS hospedado por SQL Server | Escalabilidad horizontal de SSIS (en SQL Server 2017 y versiones posteriores)<br/><br/>Soluciones personalizadas (en versiones anteriores de SQL Server) |
| En Azure | Instancia administrada de SQL o SQL Database:SQL Database o SQL Managed Instance | Azure SSIS Integration Runtime, un componente de Azure Data Factory | Opciones de escalado de Azure SSIS Integration Runtime |
| | | | |

## <a name="provision-ssis-on-azure"></a>Aprovisionar SSIS en Azure

**Aprovisionamiento**. Para poder implementar y ejecutar paquetes SSIS en Azure, debe aprovisionar el catálogo de SSIS (SSISDB) y Azure SSIS Integration Runtime.

-   Para aprovisionar SSIS en Azure en Azure Portal, siga los pasos de aprovisionamiento en este artículo: [Aprovisionamiento de Azure-SSIS Integration Runtime en Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure). 

-   Para aprovisionar SSIS en Azure con PowerShell, siga los pasos de aprovisionamiento en este artículo: [Aprovisionamiento de Azure-SSIS Integration Runtime en Azure Data Factory con PowerShell](/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

Solo tiene que aprovisionar Azure SSIS IR una vez. Después de eso, puede usar herramientas conocidas, como SQL Server Data Tools (SSDT) y SQL Server Management Studio (SSMS) para implementar, configurar, ejecutar, supervisar, programar y administrar paquetes.

> [!NOTE]
> Azure SSIS Integration Runtime todavía no está disponible en todas las regiones de Azure. Para obtener información acerca de las regiones admitidas, consulte [Productos disponibles por región - Microsoft Azure](https://azure.microsoft.com/regions/services/).

**Escalabilidad vertical y horizontal**. Al aprovisionar Azure SSIS IR, puede realizar la escalabilidad vertical y horizontal especificando los valores de las siguientes opciones:
-   El tamaño del nodo (incluido el número de núcleos) y el número de nodos del clúster.
-   La instancia existente de Azure SQL Database para hospedar la base de datos del catálogo de SSIS (SSISDB), y el nivel de servicio de la base de datos.
-   Las ejecuciones en paralelo máximas por nodo.

**Mejorar el rendimiento**. Para obtener más información, vea [Configuración de Azure SSIS Integration Runtime para conseguir un alto rendimiento](/azure/data-factory/configure-azure-ssis-integration-runtime-performance).

**Reducir los costos**. Para reducir los costos, ejecute Azure SSIS IR solo cuando lo necesite. Para más información, consulte [Inicio y detención del entorno de ejecución para la integración de SSIS en Azure según una programación](/azure/data-factory/how-to-schedule-azure-ssis-integration-runtime).

## <a name="design-packages"></a>Diseñar paquetes

Debe continuar con el **diseño y la compilación de paquetes** localmente en SSDT, o bien en Visual Studio con SSDT instalado.

### <a name="connect-to-data-sources"></a>Conectarse a orígenes de datos

Para conectarse a orígenes de datos locales desde la nube con la **autenticación de Windows**, consulte [Conexión a orígenes de datos y a recursos compartidos de archivos con la autenticación de Windows de paquetes SSIS en Azure](/azure/data-factory/ssis-azure-connect-with-windows-auth).

Para conectarse a archivos y recursos compartidos de archivos, consulte [Abrir y guardar archivos de forma local y en Azure con paquetes SSIS implementados en Azure](/azure/data-factory/ssis-azure-files-file-shares).

### <a name="available-ssis-components"></a>Componentes de SSIS disponibles

Al aprovisionar una instancia de SQL Database para hospedar SSISDB, también se instalan el paquete de características de Azure para SSIS y el acceso redistribuible. Estos componentes proporcionan conectividad a varios orígenes de datos de **Azure** y a archivos de **Excel y Access**, así como a los orígenes de datos compatibles con los componentes integrados.

También se pueden instalar componentes adicionales: por ejemplo, se puede instalar un controlador que no esté instalado de forma predeterminada. Para más información, consulte [Custom setup for the Azure-SSIS integration runtime](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) (Configuración personalizada de Azure SSIS Integration Runtime).

Si tiene una licencia de Enterprise Edition, hay componentes adicionales disponibles. Para más información, consulte [Aprovisionamiento de Enterprise Edition para una instancia de Integration Runtime para la integración de SSIS en Azure](/azure/data-factory/how-to-configure-azure-ssis-ir-enterprise-edition).

Si es un ISV, puede actualizar la instalación de los componentes con licencia para que estén disponibles en Azure. Para más información, consulte [Instalación de componentes personalizados de pago o con licencia para Azure SSIS Integration Runtime](/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components).

### <a name="transaction-support"></a>Compatibilidad con transacciones

Con SQL Server local y en máquinas virtuales de Azure, puede usar las transacciones del Coordinador de transacciones distribuidas de Microsoft (MSDTC). Para configurar MSDTC en cada nodo de Azure SSIS IR, use la capacidad de configuración personalizada. Para más información, consulte [Custom setup for the Azure-SSIS integration runtime](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) (Instalación personalizada de la instancia de Integration Runtime para la integración de SSIS en Azure).

Con Azure SQL Database solo puede usar transacciones elásticas. Para más información, consulte [Transacciones distribuidas en bases de datos en la nube](/azure/sql-database/sql-database-elastic-transactions-overview).

## <a name="deploy-and-run-packages"></a>Implementar y ejecutar paquetes

Para empezar, consulte [Tutorial: Implementación y ejecución de un paquete de SQL Server Integration Services (SSIS) en Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="prerequisites"></a>Prerrequisitos

Para implementar paquetes de SSIS en Azure, debe disponer de una de las siguientes versiones de SQL Server Data Tools (SSDT):
-   Para Visual Studio 2017, versión 15.3 o una versión posterior.
-   Para Visual Studio 2015, versión 17.2 o una versión posterior.

### <a name="connect-to-ssisdb"></a>Conectarse a SSISDB

El **nombre de SQL Database** que hospeda SSISDB se convierte en la primera parte del nombre de cuatro partes que se usará al implementar y ejecutar paquetes de SSDT y SSMS con el siguiente formato: `<sql_database_name>.database.windows.net`. Para más información sobre cómo conectarse a la base de datos del catálogo de SSIS en Azure, consulte [Conectarse al catálogo de SSIS (SSISDB) en Azure](ssis-azure-connect-to-catalog-database.md).

### <a name="deploy-projects-and-packages"></a>Implementación de proyectos y paquetes

Tiene que usar el **modelo de implementación de proyectos**, y no el modelo de implementación de paquetes, al implementar proyectos en SSISDB en Azure.

Para implementar proyectos en Azure, puede usar una de las distintas herramientas conocidas y opciones de scripting:
-   SQL Server Management Studio (SSMS)
-   Transact-SQL (de SSMS, Visual Studio Code u otra herramienta)
-   Una herramienta de línea de comandos
-   PowerShell o C#, y el modelo de objetos de administración de SSIS

El proceso de implementación valida los paquetes para garantizar que se puedan ejecutar en Azure-SSIS Integration Runtime. Para más información, consulte [Validación de paquetes de SQL Server Integration Services (SSIS) implementados en Azure](ssis-azure-validate-packages.md).

Para obtener un ejemplo de implementación en el que se usa SSMS y el Asistente para implementación de Integration Services, consulte [Tutorial: Implementación y ejecución de un paquete de SQL Server Integration Services (SSIS) en Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="version-support"></a>Compatibilidad con versiones

Puede implementar en Azure un paquete creado con cualquier versión de SSIS. Al implementar un paquete en Azure, si no hay ningún error de validación, el paquete se actualiza automáticamente al formato de paquete más reciente. Dicho de otra manera, siempre se actualiza a la versión más reciente de SSIS.

### <a name="run-packages"></a>Ejecutar paquetes

Para ejecutar paquetes de SSIS implementados en Azure, puede usar varios métodos. Para más información, consulte [Ejecutar paquetes de SQL Server Integration Services (SSIS) implementados en Azure](ssis-azure-run-packages.md).

### <a name="run-packages-in-an-azure-data-factory-pipeline"></a>Ejecutar paquetes en una canalización de Azure Data Factory

Para ejecutar un paquete de SSIS en una canalización de Azure Data Factory, use la actividad Ejecutar paquete de SSIS. Para obtener más información, vea [Ejecutar un paquete SSIS con la actividad Ejecutar paquete de SSIS en Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Al ejecutar un paquete en una canalización de Data Factory con la actividad Ejecutar paquete de SSIS, puede pasar valores para el paquete en tiempo de ejecución. Para pasar uno o más valores de tiempo de ejecución, cree entornos de ejecución de SSIS en SSISDB con SQL Server Management Studio (SSMS). En cada entorno, cree variables y asigne valores que se correspondan a los parámetros para los proyectos o paquetes. Configure los paquetes SSIS en SSMS para asociar esas variables de entorno con los parámetros del proyecto o paquete. Al ejecutar los paquetes en la canalización, cambie entre los entornos especificando otras rutas de acceso de entorno en la pestaña Configuración de la interfaz de usuario Ejecutar una actividad de paquete SSIS. Para más información sobre los entornos SSIS, consulte [Crear y asignar un entorno de servidor](../packages/deploy-integration-services-ssis-projects-and-packages.md#create-and-map-a-server-environment).

## <a name="monitor-packages"></a>Supervisar paquetes

Para supervisar paquetes en ejecución, utilice las siguientes opciones de generación de informes de SSMS.
-   Haga clic con el botón derecho en **SSISDB** y, a continuación, seleccione **Operaciones activas** para abrir el cuadro de diálogo **Operaciones activas**.
-   Seleccione un paquete en el Explorador de objetos, haga clic con el botón derecho y seleccione **Informes**. A continuación, elija **Informes estándar** y **Todas las ejecuciones**.

Para supervisar Azure SSIS Integration Runtime, vea [Monitor the Azure-SSIS integration runtime](/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime) (Supervisión de Azure SSIS Integration Runtime).

## <a name="schedule-packages"></a>Programar paquetes
Para programar la ejecución de paquetes implementados en Azure, puede usar una variedad de herramientas. Para más información, consulte [Programar la ejecución de paquetes de SQL Server Integration Services (SSIS) implementados en Azure](ssis-azure-schedule-packages.md).

## <a name="next-steps"></a>Pasos siguientes
Para comenzar con las cargas de trabajo de SSIS en Azure, consulte los artículos siguientes:
-   [Tutorial: Implementación y ejecución de un paquete de SQL Server Integration Services (SSIS) en Azure](ssis-azure-deploy-run-monitor-tutorial.md)
-   [Aprovisionamiento de Integration Runtime de Azure SSIS en Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure)