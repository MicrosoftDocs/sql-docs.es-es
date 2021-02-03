---
title: Iniciar y utilizar el Asistente para la optimización del motor de base de datos | Microsoft Docs
description: Obtenga información sobre cómo iniciar y usar Asistente para la optimización de motor de base de datos en SQL Server para crear cargas de trabajo, optimizar bases de datos y crear archivos de entrada XML.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.workload.f1
- sql13.dta.general.f1
- sql13.dta.advancedtuningoptions.f1
- sql13.dta.tuningoptions.f1
- sql13.dta.progress.f1
- sql13.dta.options.f1
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], starting
ms.assetid: a4e3226a-3917-4ec8-bdf0-472879d231c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: da72d129bac65b1b9210e49d33782b257ea9e182
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236433"
---
# <a name="start-and-use-the-database-engine-tuning-advisor"></a>Iniciar y utilizar el Asistente para la optimización de motor de base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo iniciar y usar el Asistente para la optimización de motor de base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Para obtener más información sobre cómo ver y trabajar con los resultados después de optimizar una base de datos, vea [Ver y trabajar con la salida del Asistente para la optimización de motor de base de datos](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  
  
##  <a name="initialize-the-database-engine-tuning-advisor"></a><a name="Initialize"></a> Inicializar el Asistente para la optimización de motor de base de datos  
 La primera vez que se utilice, un usuario que sea miembro del rol fijo de servidor **sysadmin** debe inicializar el Asistente para la optimización de motor de base de datos. Esto se debe a que varias tablas del sistema se deben crear en la base de datos de **msdb** para admitir las operaciones de optimización. La inicialización permite además que usuarios miembros del rol fijo de base de datos **db_owner** optimicen cargas de trabajo en tablas de bases de datos que son de su propiedad.  
  
 Un usuario que tenga permisos de administrador del sistema debe realizar una de las siguientes acciones:  
  
-   Usar la interfaz gráfica de usuario del Asistente para la optimización de motor de base de datos para conectarse a una instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Iniciar el Asistente para la optimización de motor de base de datos](#Start) más adelante en este tema.  
  
-   Use la utilidad **dta** para optimizar la primera carga de trabajo. Para obtener más información, vea [Usar la utilidad dta](#dta) más adelante en este tema.  
  
##  <a name="start-the-database-engine-tuning-advisor"></a><a name="Start"></a> Iniciar el Asistente para la optimización de motor de base de datos  
 Puede iniciar la interfaz gráfica de usuario (GUI) del Asistente para la optimización de motor de base de datos de diferentes maneras con el fin de permitir la optimización de bases de datos en diversos escenarios. Las distintas formas de iniciar el Asistente para la optimización de motor de base de datos son: desde el menú **Inicio** , desde el menú **Herramientas** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], desde el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], y desde el menú **Herramientas** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Al iniciar el Asistente para la optimización de motor de base de datos por primera vez, la aplicación muestra un cuadro de diálogo **Conectar al servidor** en el que se puede especificar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que se desea conectar.  
  
> [!WARNING]  
>  No inicie el Asistente para la optimización de motor de base de datos si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando en modo de usuario único. Si intenta iniciarlo mientras el servidor está en modo de usuario único, aparecerá un error y el Asistente para la optimización de motor de base de datos no se iniciará. Para obtener más información sobre el modo de usuario único, vea [Iniciar SQL Server en modo de usuario único](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).  
  
#### <a name="to-start-database-engine-tuning-advisor-from-the-windows-start-menu"></a>Para iniciar el Asistente para la optimización de motor de base de datos desde el menú Inicio de Windows  
  
1.  En el menú **Inicio** , elija **Todos los programas**, **Microsoft SQL Server**, **Herramientas de rendimiento** y, a continuación, haga clic en **Asistente para la optimización de motor de base de datos**.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-management-studio"></a>Para iniciar el Asistente para la optimización de motor de base de datos en SQL Server Management Studio  
  
1.  En el menú **Herramientas** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], haga clic en **Asistente para la optimización de motor de base de datos**.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-from-the-sql-server-management-studio-query-editor"></a>Para iniciar el Asistente para la optimización de motor de base de datos desde el Editor de consultas de SQL Server Management Studio  
  
