---
description: Destino de Oracle
title: Destino de Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3b28d420d66e55565c882c9fa2ae41f86e1c0764
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343229"
---
# <a name="oracle-destination"></a>Destino de Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

El destino de Oracle carga datos de manera masiva en Oracle Database.

El destino usa el Administrador de conexiones de Oracle para conectarse a un origen de datos. Para más información, consulte el artículo sobre el [Administrador de conexiones de Oracle](oracle-connection-manager.md).

Un destino de Oracle incluye asignaciones entre las columnas de entrada y las columnas en el origen de datos de destino. No es necesario asignar columnas de entrada a todas las columnas de destino, pero, según las propiedades de las columnas de destino, pueden producirse errores si no se asignan columnas de entrada a las columnas de destino. Por ejemplo, si una columna de destino no permite valores NULL, se debe asignar una columna de entrada a esa columna. Además, si los datos de entrada no son compatibles con el tipo de columna de destino, se produce un error en tiempo de ejecución. En función de la configuración de comportamiento del error, se omitirá el error, se generará un error o la fila redirigirá a la salida de error.

El destino de Oracle tiene una entrada normal y una salida de error.

Las columnas de tipos de datos no compatibles se eliminan de las columnas con una advertencia antes de la asignación.
Para más información, consulte [Compatibilidad con tipos de datos](oracle-data-type-support.md).

## <a name="load-options"></a>Opciones de carga

