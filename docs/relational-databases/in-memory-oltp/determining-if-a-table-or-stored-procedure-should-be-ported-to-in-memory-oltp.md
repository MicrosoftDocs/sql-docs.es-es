---
title: ¿Debería una tabla o un procedimiento almacenado pasarse a OLTP en memoria?
description: Utilice el informe de análisis de rendimiento de transacciones de SQL Server Management Studio para evaluar si OLTP en memoria mejorará el rendimiento de la aplicación de base de datos.
ms.custom: seo-dt-2019
ms.date: 08/02/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- Analyze, Migrate, Report
- AMR
ms.assetid: c1ef96f1-290d-4952-8369-2f49f27afee2
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 48d4d4b9602049c0c09e9e60ac8991c5c9a74154
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049262"
---
# <a name="determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp"></a>Determinar si una tabla o un procedimiento almacenado se debe pasar a OLTP en memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  El informe de análisis del rendimiento de las transacciones de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] permite evaluar si OLTP en memoria mejorará el rendimiento de la aplicación de base de datos. En el informe también se indicará cuánto trabajo debe hacer para habilitar OLTP en memoria en la aplicación. Después de identificar una tabla basada en disco para convertirla a OLTP en memoria, puede usar el [Asistente de optimización de memoria](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md)para que le ayude a migrar la tabla. De manera similar, el [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md) le permitirá convertir un procedimiento almacenado en un procedimiento almacenado compilado de forma nativa. Para obtener más información sobre las metodologías de migración, vea [OLTP en memoria: patrones de carga de trabajo comunes y consideraciones sobre la migración](/previous-versions/dn673538(v=msdn.10)).  
  
 El informe de análisis del rendimiento de las transacciones se ejecuta directamente en la base de datos de producción, o bien en una base de datos de prueba con una carga de trabajo activa similar a la carga de trabajo de producción.  
  
 Los asesores de informes y migración lo ayudarán a realizar las tareas siguientes:  
  
-   Analizar la carga de trabajo para determinar las zonas activas donde OLTP en memoria podría contribuir a mejorar el rendimiento. El informe de análisis de rendimiento de las transacciones recomienda las tablas y los procedimientos almacenados que se beneficiarían de la conversión a OLTP en memoria.  
  
-   Le ayuda a planear y ejecutar la migración a OLTP en memoria. La migración de una tabla basada en disco a una tabla optimizada para memoria puede llevar mucho tiempo. El Asesor de optimización para memoria permite identificar las incompatibilidades que debe quitar de la tabla antes de migrar la tabla a OLTP en memoria. El Asesor de optimización para memoria permite también comprender el impacto que la migración de una tabla a una tabla optimizada para memoria tendrá sobre su aplicación.  
  
     Puede ver si la aplicación se beneficiaría de OLTP en memoria, si desea planear la migración a OLTP en memoria y siempre que migre algunas de sus tablas y procedimientos almacenados a OLTP en memoria.  
  
    > [!IMPORTANT]  
    >  El rendimiento de un sistema de base de datos depende de diversos de factores, no todos los cuales puede observar y medir el recopilador de rendimiento de las transacciones. Por consiguiente, el informe de análisis del rendimiento de las transacciones no garantiza que las mejoras reales en el rendimiento coincidirán con las predicciones, en el caso de que las haya.  
  
 El informe de análisis de rendimiento de transacción y los asesores de migración se instalan como parte de SQL Server Management Studio (SSMS) cuando se selecciona **Herramientas de administración - Básica** o **Herramientas de administración - Avanzada** al instalar [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], o bien al [descargar SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).    
  
## <a name="transaction-performance-analysis-reports"></a>Informes de análisis del rendimiento de las transacciones  
 Puede generar informes de análisis del rendimiento de las transacciones en **Explorador de objetos** haciendo clic con el botón derecho en la base de datos, seleccionando **Informes** y, después, **Informes estándar** e **Información general de análisis del rendimiento de las transacciones**. La base de datos debe tener una carga de trabajo activa o haber ejecutado una recientemente para generar un informe de análisis significativo.  
  
### <a name="tables"></a>Tablas
  
 El informe de detalles de una tabla consta de tres secciones:  
  
