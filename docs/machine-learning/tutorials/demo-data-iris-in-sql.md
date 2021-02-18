---
description: Datos de demostración de Iris para tutoriales de Python y R con aprendizaje automático de SQL Server
title: Conjunto de datos de demostración de iris para tutoriales
titleSuffix: SQL machine learning
Description: Cree una base de datos que contenga el conjunto de datos de iris y los modelos predictivos. Este conjunto de datos se utiliza en los tutoriales de R y Python con aprendizaje automático de SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 25cc9c7c93786dcc0d88e8bcd152196c17bf91c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347310"
---
# <a name="iris-demo-data-for-python-and-r-tutorials-with-sql-machine-learning"></a>Datos de demostración de Iris para tutoriales de Python y R con aprendizaje automático de SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En este ejercicio va a crear una base de datos para almacenar los datos del [conjunto de datos flor Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) y los modelos basados en los mismos datos. Los datos de Iris se incluyen en las distribuciones de R y Python y se usan en los tutoriales de aprendizaje automático para SQL Machine Learning.

Para completar este ejercicio, debe tener [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) u otra herramienta que pueda ejecutar consultas de T-SQL.

Entre los tutoriales y las guías de inicio rápido que usan este conjunto de datos se incluyen los siguientes:

+ [Inicio rápido: Creación y puntuación de un modelo predictivo en Python](quickstart-python-train-score-model.md)

## <a name="create-the-database"></a>Creación de la base de datos

1. Inicie SQL Server Management Studio y abra una nueva ventana **Consultas**.  

2. Cree una nueva base de datos para este proyecto y cambie el contexto de la ventana **Consulta** para usar la nueva base de datos.

    ```sql
    CREATE DATABASE irissql
    GO
    USE irissql
    GO
    ```

3. Agregue algunas tablas vacías: una para almacenar los datos y otra para almacenar los modelos entrenados. La tabla **iris_models** se utiliza para almacenar los modelos serializados generados en otros ejercicios.

    En el código siguiente se crea la tabla para los datos de entrenamiento.

    ```sql
    DROP TABLE IF EXISTS iris_data;
    GO
    CREATE TABLE iris_data (
      id INT NOT NULL IDENTITY PRIMARY KEY
      , "Sepal.Length" FLOAT NOT NULL, "Sepal.Width" FLOAT NOT NULL
      , "Petal.Length" FLOAT NOT NULL, "Petal.Width" FLOAT NOT NULL
      , "Species" VARCHAR(100) NOT NULL, "SpeciesId" INT NOT NULL
    );
    ```

4. Ejecute el código siguiente para crear la tabla utilizada para almacenar el modelo entrenado. Para guardar los modelos de Python (o R) en SQL Server, se deben serializar y almacenar en una columna de tipo **varbinary(max)** .

    ```sql
    DROP TABLE IF EXISTS iris_models;
    GO

    CREATE TABLE iris_models (
      model_name VARCHAR(50) NOT NULL DEFAULT('default model') PRIMARY KEY,
      model VARBINARY(MAX) NOT NULL
    );
    GO
    ```

    Además del contenido del modelo, normalmente también se agregan columnas para otros metadatos útiles, como el nombre del modelo, la fecha en que se entrenó, el algoritmo y los parámetros de origen, los datos de origen, etc. Por ahora, no nos complicaremos y usaremos solo el nombre del modelo.

## <a name="populate-the-table"></a>Relleno de la tabla

Puede obtener datos de iris integrados desde R o Python. Puede usar Python o R para cargar los datos en una trama de datos y después insertarlos en una tabla de la base de datos. El traslado de los datos de entrenamiento de una sesión externa a una tabla es un proceso que consta de varios pasos:

+ Diseñar un procedimiento almacenado que obtenga los datos deseados.
+ Ejecutar el procedimiento almacenado para obtener realmente los datos.
+ Crear una instrucción INSERT para especificar dónde deben guardarse los datos recuperados.

1. En sistemas con integración de Python, cree el siguiente procedimiento almacenado que utiliza el código de Python para cargar los datos.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    from sklearn import datasets
    iris = datasets.load_iris()
    iris_data = pandas.DataFrame(iris.data)
    iris_data["Species"] = pandas.Categorical.from_codes(iris.target, iris.target_names)
    iris_data["SpeciesId"] = iris.target
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

    Al ejecutar este código, debería obtener el mensaje "Los comandos se han completado correctamente". Todo esto significa que el procedimiento almacenado se ha creado según sus especificaciones.

2. En los sistemas que tienen una integración con R, también puede crear un procedimiento que use R.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'R', 
    @script = N'
    library(RevoScaleR)
    data(iris)
    iris$SpeciesID <- c(unclass(iris$Species))
    iris_data <- iris
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

3. Para que la tabla se rellene, ejecute el procedimiento almacenado y especifique la tabla en la que se deben escribir los datos. Al ejecutarse, el procedimiento almacenado ejecuta el código de Python o R, que carga el conjunto de datos de iris integrado y después inserta los datos en la tabla **iris_data**.

    ```sql
    INSERT INTO iris_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "SpeciesId")
    EXEC dbo.get_iris_dataset;
    ```

    Si no está familiarizado con T-SQL, tenga en cuenta que la instrucción INSERT solo agrega nuevos datos, pero no comprueba si hay datos existentes ni elimina y vuelve a recompilar la tabla. Para que no se hagan varias copias de los mismos datos en una tabla, puede ejecutar esta instrucción primero: `TRUNCATE TABLE iris_data`. Con la instrucción [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md) de T-SQL se eliminan los datos existentes, pero se mantiene intacta la estructura de la tabla.

## <a name="query-the-data"></a>Consultar los datos

Como paso de validación, ejecute una consulta para confirmar que se han cargado los datos.

1. En Explorador de objetos, debajo de Bases de datos, haga clic con el botón derecho en la base de datas **irissql** e inicie una nueva consulta.

2. Ejecute algunas consultas sencillas:

    ```sql
    SELECT TOP(10) * FROM iris_data;
    SELECT COUNT(*) FROM iris_data;
    ```

## <a name="next-steps"></a>Pasos siguientes

En la guía de inicio rápido siguiente, va a crear un modelo de aprendizaje automático y a guardarlo en una tabla. Después, usará el modelo para generar los resultados previstos.

+ [Inicio rápido: Creación y puntuación de un modelo predictivo en Python](quickstart-python-train-score-model.md)