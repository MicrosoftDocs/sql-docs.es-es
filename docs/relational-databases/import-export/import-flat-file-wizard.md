---
title: Importación de archivos planos en SQL | Microsoft Docs
description: El Asistente para la importación de archivos planos es una forma muy sencilla de copiar datos de un archivo .csv o .txt a una nueva tabla de la base de datos. En este artículo se muestra cómo y cuándo usar el asistente.
ms.custom: ''
ms.date: 09/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: data-movement
ms.topic: conceptual
f1_keywords:
- sql13.swb.importflatfile.f1
author: yualan
ms.author: alayu
ms.reviewer: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 444489bc74106ca43d645e26564543b4452ac8ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351354"
---
# <a name="import-flat-file-to-sql-wizard"></a>Importación de archivos planos mediante el asistente de SQL
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
> Para obtener más información sobre el Asistente para importación y exportación, consulte [Asistente para importación y exportación de SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

El Asistente para la importación de archivos planos es una forma muy sencilla de copiar datos de un archivo plano (.csv, .txt) a una nueva tabla de la base de datos.  El Asistente para importar archivos planos admite archivos separados por comas y de ancho fijo. En la información general puede consultar los motivos por los que el asistente resulta tan útil, el proceso para encontrarlo y un ejemplo que podrá seguir fácilmente.

## <a name="why-would-i-use-this-wizard"></a>¿Por qué el asistente es tan útil?
El objetivo del asistente es mejorar la experiencia de importación actual y ofrece un marco inteligente conocido como Program Synthesis using Examples ([PROSE](https://microsoft.github.io/prose/)). Para un usuario sin conocimientos especializados en el tema de los dominios, importar datos puede ser una tarea compleja y tediosa, además de que se suelen producir errores. Este asistente permite simplificar el proceso de importación, ya que basta con seleccionar un archivo de entrada y un nombre de tabla único. El marco de PROSE se encarga del resto.

PROSE analiza los patrones de datos del archivo de entrada e infiere los nombres de las columnas, los tipos y los delimitadores, entre otros valores. El marco memoriza la estructura del archivo y hace el resto del trabajo para que los usuarios no tengan que preocuparse de ello.

Para comprender mejor la mejora de la experiencia de usuario del Asistente para la importación de archivos planos, vea este vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>Prerrequisitos
Esta característica está disponible en la versión 17.3 de SQL Server Management Studio (SSMS) u otras posteriores. Asegúrese de utilizar la última versión, que podrá encontrar [aquí](../../ssms/download-sql-server-management-studio-ssms.md).
 
## <a name="getting-started"></a><a id="started"></a>Introducción
Para acceder al Asistente para importación de archivos planos, siga estos pasos:

1. Abra **SQL Server Management Studio**.
2. Conéctese a una instancia de Motor de base de datos de SQL Server o el localhost.
3. Expanda **Bases de datos**, haga clic con el botón derecho en una base de datos (vea el ejemplo siguiente), apunte a **Tareas** y haga clic en **Importar archivo plano**, encima de Importar datos.

![Menú del asistente](media/import-flat-file-wizard/import-flat-file-menu.png)

Para obtener más información sobre las diferentes funciones del asistente, consulte el siguiente tutorial:

## <a name="tutorial"></a>Tutorial
Dados los objetivos que se contemplan en este tutorial, puede usar un archivo plano propio. En caso contrario, en este tutorial se usará el siguiente archivo .csv de Excel que puede copiar sin ningún problema. Si decide usar este archivo .csv, llámelo **ejemplo.csv** y guárdelo como .csv en una ubicación que pueda encontrar fácilmente, por ejemplo, el escritorio.

![Asistente: Excel](media/import-flat-file-wizard/import-flat-file-example.png)

Introducción:
1. [Asistente de acceso](#step-1-access-wizard-and-intro-page)
2. [Especificación del archivo de entrada](#step-2-specify-input-file)
3. [Vista previa de los datos](#step-3-preview-data)
4. [Modificación de columnas](#step-4-modify-columns)
5. [Resumen](#step-5-summary)
6. [Resultados](#step-6-results)

### <a name="step-1-access-wizard-and-intro-page"></a>Paso 1: Acceso al asistente y página de introducción
Acceda al asistente tal y como se describe [aquí](#started).

La primera página del asistente es la página principal. Si no quiere que se vuelva a mostrar, haga clic en **No volver a mostrar esta página**.

![Asistente: introducción](media/import-flat-file-wizard/import-flat-file-intro.png)

### <a name="step-2-specify-input-file"></a>Paso 2: Especificación del archivo de entrada
Haga clic en Examinar y seleccione el archivo de entrada. De forma predeterminada, el asistente buscará archivos .csv y .txt. PROSE detectará si el archivo está separado por comas o tiene un ancho fijo independientemente de la su extensión.

El nuevo nombre de la tabla debe ser único, ya que, en caso contrario, el asistente no le permitirá avanzar.

![Asistente: especificación](media/import-flat-file-wizard/import-flat-file-specify.png)

### <a name="step-3-preview-data"></a>Paso 3: Vista previa de los datos
El asistente generará una vista previa para que pueda consultar las primeras 50 filas. Si hay algún problema, haga clic en Cancelar. Si no, proceda a la página siguiente.

![Asistente: vista previa](media/import-flat-file-wizard/import-flat-file-preview.png)

### <a name="step-4-modify-columns"></a>Paso 4: Modificación de columnas
El asistente identificará lo que en teoría son los nombres de columnas, los tipos de datos, etc. En caso de que los campos no sean correctos, en este paso podrá editarlos, por ejemplo, si el tipo de datos debe ser "float" en lugar de "int".

Las columnas en las que se detecten valores vacíos tendrán activada la casilla "Permitir valores NULL". Sin embargo, si espera valores NULL en una columna y no ha activado "Permitir valores NULL", aquí puede actualizar la definición de la tabla para permitir los valores NULL en una o todas las columnas.

Una vez que todo sea correcto, continúe.

![Asistente: modificación](media/import-flat-file-wizard/import-flat-file-modify.png)

### <a name="step-5-summary"></a>Paso 5: Resumen
Esta página es simplemente un resumen en el que figura la configuración actual. Si hay algún problema puede volver a las secciones anteriores. Si no los hay, haga clic en Finalizar para intentar efectuar la importación.

![Asistente: resumen](media/import-flat-file-wizard/import-flat-file-summary.png)

### <a name="step-6-results"></a>Paso 6: Results
En esta página se indica si la importación se ha realizado correctamente. Si la operación se ha efectuado correctamente, aparecerá una marca de verificación verde. De lo contrario, deberá comprobar que no haya errores con los archivos de entrada o con la configuración.

![Asistente: resultados](media/import-flat-file-wizard/import-flat-file-results.png)

## <a name="troubleshooting"></a>Solución de problemas
El Asistente para la importación de archivos planos detecta los tipos de datos basados en las primeras 200 filas.  En escenarios en los que los datos adicionales del archivo plano no se ajustan a los tipos de datos detectados automáticamente, se producirá un error durante la importación. El mensaje de error sería similar al siguiente:
```
Error inserting data into table. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type nvarchar of the specified target column. (System.Data)
String or binary data would be truncated. (System.Data)
```
Tácticas para solucionar este error:
- La expansión de los tamaños de los tipos de datos en el [paso Modificar columnas](#step-4-modify-columns), como la longitud de una columna nvarchar, puede compensar las variaciones en los datos del resto del archivo plano.
- Al habilitar informes de errores en el [paso Modificar columnas](#step-4-modify-columns), especialmente en un número menor, se revelará qué filas del archivo plano contienen datos que no se ajustan a los tipos de datos seleccionados. Por ejemplo, en un archivo plano en el que la segunda fila presenta un error, la ejecución de la importación con informes de errores con un intervalo de 1 proporciona un mensaje de error específico.  Examinar el archivo directamente en la ubicación puede proporcionar cambios más dirigidos en los tipos de datos basados en los datos de las filas identificadas.

![Resultados de informes de errores](media/import-flat-file-wizard/import-flat-file-error.png)

```
Error inserting data into table occured while inserting rows 1 - 2. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type float of the specified target column. (System.Data)
Failed to convert parameter value from a String to a Double. (System.Data)
```


## <a name="learn-more"></a>Más información

Obtenga más información sobre el asistente.
 
- **Obtenga más información sobre cómo importar otros orígenes**. Si quiere importar otros recursos además de archivos planos, consulte [Asistente para importación y exportación de SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).
- **Obtenga más información sobre cómo conectar con orígenes de archivos planos**. Si quiere más información sobre cómo conectar con orígenes de archivos planos, consulte [Connect to a Flat File Data Source](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md) (Conexión con orígenes de datos de archivos planos).
- **Obtenga más información sobre PROSE**. Si quiere información general sobre el marco inteligente que usa este asistente, consulte [PROSE SDK](https://microsoft.github.io/prose/) (SDK de PROSE).