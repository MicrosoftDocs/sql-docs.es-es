---
description: Conexión al origen de Teradata
title: Conexión al origen de Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0170a81d73edef1e893a16df93e1bc004415739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337806"
---
# <a name="connect-to-the-teradata-source"></a>Conexión al origen de Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

El origen de Teradata extrae datos de las bases de datos de Teradata mediante:
- Una tabla o una vista.
- Los resultados de una instrucción SQL.

El origen usa el administrador de conexiones de Teradata para conectarse al origen de Teradata. Para más información, consulte [Uso del administrador de conexiones de Teradata](teradata-connection-manager.md).

## <a name="troubleshoot-the-teradata-source"></a>Solución de problemas del origen de Teradata

Puede registrar las llamadas que realiza el origen de Teradata a Teradata Parallel Transporter (TPT) API. Para ello, habilite el registro de paquetes y, luego, seleccione el evento **Diagnostic** en el nivel de paquete.

Para registrar las llamadas de conectividad abierta de bases de datos (ODBC) que el origen de Teradata realiza al controlador ODBC de Teradata, puede habilitar el seguimiento del administrador de controladores ODBC. Para más información, consulte [cómo generar un seguimiento ODBC con el administrador de orígenes de datos ODBC](../../odbc/admin/setting-tracing-options.md).

## <a name="parallelism"></a>Paralelismo

El origen de Teradata admite paralelismo, donde los trabajos de exportación pueden acceder a la misma tabla o a tablas diferentes al mismo tiempo. Una variable de base de datos denominada `MaxLoadTasks` establece el límite del número de trabajos de exportación que se pueden ejecutar al mismo tiempo. Este número máximo se puede establecer mediante la variable `MaxLoadTasks`.

## <a name="teradata-source-custom-properties"></a>Propiedades personalizadas del origen de Teradata

En la tabla siguiente se enumeran las propiedades personalizadas del origen de Teradata. Todas las propiedades son de lectura y escritura.

