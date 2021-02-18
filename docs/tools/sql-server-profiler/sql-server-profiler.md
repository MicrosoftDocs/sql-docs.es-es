---
title: SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Explore las características de SQL Server Profiler. Obtenga ayuda para solucionar problemas mediante esta herramienta para crear seguimientos y analizar y reproducir los resultados de seguimiento.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: profiler
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 05/01/2020
ms.openlocfilehash: 6208c880dd234a0e6a62f61e6f6ae87567213c12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354450"
---
# <a name="sql-server-profiler"></a>SQL Server Profiler

[!INCLUDE[sql-asdbmi](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] es una interfaz para crear y administrar seguimientos y analizar y reproducir resultados de seguimiento. Los eventos se guardan en un archivo de seguimiento que posteriormente se puede analizar o utilizar para reproducir una serie de pasos específicos cuando se diagnostica un problema.

> [!IMPORTANT]
> Seguimiento de SQL y [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] están en desuso. El espacio de nombres *Microsoft.SqlServer.Management.Trace* que contiene los objetos Trace y Replay de Microsoft SQL Server también están en desuso.
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]
> Use eventos extendidos en su lugar. Para más información sobre los [eventos extendidos](../../relational-databases/extended-events/extended-events.md), vea [Inicio rápido: Eventos extendidos en SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) y [Generador de perfiles XEvent de SSMS](../../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> Se admiten cargas de trabajo de Analysis Services en [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].

> [!NOTE]
> Al intentar conectarse a una base de datos de Azure SQL Database desde el generador de perfiles de SQL Server, se produce incorrectamente un mensaje de error engañoso como el siguiente:
>
> - Para ejecutar un seguimiento en SQL Server, debe ser miembro del rol fijo de servidor de administrador del sistema o tener el permiso ALTER TRACE.
>
> El mensaje debería haber explicado que el generador de perfiles de SQL Server no admite instancias de Azure SQL Database.

## <a name="where-is-the-profiler"></a>¿Dónde está Profiler?

Puede iniciar Profiler de diferentes maneras desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. [En este tema se enumeran las maneras de iniciar Profiler.](start-sql-server-profiler.md)

## <a name="capture-and-replay-trace-data"></a>Captura y reproducción de datos de seguimiento

En la siguiente tabla se muestran las características que se recomienda usar en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] para capturar y reproducir los datos de seguimiento.

||||
|-|-|-|
|**Característica\Carga de trabajo de destino**|**Motor relacional**|**Analysis Services**|  
|**Captura de seguimiento**|Interfaz gráfica de usuario de [Eventos extendidos](../../relational-databases/extended-events/extended-events.md) en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|  
|**Reproducción de seguimiento**|[Distributed Replay](../distributed-replay/sql-server-distributed-replay.md)|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|

## <a name="use-sql-server-profiler"></a>Uso de SQL Server Profiler

El [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] de Microsoft es una interfaz gráfica de usuario de Seguimiento de SQL que se usa para supervisar una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] o de Analysis Services. Puede capturar y guardar datos acerca de cada evento en un archivo o en una tabla para analizarlos posteriormente. Por ejemplo, puede supervisar un entorno de producción para ver qué procedimientos almacenados afectan negativamente al rendimiento al ejecutarse demasiado lentamente. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] se usa para actividades como las siguientes:

- Seguir los pasos de consultas con problemas para buscar la causa de los mismos.

- Buscar y diagnosticar consultas de ejecución lenta.

- Capturar la serie de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que ha causado un problema. El seguimiento guardado se puede utilizar después para replicar el problema en un servidor de prueba en el que se pueda diagnosticar el problema.

- Supervisar el rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para optimizar las cargas de trabajo. Para obtener información acerca de la optimización del diseño físico de bases de datos para las cargas de trabajo, vea [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).

- Establecer correlaciones entre contadores de rendimiento para diagnosticar problemas.

El [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] también es compatible con la auditoría de las acciones ejecutadas en instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La auditoría registra las acciones relacionadas con la seguridad para una revisión posterior por parte del administrador de seguridad.

## <a name="sql-server-profiler-concepts"></a>Conceptos de SQL Server Profiler

Para usar el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], debe comprender la terminología que describe cómo funciona la herramienta.

> [!NOTE]
> La comprensión de SQL Trace realmente ayuda al trabajar con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Para más información, consulte [SQL Trace](../../relational-databases/sql-trace/sql-trace.md).

