---
title: Ejecutar SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Obtenga información acerca de los programas y menús desde los que puede iniciar SQL Server Profiler y los contextos de conexión, plantillas y filtros que se utilizan con los resultados de seguimiento.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 22e57ffa-63b0-4de3-b92e-df297dda1226
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 9b4bf16638b361059bf82cb090a1705bb8cf103b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354438"
---
# <a name="run-sql-server-profiler"></a>Ejecutar SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Puede iniciar [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] de varias formas distintas para admitir la recopilación de resultados de seguimiento en diversos escenarios. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] se puede iniciar desde el menú **Inicio** de Windows 10, desde el menú **Herramientas** del Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y desde varias ubicaciones de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Al iniciar [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] por primera vez y seleccionar **Nuevo seguimiento** en el menú **Archivo**, la aplicación muestra un cuadro de diálogo **Conectar al servidor** en el que se puede especificar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse a ella.  
## <a name="to-start-sql-server-profiler-from-the-windows-10-start-menu"></a>Para iniciar SQL Server Profiler desde el menú Inicio de Windows 10  
-  Haga clic en el icono **Inicio** de Windows o presione la tecla Windows y empiece a escribir "SQL Server Profiler 17". Cuando aparezca el icono **SQL Server Profiler 17**, haga clic en él.   

## <a name="to-start-sql-server-profiler-in-database-engine-tuning-advisor"></a>Para iniciar SQL Server Profiler en el Asistente para la optimización de motor de base de datos  
-  En el menú [!INCLUDE[ssDE](../../includes/ssde-md.md)] Herramientas **del Asistente para la optimización de** , haga clic en **SQL Server Profiler**.  

## <a name="to-start-sql-server-profiler-in-sql-server-management-studio"></a>Para iniciar SQL Server Profiler en SQL Server Management Studio  
 Puede iniciar [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] desde varias ubicaciones en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cuando se inicia [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], este carga el contexto de conexión, la plantilla de seguimiento y el contexto del filtro de su punto de inicio. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] inicia cada sesión de SQL Server Profiler en su propia instancia y Profiler se sigue ejecutando si apaga [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
### <a name="to-start-sql-server-profiler-from-the-tools-menu"></a>Para iniciar SQL Server Profiler desde el menú Herramientas  
-  En el menú **Herramientas** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], haga clic en **SQL Server Profiler**.  

### <a name="to-start-sql-server-profiler-from-the-query-editor"></a>Para iniciar SQL Server Profiler desde el Editor de consultas  
- En el Editor de consultas, haga clic con el botón derecho y seleccione **Realizar seguimiento de la consulta en SQL Server Profiler**.  

  > [!NOTE]  
  >  El contexto de conexión es la conexión del editor, la plantilla de seguimiento es TSQL_SPs y el filtro aplicado es SPID = ventana de consulta SPID.  
    
### <a name="to-start-sql-server-profiler-from-activity-monitor"></a>Para iniciar SQL Server Profiler desde el Monitor de actividad  
- En el Monitor de actividad, haga clic en el panel **Procesos**, haga clic con el botón derecho en el proceso cuyo perfil quiera generar y haga clic en **Realizar seguimiento de proceso en SQL Server Profiler**.  

    > [!NOTE]  
    >  Cuando se selecciona un proceso, el contexto de conexión es la conexión del Explorador de objetos cuando se abrió el Monitor de actividad. La plantilla de seguimiento es el valor predeterminado según el tipo de servidor y el SPID es igual al SPID del proceso seleccionado.  
    
## <a name="net-framework-security"></a>Seguridad de .NET Framework  
- En el modo de autenticación de Windows, la cuenta de usuario que ejecuta [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] debe tener permiso para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- Para realizar seguimientos con el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], los usuarios también deben disponer del permiso ALTER TRACE.  

## <a name="next-steps"></a>Pasos siguientes  
 [Información general de SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Usar SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)