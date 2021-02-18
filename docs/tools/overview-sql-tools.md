---
title: Información general de las herramientas de SQL
description: Las herramientas de administración y consulta SQL para SQL Server, Azure SQL (base de datos de Azure SQL, instancia administrada de Azure SQL, máquinas virtuales de SQL) y Azure Synapse Analytics.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 3595e84b4da525894c353c8b5d702b69b19f354d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338668"
---
# <a name="sql-tools-overview"></a>Información general de las herramientas de SQL

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Para administrar la base de datos, necesita una herramienta. Tanto si las bases de datos se ejecutan en la nube, en Windows, en macOS o en [Linux](../linux/sql-server-linux-overview.md), no es necesario que la herramienta se ejecute en la misma plataforma que la base de datos.

Puede ver los vínculos a las distintas herramientas de SQL en las tablas siguientes.

> [!Note]
> Para descargar SQL Server, consulte [Instalación de SQL Server](../database-engine/install-windows/install-sql-server.md).

## <a name="recommended-tools"></a>Herramientas recomendadas

Las herramientas siguientes proporcionan una interfaz gráfica de usuario (GUI).

| Herramienta | Descripción | Sistema operativo |
|:--|:--|:--|
| [ **![Imagen de ADS](../tools/media/overview-sql-tools/azure-data-studio.svg)</br></br>Azure Data Studio**](../azure-data-studio/download.md) | Un editor ligero que puede ejecutar consultas SQL a petición, ver y guardar resultados como texto, JSON o Excel. Edite los datos, organice sus conexiones de bases de datos favoritas y examine los objetos de base de datos en una experiencia de exploración de objetos conocida. | **Windows</br>macOS</br>Linux** |
| [ **![Imagen de SSMS](../tools/media/overview-sql-tools/ssms.svg)</br></br>SQL Server Management Studio (SSMS)**](../ssms/download-sql-server-management-studio-ssms.md) | Administre una instancia de SQL Server o una base de datos con compatibilidad completa con GUI. Acceda, configure, administre y desarrolle todos los componentes de SQL Server, Azure SQL Database y Azure Synapse Analytics. Proporciona una única utilidad integral que combina un amplio grupo de herramientas gráficas con una serie de editores de script enriquecidos que permiten a desarrolladores y administradores de bases de datos de todos los niveles acceder a SQL. | **Windows** |
| [ **![Imagen de SSDT](../tools/media/overview-sql-tools/ssdt.svg)</br>SQL Server Data Tools (SSDT)**](../ssdt/download-sql-server-data-tools-ssdt.md) | Una herramienta de desarrollo moderna para crear bases de datos relacionales de SQL Server, bases de datos de Azure SQL, modelos de datos de Analysis Services (AS), paquetes de Integration Services (IS) e informes de Reporting Services (RS). Gracias a SSDT, puede diseñar e implementar cualquier tipo de contenido de SQL Server con la misma facilidad con la que desarrollaría una aplicación en **[Visual Studio](https://visualstudio.microsoft.com/downloads/)** . | **Windows** |
| [ **![Imagen de VS Code](../tools/media/overview-sql-tools/visual-studio-code.svg)</br></br>Visual Studio Code**](https://code.visualstudio.com/) | La **[extensión mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)** para Visual Studio Code es la extensión oficial de SQL Server que admite conexiones a SQL Server y la experiencia de edición enriquecida para T-SQL en Visual Studio Code. Escriba scripts T-SQL en un editor ligero. | **Windows</br>macOS</br>Linux** |

## <a name="command-line-tools"></a>Herramientas de línea de comandos

Las herramientas siguientes son las herramientas principales de la línea de comandos.

| Herramienta | Descripción | Sistema operativo |
|:--|:--|:--|
|[**bcp**](bcp-utility.md)|La utilidad de **p** rograma **d** e **p** copia masiva (**bcp**) hace copias masivas de los datos entre una instancia de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y un archivo de datos en un formato especificado por el usuario.| **Windows</br>macOS</br>Linux** |
|[**mssql-cli (preview)**](mssql-cli.md)|**mssql-cli** es una herramienta de línea de comandos interactiva para consultar SQL Server. Además, consulte SQL Server con una herramienta de línea de comandos que incluye IntelliSense, el resaltado de la sintaxis y mucho más. | **Windows</br>macOS</br>Linux** |
|[**mssql-conf**](../linux/sql-server-linux-configure-mssql-conf.md) | **mssql-conf** configura SQL Server que se ejecutan en Linux. | **Linux** |
|[**mssql-scripter (versión preliminar)**](https://github.com/Microsoft/mssql-scripter) | **mssql-scripter** es una experiencia de línea de comandos multiplataforma para la generación de scripts de bases de datos de SQL Server. | **Windows</br>macOS</br>Linux** |
| [**sqlcmd**](sqlcmd-utility.md) |La utilidad **sqlcmd** le permite insertar instrucciones Transact-SQL, procedimientos del sistema y archivos de script en el símbolo del sistema. | **Windows</br>macOS</br>Linux** |
| [**sqlpackage**](sqlpackage/sqlpackage.md) |**sqlpackage** es una utilidad de línea de comandos que automatiza las tareas de desarrollo de base de datos siguientes. |**Windows</br>macOS</br>Linux** |
|[**SQL Server PowerShell**](../powershell/sql-server-powershell.md)| **SQL Server PowerShell** proporciona cmdlets para trabajar con SQL. | **Windows</br>macOS</br>Linux** |

## <a name="migration-and-other-tools"></a>Migración y otras herramientas

Estas herramientas se usan para migrar, configurar y proporcionar otras características para las bases de datos SQL.

| Herramienta | Descripción |
|:--|:--|
| **[Configuration Manager](../tools/configuration-manager/sql-server-configuration-manager-help.md)** | Use el Administrador de configuración de SQL Server para configurar los servicios de SQL Server y la conectividad de red. Configuration Manager se ejecuta en Windows|
| **[Asistente para experimentación con bases de datos](../dea/database-experimentation-assistant-overview.md)** | Use Asistente para experimentación con bases de datos para evaluar una versión de destino de SQL para una carga de trabajo determinada. |
| **[Data Migration Assistant](../dma/dma-overview.md)** | La herramienta Data Migration Assistant ayuda a actualizar a una plataforma de datos moderna mediante la detección de problemas de compatibilidad que pueden afectar a la funcionalidad de la versión nueva de SQL Server o Azure SQL Database. |
| **[Distributed Replay](../tools/distributed-replay/install-distributed-replay-overview.md)** | Use la característica Distributed Replay para ayudarlo a evaluar el impacto de la actualizaciones futuras de SQL Server. Utilice también Distributed Replay para ayudar a evaluar el impacto de las actualizaciones de hardware y del sistema operativo y la optimización de SQL Server. |
| **[ssbdiagnose](../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)** | La utilidad ssbdiagnose informa de la existencia de problemas en las conversaciones de Service Broker o en la configuración de los servicios de Service Broker. |
| **[SQL Server Migration Assistant](../ssma/sql-server-migration-assistant.md)** | Use Microsoft SQL Server Migration Assistant para automatizar la migración de bases de datos a SQL Server desde Microsoft Access, DB2, MySQL, Oracle y Sybase.|

Si busca herramientas adicionales que no se mencionan en esta página, vea [Utilidades del símbolo del sistema de SQL](command-prompt-utility-reference-database-engine.md) y [Descarga de características y herramientas extendidas de SQL Server](download-sql-feature-packs.md)