### <a name="event"></a>Evento

Un evento es una acción generada dentro de una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Por ejemplo:  
  
- Conexiones, errores y desconexiones de inicio de sesión.    
- Instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] `SELECT`, `INSERT`, `UPDATE` y `DELETE`.
- Estado de lotes de RPC (llamada a procedimiento remoto).  
- Inicio o finalización de procedimientos almacenados.  
- Inicio o finalización de instrucciones incluidas en procedimientos almacenados.  
- Inicio o finalización de lotes SQL.  
- Errores escritos en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
- Bloqueos adquiridos o liberados en objetos de base de datos.  
- Cursores abiertos.  
- Comprobaciones de permisos de seguridad.  
  
Todos los datos generados por un evento se muestran en el seguimiento en una sola fila. Esta fila está intersectada por columnas de datos que describen el evento de forma detallada.  

### <a name="eventclass"></a>EventClass

Una clase de evento es un tipo de evento del cual se puede realizar un seguimiento. La clase de evento contiene todos los datos que puede comunicar un evento. Por ejemplo:

- **SQL:BatchCompleted**
- **Audit Login**
- **Audit Logout**
- **Lock: Acquired**
- **Lock: Released**

### <a name="eventcategory"></a>EventCategory

Una categoría de eventos define cómo se agrupan los eventos en el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Por ejemplo, todas las clases de eventos de bloqueo se agrupan dentro de la categoría de eventos **Bloqueos** . Sin embargo, las categorías de eventos solo existen en el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Este término no refleja cómo se agrupan los eventos del motor.

### <a name="datacolumn"></a>DataColumn

Una columna de datos es un atributo de una clase de evento capturada en el seguimiento. Como la clase de evento determina el tipo de datos que se pueden recopilar, no se aplicarán todas las columnas de datos a todas las clases de evento. Por ejemplo, en un seguimiento que capture la clase de evento **Lock: Acquired**, la columna de datos **BinaryData** contiene el valor del identificador o la fila de la página bloqueada, pero la columna de datos **IntegerData** no contiene ningún valor porque no es aplicable a la clase de evento que se captura.

### <a name="template"></a>Plantilla

Una plantilla define la configuración predeterminada de un seguimiento. En concreto, incluye las clases de evento que desea supervisar con el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Por ejemplo, puede crear una plantilla que especifique los eventos, las columnas de datos y los filtros que desea utilizar. Las plantillas no se ejecutan, sino que se guardan como archivos con la extensión .tdf. Una vez guardada, una plantilla controla los datos del seguimiento que se capturan cuando se inicia un seguimiento basado en la plantilla en cuestión.

### <a name="trace"></a>Seguimiento

Un seguimiento captura datos basándose en clases de evento, columnas de datos y filtros seleccionados. Por ejemplo, puede crear un seguimiento para supervisar errores de excepción. Para ello, seleccione la clase de evento **Exception** y las columnas de datos **Error**, **State** y **Severity** . Deben recopilarse los datos de estas tres columnas para que los resultados del seguimiento proporcionen datos con significado. Una vez hecho esto, puede ejecutar un seguimiento configurado de esta forma y recopilar datos de cualquier evento **Exception** que se produzca en el servidor. Los datos de seguimiento se pueden guardar o utilizar inmediatamente para el análisis. Los seguimientos se pueden volver a reproducir posteriormente, aunque ciertos eventos, como los eventos **Exception** , nunca se vuelven a reproducir. También puede guardar el seguimiento como plantilla para crear seguimientos parecidos en el futuro.  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ofrece dos formas realizar un seguimiento de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: puede hacerlo con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] o con procedimientos almacenados del sistema.  

### <a name="filter"></a>Filter

Al crear un seguimiento o una plantilla, puede definir criterios para filtrar los datos recopilados por el evento. Para que los seguimientos no sean demasiado grandes, puede filtrarlos de forma que solo se recopile un subconjunto de los datos del evento. Por ejemplo, puede limitar los nombres de usuario de Microsoft Windows del seguimiento a usuarios específicos, con lo que reducirá los datos de salida.  

Si no se establece un filtro, se devolverán todos los eventos de las clases de eventos seleccionadas en el resultado del seguimiento.

## <a name="sql-server-profiler-tasks"></a>Tareas de SQL Server Profiler