1.  Abra un archivo de script [!INCLUDE[tsql](../../includes/tsql-md.md)] en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para obtener más información, vea [Editores de consultas y texto &#40;SQL Server Management Studio&#41;](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
2.  Seleccione una consulta en el script [!INCLUDE[tsql](../../includes/tsql-md.md)] o seleccione el script completo, haga clic con el botón derecho en la selección y elija **Analizar la consulta en el Asistente para la optimización de motor de base de datos**. Se abre la GUI del Asistente para la optimización de motor de base de datos e importa el script como una carga de trabajo de archivo XML. Puede especificar un nombre de sesión y opciones de optimización para optimizar las consultas [!INCLUDE[tsql](../../includes/tsql-md.md)] seleccionadas como carga de trabajo.  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-profiler"></a>Para iniciar el Asistente para la optimización de motor de base de datos en SQL Server Profiler  
  
1.  En el menú **Herramientas** de SQL Server Profiler, haga clic en **Asistente para la optimización de motor de base de datos**.  
  
##  <a name="create-a-workload"></a><a name="Create"></a> Crear una carga de trabajo  
 Una carga de trabajo es un conjunto de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecuta en una o varias bases de datos que se desean optimizar. El Asistente para la optimización de motor de base de datos analiza estas cargas de trabajo y recomienda estrategias de partición o indización que mejorarán el rendimiento de las consultas del servidor.  
  
 Puede crear una carga de trabajo mediante uno de los métodos siguientes.  
  
-   Utilice el [Almacén de consultas](../../relational-databases/performance/how-query-store-collects-data.md) como una carga de trabajo. De este modo, podrá evitar la necesidad de tener que crear manualmente una carga de trabajo. Para más información, vea [Tuning Database Using Workload From Query Store](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md) (Optimización de la base de datos mediante carga de trabajo del Almacén de consultas).  

      ||  
      |-|  
      |**Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  

  
-   Utilice la memoria caché del plan como carga de trabajo. De este modo, podrá evitar la necesidad de tener que crear manualmente una carga de trabajo. Para obtener más información, vea [Optimizar una base de datos](#Tune) más adelante en este tema.  
  
-   Use el editor de consultas en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o su editor de texto preferido para crear manualmente cargas de trabajo de scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   Use [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para crear cargas de trabajo de tablas de seguimiento o archivos de seguimiento.  
  
    > [!NOTE]  
    >  Cuando use una tabla de seguimiento como una carga de trabajo, esa tabla debe existir en el mismo servidor en el que el Asistente para la optimización de motor de base de datos está realizando la optimización. Si crea la tabla de seguimiento en otro servidor, muévala después al servidor en el que el Asistente para la optimización de motor de base de datos está realizando la optimización.  
  
-   Las cargas de trabajo también pueden incrustarse en un archivo de entrada XML, en el que también se puede especificar un peso para cada evento. Para obtener más información sobre cómo especificar cargas de trabajo incrustadas, vea [Crear un archivo de entrada XML](#XMLInput) más adelante en este tema.  
  
###  <a name="to-create-transact-sql-script-workloads"></a><a name="SSMS"></a> Para crear cargas de trabajo de scripts Transact-SQL  
  
1.  Inicie el Editor de consultas en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para obtener más información, vea [Editores de consultas y texto &#40;SQL Server Management Studio&#41;](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
2.  Escriba el script [!INCLUDE[tsql](../../includes/tsql-md.md)] en el Editor de consultas. Este script debe contener un conjunto de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecutan en una o varias bases de datos que desea optimizar.  
  
3.  Guarde el archivo con la extensión **.sql** . La utilidad **dta** de línea de comandos y la GUI del Asistente para la optimización de motor de base de datos pueden usar este script [!INCLUDE[tsql](../../includes/tsql-md.md)] como una carga de trabajo.  
  
###  <a name="to-create-trace-file-and-trace-table-workloads"></a><a name="Profiler"></a> Para crear cargas de trabajo de tablas de seguimiento o archivos de seguimiento  
  
1.  Inicie el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] mediante uno de los métodos siguientes:  
  
    -   En el menú **Inicio** , elija **Todos los programas**, **Microsoft SQL Server**, **Herramientas de rendimiento** y, a continuación, haga clic en **SQL Server Profiler**.  
  
    -   En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], haga clic en el menú **Herramientas** y, a continuación, haga clic en **SQL Server Profiler**.  
  
2.  Cree un archivo o tabla de seguimiento, tal como se describe en los procedimientos siguientes, que use la plantilla **Tuning** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]:  
  
    -   [Crear un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)  
  
    -   [Guardar los resultados de un seguimiento en un archivo &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)  
  
         El Asistente para la optimización de motor de base de datos considera que el archivo de seguimiento de la carga de trabajo es un archivo de sustitución incremental. Para obtener más información sobre los archivos de sustitución incremental, vea [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
    -   [Guardar los resultados de un seguimiento en una tabla &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)  
  
         Asegúrese de que el seguimiento se ha detenido antes de utilizar una tabla de seguimiento como carga de trabajo.  
  
 Es recomendable usar la plantilla Tuning de SQL Server Profiler para capturar cargas de trabajo para el Asistente para la optimización de motor de base de datos.  
  
 Si desea utilizar su propia plantilla, asegúrese de que se capturan los siguientes eventos de seguimiento:  
  
-   **RPC:Completed**  
  
-   **SQL:BatchCompleted**  
  
-   **SP:StmtCompleted**  
  
 También puede utilizar las versiones **Starting** (inicio) de estos eventos de seguimiento. Por ejemplo, **SQL:BatchStarting**. Sin embargo, las versiones **Completed** (completado) de estos eventos de seguimiento incluyen la columna **Duration** , que permite al Asistente para la optimización de motor de base de datos optimizar de forma más eficaz la carga de trabajo. El Asistente para la optimización de motor de base de datos no optimiza otros tipos de eventos de seguimiento. Para obtener más información acerca de estos eventos de seguimiento, vea [Stored Procedures Event Category](../../relational-databases/event-classes/stored-procedures-event-category.md) y [TSQL Event Category](../../relational-databases/event-classes/tsql-event-category.md). Para obtener más información sobre cómo usar procedimientos almacenados de Seguimiento de SQL para crear una carga de trabajo de archivo de seguimiento, vea [Crear un seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
### <a name="trace-file-or-trace-table-workloads-that-contain-the-loginname-data-column"></a>Cargas de trabajo de archivos o tablas de seguimiento que contienen la columna de datos LoginName  
 El Asistente para la optimización de motor de base de datos envía solicitudes de plan de presentación como parte del proceso de optimización. Cuando una tabla o un archivo de seguimiento que contiene la columna de datos **LoginName** se consume como una carga de trabajo, el Asistente para la optimización de motor de base de datos suplanta al usuario especificado en **LoginName**. Si este usuario no dispone del permiso SHOWPLAN, que le permite ejecutar y generar planes de presentación para las instrucciones incluidas en el seguimiento, el Asistente para la optimización de motor de base de datos no optimizará esas instrucciones.  
  
##### <a name="to-avoid-granting-the-showplan-permission-to-each-user-specified-in-the-loginname-column-of-the-trace"></a>Para evitar conceder el permiso SHOWPLAN a cada usuario especificado en la columna LoginName del seguimiento  
  
1.  Optimice la carga de trabajo del archivo o carga de seguimiento. Para obtener más información, vea [Optimizar una base de datos](#Tune) más adelante en este tema.  
  
2.  Utilice el registro de optimización para buscar instrucciones que no se hayan optimizado por causa de permisos inadecuados. Para obtener más información, vea [Ver y trabajar con la salida del Asistente para la optimización de motor de base de datos](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  
  
3.  Cree una nueva carga de trabajo al eliminar la columna **LoginName** de los eventos que no se hayan optimizado y, a continuación, guarde únicamente los eventos no optimizados en un nuevo archivo o tabla de seguimiento. Para obtener más información sobre cómo eliminar columnas de datos de un seguimiento, vea [Especificar eventos y columnas de datos para un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md) o [Modificar un seguimiento existente &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/modify-an-existing-trace-transact-sql.md).  
  
4.  Vuelva a enviar la nueva carga de trabajo sin la columna **LoginName** al Asistente para la optimización de motor de base de datos.  
  
 El Asistente para la optimización de motor de base de datos optimizará la nueva carga de trabajo debido a que no hay información de inicio de sesión especificada en el seguimiento. Si **LoginName** no existe en una instrucción, el Asistente para la optimización de motor de base de datos optimiza esa instrucción suplantando al usuario que inició la sesión de optimización (un miembro del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** ).  
  
##  <a name="tune-a-database"></a><a name="Tune"></a> Optimizar una base de datos  
 Para optimizar una base de datos, puede usar la GUI del Asistente para la optimización de motor de base de datos o la utilidad **dta** .  
  
> [!NOTE]  
>  Asegúrese de que el seguimiento se ha detenido antes de usar una tabla de seguimiento como carga de trabajo para el Asistente para la optimización de motor de base de datos. El Asistente para la optimización de motor de base de datos no permite el uso como carga de trabajo de una tabla de seguimiento en la que aún se están escribiendo eventos de seguimiento.  
  
### <a name="use-the-database-engine-tuning-advisor-graphical-user-interface"></a>Usar la interfaz gráfica de usuario del Asistente para la optimización de motor de base de datos  
 En la GUI del Asistente para la optimización de motor de base de datos puede optimizar una base de datos utilizando la memoria caché del plan, los archivos de carga de trabajo o las tablas de carga de trabajo. Puede usar la GUI del Asistente para la optimización de motor de base de datos para ver fácilmente los resultados de la sesión de optimización actual y los resultados de sesiones anteriores. Para obtener información sobre las opciones de la interfaz de usuario, vea [Descripciones de la interfaz de usuario](#UI) más adelante en este tema. Para obtener más información sobre cómo trabajar con la salida después de optimizar una base de datos, vea [Ver y trabajar con la salida del Asistente para la optimización de motor de base de datos](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  

####  <a name="to-tune-a-database-by-using-the-query-store"></a><a name="PlanCache"></a> Para optimizar una base de datos mediante el Almacén de consultas
Para más información, vea [Optimización de la base de datos mediante carga de trabajo del Almacén de consultas](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md) (Optimización de la base de datos mediante carga de trabajo del Almacén de consultas).
  
####  <a name="to-tune-a-database-by-using-the-plan-cache"></a><a name="PlanCache"></a> Para optimizar una base de datos mediante caché de plan  
  
1.  Inicie el Asistente para la optimización de motor de base de datos e inicie sesión en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Iniciar el Asistente para la optimización de motor de base de datos](#Start) anteriormente en este tema.  
  
2.  En la pestaña **General** , escriba un nombre en **Nombre de sesión** para crear una nueva sesión de optimización. Deberá configurar los campos de la pestaña **General** antes de iniciar una sesión de optimización. No es necesario modificar las opciones de la pestaña **Opciones de optimización** antes de iniciar una sesión de optimización.  
  
3.  Seleccione **Caché del plan** como la opción de carga de trabajo. El Asistente para la optimización de motor de base de datos selecciona los 1.000 eventos principales de la memoria caché de plan para usarlos en el análisis.  
  
4.  Seleccione las bases de datos que desea optimizar y, opcionalmente, en **Tablas seleccionadas**, elija una o más tablas de cada base de datos. Para incluir las entradas de caché de todas las bases de datos, en **Opciones de optimización**, haga clic en **Opciones avanzadas** y active **Incluir eventos de caché del plan de todas las bases de datos**.  
  
5.  Seleccione **Guardar registro de optimización** para guardar una copia del registro de optimización. Desactive la casilla si no desea guardar una copia del registro de optimización.  
  
     Puede ver el registro de optimización después del análisis; para ello, abra la sesión y seleccione la pestaña **Progreso** .  
  
6.  Haga clic en la pestaña **Opciones de optimización** y seleccione las opciones que figuran en la lista.  
  
7.  Haga clic en **Iniciar análisis**.  
  
     Si desea detener la sesión de optimización una vez iniciada, en el menú **Acciones** elija una de las siguientes opciones:  
  
    -   **Detener análisis (con recomendaciones)** detiene la sesión de optimización y pregunta al usuario si quiere que el Asistente para la optimización de motor de base de datos genere recomendaciones basadas en el análisis realizado hasta este punto.  
  
    -   **Detener análisis** detiene la sesión de optimización sin generar ninguna recomendación.  
  
> [!NOTE]  
>  No se admite la detección del Asistente para la optimización de motor de base de datos. Si hace clic en el botón de la barra de herramientas **Iniciar análisis** después de hacer clic en el botón **Detener análisis** o **Detener análisis (con recomendaciones)** , el Asistente para la optimización de motor de base de datos inicia una sesión de optimización nueva.  
  
#### <a name="to-tune-a-database-using-a-workload-file-or-table-as-input"></a>Para optimizar una base de datos mediante una tabla o archivo de carga de trabajo como entrada  
  
1.  Determine las características de la base de datos (índices, vistas indizadas, creación de particiones) que desee que el Asistente para la optimización de motor de base de datos pueda agregar, quitar o retener durante el análisis.  
  
2.  Cree una carga de trabajo. Para obtener más información, vea [Crear una carga de trabajo](#Create) , anteriormente en este tema.  
  
3.  Inicie el Asistente para la optimización de motor de base de datos e inicie sesión en una instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Iniciar el Asistente para la optimización de motor de base de datos](#Start) anteriormente en este tema.  
  
4.  En la pestaña **General** , escriba un nombre en **Nombre de sesión** para crear una nueva sesión de optimización.  
  
5.  Elija **Archivo de carga de trabajo** o **Tabla** y escriba la ruta de acceso al archivo o el nombre de la tabla en el cuadro de texto adyacente.  
  
     El formato para especificar una tabla es:  
  
    ```  
    database_name.schema_name.table_name  
    ```  
  
     Para buscar una tabla o archivo de carga de trabajo, haga clic en **Examinar**. El Asistente para la optimización de motor de base de datos presupone que los archivos de carga de trabajo son archivos de sustitución incremental. Para obtener más información sobre los archivos de sustitución incremental, vea [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
     Al usar una tabla de seguimiento como una carga de trabajo, esa tabla debe existir en el mismo servidor que el Asistente para la optimización de motor de base de datos está optimizando. Si crea una tabla de seguimiento en otro servidor, muévala al servidor en el que el Asistente para la optimización de motor de base de datos está realizando la optimización antes de utilizarla como carga de trabajo.  
  
6.  Seleccione las bases de datos y las tablas en las que desea ejecutar la carga de trabajo seleccionada en el paso 5. Para seleccionar las tablas, haga clic en la flecha **Tablas seleccionadas** .  
  
7.  Seleccione **Guardar registro de optimización** para guardar una copia del registro de optimización. Desactive la casilla si no desea guardar una copia del registro de optimización.  
  
     Puede ver el registro de optimización después del análisis; para ello, abra la sesión y seleccione la pestaña **Progreso** .  
  
8.  Haga clic en la pestaña **Opciones de optimización** y seleccione las opciones que figuran en la lista.  
  
9. Haga clic en el botón **Iniciar análisis** de la barra de herramientas.  
  
     Si desea detener la sesión de optimización una vez iniciada, en el menú **Acciones** elija una de las siguientes opciones:  
  
    -   **Detener análisis (con recomendaciones)** detiene la sesión de optimización y pregunta al usuario si quiere que el Asistente para la optimización de motor de base de datos genere recomendaciones basadas en el análisis realizado hasta este punto.  
  
    -   **Detener análisis** detiene la sesión de optimización sin generar ninguna recomendación.  
  
> [!NOTE]  
>  No se admite la detección del Asistente para la optimización de motor de base de datos. Si hace clic en el botón de la barra de herramientas **Iniciar análisis** después de hacer clic en el botón **Detener análisis** o **Detener análisis (con recomendaciones)** , el Asistente para la optimización de motor de base de datos inicia una sesión de optimización nueva.  
  
###  <a name="use-the-dta-utility"></a><a name="dta"></a> Usar la utilidad dta  
 La [utilidad dta](../../tools/dta/dta-utility.md) proporciona un archivo ejecutable en el símbolo del sistema que puede usar para optimizar bases de datos. Esto permite usar la funcionalidad del Asistente para la optimización de motor de base de datos en archivos por lotes y scripts. La utilidad **dta** usa las entradas de caché del plan, los archivos de seguimiento, las tablas de seguimiento y los scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] como cargas de trabajo. También usa la entrada XML que se ajusta al esquema XML del Asistente para la optimización de motor de base de datos, que está disponible en este [sitio web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100).  
  
 Tenga en cuenta lo siguiente antes de optimizar una carga de trabajo con la utilidad **dta** :  
  
-   Al usar una tabla de seguimiento como una carga de trabajo, esa tabla debe existir en el mismo servidor que el Asistente para la optimización de motor de base de datos está optimizando. Si crea la tabla de seguimiento en otro servidor, muévala al servidor que el Asistente para la optimización de motor de base de datos está optimizando.  
  
-   Asegúrese de que el seguimiento se ha detenido antes de usar una tabla de seguimiento como carga de trabajo para el Asistente para la optimización de motor de base de datos. El Asistente para la optimización de motor de base de datos no permite el uso como carga de trabajo de una tabla de seguimiento en la que aún se están escribiendo eventos de seguimiento.  
  
-   Si una sesión de optimización continúa ejecutándose más tiempo del que había previsto, puede pulse CTRL+C para detener la sesión de optimización y generar recomendaciones basadas en el análisis que **dta** ha completado hasta ese momento. Se le solicitará que decida si desea o no generar recomendaciones. Presione CTRL+C de nuevo para detener la sesión de optimización sin generar recomendaciones.  
  
 Para más información sobre la sintaxis de la utilidad **dta** y ejemplos, vea [dta Utility](../../tools/dta/dta-utility.md).  
  
#### <a name="to-tune-a-database-by-using-the-plan-cache"></a>Para optimizar una base de datos mediante caché de plan  
  
1.  Especifique la opción **-ip** . Se analizan los primeros 1.000 eventos de caché del plan para las bases de datos seleccionadas.  
  
     En el símbolo del sistema, escriba lo siguiente:  
  
    ```  
    dta -E -D DatabaseName -ip -s SessionName  
    ```  
  
2.  Para modificar el número de eventos que se van a usar para el análisis, especifique la opción **-n**. El ejemplo siguiente aumenta el número de entradas de caché en 2.000.  
  
    ```  
    dta -E -D DatabaseName -ip -n 2000-s SessionName1  
    ```  
  
3.  Para analizar los eventos de todas las bases de datos de la instancia, especifique la opción **-ipf** .  
  
    ```  
    dta -E -D DatabaseName -ip -ipf -n 2000 -s SessionName2  
    ```  
  
#### <a name="to-tune-a-database-by-using-a-workload-and-dta-utility-default-settings"></a>Para optimizar una base de datos mediante una carga de trabajo y la configuración predeterminada de la utilidad dta  
  
1.  Determine las características de la base de datos (índices, vistas indizadas, creación de particiones) que desee que el Asistente para la optimización de motor de base de datos pueda agregar, quitar o retener durante el análisis.  
  
2.  Cree una carga de trabajo. Para obtener más información, vea [Crear una carga de trabajo](#Create) , anteriormente en este tema.  
  
3.  En el símbolo del sistema, escriba lo siguiente:  
  
    ```  
    dta -E -D DatabaseName -if WorkloadFile -s SessionName  
    ```  
  
     donde `-E` especifica que la sesión de optimización utilice una conexión de confianza (en lugar de un identificador de inicio de sesión y una contraseña); `-D` especifica el nombre de la base de datos que desea optimizar. De forma predeterminada, la utilidad se conecta a la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo local. (Use la opción `-S` para especificar una base de datos remota, como se muestra en el siguiente procedimiento, o bien para especificar una instancia con nombre.) La opción `-if` especifica el nombre y la ruta de acceso a un archivo de carga de trabajo (que puede ser un script [!INCLUDE[tsql](../../includes/tsql-md.md)] o un archivo de seguimiento) y `-s` especifica un nombre para la sesión de optimización.  
  
     Las cuatro opciones mostradas aquí (nombre de la base de datos, carga de trabajo, tipo de conexión y nombre de la sesión) son obligatorias.  
  
#### <a name="to-tune-a-remote-database-or-a-named-instance-for-a-specific-duration"></a>Para optimizar una base de datos remota o una instancia con nombre con una determinada duración  
  
1.  Determine las características de la base de datos (índices, vistas indizadas, creación de particiones) que desee que el Asistente para la optimización de motor de base de datos pueda agregar, quitar o retener durante el análisis.  
  
2.  Cree una carga de trabajo. Para obtener más información, vea [Crear una carga de trabajo](#Create) , anteriormente en este tema.  
  
3.  En el símbolo del sistema, escriba lo siguiente:  
  
    ```  
    dta -S ServerName\Instance -D DatabaseName -it WorkloadTableName   
    -U LoginID -P Password -s SessionName -A TuningTimeInMinutes  
    ```  
  
     donde `-S` especifica el nombre de una instancia y un servidor remotos (o una instancia con nombre en el servidor local) y `-D` especifica el nombre de la base de datos que desea optimizar. La opción `-it` especifica el nombre de la tabla de carga de trabajo, `-U` y `-P` especifican el identificador de inicio de sesión y la contraseña de la base de datos remota, `-s` especifica el nombre de la sesión de optimización y `-A` especifica la duración de la sesión de optimización en minutos. La utilidad **dta** usa de forma predeterminada una duración de optimización de 8 horas. Si quiere que el Asistente para la optimización de motor de base de datos optimice una carga de trabajo durante un tiempo ilimitado, especifique **0** (cero) en la opción `-A` .  
  
#### <a name="to-tune-a-database-using-an-xml-input-file"></a>Para optimizar una base de datos mediante un archivo de entrada XML  
  
1.  Determine las características de la base de datos (índices, vistas indizadas, creación de particiones) que desee que el Asistente para la optimización de motor de base de datos pueda agregar, quitar o retener durante el análisis.  
  
2.  Cree una carga de trabajo. Para obtener más información, vea [Crear una carga de trabajo](#Create) , anteriormente en este tema.  
  
3.  Cree un archivo de entrada XML. Para obtener más información, vea [Crear archivos de entrada XML](#XMLInput) más adelante en este tema.  
  
4.  En el símbolo del sistema, escriba lo siguiente:  
  
    ```  
    dta -E -S ServerName\Instance -s SessionName -ix PathToXMLInputFile  
    ```  
  
     donde `-E` especifica una conexión de confianza, `-S` especifica una instancia y un servidor remotos o una instancia con nombre en el servidor local, `-s` especifica el nombre de la sesión de optimización e `-ix` especifica el archivo de entrada XML que se usará en la sesión de optimización.  
  
5.  Cuando la utilidad termine de optimizar la carga de trabajo, puede ver los resultados de las sesiones de optimización mediante la GUI del Asistente para la optimización de motor de base de datos. Otra posibilidad es especificar que las recomendaciones de optimización se escriban en un archivo XML con la opción **-ox** . Para obtener más información, consulte [dta Utility](../../tools/dta/dta-utility.md).  
  
##  <a name="create-an-xml-input-file"></a><a name="XMLInput"></a> Crear un archivo de entrada XML  
 Si el usuario es un desarrollador de XML experimentado, puede crear archivos con formato XML que el Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] podrá utilizar para optimizar cargas de trabajo. Para crear estos archivos XML, utilice las herramientas XML para modificar un archivo de ejemplo o para generar una instancia del esquema XML del Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 El esquema XML del Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] está disponible en la instalación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la ubicación siguiente:  
  
 `C:\Program Files\Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd` 
  
 El esquema XML del Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] también se encuentra disponible en línea en este [sitio web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
 Esta dirección URL le lleva a una página en la que encontrará muchos esquemas XML de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Desplácese por la página hasta que llegue a la fila del Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
#### <a name="to-create-an-xml-input-file-to-tune-workloads"></a>Para crear un archivo de entrada XML para optimizar cargas de trabajo  
  
1.  Cree una carga de trabajo. Puede utilizar una tabla o un archivo de seguimiento mediante la plantilla de optimización en el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], o bien crear un script [!INCLUDE[tsql](../../includes/tsql-md.md)] que reproduzca una carga de trabajo representativa para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Crear una carga de trabajo](#Create) , anteriormente en este tema.  
  
2.  Cree un archivo de entrada XML mediante uno de los métodos siguientes:  
  
    -   Copie y pegue uno de los [ejemplos de archivos de entrada XML &#40;DTA&#41;](../../tools/dta/xml-input-file-samples-dta.md) en el editor XML que quiera. Cambie los valores de forma que se especifiquen los argumentos apropiados para su instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y guarde el archivo XML.  
  
    -   Mediante la herramienta XML que prefiera, genere una instancia desde el esquema XML del Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
3.  Una vez creado el archivo de entrada XML, úselo como entrada de la utilidad de línea de comandos **dta** para optimizar la carga de trabajo. Para obtener información sobre cómo utilizar los archivos de entrada XML con esta utilidad, vea la sección [Usar la utilidad dta](#dta) anteriormente en este tema.  
  
> [!NOTE]  
>  Si quiere usar una carga de trabajo insertada, que es una carga de trabajo especificada directamente en el archivo de entrada XML, recurra al ejemplo que aparece en [Ejemplo de archivo de entrada XML con carga de trabajo insertada &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-inline-workload-dta.md).  
  
##  <a name="user-interface-descriptions"></a><a name="UI"></a> Descripciones de la interfaz de usuario  
  
### <a name="tools-menuoptions-page"></a>Menú Herramientas/Página Opciones  
 Utilice este cuadro de diálogo para especificar parámetros de configuración generales para el Asistente para la optimización de motor de base de datos.  
  
 **Al iniciar**  
 Especifique qué acción debería llevar a cabo el Asistente para la optimización de motor de base de datos al iniciarse: abrirse sin una conexión a bases de datos, mostrar un cuadro de diálogo **Nueva conexión** , mostrar una nueva sesión o cargar la última sesión cargada.  
  
 **Cambiar fuente**  
 Especifica la fuente que se muestra y que utilizan las tablas del Asistente para la optimización de motor de base de datos.  
  
 **Número de elementos en las listas usadas más recientemente**  
 Especifica el número de sesiones o archivos que se mostrarán en **Sesiones recientes** o **Archivos recientes** del menú **Archivo** .  
  
 **Recordar mis últimas opciones de optimización**  
 Conserva las opciones de optimización entre sesiones. La opción está seleccionada de forma predeterminada. Desactive esta casilla para empezar siempre con los valores predeterminados del Asistente para la optimización de motor de base de datos.  
  
 **Preguntar antes de eliminar permanentemente las sesiones**  
 Muestra un cuadro de diálogo de confirmación antes de eliminar las sesiones.  
  
 **Preguntar antes de detener análisis de sesión**  
 Muestra un cuadro de diálogo de confirmación antes de detener el análisis de una carga de trabajo.  
  
#### <a name="general-tab-options"></a>Opciones de la pestaña General  
 Deberá configurar los campos de la pestaña **General** antes de iniciar una sesión de optimización. No tiene que modificar las opciones de la pestaña **Opciones de optimización** antes de iniciar una sesión de optimización.  
  
 **Nombre de sesión**  
 Especifica un nombre para la sesión. El nombre de sesión asocia un nombre a una sesión de optimización. Podrá hacer referencia a este nombre para revisar la sesión de optimización posteriormente.  
  
 **Archivo**  
 Especifica un script .sql o un archivo de seguimiento para una carga de trabajo. Especifique la ruta de acceso y el nombre de archivo en el cuadro de texto asociado. El Asistente para la optimización de motor de base de datos considera que el archivo de seguimiento de la carga de trabajo es un archivo de sustitución incremental. Para obtener más información sobre los archivos de sustitución incremental, vea [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md).  
  
 **Table**  
 Especifica una tabla de seguimiento para una carga de trabajo. Especifique el nombre completo de la tabla de seguimiento en el cuadro de texto asociado, tal y como se indica a continuación:  
  
```  
database_name.owner_name.table_name  
```  
  
-   Asegúrese de que el seguimiento se ha detenido antes de utilizar una tabla de seguimiento como carga de trabajo.  
  
-   La tabla de seguimiento debe encontrarse en el mismo servidor que el Asistente para la optimización de motor de base de datos está optimizando. Si crea la tabla de seguimiento en otro servidor, muévala al servidor que el Asistente para la optimización de motor de base de datos está optimizando.  
  
 **Caché del plan**  
 Especifique la memoria caché de plan como una carga de trabajo. De este modo, podrá evitar la necesidad de tener que crear manualmente una carga de trabajo. El Asistente para la optimización de motor de base de datos selecciona los 1.000 eventos principales que se usarán en el análisis.  
  
 **Xml**  
 No aparecerá a menos que importe una consulta de carga de trabajo desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Para importar una consulta de carga de trabajo desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  
  
1.  Escriba una consulta en el Editor de consultas y resáltela.  
  
2.  Haga clic con el botón derecho en la consulta resaltada y haga clic en **Analizar la consulta en el Asistente para la optimización de motor de base de datos**.  
  
 **Busque un archivo/una tabla de carga de trabajo**  
 Cuando haya seleccionado un **archivo** o una **tabla** como origen de la carga de trabajo, use este botón Examinar para seleccionar un destino.  
  
 **Obtenga una vista previa de la carga de trabajo XML**  
 Muestra una carga de trabajo con formato XML que se haya importado desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Base de datos para análisis de carga de trabajo**  
 Especifica la primera base de datos a la que se conecta el Asistente para la optimización de motor de base de datos al optimizar una carga de trabajo. Una vez iniciada la optimización, el Asistente para la optimización de motor de base de datos se conecta a las bases de datos especificadas en las instrucciones `USE DATABASE` que contiene la carga de trabajo.  
  
 **Seleccionar bases de datos y tablas para optimizar**  
 Especifica las bases de datos y tablas que se deben optimizar. Para especificar todas las bases de datos, seleccione la casilla del encabezado de columna **Nombre** . Para especificar bases de datos específicas, seleccione la casilla situada junto al nombre de la base de datos. De forma predeterminada, todas las tablas de las bases de datos seleccionadas se incluyen automáticamente en la sesión de optimización. Para excluir tablas, haga clic en la flecha de la columna **Tablas seleccionadas** y, a continuación, desactive las casillas situadas junto a las tablas que no desee optimizar.  
  
 Flecha abajo de **Tablas seleccionadas**  
 Expande la lista de tablas para poder seleccionar tablas individuales para su optimización.  
  
 **Guardar registro de optimización**  
 Crea un registro y graba los errores que se producen durante la sesión.  
  
> [!NOTE]  
>  El Asistente para la optimización de motor de base de datos no actualiza de forma automática la información sobre las filas de las tablas que se muestran en la pestaña **General** . En lugar de ello, se basa en los metadatos de la base de datos. Si sospecha que la información sobre las filas está obsoleta, ejecute el comando DBCC UPDATEUSAGE para los objetos apropiados.  
  
##### <a name="tuning-tab-options"></a>Pestaña Opciones de optimización  
 Utilice la pestaña **Opciones de optimización** para modificar la configuración predeterminada de las opciones generales de optimización. No tiene que modificar las opciones de la pestaña **Opciones de optimización** antes de iniciar una sesión de optimización.  
  
 **Limitar tiempo de optimización**  
 Limita el tiempo de la sesión de optimización actual. Si se proporciona más tiempo para la optimización se mejora la calidad de las recomendaciones. Para garantizar que se dispone de las mejores recomendaciones, no conviene activar esta opción.  
  
> [!NOTE]  
> [!INCLUDE[ssDE](../../includes/ssde-md.md)] consume recursos de sistema durante el análisis. Utilice **Limitar tiempo de optimización** para detener la optimización antes de períodos de gran carga de trabajo prevista en el servidor que se optimiza.  
  
 **Opciones avanzadas**  
 Use el cuadro de diálogo **Opciones avanzadas de optimización** para configurar el espacio máximo, el número máximo de columnas de clave y las recomendaciones de índice en línea.  
  
 **Definir espacio máximo para recomendaciones (MB)**  
 Especifique la cantidad máxima de espacio recomendado por el Asistente para la optimización de motor de base de datos que deben utilizar las estructuras de diseño físico.  
  
 Si no se especifica ningún valor, el Asistente para la optimización de motor de base de datos asume el valor más pequeño de los límites de espacio siguientes:  
  
-   Tres veces el tamaño actual de los datos sin procesar, lo que incluye el tamaño total de los montones e índices clúster de las tablas de la base de datos.  
  
-   El espacio disponible en todas las unidades de disco adjuntas más el tamaño de los datos sin procesar.  
  
 **Incluir eventos de caché del plan de todas las bases de datos**  
 Especifique que los eventos de caché del plan de todas las bases de datos están analizados.  
  
 **Máximo de columnas por índice**  
 Especifique el número máximo de columnas que deben incluirse en los índices. El valor predeterminado es 1023.  
  
 **Todas las recomendaciones están sin conexión**  
 Se generan las mejores recomendaciones posibles, pero no se recomienda crear en línea ninguna estructura de diseño físico.  
  
 **Generar recomendaciones en línea si es posible**  
 Cuando cree instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] para implementar las recomendaciones, elija métodos que puedan implementarse con el servidor en línea, aun cuando haya un método sin conexión más rápido disponible.  
  
 **Generar solo recomendaciones en línea**  
 Solo se realizan recomendaciones que permiten que el servidor permanezca en línea.  
  
 **Detener el**  
 Indique la fecha y hora en que debe detenerse el Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 **Índices y vistas indizadas**  
 Si activa esta casilla, incluye recomendaciones para agregar clúster, índices no clúster y vistas indizadas.  
  
 **Vistas indizadas**  
 Solo se incluyen recomendaciones para agregar vistas indizadas. No se recomiendan los clúster y no clúster.  
  
 **Incluir índices filtrados**  
 Se incluyen recomendaciones para agregar índices filtrados. Esta opción está disponible si selecciona una de estas estructuras de diseño físico: **índices y vistas indexadas**, **índices** o **índices no agrupados**.  
  
 **Índices**  
 Solo se incluyen recomendaciones para agregar clúster y no clúster. No se recomiendan las vistas indizadas.  
  
 **Índices no clúster**  
 Solo se incluyen recomendaciones para los índices no clúster. No se recomiendan los índices clúster ni las vistas indizadas.  
  
 **Evaluar utilización solo de estructuras de diseño físico (PDS) existentes**  
 Evalúa la efectividad de los índices actuales, pero no recomienda índices adicionales ni vistas indizadas.  
  
 **No crear particiones**  
 No recomienda la creación de particiones.  
  
 **Particiones completas**  
 Se incluyen recomendaciones para crear particiones.  
  
 **Particiones alineadas**  
 Las nuevas particiones recomendadas se alinearán para facilitar su mantenimiento.  
  
 **No mantener ninguna PDS existente**  
 Recomienda la eliminación de índices, vistas y particiones existentes que no se necesiten. Si una estructura de diseño físico (PDS) existente es útil para la carga de trabajo, el Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] no recomienda quitarla.  
  
 **Mantener solo índices**  
 Mantiene todos los índices existentes, pero recomienda la eliminación de vistas indizadas y particiones innecesarias.  
  
 **Mantener todas las PDS existentes**  
 Mantiene todos los índices, vistas indizadas y particiones existentes.  
  
 **Mantener solo clúster**  
 Mantiene todos los clúster existentes, pero recomienda la eliminación de vistas indizadas, particiones e índices no clúster innecesarios.  
  
 **Mantener particiones alineadas**  
 Mantiene las estructuras de particiones que están alineadas, pero recomienda la eliminación de vistas indizadas, índices y particiones no alineadas innecesarios. Las particiones adicionales recomendadas se alinearán con el esquema de particiones actual.  
  
###### <a name="progress-tab-options"></a>Opciones de la pestaña Progreso  
 La pestaña **Progreso** del Asistente para la optimización de motor de base de datos aparece una vez que este asistente comienza el análisis de la carga de trabajo.  
  
 Si desea detener la sesión de optimización una vez iniciada, en el menú **Acciones** elija una de las siguientes opciones:  
  
-   **Detener análisis (con recomendaciones)** detiene la sesión de optimización y pregunta al usuario si quiere que el Asistente para la optimización de motor de base de datos genere recomendaciones basadas en el análisis realizado hasta este punto.  
  
-   **Detener análisis** detiene la sesión de optimización sin generar ninguna recomendación.  
  
 **Progreso de la optimización**  
 Indica el estado actual del progreso. Contiene el número de acciones realizadas y el número de mensajes de error, finalización correcta y advertencia que se reciben.  
  
 **Detalles**  
 Contiene un icono que indica el estado.  
  
 **Acción**  
 Muestra los pasos que se llevan a cabo.  
  
 **Estado**  
 Muestra el estado del paso de la acción.  
  
 **Mensaje**  
 Contiene los mensajes generados por los pasos de la acción.  
  
 **Registro de optimización**  
 Contiene información sobre esta sesión de optimización. Para imprimir este registro, haga clic con el botón derecho en él y, después, haga clic en **Imprimir**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y trabajar con la salida del Asistente para la optimización de motor de base de datos](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [Utilidad dta](../../tools/dta/dta-utility.md)    
 [Tutorial: Asistente para la optimización de motor de base de datos](../../tools/dta/tutorial-database-engine-tuning-advisor.md)
