---
title: Editor de consultas SSMS
description: Editor de consultas de SQL Server Management Studio (SSMS)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.query.advanced.f1
- sql13.swb.query.ansi.f1
- sql13.swb.query.general.f1
- sql13.swb.query.grid.f1
- sql13.swb.sqleditors.multiserverresultssettings
- sql13.swb.tsqlquery.f1
- sql13.swb.tsqlresults.f1
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], editor
- SQL Server Management Studio [SQL Server], Database Engine Query Editor
- SQL Server Management Studio [SQL Server], templates
- SQL Server Management Studio [SQL Server], query editor
- Query Editor [SQL Server Management Studio]
- Query Editor [Database Engine]
- Query Editor [Database Engine], Features
- Query Editor [Database Engine], Toolbar
- Query Editor [SQL Server Management Studio], full screen mode
- Query Editor [Database Engine], templates
- Query Editor [SQL Server Management Studio], about Query Editor
- Transact-SQL Editor See Query Editor [Database Engine]
- Database Engine Query Editor See Query Editor [Database Engine]
- Code Editor [SQL Server Management Studio], about Query Editor
- editors [SQL Server Management Studio], Database Engine Query Editor
- full screen mode [SQL Server Management Studio]
- writing scripts
- modifying scripts
- writing queries
- scripts [SQL Server], SQL Server Management Studio
- queries [SQL Server], SQL Server Management Studio
ms.assetid: 05cfae9b-96d5-4a35-a098-0bc3a548bcfc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019, contperf-fy21q1
ms.date: 08/28/2020
ms.openlocfilehash: 6ce879af451abb20e108e20b36fbe129024f4806
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355159"
---
# <a name="sql-server-management-studio-ssms-query-editor"></a>Editor de consultas de SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

En este artículo se explican las características y funciones del editor de consultas de SQL Server Management Studio (SSMS).