-   Sección de estadísticas de recorrido  
  
     Esta sección incluye una única tabla que muestra las estadísticas recopiladas acerca de los recorridos en la tabla de base de datos. Las columnas son:  
  
    -   Porcentaje de accesos en total. El porcentaje de recorridos y de búsquedas en esta tabla con respecto a la actividad de toda la base de datos. Cuanto mayor sea el porcentaje, más se usa la tabla en comparación con otras de la base de datos.  
  
    -   Estadísticas de búsqueda y de recorrido de intervalos. Esta columna registra el número de búsquedas de puntos y de recorridos de intervalo (recorridos de índice y recorridos de tabla) realizados en la tabla durante la generación de perfiles. El promedio por transacción es una estimación.  
    
-   Sección de estadísticas de contención  
  
     Esta sección incluye una tabla que muestra la contención en la tabla de base de datos. Para obtener más información sobre los bloqueos y los bloqueos temporales de la base de datos, consulte Locking Architecture (Arquitectura de bloqueo). Estas son sus columnas:  
  
    -   Porcentaje de esperas en total. El porcentaje de esperas por bloqueos y bloqueos temporales en esta tabla de base de datos en comparación con la actividad de la base de datos. Cuanto mayor sea el porcentaje, más se usa la tabla en comparación con otras de la base de datos.  
  
    -   Estadísticas de bloqueos temporales. Estas columnas registran el número de esperas por bloqueos temporales para las consultas que implican a esta tabla. Para obtener información sobre los bloqueos temporales, consulte Latching (Bloqueos temporales). Cuanto mayor sea este número, más contención por bloqueos temporales hay en la tabla.  
  
    -   Estadísticas de bloqueos. Este grupo de columnas registra el número de esperas y de adquisiciones de bloqueos de página para las consultas de esta tabla. Para obtener más información sobre los bloqueos, consulte Understanding Locking in SQL Server (Descripción de los bloqueos en SQL Server). Cuantas más esperas hay, más contención por bloqueos se producen en la tabla.  
  
-   Sección de dificultades de migración  
  
     Esta sección incluye una tabla que muestra las dificultades de convertir esta tabla de base de datos en una tabla optimizada para memoria. Una clasificación de mayor dificultad indica que convertir la tabla es más difícil. Para ver información sobre cómo convertir esta tabla de base de datos, utilice el Asesor de optimización de memoria.  
  
Las estadísticas de contención del informe de detalles de la tabla se recopilan y agregan desde sys.dm_db_index_operational_stats (Transact-SQL).  

### <a name="stored-procedures"></a>Procedimientos almacenados

 Un procedimiento almacenado con un proporción alta de tiempo de CPU/tiempo transcurrido es un candidato a la migración. El informe muestra todas las referencias de tabla, ya que los procedimientos almacenados compilados de forma nativa solo pueden hacer referencia a tablas optimizadas para memoria, que pueden incrementar el costo de la migración.  
  
 El informe de detalles de un procedimiento almacenado consta de dos secciones:  
  
-   Sección de estadísticas de ejecución  
  
     Esta sección incluye una tabla en la que se muestran las estadísticas recopiladas sobre las ejecuciones del procedimiento almacenado. Estas son sus columnas:  
  
    -   Tiempo en caché. El tiempo que se almacena en caché este plan de ejecución. Si el procedimiento almacenado se sale de la memoria caché de planes y vuelve a entrar, habrá datos para cada caché.  
  
    -   Tiempo total de CPU. El tiempo total de CPU que el procedimiento almacenado usó durante la generación de perfiles. Cuanto mayor es este número, más CPU usó el procedimiento almacenado.  
  
    -   Tiempo total de ejecución. El tiempo total de ejecución que el procedimiento almacenado usó durante la generación de perfiles. Cuanto más alta es la diferencia entre este número y el tiempo de CPU, menos eficiente es el procedimiento almacenado en el uso de la CPU.  
  
    -   Total de errores de caché. El número de errores de caché (lecturas del almacenamiento físico) que se debe a las ejecuciones del procedimiento almacenado durante la generación de perfiles.  
  
    -   Recuento de ejecución. El número de veces que este procedimiento almacenado se ha ejecutado durante la generación de perfiles.  
  
-   Sección de referencias de tabla  
  
     Esta sección incluye una tabla que muestra las tablas a las que este procedimiento almacenado hace referencia. Antes de convertir el procedimiento almacenado en un procedimiento almacenado compilado de forma nativa, todas estas tablas deben convertirse en tablas optimizadas para memoria y deben permanecer en el mismo servidor y base de datos.  
  
 Las estadísticas de ejecución del informe de detalles del procedimiento almacenado se recopilan y agregan desde sys.dm_exec_procedure_stats (Transact-SQL). Las referencias se obtienen de sys.sql_expression_dependencies (Transact-SQL).  
  
 Para ver información sobre cómo convertir un procedimiento almacenado en un procedimiento almacenado compilado de forma nativa, utilice el Asistente para compilación nativa.  
  