|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Enumera las plantillas predefinidas que SQL Server proporciona para supervisar determinados tipos de eventos y los permisos necesarios para la reproducción de seguimientos.|[Plantillas y permisos de SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)|  
|Describe cómo ejecutar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler.|[Permisos necesarios para ejecutar SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)|  
|Describe cómo crear un seguimiento.|[Crear un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)|  
|Describe cómo especificar eventos y columnas de datos para un archivo de seguimiento.|[Especificar eventos y columnas de datos para un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)|  
|Describe cómo guardar los resultados de seguimiento en un archivo.|[Guardar los resultados de un seguimiento en un archivo &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)|  
|Describe cómo guardar los resultados de seguimiento en una tabla.|[Guardar los resultados de un seguimiento en una tabla &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)|  
|Describe cómo filtrar los eventos de un seguimiento.|[Filtrar eventos en un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md)|  
|Describe cómo ver la información de filtro.|[Ver información de un filtro &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/view-filter-information-sql-server-profiler.md)|  
|Describe cómo modificar un filtro.|[Modificar un filtro &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/modify-a-filter-sql-server-profiler.md)|  
|Describe cómo establecer un tamaño máximo de archivo para un archivo de seguimiento ([!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]).|[Establecer un tamaño máximo de archivo para un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-a-maximum-file-size-for-a-trace-file-sql-server-profiler.md)|  
|Describe cómo establecer un tamaño máximo de tabla para una tabla de seguimiento.|[Establecer un tamaño máximo de tabla para una tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-a-maximum-table-size-for-a-trace-table-sql-server-profiler.md)|  
|Describe cómo iniciar un seguimiento.|[Iniciar un seguimiento](../../tools/sql-server-profiler/start-a-trace.md)|  
|Describe cómo iniciar un seguimiento automáticamente después de conectarse a un servidor.|[Iniciar un seguimiento automáticamente después de conectarse a un servidor &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)|  
|Describe cómo filtrar eventos basándose en la hora de inicio del evento.|[Filtrar eventos basándose en la hora de inicio del evento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-based-on-the-event-start-time-sql-server-profiler.md)|  
|Describe cómo filtrar eventos basándose en la hora de finalización del evento.|[Filtrar eventos basándose en la hora de finalización del evento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-based-on-the-event-end-time-sql-server-profiler.md)|  
|Describe cómo filtrar los identificadores de proceso de servidor (SPID) en un seguimiento.|[Filtrar los Id. de proceso de servidor &#40;SPID&#41; en un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-server-process-ids-spids-in-a-trace-sql-server-profiler.md)|  
|Describe cómo pausar un seguimiento.|[Pausar un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)|  
|Describe cómo detener un seguimiento.|[Detener un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)|  
|Describe cómo ejecutar un seguimiento que se ha pausado o detenido.|[Ejecutar un seguimiento después de haberlo pausado o detenido &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)|  
|Describe cómo borrar una ventana de seguimiento.|[Borrar el contenido de una ventana de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)|  
|Describe cómo cerrar una ventana de seguimiento.|[Cerrar una ventana de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/close-a-trace-window-sql-server-profiler.md)|  
|Describe cómo establecer valores predeterminados de definición de seguimiento.|[Configurar los valores predeterminados de definición de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md)|  
|Describe cómo establecer valores predeterminados de presentación de seguimiento.|[Establecer los valores predeterminados de presentación de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-display-defaults-sql-server-profiler.md)|  
|Describe cómo abrir un archivo de seguimiento.|[Abrir un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)|  
|Describe cómo abrir una tabla de seguimiento.|[Abrir una tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)|  
|Describe cómo reproducir una tabla de seguimiento.|[Reproducir una tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)|  
|Describe cómo reproducir un archivo de seguimiento.|[Reproducir un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md)|  
|Describe cómo reproducir un solo evento cada vez.|[Reproducir un único evento cada vez &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-single-event-at-a-time-sql-server-profiler.md)|  
|Describe cómo reproducir hasta un punto de interrupción.|[Reproducir hasta un punto de interrupción &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)|  
|Describe cómo reproducir hasta un cursor.|[Reproducir hasta un cursor &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-to-a-cursor-sql-server-profiler.md)|  
|Describe cómo reproducir un script [!INCLUDE[tsql](../../includes/tsql-md.md)].|[Reproducir un script Transact-SQL &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-transact-sql-script-sql-server-profiler.md)|  
|Describe cómo crear una plantilla de seguimiento.|[Crear una plantilla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)|  
|Describe cómo modificar una plantilla de seguimiento.|[Modificar una plantilla de seguimiento &#40;SQL Server Profiler&#41;](./modify-trace-templates.md)|  
|Describe cómo establecer opciones globales de seguimiento.|[Establecer opciones globales de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-global-trace-options-sql-server-profiler.md)|  
|Describe cómo buscar un valor o una columna de datos durante una traza.|[Buscar un valor o una columna de datos durante la ejecución de un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/find-a-value-or-data-column-while-tracing-sql-server-profiler.md)|  
|Describe cómo derivar una plantilla a partir de un seguimiento en ejecución.|[Derivar una plantilla a partir de un seguimiento en ejecución &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)|  
|Describe cómo derivar una plantilla de un archivo de seguimiento o de una tabla de seguimiento.|[Derivar una plantilla a partir de un archivo o tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)|  
|Describe cómo crear un script [!INCLUDE[tsql](../../includes/tsql-md.md)] para ejecutar un seguimiento.|[Crear un script de Transact-SQL para ejecutar un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-transact-sql-script-for-running-a-trace-sql-server-profiler.md)|  
|Describe cómo exportar una plantilla de seguimiento.|[Exportar una plantilla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)|  
|Describe cómo importar una plantilla de seguimiento.|[Importar una plantilla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)|  
|Describe cómo extraer un script de un seguimiento.|[Extraer un script de un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/extract-a-script-from-a-trace-sql-server-profiler.md)|  
|Describe cómo establecer correlaciones de un seguimiento con datos del registro de rendimiento de Windows.|[Establecer correlaciones de un seguimiento con datos del registro de rendimiento de Windows &#40;SQL Server Profiler&#41;](./correlate-a-trace-with-windows-performance-log-data.md)|  
|Describe cómo organizar las columnas mostradas en un seguimiento.|[Organizar las columnas mostradas en un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/organize-columns-displayed-in-a-trace-sql-server-profiler.md)|  
|Describe cómo iniciar [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Iniciar SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md)|  
|Describe cómo guardar seguimientos y plantillas de seguimiento.|[Guardar seguimientos y plantillas de seguimiento](../../tools/sql-server-profiler/save-traces-and-trace-templates.md)|  
|Describe cómo modificar plantillas de seguimiento.|[Modificar plantillas de seguimiento](../../tools/sql-server-profiler/modify-trace-templates.md)|  
|Describe cómo establecer correlaciones de un seguimiento con datos del registro de rendimiento de Windows.|[Correlacionar un seguimiento con los datos del registro de rendimiento de Windows](../../tools/sql-server-profiler/correlate-a-trace-with-windows-performance-log-data.md)|  
|Describe cómo ver y analizar seguimientos con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Ver y analizar seguimientos con SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)|  
|Describe cómo analizar interbloqueos con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Analizar interbloqueos con SQL Server Profiler](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)|  
|Describe cómo analizar consultas con resultados de SHOWPLAN en SQL Server Profiler.|[Analizar consultas con resultados de SHOWPLAN en SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)|  
|Describe cómo filtrar seguimientos con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Filtrar seguimientos con SQL Server Profiler](../../tools/sql-server-profiler/filter-traces-with-sql-server-profiler.md)|  
|Describe cómo utilizar las características de reproducción de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Reproducir seguimientos](../../tools/sql-server-profiler/replay-traces.md)|  
|Muestra los temas de ayuda contextuales para [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[SQL Server Profiler (Ayuda F1)](../../tools/sql-server-profiler/sql-server-profiler-f1-help.md)|  
|Enumera los procedimientos almacenados del sistema que usa [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para supervisar el rendimiento y la actividad.|[Procedimientos almacenados de SQL Server Profiler &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|  

## <a name="see-also"></a>Consulte también

- [Bloqueos (categoría de eventos)](../../relational-databases/event-classes/locks-event-category.md)
- [Sesiones (categoría de eventos)](../../relational-databases/event-classes/sessions-event-category.md)
- [Procedimientos almacenados (categoría de eventos)](../../relational-databases/event-classes/stored-procedures-event-category.md)
- [TSQL (categoría de eventos)](../../relational-databases/event-classes/tsql-event-category.md)
- [Supervisión de la actividad y rendimiento del servidor](../../relational-databases/performance/server-performance-and-activity-monitoring.md)