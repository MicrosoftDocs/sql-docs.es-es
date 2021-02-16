---
description: Destino de Teradata
title: Destino de Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 83453cfa36ae4b2e9f6596acc7206162c2b82b76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342270"
---
# <a name="teradata-destination"></a>Destino de Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

El destino de Teradata carga datos de forma masiva en la base de datos de Teradata.

El destino usa el administrador de conexiones de Teradata para conectarse a un origen de datos. Para más información, consulte [Administrador de conexiones de Teradata](teradata-connection-manager.md).

## <a name="load-options"></a>Opciones de carga

El destino de Teradata admite dos modos de carga de datos:

- **TPT Stream**: este modo usa el operador TPT API Stream (protocolo Teradata TPump).

- **TPT Load** (carga masiva rápida): este modo usa el operador TPT API Load (protocolo Teradata FastLoad) para la carga masiva rápida.

El modo de carga rápida tiene las siguientes restricciones:

- El límite de sesiones de la base de datos de Teradata viene determinado por el factor que se encuentre en primer lugar:
    - Límites de sesión establecidos mediante el comando SESSION
    - El límite de la base de datos de Teradata de una sesión por AMP
    - El límite de plataforma sobre el número máximo de sesiones por aplicación: Definido por la variable MaxSess en el archivo de software de la interfaz del procesador de comunicaciones (COP), CLISPB. DAT. Puede usar el comando SET MAXSESSIONS de TDP para especificar un límite de plataforma. El límite predeterminado es igual al valor MAXSESS del servidor.

- No se admiten índices de combinación.
- No se admiten referencias de claves externas en tablas de destino.
- No se admiten tablas de destino definidas con un índice secundario.

Para más información sobre las restricciones de carga rápida de Teradata, consulte la referencia de carga rápida de Teradata.

