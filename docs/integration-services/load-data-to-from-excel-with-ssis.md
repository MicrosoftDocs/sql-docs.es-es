---
title: Importación desde Excel o exportación a Excel con SSIS | Microsoft Docs
description: Obtenga información sobre cómo importar o exportar datos de Excel con SQL Server Integration Services (SSIS), junto con los requisitos previos, los problemas conocidos y las limitaciones.
ms.date: 06/29/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e497e0f2781aedc2adccc4263cc3b37e756a86da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345702"
---
# <a name="import-data-from-excel-or-export-data-to-excel-with-sql-server-integration-services-ssis"></a>Importación de datos desde Excel o exportación de datos a Excel con SQL Server Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



En este artículo se describe la información de conexión que debe proporcionar y los valores que debe configurar para importar datos desde Excel o exportar datos a Excel con SQL Server Integration Services (SSIS).

En las secciones siguientes se incluye la información necesaria para usar Excel correctamente con SSIS y para entender los problemas comunes y solucionarlos:

1.  Las [herramientas](#tools) que puede usar.

2.  Los [archivos](#files-you-need) que necesita.

3.  La información de conexión que debe proporcionar y los valores que tiene que configurar cuando carga datos desde y hacia Excel con SSIS.
    -   [Especifique Excel](#specify-excel) como origen de los datos.
    -   Proporcione el [nombre de archivo y ruta de acceso de Excel](#excel-file).
    -   Seleccione la [versión de Excel](#excel-version).
    -   Especifique si la [primera fila de datos contiene nombres de columna](#first-row).
    -   Proporcione la [hoja de cálculo o rango que contiene los datos](#sheets-ranges).

4.  Limitaciones y problemas conocidos.
    -   Problemas con los [tipos de datos](#issues-types).
    -   Problemas de [importación](#issues-importing).
    -   Problemas de [exportación](#issues-exporting).

## <a name="tools-you-can-use"></a><a name="tools"></a> Herramientas que puede usar

Puede importar o exportar datos hacia o desde Excel con SSIS con una de estas herramientas:

-   **SQL Server Integration Services (SSIS)** . Cree un paquete SSIS que use el origen o el destino de Excel con el Administrador de conexiones de Excel. (En este artículo no se describe cómo crear paquetes SSIS).

-   El **Asistente para importación o exportación de SQL Server**, que está integrado en SSIS. Para más información, consulte [Importar y exportar datos con el Asistente para importación y exportación de SQL Server](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) y [Conectarse a un origen de datos de Excel (Asistente para importación y exportación de SQL Server)](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md).

## <a name="get-the-files-you-need-to-connect-to-excel"></a><a name="files-you-need"></a> Obtener los archivos necesarios para conectarse a Excel

Para poder importar datos desde Excel o exportarlos a Excel con SSIS, tendrá que descargar los componentes de conectividad de Excel si no están instalados. Los componentes de conectividad de Excel no están instalados de forma predeterminada.

Descargue la versión más reciente de los componentes de conectividad de Excel aquí: [Microsoft Access Database Engine 2016 Redistributable](https://www.microsoft.com/download/details.aspx?id=54920). La versión más reciente de los componentes puede abrir los archivos creados con versiones anteriores de los programas de Excel.

### <a name="notes-about-the-download-and-installation"></a>Notas sobre la descarga y la instalación

-   Asegúrese de descargar Access Database Engine 2016 *Redistributable* y no Microsoft Access 2016 *Runtime*.

-   Si el equipo ya tiene una versión de 32 bits de Office, tendrá que instalar la versión de 32 bits de los componentes. También tendrá que asegurarse de ejecutar el paquete de SSIS en modo de 32 bits o de ejecutar la versión de 32 bits del Asistente para importación y exportación.

-   Si tiene una suscripción de Microsoft 365, puede que vea un mensaje de error al ejecutar el programa de instalación. El error indica que no se puede instalar la descarga en paralelo con componentes para hacer clic y ejecutar de Office. Para omitir este mensaje de error, ejecute la instalación en modo silencioso abriendo una ventana del símbolo del sistema y ejecutando el archivo .EXE que descargó con el modificador `/quiet`. Por ejemplo:

    `C:\Users\<user_name>\Downloads\AccessDatabaseEngine.exe /quiet`

    Si tiene problemas para instalar la versión 2016 Redistributable, instale la versión 2010 Redistributable desde aquí: [Microsoft Access Database Engine 2010 Redistributable](https://www.microsoft.com/download/details.aspx?id=13255) (no hay ninguna versión redistribuible para Excel 2013).

## <a name="specify-excel-as-your-data-source"></a><a name="specify-excel"></a> Especificar Excel como el origen de los datos

El primer paso consiste en indicar que desea conectarse a Excel.

### <a name="in-ssis"></a>En SSIS
En SSIS, cree un administrador de conexiones de Excel para conectarse al archivo de origen o de destino de Excel. Existen varias formas de crear el administrador de conexiones:

-   En el área **Administradores de conexión**, haga clic con el botón derecho y seleccione **Nueva conexión**. En el cuadro de diálogo **Agregar administrador de conexiones SSIS**, seleccione **EXCEL** y, después, **Agregar**.
 
-   En el menú **SSIS**, seleccione **Nueva conexión**. En el cuadro de diálogo **Agregar administrador de conexiones SSIS**, seleccione **EXCEL** y, después, **Agregar**.

-   Cree el administrador de la conexión al mismo tiempo que configure el **origen de Excel** o el **destino de Excel** en la página **Administrador de conexiones** del  **Editor de origen de Excel** o del **Editor de destino de Excel**.

### <a name="in-the-sql-server-import-and-export-wizard"></a>En el Asistente para importación y exportación de SQL Server
En el Asistente para importación y exportación, en la página **Elegir un origen de datos** o **Elegir un destino**, seleccione **Microsoft Excel** en la lista de **orígenes de datos**.

Si no ve Excel en la lista de orígenes de datos, asegúrese de que está ejecutando el asistente de 32 bits. Los componentes de conectividad de Excel suelen ser archivos de 32 bits que no están visibles en el asistente de 64 bits.

## <a name="excel-file-and-file-path"></a><a name="excel-file"></a> Archivo y ruta de acceso del archivo de Excel

Lo primero que se debe proporcionar es la ruta de acceso y el nombre del archivo de Excel. Debe proporcionar esta información en el **Editor del administrador de conexiones de Excel** de un paquete de SSIS o bien en la página **Elegir un origen de datos** o **Elegir un destino** del Asistente para importación y exportación.

Escriba la ruta de acceso y el nombre de archivo con el siguiente formato:

-   En el caso de un archivo ubicado en el equipo local, **C:\\TestData.xlsx**.

-   En el caso de un archivo ubicado en un recurso compartido de red, **\\\\Sales\\Data\\TestData.xlsx**.

También puede hacer clic en **Examinar** para buscar la hoja de cálculo usando el cuadro de diálogo **Abrir**.  
  
> [!IMPORTANT]
> No se puede conectar a un archivo de Excel protegido mediante contraseña.

## <a name="excel-version"></a><a name="excel-version"></a> Versión de Excel

El segundo dato que debe proporcionar es la versión del archivo de Excel. Debe proporcionar esta información en el **Editor del administrador de conexiones de Excel** de un paquete de SSIS o bien en la página **Elegir un origen de datos** o **Elegir un destino** del Asistente para importación y exportación.

Seleccione la versión de Microsoft Excel usada para crear el archivo u otra versión compatible. Por ejemplo, si ha tenido problemas para instalar los componentes de conectividad de la versión 2016, puede instalar los componentes de la versión 2010 y seleccionar **Microsoft Excel 2007-2010** en esta lista.

Es posible que no pueda seleccionar las versiones más recientes de Excel de la lista si solo tiene instaladas versiones anteriores de los componentes de conectividad. La lista de **versiones de Excel** incluye todas las versiones de Excel compatibles con SSIS. La presencia de elementos en esta lista no indica que los componentes de conectividad necesarios estén instalados. Por ejemplo, **Microsoft Excel 2016** aparece en la lista aunque no tenga instalados los componentes de conectividad de la versión 2016.

## <a name="first-row-has-column-names"></a><a name="first-row"></a> La primera fila tiene nombres de columna

Si va a importar datos desde Excel, el siguiente paso consiste en indicar si la primera fila de los datos contiene nombres de columna. Debe proporcionar esta información en el **Editor del administrador de conexiones de Excel** de un paquete de SSIS o bien en la página **Elegir un origen de datos** del Asistente para importación y exportación.

-   Si deshabilita esta opción porque el origen de datos no contiene nombres de columna, el asistente usará F1, F2, etc. como encabezados de columna.
-   Si los datos contienen nombres de columna, pero deshabilita esta opción, el asistente importará los nombres de columna como la primera fila de datos.
-   Si los datos no contienen nombres de columna, pero se habilita esta opción, el asistente usa la primera fila de los datos de origen como los nombres de columna. En este caso, la primera fila de los datos de origen ya no se incluirá en los propios datos.

Si va a exportar datos desde Excel y habilita esta opción, la primera fila de los datos exportados incluirá los nombres de columna.

## <a name="worksheets-and-ranges"></a><a name="sheets-ranges"></a> Hojas de cálculo y rangos

Hay tres tipos de objetos de Excel que se pueden usar como origen o destino de los datos: una hoja de cálculo, un rango con nombre o un rango de celdas sin nombre que se especifique con su dirección.

-   **Hoja de cálculo.** Para especificar una hoja de cálculo, anexe el carácter `$` al final del nombre de la hoja y agregue delimitadores alrededor de la cadena (por ejemplo, **[Hoja1$]** ). También puede buscar un nombre que acabe con el carácter `$` en la lista de las tablas y vistas existentes.

-   **Rango con nombre.** Para especificar un rango con nombre, proporcione el nombre del rango (por ejemplo, **MiRangoDeDatos**). También puede buscar un nombre que no acabe con el carácter `$` en la lista de las tablas y vistas existentes.
    
-   **Rango sin nombre.** Para especificar un rango de celdas al que no ha asignado un nombre, anexe el carácter $ al final del nombre de la hoja, agregue la especificación del rango y agregue delimitadores alrededor de la cadena (por ejemplo, **[Hoja1$A1:B4]** ).

Para seleccionar o especificar el tipo de objeto de Excel que quiere usar como origen o destino de los datos, lleve a cabo una de las siguientes acciones:

### <a name="in-ssis"></a>En SSIS

En SSIS, en la página **Administrador de conexiones** del **Editor de origen de Excel** o del **Editor de destino de Excel**, lleve a cabo una de las siguientes acciones:

-   Para usar una **hoja de cálculo** o un **rango con nombre**, seleccione **Tabla o vista** como **modo de acceso a datos**. Luego, en la lista **Nombre de la hoja de Excel**, seleccione la hoja de cálculo o el rango con nombre.

-   Para usar un **rango sin nombre** que se especifique con su dirección, seleccione **Comando SQL** como **modo de acceso a datos**. Luego, en el campo **Texto de comando SQL**, escriba una consulta similar al siguiente ejemplo:

    ```sql
    SELECT * FROM [Sheet1$A1:B5]
    ```

### <a name="in-the-sql-server-import-and-export-wizard"></a>En el Asistente para importación y exportación de SQL Server
En el Asistente para importación y exportación, lleve a cabo una de las siguientes acciones:

-   Si va a **importar** desde Excel, lleve a cabo una de las siguientes acciones:

    -   Para usar una **hoja de cálculo** o un **rango con nombre**, en la página **Especificar copia de tabla o consulta**, seleccione **Copiar los datos de una o varias tablas o vistas**. Luego, en la página **Seleccionar tablas y vistas de origen**, en la columna **Origen**, seleccione las hojas de cálculo y los rangos con nombre de origen.

    -   Para usar un **rango sin nombre** que se especifique con su dirección, en la página **Especificar copia de tabla o consulta**, seleccione **Escribir una consulta para especificar los datos que se van a transferir**. Luego, en la página **Proporcionar una consulta de origen**, proporcione una consulta similar al ejemplo siguiente:

        ```sql
        SELECT * FROM [Sheet1$A1:B5]
        ```

-   Si va a **exportar** a Excel, lleve a cabo una de las siguientes acciones:

    -   Para usar una **hoja de cálculo** o un **rango con nombre**, en la página **Seleccionar tablas y vistas de origen**, en la columna **Destino**, seleccione las hojas de cálculo y los rangos con nombre de destino.

    -   Para usar un **rango sin nombre** que se especifique con su dirección, en la página **Seleccionar tablas y vistas de origen**, en la columna **Destino**, indique el rango con el siguiente formato y sin delimitadores: `Sheet1$A1:B5`. El asistente agrega los delimitadores.

Después de seleccionar o de escribir los objetos de Excel que se van a importar o exportar, también puede llevar a cabo las siguientes acciones en la página **Seleccionar tablas y vistas de origen** del asistente:

-   Revisar las asignaciones de columnas entre el origen y el destino seleccionando **Editar asignaciones**.

-   Obtener una vista previa de los datos de ejemplo para asegurarse de que es lo que esperaba. Para ello, debe seleccionar **Vista previa**.

## <a name="issues-with-data-types"></a><a name="issues-types"></a> Problemas con los tipos de datos

### <a name="data-types"></a>Tipos de datos

El controlador de Excel reconoce solo un conjunto limitado de tipos de datos. Por ejemplo, todas las columnas numéricas se interpretan como dobles (DT_R8) y todas las columnas de cadena (a excepción de las columnas memorando) se interpretan como cadenas Unicode de 255 caracteres (DT_WSTR). SSIS asigna los tipos de datos de Excel de la siguiente manera:

-   Numérico flotante de doble precisión (DT_R8)

-   Moneda: moneda (DT_CY)

-   Booleano: booleano (DT_BOOL)

-   Fecha y hora: datetime (DT_DATE)

-   Cadena: cadena Unicode, longitud de 255 caracteres (DT_WSTR)

-   Memorando: flujo de texto Unicode (DT_NTEXT)

### <a name="data-type-and-length-conversions"></a>Conversiones de tipo de datos y de longitud

SSIS no convierte los tipos de datos de forma implícita. Como resultado, probablemente deba usar las transformaciones Columna derivada o Conversión de datos para convertir datos de Excel de forma explícita antes de cargarlos en un destino que no sea Excel, o para convertir datos de un origen que no sea Excel antes de cargarlos en un destino de Excel.

A continuación se muestran algunos ejemplos de las conversiones que pueden ser necesarias:  
  
-   Conversión entre columnas de cadena de Excel Unicode y columnas de cadena no Unicode con una página de códigos específica.
  
-   Conversión entre columnas de cadena de Excel de 255 caracteres y columnas de cadena de diferentes longitudes.
  
-   Conversión entre columnas numéricas de Excel de doble precisión y columnas numéricas de otros tipos.

> [!TIP]
> Si usa el Asistente para importación y exportación y los datos requieren algunas de estas conversiones, el asistente configurará automáticamente las conversiones necesarias. Como resultado, incluso cuando quiera usar un paquete de SSIS, puede resultar útil crear el paquete inicial con el Asistente para importación y exportación. Deje que el asistente cree y configure de forma automática los administradores de conexión, los orígenes, las transformaciones y los destinos.

## <a name="issues-with-importing"></a><a name="issues-importing"></a> Problemas de importación

### <a name="empty-rows"></a>Filas vacías

Al especificar una hoja de cálculo o un rango con nombre como origen, el controlador lee los bloques *contiguos* de celdas, comenzando con la primera celda no vacía en la esquina superior izquierda de la hoja de cálculo o rango. Como resultado, no es necesario que los datos empiecen en la fila 1, aunque no puede haber filas vacías en los datos de origen. Por ejemplo, no puede tener una fila vacía entre los encabezados de columna y las filas de datos, así como tampoco un título seguido por filas vacías en la parte superior de la hoja de cálculo.

Si hay filas vacías por encima de los datos, no podrá consultar los datos como una hoja de cálculo. En Excel tiene que seleccionar el rango de datos, asignarle un nombre y, después, consultar el rango con nombre en vez de la hoja de cálculo.

### <a name="missing-values"></a>Valores que faltan

Este controlador lee un cierto número de filas (de forma predeterminada, ocho filas) en el origen especificado para elegir el tipo de datos de cada columna. Cuando una columna parece contener varios tipos de datos, especialmente numéricos y de texto, el controlador elige el tipo más usado y devuelve valores NULL en las celdas que contienen datos de los demás tipos. (En caso de empate, el tipo numérico tiene prioridad.) La mayoría de las opciones de formato en las hojas Excel parecen no afectar a esta determinación de tipos de datos.

Para modificar este comportamiento del controlador de Excel, especifique Modo de importación para importar todos los valores como texto. Para especificar el modo de importación, agregue `IMEX=1` al valor de **Propiedades extendidas** en la cadena de conexión del Administrador de conexiones con Excel en la ventana Propiedades. 

### <a name="truncated-text"></a>Texto truncado

Cuando el controlador determina que una columna de Excel contiene datos de texto, el controlador selecciona el tipo de datos (cadena o memorando) en función del valor más largo que muestrea. Si el controlador no descubre valores de más de 255 caracteres en las filas que muestrea, trata a la columna como una columna de cadena de 255 caracteres en lugar de una columna memorando. Así, es posible que se trunquen las cadenas con más de 255 caracteres.

Para importar datos de una columna de memorando sin que se trunquen, tiene dos opciones:

-   Asegúrese de que la columna de memorando de al menos una de las filas muestreadas contenga un valor de más de 255 caracteres

-   Aumente el número de filas muestreadas por el controlador para que incluya dicha fila. Puede incrementar el número de filas muestreadas aumentando el valor de **TypeGuessRows** en la siguiente clave del Registro:

| Versión de los componentes redistribuibles | Clave del Registro |
|---|---|
| Excel 2016 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\16.0\Access Connectivity Engine\Engines\Excel |
| Excel 2010 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\14.0\Access Connectivity Engine\Engines\Excel |
| | |

## <a name="issues-with-exporting"></a><a name="issues-exporting"></a> Problemas de exportación

### <a name="create-a-new-destination-file"></a>Crear un archivo de destino

#### <a name="in-ssis"></a>En SSIS

Cree un administrador de conexiones de Excel con la ruta de acceso y el nombre del nuevo archivo de Excel que quiere crear. Luego, en el **Editor de destino de Excel**, en **Nombre de la hoja de Excel**, seleccione **Nuevo** para crear la hoja de cálculo de destino. En este punto SSIS crea el archivo de Excel con la hoja de cálculo especificada.

#### <a name="in-the-sql-server-import-and-export-wizard"></a>En el Asistente para importación y exportación de SQL Server

En la página **Elegir un destino**, seleccione **Examinar**. En el cuadro de diálogo **Abrir**, vaya a la carpeta en la que quiere que se cree el archivo de Excel, proporcione un nombre para el archivo y seleccione **Abrir**.

### <a name="export-to-a-large-enough-range"></a>Exportar a un rango que sea suficientemente grande

Al especificar un rango como destino, se produce un error si el rango tiene menos *columnas* que el origen de datos. En cambio, si el rango que especifica tiene menos *filas* que los datos de origen, el asistente sigue escribiendo filas sin errores y extiende la definición del rango para que coincida con el nuevo número de filas.

### <a name="export-long-text-values"></a>Exportar valores de texto largo

Para poder guardar correctamente cadenas de más de 255 caracteres en una columna de Excel, el controlador debe reconocer el tipo de datos de la columna de destino como **memorando** y no como **cadena**.

-   Si una tabla de destino existente ya contiene filas de datos, las primeras filas que muestree el controlador deberán contener por lo menos una instancia de un valor de más de 255 caracteres en la columna de memorando.

## <a name="related-content"></a>Contenido relacionado

Para obtener más información sobre los componentes y procedimientos descritos en este artículo, vea los siguientes artículos:

### <a name="about-ssis"></a>Acerca de SSIS
[Administrador de conexiones de Excel](connection-manager/excel-connection-manager.md)  
[Origen de Excel](data-flow/excel-source.md)  
[Destino de Excel](data-flow/excel-destination.md)  
[Crear bucles entre archivos y tablas de Excel mediante un contenedor de bucles ForEach](control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)  
[Trabajar con archivos de Excel con la tarea Script](extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)

### <a name="about-the-sql-server-import-and-export-wizard"></a>Acerca del Asistente para importación y exportación de SQL Server
[Conectarse a un origen de datos de Excel](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)  
[Comenzar con este sencillo ejemplo del Asistente para importar y exportar](import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)

### <a name="other-articles"></a>Otros artículos
[Importación de datos de Excel a SQL Server o Azure SQL Database](../relational-databases/import-export/import-data-from-excel-to-sql.md)  
