---
title: 'Inicio rápido: Ejecutar scripts de R'
titleSuffix: SQL machine learning
description: Ejecute scripts de R sencillos con aprendizaje automático de SQL. Obtenga información sobre cómo usar el procedimiento almacenado sp_execute_external_script para ejecutar el script.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: eaecfcf9cd4944d48099a5bf1582e8144c1f62e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272996"
---
# <a name="quickstart-run-simple-r-scripts-with-sql-machine-learning"></a>Inicio rápido: Ejecución de scripts de R sencillos con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En este inicio rápido, ejecutará un conjunto de scripts de R sencillos mediante [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) o [clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md). Aprenderá a usar el procedimiento almacenado [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para ejecutar el script en una instancia de SQL Server.
::: moniker-end
::: moniker range="=sql-server-2017"
En este inicio rápido, ejecutará un conjunto de scripts de R sencillos mediante [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Aprenderá a usar el procedimiento almacenado [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para ejecutar el script en una instancia de SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
En este inicio rápido, ejecutará un conjunto de scripts de R sencillos mediante [SQL Server R Services](../r/sql-server-r-services.md). Aprenderá a usar el procedimiento almacenado [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para ejecutar el script en una instancia de SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En este inicio rápido, ejecutará un conjunto de scripts de R sencillos mediante [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview). Descubrirá cómo usar el procedimiento almacenado [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para ejecutar el script en la base de datos.
::: moniker-end

## <a name="prerequisites"></a>Prerrequisitos

Para ejecutar este inicio rápido, debe cumplir los siguientes requisitos previos.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server Machine Learning Services. Para instalar Machine Learning Services, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md) o la [Guía de instalación para Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). También puede [habilitar Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server Machine Learning Services. Para instalar Machine Learning Services, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services. Para instalar R Services, consulte la [Guía de instalación de Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Machine Learning Services en Azure SQL Managed Instance. Para obtener información, vea [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Una herramienta para ejecutar consultas de SQL que contengan scripts de R. En este inicio rápido se utiliza [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Ejecución de un script sencillo

Para ejecutar un script de R, necesita pasarlo como un argumento al procedimiento almacenado del sistema, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Este procedimiento almacenado del sistema inicia el entorno de ejecución de R, pasa datos a R, administra de forma segura las sesiones de usuario de R y devuelve los resultados al cliente.

En los pasos siguientes, deberá ejecutar este script de R de ejemplo:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Abra **Azure Data Studio** y conéctese al servidor.

1. Pase todo el script de R al procedimiento almacenado `sp_execute_external_script`.

   El script se pasa mediante el argumento `@script`. Todo lo que contenga el argumento `@script` tiene que ser código de R válido.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Se calcula el resultado correcto y la función de R `print` devuelve el resultado a la ventana **Mensajes**.

   Necesita tener el siguiente aspecto.

    **Resultados**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Ejecución de un script Hola mundo

Un script de ejemplo típico es uno que simplemente muestra la cadena "Hola mundo". Ejecute el siguiente comando:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Entre las entradas para el procedimiento almacenado `sp_execute_external_script`, se incluyen las siguientes:

| Entrada | Descripción |
|-|-|
| @language | define la extensión del lenguaje a la que se llamará (en este caso, R). |
| @script | define los comandos que se pasarán al entorno de ejecución de R. Su script de R completo debe estar incluido en este argumento, como texto Unicode. También puede agregar texto a una variable del tipo **nvarchar** y, después, llamar a la variable. |
| @input_data_1 | Los datos devueltos por la consulta se pasan al entorno de ejecución de R, que los devuelve a su vez como una trama de datos. |
|WITH RESULT SETS | Cláusula que define el esquema de la tabla de datos devuelta (se agrega "Hola mundo" como el nombre de columna e **int** para el tipo de datos). |

El comando muestra el texto siguiente:

| Hola mundo |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Uso de entradas y salidas

De forma predeterminada, `sp_execute_external_script` acepta un único conjunto de datos como entrada, que se suele proporcionar como una consulta SQL válida. Después, devuelve una única trama de datos de R como salida.

Por ahora, usaremos las variables de entrada y salida predeterminadas de `sp_execute_external_script`: **InputDataSet** y **OutputDataSet**.

1. Cree una tabla pequeña con datos de prueba.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Use la instrucción `SELECT` para consultar la tabla.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Resultados**

    ![Contenido de la tabla RTestData](./media/select-rtestdata.png)

1. Ejecute el siguiente script de R. Recupera los datos de la tabla mediante la instrucción `SELECT`, los pasa mediante el entorno de ejecución de R y devuelve los datos como una trama de datos. La cláusula `WITH RESULT SETS` define el esquema de la tabla de datos devuelta para SQL y agrega el nombre de columna *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Resultados**

    ![Salida del script de R que devuelve datos de una tabla](./media/r-output-rtestdata.png)

1. Ahora, cambie los nombres de las variables de entrada y salida. Los nombres predeterminados de las variables de entrada y salida son **InputDataSet** y **OutputDataSet**; este script cambia los nombres a **SQL_in** y **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Tenga en cuenta que R distingue mayúsculas de minúsculas. Las variables de entrada y salida usadas en el script de R (**SQL_out** y **SQL_in**) tienen que coincidir con los nombres definidos con `@input_data_1_name` y `@output_data_1_name`, incluido el uso de mayúsculas.

   > [!TIP]
   > Solo se puede pasar un conjunto de datos de entrada como parámetro, y solo se puede devolver un conjunto de datos. Sin embargo, puede llamar a otros conjuntos de datos desde el interior del código R y puede devolver salidas de otros tipos además del conjunto de datos. También puede agregar la palabra clave OUTPUT a cualquier parámetro para que se devuelva con los resultados.

1. También puede generar valores con el script de R sin datos de entrada (`@input_data_1` se establece en blanco).

   El script siguiente genera el texto "hola" y "mundo".

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Resultados**

    ![Resultados de consultas con @script como entrada](./media/r-data-generated-output.png)

## <a name="check-r-version"></a>Comprobación de la versión de R

Si quiere ver qué versión de R está instalada, ejecute el script siguiente.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

La función `print` de R devuelve la versión en la ventana **Mensajes**. En la salida de ejemplo siguiente, puede ver que, en este caso, la versión de R instalada es 3.4.4.

**Resultados**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>Lista de paquetes de R
::: moniker range=">=sql-server-2017"
Microsoft proporciona varios paquetes de R preinstalados con Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Microsoft proporciona varios paquetes de R preinstalados con R Services.
::: moniker-end

Para ver una lista de los paquetes de R instalados (además de la versión, las dependencias, la licencia y la información de la ruta de la biblioteca), ejecute el script siguiente.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

El resultado es `installed.packages()` de R y se devuelve como un conjunto de resultados.

**Resultados**

![Paquetes instalados en R](./media/rsql-installed-packages.png) 

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo usar estructuras de datos al usar R en aprendizaje automático de SQL, siga este inicio rápido:

> [!div class="nextstepaction"]
> [Administración de tipos de datos y objetos mediante R en aprendizaje automático de SQL](quickstart-r-data-types-and-objects.md)