Puede establecer el modo en el [Editor de destino de Teradata (página Administrador de conexiones)](#teradata-destination-editor-connection-manager-page).

## <a name="error-handling"></a>Control de errores

Los errores devueltos durante el proceso de carga se escriben en tablas de errores temporales que se bloquean durante el proceso de carga.
La propiedad **Número máximo de errores (MaxErrors)** del Editor avanzado establece el número máximo de errores que se pueden escribir en estas tablas.

Si el número máximo de errores es mayor que cero, se generan tablas de errores con nombres únicos y se imprime un mensaje informativo en el registro de paquetes. Los errores se pueden recuperar mediante la salida de error del componente SSIS estándar.

Las tablas temporales se quitan una vez finalizado el proceso de carga. Si el destino de Teradata no puede leer las tablas temporales, no se quitan a menos que la propiedad **Eliminar siempre la tabla de errores** esté activada.  Si el proceso de carga se detiene antes de que finalice, deberá quitar manualmente estas tablas si es necesario. Estas tablas se encuentran en la misma base de datos que la tabla de destino.

Cuando se alcanza el **número máximo de errores**, el estado de la tabla de destino depende del modo en que se use.

- En el modo de carga rápida, la tabla de destino no se puede usar. Para volver a ejecutarla, debe truncar o quitar la tabla de destino y volver a crearla. No se admite la reversión.
- En el modo de operador de TPT Steam, el destino de Teradata se ejecuta mediante un mecanismo de filas almacenadas en búfer. Si se produce un error en el trabajo, todos los cambios que se completaron (se enviaron a los búferes) en el momento del error son permanentes en las tablas de destino. No hay ningún concepto de reversión. Se quitarán las tablas de error.

El destino de Teradata tiene una salida de error. Para más información, consulte [Editor de destino de Teradata (página Salida de error)](#teradata-destination-editor-error-output-page).

## <a name="parallelism"></a>Paralelismo

El paralelismo está restringido en el modo de carga rápida; varios trabajos de carga rápida independientes no pueden tener acceso a la misma tabla al mismo tiempo. Además, el número de trabajos de carga rápida simultáneos está limitado por la variable de base de datos **MaxLoadTasks**.

No hay ninguna restricción de paralelismo en el modo TPT Stream. Es posible ejecutar varios destinos de Teradata a la vez en la misma tabla, aunque se puede reducir el rendimiento por Teradata. Consulte la documentación de Teradata para más información.

## <a name="troubleshooting-the-teradata-destination"></a>Solución de problemas del destino de Teradata

Puede registrar las llamadas que realiza el origen de Teradata a Teradata Parallel Transporter API (TPT API). Puede habilitar el registro de paquetes y seleccionar el evento **Diagnostic** en el nivel de paquete para registrar las llamadas.

Puede registrar las llamadas ODBC que realiza el origen de Teradada al controlador ODBC de Teradata si habilita el seguimiento del administrador de controladores ODBC. Para obtener más información, vea la documentación de Microsoft sobre *cómo generar un seguimiento ODBC con el administrador de orígenes de datos*.

## <a name="teradata-destination-custom-properties"></a>Propiedades personalizadas de destino de Teradata

En la tabla siguiente se describen las propiedades personalizadas del destino de Teradata. Todas las propiedades son de lectura y escritura.

|Nombre de propiedad|Tipo de datos|Descripción|
|:-|:-|:-|
|AlwaysDropErrorTable|Boolean|El valor predeterminado es **false**. Si es **true**, quite todas las tablas de error incluso si se produce un error de lectura en el destino de Teradata.|
|ArraySupport|Boolean|El valor predeterminado es **true**. Si es **true**, los grupos DML usan ArraySupport. Solo es aplicable a TPT Stream. Esta propiedad está en el **Editor avanzado**.|
|Búferes|Entero|El número de búferes de solicitud que se aumentará; el valor se puede establecer entre 2 y 64. Solo es aplicable a TPT Stream. Esta propiedad está en el **Editor avanzado**.|
|BufferMode|Boolean|El valor predeterminado es **true**. Debe ser **true** si se usa la característica PutBuffer. Esta propiedad está en el **Editor avanzado**.|
|BufferSize|Entero|Tamaño del búfer de salida (en KB) que se usa para enviar los paquetes de carga. El valor predeterminado es 1024. Solo se aplica a TPT Load. Esta propiedad está en el **Editor avanzado**.|
|DataEncryption|Boolean|El valor predeterminado es **false**. Si es **true**, se usa el cifrado de seguridad completo.|
|DefaultCodePage|Entero|La página de códigos que se usa cuando el origen de datos no tiene información de página de códigos. <br>**Nota**: Esta propiedad está en el **Editor avanzado**.|
|DetailedTracingLevel|Integer (enumeración)|Seleccione una de las siguientes opciones de seguimiento avanzado: <br> **Off**: sin registro avanzado. <br> **General**: se registra el seguimiento general de las actividades específicas del controlador. <br> **CLI**: se registra el seguimiento de actividades relacionadas con CLIv2. <br> **Notify Method** (Método de notificación): se registra el seguimiento de actividades relacionadas con la característica de notificación. <br> **Common Library**: se registra el seguimiento de actividades de la biblioteca opcommon. <br> **Todos**: se registra el seguimiento de todas las actividades anteriores. <br> El archivo de registro de seguimiento avanzado se define en la propiedad **DetailedTracingFile**. <br> La propiedad **DetailedTracingFile** debe establecerse si la opción no está desactivada. <br> Esta propiedad está en el **Editor avanzado**.|
|DetailedTracingFile|String|La ruta de acceso del archivo de registro que se genera automáticamente cuando **DetailedTracingLevel** no es **Off** (Desactivado). Esta propiedad está en el **Editor avanzado**.|
|DiscardLargeRow|Boolean|El valor predeterminado es **false**. Si es **true**, se descartan las filas grandes (más de 64 K).|
|ErrorTableName|String|Nombre de la tabla de errores. El valor predeterminado es el nombre de la tabla de destino|
|ExtendedStringColumnsAllocation|Boolean|Si es **true**, se usa **Maximal Transfer Character Allocation Factor** (Factor máximo de asignación de caracteres de transferencia). <br> Este valor debe establecerse en **true** si la propiedad **Export Width Table ID** (Exportar id. de ancho de tabla) de la base de datos de Teradata está establecida en **Maximal Defaults** (Valores predeterminados máximos). <br> El valor predeterminado es **false**.|
|FastLoad|Boolean|Si es **true**, se usa la carga rápida. El valor predeterminado es **false**. También se puede establecer en el [Editor del destino de Teradata (página Administrador de conexiones)](#teradata-destination-editor-connection-manager-page).|
|MaxErrors|Entero|La cantidad de errores que pueden ocurrir antes de que se detenga el flujo de datos. El valor predeterminado es **0**, lo que significa que no hay ningún límite de número de errores.<br> Si está seleccionada la opción **Redirigir fila** en la página **Control de errores**. Antes de que se alcance el límite de número de errores, todos los errores se devuelven en la salida de error. Para más información, consulte [Editor de destino de Teradata (página Salida de error)](#teradata-destination-editor-error-output-page).|
|MaxSessions|Entero|El número máximo de sesiones iniciadas. Este valor debe ser superior a uno. El valor predeterminado es una sesión por cada AMP disponible.|
|MinSessions|Entero|El número mínimo de sesiones iniciadas. Este valor debe ser superior a uno. El valor predeterminado es una sesión por cada AMP disponible.|
|Pack|Entero|El número de instrucciones que se van a empaquetar en una solicitud de varias instrucciones. El valor predeterminado es 20; el máximo permitido es 2400. Solo es aplicable a TPT Stream. Esta propiedad está en el **Editor avanzado**.|
|PackMaximum|Boolean|Si es **true**, determina dinámicamente el factor de paquete máximo del trabajo de secuencia actual. Solo es aplicable a TPT Stream. Esta propiedad está en el **Editor avanzado**.|
|QueryBandSessInfo|Varchar|Una expresión de banda de consulta basada en sesión definida por el usuario, para habilitar la supervisión y la regulación de recargos. Esta propiedad debe estar en formato de cadena de conexión. Esta propiedad está en el **Editor avanzado**.|
|ReplicationOveride|Integer (enumeración)|Opciones: <br> **Valor predeterminado**: no se envía ninguna instrucción SET SESSION OVERRIDE REPLICATION a la base de datos. Se usa la configuración predeterminada de la base de datos. <br> **El**: se reemplazan los controles normales del servicio de replicación. <br> **Off**: se usan los controles normales del servicio de replicación. <br> Esta propiedad solo es aplicable a TPT Stream. <br> Esta propiedad está en el **Editor avanzado**.|
|Robust|Boolean|Si es **true**, la lógica de reinicio de Robust se usa para operaciones de recuperación y reinicio. Esta propiedad solo es aplicable a **TPT Stream**. Esta propiedad está en el **Editor avanzado**.|
|TableName|String|El nombre de la tabla con los datos que se están usando.|
|TenacityHours|Entero|El número de horas que el controlador TPT intenta iniciar sesión cuando ya está en ejecución el número máximo de operaciones de carga y exportación. El valor predeterminado es 4 horas. Esta propiedad está en el **Editor avanzado**.|
|TenacitySleep|Entero|Minutos que el controlador TPT se detiene antes de intentar iniciar sesión cuando se alcanza el límite. El límite se define mediante las propiedades **MaxSessions** y **TenacityHours**. El valor predeterminado es seis minutos. Esta propiedad está en el **Editor avanzado**.|
|UnicodePassThrough|Boolean|Off (valor predeterminado): deshabilita el paso a través de Unicode. <br>On: habilita el paso a través de Unicode.|

## <a name="configuring-the-teradata-destination"></a>Configuración del destino de Teradata

El destino de Teradata se puede configurar mediante programación o a través del Diseñador SSIS.

El Editor de destino de Teradata se muestra en la imagen siguiente. Contiene la página Administrador de conexiones, la página Asignaciones y la página Salida de error.

Para obtener más información, vea uno de los siguientes temas:

- [Editor de destino de Teradata (página Administrador de conexiones)](#teradata-destination-editor-connection-manager-page)
- [Editor de destino de Teradata (página Asignaciones)](#teradata-destination-editor-mappings-page)
- [Editor de destino de Teradata (página Salida de error)](#teradata-destination-editor-error-output-page)

![editor de destino](media/teradata-destination.png)

El cuadro de diálogo **Editor avanzado** contiene las propiedades que se pueden establecer mediante programación.
Para abrir el cuadro de diálogo **Editor avanzado** :

- En la pantalla **Flujo de datos** del proyecto de Integration Services, haga clic con el botón derecho en el destino de Teradata y seleccione **Mostrar editor avanzado**.

Para más información sobre las propiedades que se pueden establecer en el cuadro de diálogo Editor avanzado, consulte [Propiedades personalizadas de destino de Teradata](#teradata-destination-custom-properties).

## <a name="teradata-destination-editor-connection-manager-page"></a>Editor de destino de Teradata (página Administrador de conexiones)

Use la página **Administrador de conexiones** del cuadro de diálogo **Editor de destino de Teradata** para seleccionar el administrador de conexiones de Teradata del destino. Esta página también permite seleccionar una tabla o vista de la base de datos.

Para abrir la página Administrador de conexiones del Editor de destino de Teradata, siga estos pasos:

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el destino de Teradata.

- En la pestaña Flujo de datos, haga doble clic en el destino de Teradata.

- En el Editor de destino de Teradata, haga clic en Administrador de conexiones.

### <a name="options"></a>Opciones

**Connection manager**

Seleccione un administrador de conexiones existente de la lista o haga clic en **Nuevo** para crear un administrador de conexiones de Teradata.

**Nuevo**

Haga clic en **Nueva**. Se abre el cuadro de diálogo **Editor del administrador de conexiones de Teradata** donde puede crear un administrador de conexiones.

**Modo de acceso a datos**

Elija el método para la selección de datos desde el origen. Las opciones se muestran en la tabla siguiente:

|Opción|Descripción|
|:-|:-|
|Nombre de tabla: TPT Stream|Modo incremental mediante el operador TPT Stream. <br>**Nombre de la tabla o la vista**: seleccione una tabla o vista existentes de la lista. Esta lista solo muestra las primeras 1000 tablas. Puede escribir el prefijo del nombre de la tabla o usar cualquier parte del nombre con el carácter comodín (*) para mostrar la tabla o las tablas que desea usar.|
|Nombre de la tabla: TPL Load|Modo de carga rápida (ruta de acceso directa) mediante el operador TPT API Load (protocolo Teradata FastLoad), que requiere que la tabla de destino esté vacía. <br>**Nombre de la tabla o la vista**: seleccione una tabla o vista existentes de la lista. Esta lista solo muestra las primeras 1000 tablas. Puede escribir el prefijo del nombre de la tabla o usar cualquier parte del nombre con el carácter comodín (*) para mostrar la tabla o las tablas que desea usar.|

**Cifrado de datos**: casilla para habilitar el cifrado de datos. El valor predeterminado es desactivada.

**Eliminar siempre la tabla de errores**: casilla para quitar las tablas de errores de todas las instancias.

**Tabla de errores**: nombre de la tabla en la que se escriben los errores.

**Número mínimo de sesiones**: el número mínimo de sesiones iniciadas. El valor predeterminado es una sesión por cada AMP disponible. El valor debe ser mayor que uno.

**Número máximo de sesiones**: el número máximo de sesiones iniciadas. El valor predeterminado es una sesión por cada AMP disponible. El valor debe ser mayor que uno.

**Número máximo de errores**: el número máximo de errores que se pueden devolver antes de que se detenga o redirija el flujo de datos. 

## <a name="teradata-destination-editor-mappings-page"></a>Editor de destino de Teradata (página Asignaciones)

Use la página **Asignaciones** del cuadro de diálogo **Editor de destino de Teradata** para asignar columnas de entrada a columnas de destino.

Para abrir la página Asignaciones del Editor de destino de Teradata, siga estos pasos:

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el destino de Teradata.

- En la pestaña Flujo de datos, haga doble clic en el destino de Teradata.

- En el Editor de destino de Teradata, haga clic en Asignaciones.

### <a name="options"></a>Opciones

**Columnas de entrada disponibles**

Lista de columnas de entrada disponibles. Arrastre y coloque una columna de entrada en una columna de destino disponible para asignar las columnas.

**Columnas de destino disponibles**

Lista de columnas de destino disponibles. Arrastre y coloque una columna de destino en una columna de entrada disponible para asignar las columnas.

**Columna de entrada**

Permite ver las columnas de entrada seleccionadas. Para quitar asignaciones, seleccione **< ignore >** con el fin de excluir columnas de la salida.

**Columna de destino**

Muestra todas las columnas de destino disponibles, tanto las asignadas como las no asignadas.

>[!NOTE]
>
>Las columnas de tipos de datos no compatibles se eliminarán de la asignación con una advertencia.

## <a name="teradata-destination-editor-error-output-page"></a>Editor de destino de Teradata (página Salida de error)
Use la página Salida de error del cuadro de diálogo Editor de destino de Teradata para seleccionar las opciones de control de errores.

**Para abrir la página Salida de error del Editor de destino de Teradata**, siga estos pasos:

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el destino de Teradata.

- En la pestaña Flujo de datos, haga doble clic en el destino de Teradata.

- En el Editor de destino de Teradata, haga clic en Salida de error.

### <a name="options"></a>Opciones

**Comportamiento de error**

Seleccione la forma en la que el destino de Teradata debe controlar errores de un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.

**Temas relacionados**: [Control de errores en los datos](./error-handling-in-data.md)

**Truncamiento**

Seleccione la forma en la que el destino de Teradata debe controlar los truncamientos de un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.

## <a name="next-steps"></a>Pasos siguientes

- Configurar el [administrador de conexiones de Teradata](teradata-connection-manager.md)
- Configurar el [origen de Teradata](teradata-source.md)
- Configurar el [destino de Teradata](teradata-destination.md)
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA6iwdw).
