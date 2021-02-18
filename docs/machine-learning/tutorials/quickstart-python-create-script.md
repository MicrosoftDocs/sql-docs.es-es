---
title: 'Inicio rápido: Ejecución de scripts de Python'
titleSuffix: SQL machine learning
description: Ejecute un conjunto de scripts de Python sencillos con Machine Learning Services en SQL Server, en clústeres de macrodatos o en instancias de Azure SQL Managed Instance. Obtenga información sobre cómo usar el procedimiento almacenado sp_execute_external_script para ejecutar el script.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q1
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: ddee800cdba3a90414b0ee9d3eb9638b9a343e67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351528"
---
# <a name="quickstart-run-simple-python-scripts-with-sql-machine-learning"></a>Inicio rápido: Ejecución de scripts de Python sencillos con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

En este inicio rápido, ejecutará un conjunto de scripts de Python sencillos mediante [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) o [Clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md). Aprenderá a usar el procedimiento almacenado [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para ejecutar el script en una instancia de SQL Server.

## <a name="prerequisites"></a>Prerrequisitos

Para ejecutar este inicio rápido, debe cumplir los siguientes requisitos previos.

- Una base de datos SQL en una de estas plataformas:
  - [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Para realizar la instalación, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md) o la [Guía de instalación para Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Clústeres de macrodatos de SQL Server. Consulte [Habilitación de Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services en Azure SQL Managed Instance. Para obtener información, vea [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Una herramienta para ejecutar consultas de SQL que contengan scripts de Python. En este inicio rápido se utiliza [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Ejecución de un script sencillo

Para ejecutar un script de Python, necesita pasarlo como argumento al procedimiento almacenado del sistema, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Este procedimiento almacenado del sistema inicia el entorno de ejecución de Python en el contexto de aprendizaje automático de SQL, pasa datos a Python, administra de forma segura las sesiones de usuario de Python y devuelve los resultados al cliente.

En los pasos siguientes, deberá ejecutar este script de Python de ejemplo en la base de datos:

```python
a = 1
b = 2
c = a/b
d = a*b
print(c, d)
```

1. Abra una nueva ventana de consulta en **Azure Data Studio** conectada a su instancia de SQL.

1. Pase todo el script de Python al procedimiento almacenado `sp_execute_external_script`.

   El script se pasa mediante el argumento `@script`. Todo lo que contenga el argumento `@script` tiene que ser código de Python válido.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Se calcula el resultado correcto y la función `print` de Python devuelve el resultado a la ventana **Mensajes**.

   Necesita tener el siguiente aspecto.

    **Resultados**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Ejecución de un script Hola mundo

Un script de ejemplo típico es uno que simplemente muestra la cadena "Hola mundo". Ejecute el siguiente comando:

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'OutputDataSet = InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Entre las entradas para el procedimiento almacenado `sp_execute_external_script`, se incluyen las siguientes:

| Entrada | Descripción |
|-|-|
| @language | define la extensión del lenguaje de la llamada (en este caso, Python). |
| @script | define los comandos que se pasarán al entorno de ejecución de Python. Es necesario incluir todo el script de Python en este argumento como texto Unicode. También puede agregar texto a una variable del tipo **nvarchar** y, después, llamar a la variable. |
| @input_data_1 | Los datos devueltos por la consulta se pasan al entorno de ejecución de Python, que los devuelve a su vez como una trama de datos. |
| WITH RESULT SETS | cláusula que define el esquema de la tabla de datos devuelta a aprendizaje automático de SQL (se agrega "Hola mundo" como el nombre de columna e **int** para el tipo de datos). |

El comando muestra el texto siguiente:

| Hola mundo |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Uso de entradas y salidas

De forma predeterminada, `sp_execute_external_script` acepta un único conjunto de datos como entrada, que se suele proporcionar como una consulta SQL válida. Después, devuelve una única trama de datos de Python como salida.

Por ahora, usaremos las variables de entrada y salida predeterminadas de `sp_execute_external_script`: **InputDataSet** y **OutputDataSet**.

1. Cree una tabla pequeña con datos de prueba.

    ```sql
    CREATE TABLE PythonTestData (col1 INT NOT NULL)
    
    INSERT INTO PythonTestData
    VALUES (1);
    
    INSERT INTO PythonTestData
    VALUES (10);
    
    INSERT INTO PythonTestData
    VALUES (100);
    GO
    ```

1. Use la instrucción `SELECT` para consultar la tabla.
  
    ```sql
    SELECT *
    FROM PythonTestData
    ```

    **Resultados**

    ![Contenido de la tabla PythonTestData](./media/select-pythontestdata.png)

1. Ejecute el siguiente script de Python. Recupera los datos de la tabla mediante la instrucción `SELECT`, los pasa mediante el entorno de ejecución de Python y devuelve los datos como una trama de datos. La cláusula `WITH RESULT SETS` define el esquema de la tabla de datos devuelta para SQL y agrega el nombre de columna *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'OutputDataSet = InputDataSet;'
        , @input_data_1 = N'SELECT * FROM PythonTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Resultados**

    ![Salida del script de Python que devuelve datos de una tabla](./media/python-output-pythontestdata.png)

1. Ahora, cambie los nombres de las variables de entrada y salida. Los nombres predeterminados de las variables de entrada y salida son **InputDataSet** y **OutputDataSet**; el script siguiente cambia los nombres a **SQL_in** y **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'SQL_out = SQL_in;'
        , @input_data_1 = N'SELECT 12 as Col;'
        , @input_data_1_name  = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Tenga en cuenta que Python distingue mayúsculas de minúsculas. Las variables de entrada y salida usadas en el script de Python (**SQL_out** y **SQL_in**) tienen que coincidir con los nombres definidos con `@input_data_1_name` y `@output_data_1_name`, incluido el uso de mayúsculas.

    > [!TIP]
    > Solo se puede pasar un conjunto de datos de entrada como parámetro, y solo se puede devolver un conjunto de datos. Pero puede llamar a otros conjuntos de datos desde dentro del código de Python y devolver salidas de otros tipos, además del conjunto de datos. También puede agregar la palabra clave OUTPUT a cualquier parámetro para que se devuelva con los resultados.

1. También puede generar valores con el script de Python sin datos de entrada (`@input_data_1` se establece en blanco).

    El script siguiente genera el texto "hola" y "mundo".

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    import pandas as pd
    mytextvariable = pandas.Series(["hello", " ", "world"]);
    OutputDataSet = pd.DataFrame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Resultados**

    ![Resultados de consultas con @script como entrada](./media/python-data-generated-output.png)

> [!NOTE]
> Python usa espacios iniciales para agrupar instrucciones. Por lo tanto, cuando el script de Python insertado abarca varias líneas, como en el script anterior, no intente agregar sangría a los comandos de Python para que estén alineados con los comandos de SQL. Por ejemplo, este script produciría un error:
>
> ```sql
> EXECUTE sp_execute_external_script @language = N'Python'
>       , @script = N'
>       import pandas as pd
>       mytextvariable = pandas.Series(["hello", " ", "world"]);
>       OutputDataSet = pd.DataFrame(mytextvariable);
>       '
>       , @input_data_1 = N''
> WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
> ```

## <a name="check-python-version"></a>Comprobación de la versión de Python

Si quiere ver qué versión de Python está instalada en el servidor, ejecute el script siguiente.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import sys
print(sys.version)
'
GO
```

La función `print` de Python devuelve la versión en la ventana **Mensajes**. En la salida de ejemplo siguiente, puede ver que, en este caso, la versión de Python instalada es 3.5.2.

**Resultados**

```text
STDOUT message(s) from external script:
3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
```

## <a name="list-python-packages"></a>Lista de paquetes de Python

Microsoft proporciona varios paquetes de Python preinstalados con Machine Learning Services.

Para ver una lista de los paquetes de Python instalados, incluida la versión, ejecute el script siguiente.

```SQL
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import pkg_resources
import pandas
dists = [str(d) for d in pkg_resources.working_set]
OutputDataSet = pandas.DataFrame(dists)
'
WITH RESULT SETS(([Package] NVARCHAR(max)))
GO
```

La lista es de `pkg_resources.working_set` en Python y se devuelve a SQL como una trama de datos.

**Resultados**

:::image type="content" source="media/python-package-list.png" alt-text="Visualización de los paquetes de Python instalados":::

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo usar estructuras de datos al usar Python en aprendizaje automático de SQL, siga este inicio rápido:

> [!div class="nextstepaction"]
> [Inicio rápido: Objetos y estructuras de datos con Python](quickstart-python-data-structures.md)