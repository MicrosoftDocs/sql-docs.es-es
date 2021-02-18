---
title: Utilidades del símbolo del sistema de SQL (motor de base de datos)
description: Las utilidades de símbolo del sistema permiten incluir en scripts las operaciones de SQL Server. En este artículo se enumeran muchas utilidades del símbolo del sistema que se incluyen con SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server]
- command prompt utilities [SQL Server], about command prompt utilities
- command prompt [SQL Server]
- utilities [SQL Server], command prompt
- command prompt [SQL Server], utilities
ms.assetid: 48364bd9-6ea7-45e9-a332-acf3d81bbfae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: e948ad2c4d9e08d3fb4c5da91f7bfd76a563c64a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349963"
---
# <a name="sql-command-prompt-utilities-database-engine"></a>Utilidades del símbolo del sistema de SQL (motor de base de datos)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Las utilidades de símbolo del sistema permiten incluir en scripts las operaciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . La siguiente tabla contiene una lista de utilidades de símbolo del sistema que se suministran junto con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Para obtener información sobre la *principal* interfaz gráfica de usuario de SQL y las herramientas de la línea de comandos, consulte [Herramientas de administración y consulta SQL para SQL Server](overview-sql-tools.md).

|**Utilidad**|**Descripción**|**Instalada en**|  
|-----------------|---------------------|----------------------|  
|[bcp (utilidad)](../tools/bcp-utility.md)|Se usa para copiar datos entre una instancia de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y un archivo de datos en un formato especificado por el usuario.|\<*drive*:>\Archivos de programa\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[dta (utilidad)](../tools/dta/dta-utility.md)|Se utiliza para analizar una carga de trabajo y recomendar estructuras de diseño físico para optimizar el rendimiento del servidor para esa carga de trabajo.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[dtexec (utilidad)](../integration-services/packages/dtexec-utility.md)|Se usa para configurar y ejecutar un paquete de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Una versión de interfaz de usuario de esta utilidad del símbolo del sistema se denomina **DTExecUI** y abre la utilidad de ejecución de paquetes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[dtutil (utilidad)](../integration-services/dtutil-utility.md)|Se usa para administrar paquetes SSIS.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Implementar soluciones de modelos con la utilidad de implementación](/analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility)|Se utiliza para implementar proyectos de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] en instancias de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VShell\Common7\IDE|   
|[osql (utilidad)](../tools/osql-utility.md)|Permite especificar instrucciones, procedimientos del sistema y archivos de scripts de [!INCLUDE[tsql](../includes/tsql-md.md)] en el símbolo del sistema.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilidad de generador de perfiles](../tools/profiler-utility.md)|Se utiliza para iniciar [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] desde un símbolo del sistema.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Utilidad RS.exe &#40;SSRS&#41;](../reporting-services/tools/rs-exe-utility-ssrs.md)|Se utiliza para ejecutar scripts diseñados para administrar servidores de informes de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[rsconfig (utilidad) &#40;SSRS&#41;](../reporting-services/tools/rsconfig-utility-ssrs.md)|Se utiliza para configurar una conexión de servidor de informes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[rskeymgmt (utilidad) &#40;SSRS&#41;](../reporting-services/tools/rskeymgmt-utility-ssrs.md)|Se utiliza para administrar claves de cifrado en un servidor de informes.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlagent90 (aplicación)](../tools/sqlagent90-application.md)|Se usa para iniciar el Agente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde un símbolo del sistema.|\<drive>:\Archivos de programa\Microsoft SQL Server\\<*nombre_instancia*>\MSSQL\Binn|  
|[Utilidad sqlcmd](../tools/sqlcmd-utility.md)|Permite especificar instrucciones, procedimientos del sistema y archivos de scripts de [!INCLUDE[tsql](../includes/tsql-md.md)] en el símbolo del sistema.|\<*drive*:>\Archivos de programa\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn|  
|[SQLdiag (utilidad)](../tools/sqldiag-utility.md)|Se usa para recopilar información de diagnóstico para el Servicio de soporte y atención al cliente de [!INCLUDE[msCoName](../includes/msconame-md.md)] .|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Aplicación sqllogship](../tools/sqllogship-application.md)|Las aplicaciones lo utilizan para realizar operaciones de copia de seguridad, copia y restauración, y tareas de limpieza asociadas en una configuración de trasvase de registros sin ejecutar trabajos de copia de seguridad, copia y restauración.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[SqlLocalDB (utilidad)](../tools/sqllocaldb-utility.md)|Un modo de ejecución de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] destinado a los desarrolladores de programas.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlmaint (utilidad)](../tools/sqlmaint-utility.md)|Se usa para ejecutar los planes de mantenimiento de bases de datos creados en versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|\<drive>:\Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Utilidad sqlps](../tools/sqlps-utility.md)|Se usa para ejecutar comandos y scripts de PowerShell. Carga y registra el proveedor de PowerShell de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y los cmdlets.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlservr (aplicación)](../tools/sqlservr-application.md)|Se usa para iniciar y detener una instancia de [!INCLUDE[ssDE](../includes/ssde-md.md)] desde el símbolo del sistema para solucionar problemas.|\<drive>:\Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Ssms (Utilidad)](../ssms/ssms-utility.md)|Se utiliza para iniciar [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] desde un símbolo del sistema.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VSShell\Common7\IDE|  
|[Utilidad tablediff](../tools/tablediff-utility.md)|Se utiliza para comparar los datos de dos tablas y ver si no convergen, lo que es útil para solucionar problemas de una topología de replicación.|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]COM|  

## <a name="command-prompt-utilities-syntax-conventions"></a>Convenciones de la sintaxis de las herramientas del símbolo del sistema  
  
|**Convención**|**Se usa para**|  
|--------------------|------------------|  
|MAYÚSCULAS|Instrucciones y términos usados en el sistema operativo.|  
|`monospace`|Comandos y código de programación de ejemplo.|  
|*cursiva*|Parámetros proporcionados por el usuario.|  
|**Negrita**|Comandos, parámetros y otros elementos de sintaxis que deben escribirse exactamente como se muestran.|  

## <a name="see-also"></a>Consulte también

* [Replication Distribution Agent](../relational-databases/replication/agents/replication-distribution-agent.md)
* [Agente de registro del LOG de replicación](../relational-databases/replication/agents/replication-log-reader-agent.md)
* [Replication Merge Agent](../relational-databases/replication/agents/replication-merge-agent.md)
* [Agente de lectura de cola de replicación](../relational-databases/replication/agents/replication-queue-reader-agent.md)
* [Replication Snapshot Agent](../relational-databases/replication/agents/replication-snapshot-agent.md)