> [!Note]
> Si desea obtener información sobre cómo usar la Ayuda F1 de Transact-SQL (T-SQL), consulte la sección [Transact-SQL (Ayuda F1)](#transact-sql-f1-help).
>
> Si desea obtener información sobre las tareas que puede realizar con el editor, visite la sección [Tareas del editor](#editor-tasks).

Los editores de SSMS comparten una arquitectura típica. El editor de texto implementa el nivel base de funcionalidad y se puede usar como un editor básico para archivos de texto. Los otros editores, o los editores de consultas, amplían esta base de la funcionalidad mediante la inclusión de un servicio de lenguaje que define la sintaxis de uno de los lenguajes admitidos en SQL Server. Los editores de consultas también implementan niveles variables de compatibilidad con las características de editor como IntelliSense y la depuración. Los editores de consultas incluyen el editor de consultas del motor de base de datos para usarlo en la compilación de scripts que contienen instrucciones T-SQL y XQuery, el editor MDX del lenguaje MDX, el editor DMX para el lenguaje DMX y el editor XML/A para el lenguaje XML for Analysis.
Puede usar el editor de consultas para crear y ejecutar scripts que contengan instrucciones Transact-SQL.

![Nueva consulta](media/database-engine-query-editor-sql-server-management-studio/new-query.png)

## <a name="sql-editor-toolbar"></a>Barra de herramientas del Editor SQL

Cuando el Editor de consultas está abierto, la barra de herramientas del Editor SQL aparece con los botones siguientes.

También puede agregar la barra de herramientas del Editor SQL seleccionando el menú **Ver** y, a continuación, las opciones **Barras de herramientas** y **Editor SQL**. Si agrega la barra de herramientas del Editor SQL cuando no está abierta ninguna ventana del Editor de consultas, no habrá ningún botón disponible.

![Barra de herramientas del editor](media/database-engine-query-editor-sql-server-management-studio/editor-toolbar.png)

### <a name="connect-using-the-editor-toolbar"></a>Conexión mediante la barra de herramientas del editor

Se abre el cuadro de diálogo **[Conectar al servidor](connect-to-server-database-engine.md)** . Utilice este cuadro de diálogo para establecer una conexión a un servidor.

También puede conectarse a la base de datos mediante el [menú contextual](#connection-using-the-context-menu).

### <a name="change-connection-using-the-editor-toolbar"></a>Cambio de la conexión mediante la barra de herramientas del editor

Se abre el cuadro de diálogo **Conectar al servidor** . Use este cuadro de diálogo para establecer una [conexión a un servidor diferente](f1-help-for-server-connections-sql-server-management-studio.md).

También puede cambiar las conexiones mediante el [menú contextual](#connection-using-the-context-menu).

### <a name="available-databases-using-the-editor-toolbar"></a>Bases de datos disponibles mediante la barra de herramientas del editor

Cambia la conexión a una base de datos distinta del mismo servidor.

### <a name="execute-using-the-editor-toolbar"></a>Ejecución mediante la barra de herramientas del editor

Ejecuta el código seleccionado o, si no se ha seleccionado ningún código, ejecuta todo el código del editor de consultas.

También puede **ejecutar** una consulta con F5 o en el [menú contextual](#execute-using-the-context-menu).

### <a name="cancel-executing-query-using-the-editor-toolbar"></a>Cancelación de la ejecución de consultas mediante la barra de herramientas del editor

Envía una solicitud de cancelación al servidor. Algunas consultas no pueden cancelarse inmediatamente, sino que deben esperar a una condición de cancelación adecuada. Cuando se cancelan las transacciones, podrían producirse retrasos mientras se revierten.

También puede cancelar una consulta en ejecución si selecciona Alt + Interrumpir.

### <a name="parse-using-the-editor-toolbar"></a>Análisis mediante la barra de herramientas del editor

Comprueba la sintaxis del código seleccionado. Si no se ha seleccionado ningún código, comprueba la sintaxis de todo el código en la ventana del Editor de consultas.

También puede comprobar el código en el Editor de consultas con Ctrl + F5.

### <a name="display-estimated-execution-plan-using-the-editor-toolbar"></a>Visualización del plan de ejecución estimado mediante la barra de herramientas del editor

Solicita un plan de ejecución de consulta desde el procesador de consultas sin ejecutar la consulta y muestra el plan en la ventana **Plan de ejecución**. Este plan utiliza estadísticas de índice para calcular el número de filas que se prevé que van a devolverse en cada parte de la ejecución de la consulta. El plan de consultas real que se utiliza puede ser diferente del plan de ejecución calculado. Esto puedo ocurrir si el número de filas devueltas es distinto del calculado y el procesador de consultas cambia el plan para conseguir mayor eficacia.

También puede mostrar un plan de ejecución estimado con Ctrl + L o en el [menú contextual](#display-estimated-execution-plan-using-the-context-menu).

### <a name="query-options-using-the-editor-toolbar"></a>Opciones de consulta mediante la barra de herramientas del editor

Abre el cuadro de diálogo **Opciones de consulta** . Use este cuadro de diálogo para configurar las opciones predeterminadas de la ejecución de consultas y los resultados de las mismas.

También puede seleccionar **Opciones de consulta** en el [menú contextual](#query-options-using-the-context-menu).

### <a name="intellisense-enabled-using-the-editor-toolbar"></a>IntelliSense habilitado mediante la barra de herramientas del editor

Especifica si la funcionalidad de [IntelliSense](../scripting/configure-intellisense-sql-server-management-studio.md) está disponible en el editor de consultas del motor de base de datos. Esta opción está establecida de forma predeterminada.

También puede seleccionar **IntelliSense habilitado** si presiona Ctrl + B y luego Ctrl I, o bien en el [menú contextual](#intellisense-enabled-using-the-context-menu).

### <a name="include-actual-execution-plan-using-the-editor-toolbar"></a>Inclusión del plan de ejecución real mediante la barra de herramientas del editor

Ejecuta la consulta, devuelve los resultados de la consulta y usa el plan de ejecución para la consulta. Las consultas se muestran como un plan de consultas gráfico en la ventana **Plan de ejecución**.

También puede seleccionar **Incluir plan de ejecución real** con Ctrl + M o en el [menú contextual](#include-actual-execution-plan-using-the-context-menu).

### <a name="include-live-query-statistics-using-the-editor-toolbar"></a>Inclusión de estadísticas de consultas activas mediante la barra de herramientas del editor

Proporciona información en tiempo real sobre el proceso de ejecución de consultas a medida que los controles fluyen de un operador de plan de consulta a otro.

También puede seleccionar **Include Live Query Statistics** (Incluir estadísticas de consultas activas) en el [menú contextual](#include-live-query-statistics-using-the-context-menu).

### <a name="include-client-statistics-using-the-editor-toolbar"></a>Inclusión de estadísticas de cliente mediante la barra de herramientas del editor

Incluye una ventana **Estadísticas de clientes** que contiene estadísticas sobre la consulta y los paquetes de red, así como el tiempo transcurrido de la consulta.

También puede seleccionar **Include Live Query Statistics** (Incluir estadísticas de consultas activas) mediante Mayús + Alt + S o en el [menú contextual](#include-client-statistics-using-the-context-menu).

### <a name="results-to-text-using-the-editor-toolbar"></a>Resultados a texto mediante la barra de herramientas del editor

Devuelve los resultados de la consulta como texto en la ventana **Resultados** .

También puede devolver los resultados a texto con Ctrl + T o en el [menú contextual](#results-using-the-context-menu).

### <a name="results-to-grid-using-the-editor-toolbar"></a>Resultados a cuadrícula mediante la barra de herramientas del editor

Devuelve los resultados de la consulta como una o varias cuadrículas en la ventana **Resultados** . Esta opción está habilitada de forma predeterminada.

También puede devolver los resultados a texto con Ctrl + D o en el [menú contextual](#results-using-the-context-menu).

### <a name="results-to-file-using-the-editor-toolbar"></a>Resultados a archivo mediante la barra de herramientas del editor

Cuando se ejecuta la consulta, se abre el cuadro de diálogo **Guardar resultados** . En **Guardar en**, seleccione la carpeta en la que desea guardar el archivo. En **Nombre de archivo**, escriba el nombre del archivo y, luego, seleccione **Guardar** para guardar los resultados de la consulta como un archivo de **Informe** con la extensión .rpt. Para acceder a las opciones avanzadas, seleccione la flecha abajo en el botón **Guardar** y, luego, seleccione **Guardar con codificación**.

También puede devolver los resultados a texto con Ctrl + Mayús + F o en el [menú contextual](#results-using-the-context-menu).

### <a name="comment-out-the-selected-lines-using-the-editor-toolbar"></a>Conversión en comentario de las líneas seleccionadas mediante la barra de herramientas del editor

Convierte la línea actual en comentario agregando al principio de la línea un operador de comentario (--).

También puede convertir en comentario una línea con Ctrl + K y Ctrl + C.

### <a name="uncomment-the-selected-lines-using-the-editor-toolbar"></a>Eliminación de las marcas de comentario de las líneas seleccionadas mediante la barra de herramientas del editor

Convierte la línea actual en una instrucción de código activo quitando los operadores de comentario (--) del principio de la línea.

También puede quitar la marca de comentario de una línea si presiona Ctrl + K y, luego, Ctrl + U.

### <a name="decrease-indent-using-the-editor-toolbar"></a>Reducción de la sangría mediante la barra de herramientas del editor

Mueve el texto de la línea a la izquierda quitando los espacios en blanco al principio de la línea.

### <a name="increase-line-indent-using-the-editor-toolbar"></a>Aumento de la sangría de línea mediante la barra de herramientas del editor

Mueve el texto de la línea a la derecha agregando espacios en blanco al principio de la línea.

### <a name="specify-values-for-template-parameters-using-the-editor-toolbar"></a>Especificación de los valores de los parámetros de plantilla mediante la barra de herramientas del editor

Abre un cuadro de diálogo que puede utilizar para especificar los valores de los parámetros en los procedimientos almacenados y funciones.

## <a name="context-menu"></a>Menú contextual

Puede acceder al menú contextual si *hace clic con el botón derecho* en cualquier parte del Editor de consultas. Las opciones del menú contextual son similares a las de la barra de herramientas del Editor SQL. Con el menú contextual, verá las mismas opciones que **Conectar** y **Ejecutar**, pero también obtiene otras opciones, como **Insertar fragmento de código** y **Envolver con**.

![Opciones](media/database-engine-query-editor-sql-server-management-studio/context-menu.png)

### <a name="insert-snippet-using-the-context-menu"></a>Inserción de un fragmento de código mediante el menú contextual

Un [fragmento de código de T-SQL](../scripting/add-transact-sql-snippets.md) es una plantilla que se puede usar como un punto inicial al escribir nuevas instrucciones de Transact-SQL en el editor de consultas.

### <a name="surround-with-using-the-context-menu"></a>Envolver con mediante el menú contextual

Un fragmento de código "envolver con" es una plantilla que puede usar como punto de partida al encerrar un conjunto de instrucciones de Transact-SQL en un bloque BEGIN, IF o WHILE.

### <a name="connection-using-the-context-menu"></a>Conexión mediante el menú contextual

![Conexiones](media/database-engine-query-editor-sql-server-management-studio/context-menu-connections.png)

Hay más opciones de **Conexión** en el menú contextual en comparación con las opciones de la barra de herramientas de SSMS.

- **Conectar**: abre el cuadro de diálogo Conectar al servidor. Utilice este cuadro de diálogo para establecer una conexión a un servidor.

- **Desconectar**: desconecta el Editor de consultas actual del servidor.

- **Desconectar todas las consultas**: desconecta todas las conexiones de consulta.

- **Cambiar conexión**: abre el cuadro de diálogo Conectar al servidor. Utilice este cuadro de diálogo para establecer una conexión a un servidor diferente.

### <a name="open-server-in-object-explorer-using-the-context-menu"></a>Apertura del servidor en el Explorador de objetos mediante el menú contextual

El Explorador de objetos proporciona un interfaz jerárquica para ver y administrar los objetos de cada instancia de SQL Server. El panel Detalles del Explorador de objetos muestra una vista tabular de los objetos de instancia y la capacidad de buscar objetos específicos. Las funciones del Explorador de objetos varían ligeramente según el tipo de servidor, aunque, por lo general, incluyen características de desarrollo de bases de datos y características de administración para todo tipo de servidores.

### <a name="execute-using-the-context-menu"></a>Ejecución mediante el menú contextual

Ejecuta el código seleccionado o, si no se ha seleccionado ningún código, ejecuta todo el código del Editor de consultas.

### <a name="display-estimated-execution-plan-using-the-context-menu"></a>Visualización del plan de ejecución estimado mediante el menú contextual

Solicita un plan de ejecución de consulta desde el procesador de consultas sin ejecutar realmente la consulta y muestra el plan en la ventana **Plan de ejecución** . Este plan utiliza estadísticas de índice para calcular el número de filas que se prevé que van a devolverse en cada parte de la ejecución de la consulta. El plan de consultas real que se utiliza puede ser diferente del plan de ejecución calculado. Esto puede ocurrir si el número de filas que se devuelven es diferente del calculado y el procesador de consultas cambia el plan para obtener una mayor eficacia.

### <a name="intellisense-enabled-using-the-context-menu"></a>IntelliSense habilitado mediante el menú contextual

Especifica si la funcionalidad de IntelliSense está disponible en el editor de consultas del motor de base de datos. Esta opción está establecida de forma predeterminada.

### <a name="trace-query-in-sql-server-profiler-using-the-context-menu"></a>Seguimiento de consultas en SQL Server Profiler mediante el menú contextual

SQL Server Profiler es una interfaz para crear y administrar seguimientos y analizar y reproducir resultados de seguimiento. Los eventos se guardan en un archivo de seguimiento que posteriormente se puede analizar o utilizar para reproducir una serie de pasos específicos cuando se intenta diagnosticar un problema.

### <a name="analyze-query-in-database-engine-tuning-advisor-using-the-context-menu"></a>Análisis de consultas en el Asistente para la optimización de motor de base de datos mediante el menú contextual

El Asistente para la optimización de motor de base de datos (DTA) de Microsoft analiza las bases de datos y hace recomendaciones que puede usar para optimizar el rendimiento de las consultas. Use el Asistente para la optimización de motor de base de datos a fin de seleccionar y crear un conjunto óptimo de índices, vistas indexadas o particiones de tabla sin necesidad de conocer detalladamente la estructura de la base de datos ni el funcionamiento interno de SQL Server. Con DTA, puede realizar las siguientes tareas.

### <a name="design-query-in-editor-using-the-context-menu"></a>Diseño de consultas en el editor mediante el menú contextual

El Diseñador de consultas y vistas se ejecuta cuando se abre la definición de una vista, cuando se muestran los resultados de una consulta o una vista, o cuando se crea o se abre una consulta.

### <a name="include-actual-execution-plan-using-the-context-menu"></a>Inclusión del plan de ejecución real mediante el menú contextual

Ejecuta la consulta, devuelve los resultados de la consulta y usa el plan de ejecución para la consulta. Las consultas se muestran como un plan de consultas gráfico en la ventana **Plan de ejecución**.

### <a name="include-live-query-statistics-using-the-context-menu"></a>Inclusión de estadísticas de consultas activas mediante el menú contextual

Proporciona información en tiempo real sobre el proceso de ejecución de consultas a medida que los controles fluyen de un operador de plan de consulta a otro.

### <a name="include-client-statistics-using-the-context-menu"></a>Inclusión de estadísticas de cliente mediante el menú contextual

Incluye una ventana **Estadísticas de clientes** que contiene estadísticas sobre la consulta y los paquetes de red, así como el tiempo transcurrido de la consulta.

### <a name="results-using-the-context-menu"></a>Resultados mediante el menú contextual

![Opciones de resultados](media/database-engine-query-editor-sql-server-management-studio/context-menu-results.png)

Puede seleccionar cualquiera de las opciones de *Resultado* que prefiera en el menú contextual.

- **Resultados a texto**: devuelve los resultados de la consulta como texto en la ventana **Resultados**.

- **Resultados a cuadrícula**: devuelve los resultados de la consulta como una o varias cuadrículas en la ventana **Resultados**.

- **Resultados a archivo**: cuando se ejecuta la consulta, se abre el cuadro de diálogo **Guardar resultados**. En **Guardar en**, seleccione la carpeta en la que desea guardar el archivo. En **Nombre de archivo**, escriba el nombre del archivo y, luego, seleccione **Guardar** para guardar los resultados de la consulta como un archivo de **Informe** con la extensión .rpt. Para acceder a las opciones avanzadas, seleccione la flecha abajo en el botón **Guardar** y, luego, seleccione **Guardar con codificación**.

### <a name="properties-window-using-the-context-menu"></a>Ventana de propiedades mediante el menú contextual

La [ventana de propiedades](../menu-help/properties-window-f1-help-management-studio.md) describe el estado de un elemento en SQL Server Management Studio, como una conexión o un operador de plan de presentación, y proporciona información sobre objetos de la base de datos, como tablas, vistas y diseñadores.

Use la ventana Propiedades para ver las propiedades de la conexión actual. Muchas propiedades son de solo lectura en la ventana de propiedades, aunque se pueden cambiar en otra parte de Management Studio. Por ejemplo, la propiedad Database de una consulta es de solo lectura en la ventana Propiedades, aunque se puede cambiar en la barra de herramientas.

### <a name="query-options-using-the-context-menu"></a>Opciones de consulta mediante el menú contextual

Abre el cuadro de diálogo **Opciones de consulta** . Use este cuadro de diálogo para configurar las opciones predeterminadas de la ejecución de consultas y los resultados de las mismas.

## <a name="transact-sql-f1-help"></a>Transact-SQL (Ayuda F1)

El Editor de consultas admite la vinculación al tema de referencia de una instrucción específica de Transact-SQL al seleccionar F1. Para ello, resalte el nombre de una instrucción Transact-SQL y presione F1. El motor de búsqueda de ayuda busca entonces un tema que tenga un atributo de ayuda F1 que coincida con la cadena resaltada.

Si el motor de búsqueda de ayuda no encuentra un tema con una palabra clave de ayuda F1 que coincida exactamente con la cadena resaltada, se mostrará este tema. En ese caso, hay dos métodos para encontrar la ayuda que busca:

- Copiar y pegar la cadena del editor que resaltó en la pestaña de búsqueda de los Libros en pantalla de SQL Server y realizar una búsqueda.

- Resaltar solo la parte de la instrucción Transact-SQL que sea más probable que coincida con una palabra clave de Ayuda F1 aplicada a un tema y vuelva a presionar F1. El motor de búsqueda requiere una correspondencia exacta entre la cadena resaltada y una palabra clave de Ayuda F1 asignada a un tema. Si la cadena resaltada contiene elementos únicos de su entorno, como nombres de columna o parámetro, el motor de búsqueda no obtendrá una coincidencia. Entre los ejemplos de cadenas que se van a resaltar se incluyen los siguientes:

  - El nombre de una instrucción Transact-SQL, como SELECT, CREATE DATABASE o BEGIN TRANSACTION.

  - El nombre de una función integrada, como SERVERPROPERTY o @@VERSION.

  - El nombre de una tabla de procedimiento almacenado del sistema o de las vistas, como sys.data_spaces o sp_tableoption.

## <a name="editor-tasks"></a>Tareas del editor

| Descripción de la tarea | Tema |
|------------------|-------|
| Describe las distintas formas en que puede abrir los editores en SSMS.| [Abrir un editor](../scripting/open-an-editor-sql-server-management-studio.md) |
| Configurar las opciones de los diferentes editores, como la numeración de líneas y las opciones de IntelliSense. | [Configurar editores](../scripting/configure-editors-sql-server-management-studio.md) |
| Administrar el modo de vista, como el ajuste de línea, la división de una ventana o las tabulaciones.| [Administrar el editor y el modo de vista](../scripting/manage-the-editor-and-view-mode.md) |
| Establecer las opciones de formato, como el texto oculto o la sangría. | [Administrar formato de código](../scripting/manage-code-formatting.md) |
| Navegar por el texto en una ventana del editor mediante características tales como la búsqueda incremental o el desplazamiento a una determinada parte. | [Navegar por código y texto](../scripting/navigate-code-and-text.md) |
| Establecer las opciones de codificación de color para los distintos tipos de sintaxis, lo que facilita la lectura de instrucciones complejas. | [Codificación de colores en el Editor de consultas](../scripting/color-coding-in-query-editors.md) |
| Arrastrar el texto desde una ubicación en un script y colocarlo en una nueva ubicación.| [Arrastrar y colocar texto](../scripting/drag-and-drop-text.md) |
| Establecer los marcadores para encontrar más fácilmente fragmentos de código importantes. | [Administrar marcadores](../scripting/manage-bookmarks.md) |
| Imprimir los scripts o los resultados en una ventana o cuadrícula.| [Imprimir código y resultados](../scripting/print-code-and-results.md) |
| Ver y usar las características básicas del editor de consultas de MDX. | [Crear scripts de Analysis Services](/analysis-services/instances/create-analysis-services-scripts-in-management-studio) |
| Ver y usar las características básicas del editor de consultas DMX. | [Crear una consulta DMX](/analysis-services/data-mining/create-a-dmx-query-in-sql-server-management-studio) |
| Ver y usar las características básicas del editor XML/A. | [Editor XML](../scripting/xml-editor-sql-server-management-studio.md) |
| Usar las características sqlcmd en el editor de consultas del motor de base de datos.| [Editar scripts SQLCMD](../scripting/edit-sqlcmd-scripts-with-query-editor.md) |
| Usar los fragmentos de código del editor de consultas del motor de base de datos. Los fragmentos son plantillas para instrucciones o bloques que se usan habitualmente, y se pueden personalizar o ampliar para incluir fragmentos específicos del sitio.| [Fragmentos de código de T-SQL](../scripting/add-transact-sql-snippets.md) |
| Usar el depurador de Transact\-SQL para recorrer el código y consultar la información de depuración, como los valores de variables y parámetros.| [Depurador de T-SQL](../scripting/transact-sql-debugger.md) |

## <a name="see-also"></a>Consulte también

- [Personalizar los menús y los métodos abreviados de teclado](../customize-menus-and-shortcut-keys.md)
- [Métodos abreviados de teclado de SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)