|Nombre de propiedad|Tipo de datos|Descripción|
|:-|:-|:-|
|AccessMode|Integer (enumeración)|Modo que se usa para tener acceso a la base de datos. Los valores posibles son *Nombre de tabla* y *Comando SQL*. El valor predeterminado es *Nombre de tabla*.|
|BlockSize|Entero|Tamaño de bloque, en bytes, que se usa al devolver datos al cliente. El valor predeterminado es 1048576 (1 MB). El valor mínimo es 256 bytes. El valor máximo es 16775168 bytes.<br> Esta propiedad se encuentra en el panel **Editor avanzado**.|
|BufferMaxSize|Entero|Tamaño máximo total del búfer de datos devuelto por la función GetBuffer. Este tamaño debe ser lo suficientemente grande como para contener al menos una fila de datos, incluidos el encabezado de fila, la fila de datos real y el finalizador del búfer. El tamaño máximo total predeterminado del búfer de datos es de 16775552 bytes. <br>Para más información, consulte [Exportación de datos de una base de datos de Teradata mediante GetBuffer](https://docs.teradata.com/reader/TvVKKmxaBAoyETJZD8zz_g/oaxiwNJmnCa6UctY4k498w).|
|BufferMode|Boolean|El valor predeterminado es *True*. El valor debe ser *True* si se usa la característica PutBuffer. Esta propiedad se encuentra en el panel **Editor avanzado**.|
|DataEncryption|Boolean|El valor predeterminado es *False*. Si el valor es *true*, se usa el cifrado de seguridad completo.|
|DefaultCodePage|Entero|La página de códigos que se usa cuando el origen de datos no tiene información sobre páginas de códigos. Esta propiedad se encuentra en el panel **Editor avanzado**.|
|DetailedTracingLevel|Integer (enumeración)|Seleccione una de las siguientes opciones de seguimiento avanzado: <br> *Off*: sin registro avanzado. <br> *General*: se registra el seguimiento general de las actividades específicas del controlador. <br> *CLI*: se registra el seguimiento de actividades relacionadas con CLIv2. <br> *Notify Method* (Método de notificación): se registra el seguimiento de actividades relacionadas con la característica de notificación. <br> *Common Library*: se registra el seguimiento de actividades de la biblioteca opcommon. <br> *Todos*: se registra todo el seguimiento de actividad anterior. <br> El archivo de registro de seguimiento avanzado se define en la propiedad `DetailedTracingFile`. <br> La propiedad `DetailedTracingFile` se debe establecer si la opción no es *Off*. Esta propiedad se encuentra en el panel **Editor avanzado**.|
|DetailedTracingFile|String|La ruta de acceso del archivo de registro que se genera automáticamente cuando *DetailedTracingLevel* no es *Off*. Esta propiedad se encuentra en el panel **Editor avanzado**.|
|DiscardLargeRow|Boolean|El valor predeterminado es *False*. Si el valor es *true*, se descartan filas grandes (más de 64 KB).|
|ExtendedStringColumnsAllocation|Boolean|Si es *true*, se usa *Maximal Transfer Character Allocation Factor*. <br> Este valor debe establecerse en *true* si la propiedad `Export Width Table ID` de la base de datos de Teradata está establecida en *Maximal Defaults*. <br> El valor predeterminado es *False*.|
|JobMaxRowSize|Entero|Se puede admitir el tamaño máximo de la fila. Este valor es necesario si el valor de `DiscardLargeRow` es *true*.<br>Valores válidos: <br>*64* (valor predeterminado): se pueden admitir longitudes de fila de 2 bytes. <br>*1024*: se pueden admitir longitudes de fila de 4 bytes.|
|MaxSessions|Entero|El número máximo de sesiones que han iniciado sesión. Este valor debe ser superior a uno. El valor predeterminado es una sesión para cada procesador del módulo de acceso (AMP) disponible.|
|MinSessions|Entero|Número mínimo de sesiones iniciadas. Este valor debe ser superior a uno. El valor predeterminado es una sesión por cada AMP disponible.|
|QueryBandSessInfo|Varchar|Expresión de banda de consulta basada en sesión y definida por el usuario en un formato de cadena de conexión. Esta propiedad se usa para la supervisión y la regulación de recargos. Esta propiedad se encuentra en el panel **Editor avanzado**.|
|SpoolMode|Varchar|Los valores válidos son: <br>*Spool*: usar el valor predeterminado *Spool*. <br> *NoSpool*: no usar *Spool*. Este valor solo es válido si el servidor de bases de datos (DBS) admite *NoSpool*. <br>  *NoSpoolOnly*: no usar *Spool* en ningún caso. El trabajo se terminará con un error si el DBS no admite *NoSpool*.|
|SqlCommand|String|El comando SQL que se va a ejecutar cuando `AccessMode` se establece en *Comando SQL*.|
|TableName|String|El nombre de la tabla con los datos que se van a usar cuando `AccessMode` está establecido en *Nombre de tabla*.|
|TenacityHours|Entero|El número de horas que el controlador TPT intenta iniciar sesión cuando ya está en ejecución el número máximo de operaciones de carga y exportación. El valor predeterminado es *4 horas*. Esta propiedad se encuentra en el panel **Editor avanzado**.|
|TenacitySleep|Entero|El número de minutos que el controlador de TPT pausa antes de intentar iniciar sesión cuando se alcanza el límite. El límite se define mediante las propiedades `MaxSessions` y `TenacityHours`. El valor predeterminado es 6 minutos. Esta propiedad se encuentra en el panel **Editor avanzado**.|
|UnicodePassThrough|Boolean|*Off* (valor predeterminado): deshabilita el paso a través de Unicode. <br>*El*: habilita el paso a través de Unicode.|

## <a name="configure-the-teradata-source"></a>Configuración del origen de Teradata

Puede configurar el origen de Teradata mediante programación o mediante el Diseñador de SQL Server Integration Services (SSIS).

En la imagen siguiente se muestra el panel **Editor de código fuente de Teradata**. Para más información, vaya a cada una de las siguientes secciones del Editor de código fuente de Teradata:

- [El panel Administrador de conexiones](#the-connection-manager-pane)
- [El panel Columnas](#the-columns-pane)
- [El panel Salida de error](#the-error-output-pane)

![Editor de código fuente de Teradata](media/teradata-source.png)

El panel **Editor avanzado** contiene las propiedades que se pueden establecer mediante programación. Para abrir el panel, siga estos pasos:
- En la pantalla **Flujo de datos** del proyecto de Integration Services, haga clic con el botón derecho en el origen de Oracle y seleccione **Mostrar Editor avanzado**.

Para más información sobre las propiedades que se pueden establecer en el panel **Editor avanzado**, consulte [Propiedades personalizadas del origen de Teradata](#teradata-source-custom-properties).

## <a name="the-connection-manager-pane"></a>El panel Administrador de conexiones

Use el panel **Administrador de conexiones** para seleccionar la instancia del administrador de conexiones de Teradata del origen. En este panel, también puede seleccionar una tabla o una vista de la base de datos. Para abrir el panel, siga estos pasos:

1. En SQL Server Data Tools, abra el paquete SSIS que contiene el origen de Teradata.

1. En la pestaña **Flujo de datos**, haga doble clic en el origen de Teradata.

1. En el Editor de código fuente de Teradata, seleccione la pestaña **Administrador de conexiones**.

### <a name="options"></a>Opciones

**Connection manager**

* Seleccione un administrador de conexiones existente de la lista o seleccione **Nuevo** para crear una instancia del administrador de conexiones de Teradata.

**Nuevo**

* Seleccione **Nuevo**. Se abre el panel **Editor del administrador de conexiones de Teradata**. En este panel puede crear un administrador de conexiones.

**Modo de acceso a los datos**

* Elija un método para la selección datos del origen. Las opciones se muestran en la tabla siguiente:

    |Opción|Descripción|
    |:-|:-|
    |Nombre de tabla: TPT Export|Recupera datos de una tabla o vista del origen de datos de Teradata. Cuando esta opción esté seleccionada, seleccione una tabla o vista disponible en la lista para **Nombre de la tabla o la vista**.|
    |Comando SQL: TPT Export|Recupera datos del origen de datos de Teradata mediante una consulta SQL. Cuando seleccione esta opción, escriba una consulta de una de las siguientes maneras: <ul><li>Escriba el texto de la consulta SQL en el campo de **Texto de comando SQL** .</li><li>Seleccione **Examinar** para cargar la consulta SQL desde un archivo de texto.</li><li>Seleccione **Analizar consulta** para comprobar la sintaxis del texto de la consulta.</li></ul>|

**Versión preliminar**

* Seleccione **Vista previa** para ver hasta las 200 primeras filas de los datos extraídos de la tabla o la vista seleccionadas.

## <a name="the-columns-pane"></a>El panel Columnas

Use el panel **Columnas** para asignar una columna de salida a cada columna externa (origen). Para abrir el panel, siga estos pasos:

1. En SQL Server Data Tools, abra el paquete SSIS que contiene el origen de Teradata.

1. En la pestaña **Flujo de datos**, haga doble clic en el origen de Teradata.

1. En el Editor de código fuente de Teradata, seleccione la pestaña **Columnas**.

### <a name="options"></a>Opciones

**Columnas externas disponibles**

En esta tabla se enumeran las columnas externas disponibles que puede seleccionar para agregarlas a la lista **Columna externa**. Puede enumerar las columnas en el orden que elija. Esta tabla no se puede usar para agregar o quitar columnas.

* Para elegir todas las columnas, active la casilla **Seleccionar todo**.

**Columnas externas**

Las columnas externas (origen) que seleccionó aparecen en orden. Para cambiar el orden, borre primero la lista **Columna externa disponible** y, luego, seleccione las columnas en otro orden.

**Columna de salida**

Aunque el nombre de la columna externa (origen) seleccionada es el nombre de salida predeterminado, puede escribir cualquier nombre único.

>[!NOTE]
>>Si hay columnas que contienen tipos de datos no admitidos, recibirá una advertencia que muestra esos tipos de datos no admitidos, y las columnas correspondientes se quitarán de las columnas de asignación.

## <a name="the-error-output-pane"></a>El panel Salida de error

Use el panel **Salida de error** para seleccionar opciones de control de errores. Para abrir el panel, siga estos pasos:

1. En SQL Server Data Tools, abra el paquete SSIS que contiene el origen de Teradata.

1. En la pestaña **Flujo de datos**, haga doble clic en el origen de Teradata.

1. En el Editor de código fuente de Teradata, seleccione la pestaña **Salida de error**.

### <a name="options"></a>Opciones

**Comportamiento de error**

* Seleccione el modo en que el origen de Teradata debe controlar los errores en un flujo: 
  * Pasar por alto el error
  * Redirigir la fila
  * Hacer que el componente genere un error.

**Temas relacionados:** : Consulte [Control de errores en los datos](error-handling-in-data.md).

**Truncamiento**

* Seleccione el modo en que el origen de Teradata debe controlar el truncamiento en un flujo: 
  * Pasar por alto el error
  * Redirigir la fila
  * Hacer que el componente genere un error.

## <a name="next-steps"></a>Pasos siguientes

- Configurar el [destino de Teradata](teradata-destination.md).
- Si tiene alguna pregunta, visite [Tech Community](https://aka.ms/AA6iwdw).