## <a name="generating-in-memory-oltp-migration-checklists"></a>Generación de listas de comprobación de migración de OLTP en memoria  
 Las listas de comprobación de migración identifican las características de procedimiento almacenado que no son compatibles con las tablas optimizadas en memoria o los procedimientos almacenados compilados de forma nativa o tabla. El Asesor de optimización de memoria y el Asistente para compilación nativa pueden generar una lista de comprobación de una sola tabla basada en disco o un procedimiento almacenado de T-SQL interpretado. También se pueden generar listas de comprobación de varias tablas de migración y procedimientos almacenados de una base de datos.  
  
 Puede generar una lista de comprobación de migración en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usando el comando **Generar listas de comprobación de migración de OLTP en memoria** o a través de PowerShell.  
  
**Pasos para generar una lista de comprobación de migración mediante el comando de la interfaz de usuario**  
  
1.  En **Explorador de objetos**, haga clic con el botón derecho en una base de datos que no sea la del sistema, en **Tareas** y, después, en **Generar listas de comprobación de migración de OLTP en memoria**.  
  
2.  En el cuadro de diálogo Generar listas de comprobación de migración de OLTP en memoria, haga clic en Siguiente para ir a la página **Configurar opciones de generación de lista de comprobación** . En esta página, realice lo siguiente:  
  
    1.  Escriba una ruta de carpeta en el cuadro **Save checklist to** (Guardar lista de comprobación en).  
  
    2.  Compruebe que la opción **Generar listas de comprobación para tablas y procedimientos almacenados específicos** está seleccionada.  
  
    3.  Expanda los nodos **Tabla** y **Procedimiento almacenado** del cuadro de sección.  
  
    4.  Seleccione unos pocos objetos en el cuadro de selección.  
  
3.  Haga clic en **Siguiente** y confirme que la lista de tareas coincide con la configuración de la página **Configure Checklist Generation Options** (Configurar opciones de generación de lista de comprobación).  
  
4.  Haga clic en **Finalizar** y, después, confirme que se han generado los informes de lista de comprobación de migración solo para los objetos seleccionados.  

 Puede comprobar la precisión de los informes comparándolos con los generados mediante la herramienta Asesor de optimización de memoria y el Asistente para compilación nativa. Para obtener más información, consulte [Memory Optimization Advisor](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) y [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md).  
  
**Pasos para generar una lista de comprobación de migración mediante SQL Server PowerShell**  
  
1.  En **Explorador de objetos**, haga clic en una base de datos y, luego, en **Iniciar PowerShell**. Compruebe que aparece el siguiente aviso.  
  
    ```  
    PS SQLSERVER: \SQL\{Instance Name}\DEFAULT\Databases\{two-part DB Name}>  
    ```  
  
2.  Escriba el comando siguiente:  
  
    ```  
    Save-SqlMigrationReport -FolderPath "<folder_path>"  
    ```  
  
3.  Compruebe lo siguiente:  
  
    -   Se ha creado la ruta de acceso de la carpeta, si todavía no existe.  
  
    -   El informe de lista de comprobación de migración se ha generado para todas las tablas y los procedimientos almacenados de la base de datos y el informe se encuentra en la ubicación especificada mediante ruta_carpeta.  
  
**Pasos para generar una lista de comprobación de migración con Windows PowerShell**  
  
1.  Inicie una sesión de Windows PowerShell con privilegios elevados.  
  
2.  Escriba los comandos siguientes. El objeto puede ser una tabla o un procedimiento almacenado.  
  
    ```  
    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SMO')  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -FolderPath "<folder_path1>"  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -Object <object_name> -FolderPath "<folder_path2>"  
  
    ```  
  
3.  Compruebe lo siguiente:  
  
    -   Se ha generado un informe de lista de comprobación de migración para todas las tablas y los procedimientos almacenados de la base de datos y el informe se encuentra en la ubicación especificada mediante folder_path.  
  
    -   Un informe de lista de comprobación de migración para <nombre_objeto> es el único informe que hay en la ubicación especificada por folder_path2.  
  
## <a name="see-also"></a>Consulte también  
 [Migrar a OLTP en memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
