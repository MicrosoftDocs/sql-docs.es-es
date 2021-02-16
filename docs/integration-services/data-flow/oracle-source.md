---
description: Origen de Oracle
title: Origen de Oracle
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6d26daafb62777091cb962332d47d8c00620e4d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343206"
---
# <a name="oracle-source"></a>Origen de Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

El origen de Oracle usa los modos siguientes para extraer datos de Oracle Database:

- Una tabla o vista.

- Los resultados de una instrucción SQL.

El origen usa un administrador de conexiones de Oracle para conectarse al origen de Oracle. Para más información, consulte el artículo sobre el [Administrador de conexiones de Oracle](oracle-connection-manager.md).

## <a name="error-output"></a>Salida de error

La salida de error incluye estas columnas:  

- **Código de error**: un número que representa el tipo de error del error actual. El código de error puede ser de:
    - Servidor de Oracle. Consulte la descripción detallada del error en la documentación de Oracle Database.
    - Entorno de ejecución de SSIS. Para obtener una lista de códigos de errores de SSIS, vea la referencia Código de errores y mensajes de SSIS.
- **Columna de error**: el número de la columna de origen que genera los errores de conversión.

- **Columnas de datos de error**: los datos que generan el error.

En la salida de error, el origen de Oracle devuelve los errores que se producen en el proceso de carga y extracción. Para más información, consulte [Editor de origen de Oracle (página Salida de error)](#oracle-source-editor-error-output-page).

## <a name="troubleshooting-the-oracle-source"></a>Solución de problemas del origen de Oracle

Puede registrar las llamadas ODBC que el origen de Oracle hace a los orígenes de datos de Oracle para solucionar problemas de la exportación de datos. Para registrar las llamadas ODBC que el origen de Oracle hace a los orígenes de datos de Oracle, habilite el seguimiento del administrador de controladores ODBC. Para obtener más información, vea la documentación de Microsoft sobre *cómo generar un seguimiento ODBC con el administrador de orígenes de datos.*

## <a name="oracle-source-custom-properties"></a>Propiedades personalizadas del origen de Oracle

Las propiedades personalizadas del origen de Oracle son las siguientes. Todas las propiedades son de lectura y escritura.

|Nombre de propiedad|Tipo de datos|Descripción|
|:-|:-|:-|
|AccessMode|Integer (enumeración)|Modo que se usa para tener acceso a la base de datos. Los valores posibles son **Nombre de tabla** y **Comando SQL**. El valor predeterminado es **Nombre de tabla**.|
|BatchSize|Entero|Tamaño de lote para la carga masiva. Es el número de registros extraídos como matriz. <br>Esta propiedad solo se establece mediante el **Editor avanzado**.|
|DefaultCodePage|Entero|La página de códigos que se usa cuando el origen de datos no tiene información de página de códigos. <br>Esta propiedad solo se establece mediante el **Editor avanzado**.|
|PreFetchCount|Entero|El número de filas capturadas previamente. <br>Esta propiedad solo se establece mediante el **Editor avanzado**.|
|SqlCommand|String|Comando SQL que se va a ejecutar cuando AccessMode se establece en SQL Command.|
|TableName|String|El nombre de la tabla con los datos que se van a usar cuando AccessMode esté establecido en Nombre de tabla.|

## <a name="configuring-the-oracle-source"></a>Configuración del origen de Oracle

Puede configurar el origen de Oracle mediante programación o través del Diseñador SSIS.

El Editor de origen de Oracle se muestra en la imagen siguiente. Contiene la página Administrador de conexiones, la página Columnas y la página Salida de error.

Para más información, consulte una de estas secciones:

- [Editor de origen de Oracle (página Administrador de conexiones)](#oracle-source-editor-connection-manager-page)
- [Editor de origen de Oracle (página Columnas)](#oracle-source-editor-columns-page)
- [Editor de origen de Oracle (página Salida de error)](#oracle-source-editor-error-output-page)

![Origen de Oracle](media/oracle-source.png)

El cuadro de diálogo **Editor avanzado** contiene las propiedades que se pueden establecer mediante programación.

Para abrir el cuadro de diálogo **Editor avanzado** :

- En la pantalla **Flujo de datos** del proyecto de Integration Services, haga clic con el botón derecho en el origen de Oracle y seleccione **Mostrar Editor avanzado**.

Para más información sobre las propiedades que se pueden establecer en el cuadro de diálogo **Editor avanzado**, consulte [Propiedades personalizadas del origen de Oracle](#oracle-source-custom-properties).

## <a name="oracle-source-editor-connection-manager-page"></a>Editor de origen de Oracle (página Administrador de conexiones)

En la página **Administrador de conexiones**, el cuadro de diálogo **Editor de origen de Oracle** permite seleccionar Oracle Database como origen, tabla o vista desde la base de datos.

**Para abrir la página Administrador de conexiones del Editor de origen de Oracle**

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el origen de Oracle.

- En la pestaña Flujo de datos, haga doble clic en el origen de Oracle.
### <a name="options"></a>Opciones

**Connection manager**

Seleccione un administrador de conexiones existente de la lista o haga clic en **Nuevo** para crear un nuevo administrador de conexiones de Oracle.

**Nuevo**

Haga clic en **Nueva**. Se abrirá el cuadro de diálogo **Editor del administrador de conexiones de Oracle** donde puede crear un administrador de conexiones.

**Modo de acceso a los datos**

Elija el método para la selección de datos desde el origen. Las opciones se muestran en la tabla siguiente:

|Opción|Descripción|
|:-|:-|
|Tabla o vista|Recupera datos de una tabla o vista del origen de datos de Oracle. Cuando esta opción esté seleccionada, seleccione una tabla o vista disponible en la lista para **Nombre de la tabla o la vista**.|
|Comando SQL|Recupera datos del origen de datos de Oracle mediante una consulta SQL. Cuando seleccione esta opción, escriba una consulta de una de las siguientes maneras: <br>Escriba el texto de la consulta SQL en el campo de **Texto de comando SQL** . <br>Haga clic en **Examinar** para cargar la consulta SQL desde un archivo de texto. <br>Haga clic en **Analizar consulta** para comprobar la sintaxis del texto de la consulta.|

**Versión preliminar**

Haga clic en **Vista previa** para ver hasta las 200 primeras filas de los datos extraídos de la tabla o la vista seleccionada.

## <a name="oracle-source-editor-columns-page"></a>Editor de origen de Oracle (página Columnas)

En la página **Columnas**, el cuadro de diálogo **Editor de origen de Oracle** se usa para asignar una columna de salida a cada columna externa (origen).

**Para abrir la página Columnas del Editor de origen de Oracle**

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el origen de Oracle.

- En la pestaña Flujo de datos, haga doble clic en el origen de Oracle.

- En el Editor de origen de Oracle, haga clic en Columnas.

### <a name="options"></a>Opciones

**Columnas externas disponibles**

Una lista de las columnas externas disponibles que puede seleccionar para agregar a la lista **Columna externa** en el orden en que se seleccionan. Esta tabla no se puede usar para agregar o eliminar columnas.

Active la casilla **Seleccionar todo** para seleccionar todas las columnas.

**Columnas externas**

Las columnas externas (origen) que seleccionó aparecen en orden.
Para cambiar el orden, borre primero la lista **Columna externa disponible" y, luego, seleccione las columnas en otro orden.

**Columna de salida**

El nombre de la columna externa (origen) seleccionada es el nombre de salida predeterminado, aunque puede especificar cualquier nombre único.
> [!NOTE]
>
>Si hay columnas con tipos de datos no compatibles, se mostrará una advertencia que indique que no se admiten los tipos de datos y que las columnas relacionadas se quitarán de las columnas de asignación.

## <a name="oracle-source-editor-error-output-page"></a>Editor de origen de Oracle (página Salida de error)

Use la página **Salida de error** del cuadro de diálogo **Editor de origen de Oracle** para seleccionar las opciones de control de errores.

**Para abrir la página Salida de error del Editor de origen de Oracle**

- En SQL Server Data Tools, abra el paquete de SQL Server Integration Services (SSIS) que tiene el origen de Oracle.

- En la pestaña Flujo de datos, haga doble clic en el origen de Oracle.

- En el Editor de origen de Oracle, haga clic en Salida de error.

### <a name="options"></a>Opciones

**Comportamiento de error**

Seleccione la forma en la que el origen de Oracle debe controlar los errores de un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.
**Sección relacionada**: [Control de errores en los datos](./error-handling-in-data.md)

**Truncamiento**

Seleccione la forma en la que el origen de Oracle debe controlar el truncamiento de un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.

## <a name="next-steps"></a>Pasos siguientes

- Configure el [destino de Oracle](oracle-destination.md).
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA5u35j).