Se admiten dos modos de carga de acceso. El modo se puede establecer en el [Editor de destino de Oracle (página Administrador de conexiones)](#oracle-destination-editor-connection-manager-page). Los dos modos son:

- Carga por lotes: este modo consiste en cargar los datos por lotes en una tabla de Oracle y todo el lote se inserta en la misma transacción.
Para detalles sobre cómo configurar este modo, consulte [Editor de destino de Oracle (página Administrador de conexiones)](#oracle-destination-editor-connection-manager-page) y [Propiedades personalizadas del destino de Oracle](#oracle-destination-custom-properties).

- Carga rápida mediante ruta directa: este modo consiste en usar el modo de ruta directa del controlador para cargar una tabla de Oracle. Existen restricciones cuando se usa este modo, consulte la documentación de Oracle para más detalles.  
Para detalles sobre cómo configurar este modo, consulte [Editor de destino de Oracle (página Administrador de conexiones)](#oracle-destination-editor-connection-manager-page) y [Propiedades personalizadas del destino de Oracle](#oracle-destination-custom-properties).

## <a name="error-handling"></a>Control de errores

El destino de Oracle tiene una salida de error. La salida de error del componente incluye las columnas de salida siguientes:

- **Código de error**: un número que representa el tipo de error del error actual. El código de error puede ser de:
    - Servidor de Oracle. Consulte la descripción detallada del error en la documentación de Oracle Database.
    - Entorno de ejecución de SSIS. Para obtener una lista de códigos de errores de SSIS, vea la referencia Código de errores y mensajes de SSIS.
- **Columna de error**: el número de la columna de origen que genera los errores de conversión.

- **Columnas de datos de error**: los datos que generan el error.

Los tipos de errores de salida durante el proceso de carga compatibles son: conversión de datos, truncamiento, infracción de restricción, etc. Consulte [Editor de destino de Oracle (página Salida de error)](#oracle-destination-editor-error-output-page).

La propiedad **Número máximo de errores (MaxErrors)** establece el número máximo de errores que se pueden generar. La ejecución se detiene y devuelve errores cuando se alcanza el número máximo y solo se incluyen en la tabla de destino los registros de ejecución antes de que se alcance el número máximo de errores. Consulte [Editor de destino de Oracle (página Administrador de conexiones)](#oracle-destination-editor-connection-manager-page) para ver los detalles de la configuración.

## <a name="parallelism"></a>Paralelismo

En el modo de carga por lotes, no hay ninguna restricción en la configuración de la ejecución en paralelo, pero el rendimiento podría verse afectado por el mecanismo de bloqueo de registros estándar. La cantidad de pérdida de rendimiento depende de la organización de los datos y las tablas.

En el protocolo de ruta directa (carga rápida), solo se puede configurar un destino de Oracle para que se ejecute en la misma tabla al mismo tiempo, pero se puede usar el modo paralelo.

Una ruta directa paralela permite varias cargas de rutas directas, con las que se pueden configurar varios destinos de Oracle para ejecutarse de manera simultánea en la misma tabla al mismo tiempo. Oracle no bloquea la tabla de destino exclusivamente para usarla en la sesión de carga rápida, lo que permite ejecutar componentes adicionales de destino de carga rápida para cargar la misma tabla de destino en paralelo.
La ruta directa paralela es más restrictiva y todo uso de paralelismo debe planearse de antemano.

No hay ninguna razón para usar una sesión paralela única.

Consulte la documentación de Oracle relativa a las restricciones cuando use las cargas de rutas directas paralelas.

Para más información, consulte [Propiedades personalizadas del destino de Oracle](#oracle-destination-custom-properties).

## <a name="troubleshooting-the-oracle-destination"></a>Solución de problemas del destino de Oracle

Puede registrar las llamadas ODBC que el origen de Oracle hace a los orígenes de datos de Oracle para solucionar problemas de la exportación de datos. Para registrar las llamadas ODBC que el origen de Oracle hace a los orígenes de datos de Oracle, habilite el seguimiento del administrador de controladores ODBC. Para obtener más información, vea la documentación de Microsoft sobre *cómo generar un seguimiento ODBC con el administrador de orígenes de datos.*

## <a name="oracle-destination-custom-properties"></a>Propiedades personalizadas del destino de Oracle

En la tabla siguiente se describen las propiedades personalizadas del destino de Oracle. Todas las propiedades son de lectura y escritura.

|Nombre de propiedad|Tipo de datos|Descripción|Modo de carga|
|:-|:-|:-|:-|
|BatchSize|Entero|Tamaño de lote para la carga masiva. Es el número de filas cargadas como un lote.|Solo se usa en el modo por lotes.|
|DefaultCodePage|Entero|La página de códigos que se usa cuando el origen de datos no tiene información de página de códigos. <br>**Nota**: Esta propiedad solo se establece mediante el **Editor avanzado**.|Se usa para ambos modos.|
|FastLoad|Boolean|Indica si se usa la carga rápida. El valor predeterminado es **false**. También se puede establecer en el [Editor del destino de Oracle (página Administrador de conexiones)](#oracle-destination-editor-connection-manager-page). |Se usa para ambos modos.|
|MaxErrors|Entero|La cantidad de errores que pueden ocurrir antes de que se detenga el flujo de datos. El valor predeterminado es **0**, lo que significa que no hay ningún límite de número de errores.<br> Si está seleccionada la opción **Redirigir fila** en la página **Control de errores**. Antes de que se alcance el límite de número de errores, todos los errores se devuelven en la salida de error. Para más información, consulte [Control de errores](#error-handling).|Solo se usa en el modo de carga rápida.|
|NoLogging|Boolean|Si el registro de base de datos está deshabilitado. El valor predeterminado es **False**, lo que significa que el registro está habilitado.|Se usa para ambos modos.|
|Paralelo|Boolean|Si se permite la carga en paralelo. **True** indica que se permite que otras sesiones de carga se ejecuten en la misma tabla de destino.<br> Para más información, consulte [Paralelismo](#parallelism).|Solo se usa en el modo de carga rápida.|
|TableName|String|El nombre de la tabla con los datos que se están usando.|Se usa para ambos modos.|
|TableSubName|String|El subnombre o la subpartición. Este valor es opcional.<br> **Nota**: Esta propiedad solo se puede establecer en el **Editor avanzado**.|Solo se usa en el modo de carga rápida.|
|TransactionSize|Entero|El número de inserciones que se pueden hacer en una sola transacción. El valor predeterminado es **BatchSize**.|Solo se usa en el modo por lotes.|
|TransferBufferSize|Entero|El tamaño del búfer de transferencia. El valor predeterminado es 64 KB.|Solo se usa en el modo de carga rápida.|

## <a name="configuring-the-oracle-destination"></a>Configuración del destino de Oracle

El destino de Oracle se puede configurar mediante programación o a través del Diseñador SSIS.

El Editor de destino de Oracle se muestra en la imagen siguiente. Contiene la página Administrador de conexiones, la página Asignaciones y la página Salida de error.

Para más información, consulte una de estas secciones:

- [Editor de destino de Oracle (página Administrador de conexiones)](#oracle-destination-editor-connection-manager-page)
- [Editor de destino de Oracle (página Asignaciones)](#oracle-destination-editor-mappings-page)
- [Editor de destino de Oracle (página Salida de error)](#oracle-destination-editor-error-output-page)

![Destino de Oracle](media/oracle-destination.png)

El cuadro de diálogo **Editor avanzado** contiene las propiedades que se pueden establecer mediante programación.
Para abrir el cuadro de diálogo **Editor avanzado** :

- En la pantalla **Flujo de datos** del proyecto de Integration Services, haga clic con el botón derecho en el destino de Oracle y seleccione **Mostrar Editor avanzado**.

Para más información sobre las propiedades que se pueden establecer en el cuadro de diálogo Editor avanzado, consulte [Propiedades personalizadas del destino de Oracle](#oracle-destination-custom-properties).

## <a name="oracle-destination-editor-connection-manager-page"></a>Editor de destino de Oracle (página Administrador de conexiones)

Use la página **Administrador de conexiones** del cuadro de diálogo **Editor de destino de Oracle** para seleccionar el administrador de conexiones de Oracle para el destino. Esta página también permite seleccionar una tabla o vista de la base de datos.

**Para abrir la página Administrador de conexiones del Editor de destino de Oracle**

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el destino de Oracle.

- En la pestaña Flujo de datos, haga doble clic en el destino de Oracle.

- En el Editor de destino de Oracle, haga clic en Administrador de conexiones.

### <a name="options"></a>Opciones

**Connection manager**

Seleccione un administrador de conexiones existente de la lista o haga clic en **Nuevo** para crear un nuevo administrador de conexiones de Oracle.

**Nuevo**

Haga clic en **Nueva**. Se abrirá el cuadro de diálogo **Editor del administrador de conexiones de Oracle** donde puede crear un administrador de conexiones.

**Modo de acceso a datos**

Elija el método para la selección de datos desde el origen. Las opciones se muestran en la tabla siguiente:

|Opción|Descripción|
|:-|:-|
|Nombre de la tabla|Configure el destino de Oracle para que funcione en el modo por lotes. Opciones:<br><br> **Nombre de la tabla o la vista**: seleccione en la lista una tabla o vista disponible en la base de datos.<br><br> **Tamaño de transacción**: indique el número de inserciones que puede haber en una sola transacción. El valor predeterminado es **BatchSize**.<br><br> **Tamaño del lote**: escriba el tamaño (el número de filas cargadas) del lote para la carga masiva.
|Nombre de tabla – Carga rápida|Configure el destino de Oracle para que funcione en el modo de carga rápida (ruta directa). <br><br>Hay opciones disponibles:<br><br> **Nombre de la tabla o la vista**: seleccione en la lista una tabla o vista disponible en la base de datos.<br><br> **Carga paralela**: si está habilitada la carga en paralelo. Para más información, consulte [Paralelismo](#parallelism).<br><br> **Sin registro**: active esta casilla para deshabilitar el registro de las bases de datos. Este registro es una base de datos de Oracle que se usa con fines de recuperación, sin relación con el seguimiento.<br><br> **Número máximo de errores**: el número máximo de errores que pueden ocurrir antes de que se detenga el flujo de datos. El valor predeterminado es 0, lo que significa que no hay ningún límite de número.<br><br> Todos los errores que puedan ocurrir se devuelven en la salida de error.<br><br> **Tamaño de búfer de transferencia (KB)** : indique el tamaño del búfer de transferencia. El tamaño predeterminado es 64 KB.|

**Datos existentes de la vista**

Haga clic en **Ver los datos existentes** para ver hasta 200 filas de datos para la tabla que seleccionó.

## <a name="oracle-destination-editor-mappings-page"></a>Editor de destino de Oracle (página Asignaciones)

Use la página **Asignaciones** del cuadro de diálogo **Editor de destino de Oracle** para asignar columnas de entrada a columnas de destino.

**Para abrir la página Asignaciones del Editor de destino de Oracle**

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el destino de Oracle.

- En la pestaña Flujo de datos, haga doble clic en el destino de Oracle.

- En el Editor de destino de Oracle, haga clic en Asignaciones.

### <a name="options"></a>Opciones

**Columnas de entrada disponibles**

Lista de columnas de entrada disponibles. Arrastre y coloque una columna de entrada en una columna de destino disponible para asignar las columnas.

**Columnas de destino disponibles**

Lista de columnas de destino disponibles. Arrastre y coloque una columna de destino en una columna de entrada disponible para asignar las columnas.

**Columna de entrada**

Permite ver las columnas de entrada seleccionadas. Para quitar asignaciones, seleccione **< ignore >** con el fin de excluir columnas de la salida.

**Columna de destino**

Muestra todas las columnas de destino disponibles, tanto las asignadas como las no asignadas.

> [!NOTE]
>
>Las columnas de tipos de datos no compatibles se eliminarán de la asignación con una advertencia.

## <a name="oracle-destination-editor-error-output-page"></a>Editor de destino de Oracle (página Salida de error)

Use la página Salida de error del cuadro de diálogo Editor de destino de Oracle para seleccionar las opciones de control de errores.

**Para abrir la página Salida de error del Editor de destino de Oracle**

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el destino de Oracle.

- En la pestaña Flujo de datos, haga doble clic en el destino de Oracle.

- En el Editor de destino de Oracle, haga clic en Salida de error.

### <a name="options"></a>Opciones

**Comportamiento de error**

Seleccione la forma en la que el origen de Oracle debe controlar los errores de un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.
**Sección relacionada**: [Control de errores en los datos](./error-handling-in-data.md)

**Truncamiento**

Seleccione la forma en la que el origen de Oracle debe controlar el truncamiento de un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.

## <a name="next-steps"></a>Pasos siguientes

- Configure el [Administrador de conexiones de Oracle](oracle-connection-manager.md).
- Configure el [origen de Oracle](oracle-source.md).
- Configure el [destino de Oracle](oracle-destination.md).
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA5u35j).
