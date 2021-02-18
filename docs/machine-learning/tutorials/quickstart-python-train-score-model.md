---
title: 'Inicio rápido: Entrenamiento de un modelo en Python'
titleSuffix: SQL machine learning
description: En este inicio rápido, creará y entrenará un modelo predictivo con Python. Guardará el modelo en una tabla en la base de datos y, después, usará el modelo para predecir los valores de los datos nuevos con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 85cc5fafd2efb3fba605cc80da28946bc5215941
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273006"
---
# <a name="quickstart-create-and-score-a-predictive-model-in-python-with-sql-machine-learning"></a>Inicio rápido: Creación y puntuación de un modelo predictivo en Python con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

En este inicio rápido, creará y entrenará un modelo predictivo con Python. Guardará el modelo en una tabla en la instancia de SQL Server y, a continuación, lo usará para predecir los valores de los nuevos datos con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) o [Clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).

Creará y ejecutará dos procedimientos almacenados que se ejecutan en SQL. En el primero, se usa el conjunto de datos de flores Iris clásico y se genera un modelo de Bayes naive para predecir una especie de Iris basándose en las características florales. El segundo procedimiento es para puntuación: realiza una llamada al modelo generado en el primer procedimiento para generar un conjunto de predicciones basadas en datos nuevos. Al colocar código de Python en un procedimiento almacenado en SQL, las operaciones se incluyen en SQL, son reutilizables y pueden recibir llamadas de otros procedimientos almacenados y aplicaciones cliente.

Después de completar este inicio rápido, aprenderá a:

> [!div class="checklist"]
> - Insertar código de Python en un procedimiento almacenado
> - Pasar entradas en el código mediante entradas en el procedimiento almacenado
> - Usar procedimientos almacenados para hacer operativos los modelos

## <a name="prerequisites"></a>Prerrequisitos

Para ejecutar este inicio rápido, debe cumplir los siguientes requisitos previos.

- Una base de datos SQL en una de estas plataformas:
  - [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Para realizar la instalación, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md) o la [Guía de instalación para Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Clústeres de macrodatos de SQL Server. Consulte [Habilitación de Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services en Azure SQL Managed Instance. Para obtener información, vea [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Una herramienta para ejecutar consultas de SQL que contengan scripts de Python. En este inicio rápido se utiliza [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

- Los datos de ejemplo usados en este ejercicio son los datos de ejemplo de Iris. Siga las instrucciones en [Datos de demo de Iris](demo-data-iris-in-sql.md) para crear la base de datos de ejemplo **irissql**.

## <a name="create-a-stored-procedure-that-generates-models"></a>Creación de un procedimiento almacenado que genera modelos

En este paso, creará un procedimiento almacenado que genera un modelo para la predicción de resultados.

1. Abra Azure Data Studio, conéctese a su instancia de SQL y abra una nueva ventana de consulta.

1. Conéctese a la base de datos irissql.

    ```sql
    USE irissql
    GO
    ```

1. Copie el código siguiente para crear un procedimiento almacenado.

   Cuando se ejecute, el procedimiento llama a [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para iniciar una sesión de Python. 

   Las entradas que necesita el código de Python se pasan como parámetros de entrada en este procedimiento almacenado. La salida será un modelo entrenado basado en la biblioteca de Python **scikit-learn** para el algoritmo de aprendizaje automático.

   Este código usa [**pickle**](https://docs.python.org/2/library/pickle.html) para serializar el modelo. El modelo se entrenará con los datos de las columnas 0 a 4 de la tabla **iris_data**. 

   Los parámetros que verá en la segunda parte del procedimiento articulan entradas de datos y salidas del modelo. Siempre que sea posible, intente que el código de Python que se ejecute en un procedimiento almacenado tenga entradas y salidas bien definidas asignadas a las entradas y salidas del procedimiento almacenado pasado en tiempo de ejecución.

    ```sql
    CREATE PROCEDURE generate_iris_model (@trained_model VARBINARY(max) OUTPUT)
    AS
    BEGIN
        EXECUTE sp_execute_external_script @language = N'Python'
            , @script = N'
    import pickle
    from sklearn.naive_bayes import GaussianNB
    GNB = GaussianNB()
    trained_model = pickle.dumps(GNB.fit(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]], iris_data[["SpeciesId"]].values.ravel()))
    '
            , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
            , @input_data_1_name = N'iris_data'
            , @params = N'@trained_model varbinary(max) OUTPUT'
            , @trained_model = @trained_model OUTPUT;
    END;
    GO
    ```

1. Asegúrese de que el procedimiento almacenado exista. 

   Si el script de T-SQL del paso anterior se ha ejecutado sin errores, se creará un procedimiento almacenado denominado **generate_iris_model** y se agregará a la base de datos **irissql**. Encontrará los procedimientos almacenados en el **Explorador de objetos** de Azure Data Studio, en **Programación**.

## <a name="execute-the-procedure-to-create-and-train-models"></a>Ejecución del procedimiento para crear y entrenar modelos

En este paso, ejecutará el procedimiento del código insertado para crear un modelo entrenado y serializado como resultado. 

Los modelos almacenados para reutilizarlos en la base de datos se serializan como un flujo de bytes y se almacenan en una columna VARBINARY(MAX) de una tabla de base de datos. Después de crear, entrenar, serializar y guardar el modelo en una base de datos, otros procedimientos pueden llamarlo, o bien puede usarse la función [PREDICT de T-SQL](../../t-sql/queries/predict-transact-sql.md) en cargas de trabajo de puntuación.

1. Ejecute el script siguiente para realizar el procedimiento. La instrucción específica para ejecutar un procedimiento almacenado es `EXECUTE` en la cuarta línea.

   Este script específico elimina un modelo existente del mismo nombre (“Bayes naive”) para liberar espacio para los nuevos modelos que se crearán al volver a ejecutar el mismo procedimiento. Si no se elimina el modelo, se producirá un error que indica que el objeto ya existe. El modelo se almacena en una tabla llamada **iris_models**, que se aprovisiona al crear la base de datos **irissql**.

    ```sql
    DECLARE @model varbinary(max);
    DECLARE @new_model_name varchar(50)
    SET @new_model_name = 'Naive Bayes'
    EXECUTE generate_iris_model @model OUTPUT;
    DELETE iris_models WHERE model_name = @new_model_name;
    INSERT INTO iris_models (model_name, model) values(@new_model_name, @model);
    GO
    ```

1. Asegúrese de que se haya insertado el modelo.

    ```sql
    SELECT * FROM dbo.iris_models
    ```

    **Resultados**

    | model_name  | model |
    |---|-----------------|
    | Bayes naive | 0x800363736B6C6561726E2E6E616976655F62617965730A… | 

## <a name="create-and-execute-a-stored-procedure-for-generating-predictions"></a>Creación y ejecución de un procedimiento almacenado para generar predicciones

Después de crear, entrenar y guardar un modelo, continúe con el paso siguiente: crear un procedimiento almacenado que genere predicciones. Para hacerlo, realice una llamada a `sp_execute_external_script` para que ejecute un script de Python que cargue el modelo serializado y proporcione entradas de datos nuevos para puntuarlos.

1. Ejecute el código siguiente para crear el procedimiento almacenado que realiza la puntuación. En tiempo de ejecución, este procedimiento cargará un modelo binario que usará las columnas `[1,2,3,4]` como entradas y especificará las columnas `[0,5,6]` como salida.

   ```sql
   CREATE PROCEDURE predict_species (@model VARCHAR(100))
   AS
   BEGIN
       DECLARE @nb_model VARBINARY(max) = (
               SELECT model
               FROM iris_models
               WHERE model_name = @model
               );
   
       EXECUTE sp_execute_external_script @language = N'Python'
           , @script = N'
   import pickle
   irismodel = pickle.loads(nb_model)
   species_pred = irismodel.predict(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]])
   iris_data["PredictedSpecies"] = species_pred
   OutputDataSet = iris_data[["id","SpeciesId","PredictedSpecies"]] 
   print(OutputDataSet)
   '
           , @input_data_1 = N'select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
           , @input_data_1_name = N'iris_data'
           , @params = N'@nb_model varbinary(max)'
           , @nb_model = @nb_model
       WITH RESULT SETS((
                   "id" INT
                 , "SpeciesId" INT
                 , "SpeciesId.Predicted" INT
                   ));
   END;
   GO
   ```

2. Ejecute el procedimiento almacenado y asigne al modelo el nombre "Bayes naive" para que el procedimiento pueda identificar el modelo que tiene que usar.

   ```sql
   EXECUTE predict_species 'Naive Bayes';
   GO
   ```

   Al ejecutar el procedimiento almacenado, se devuelve un elemento data.frame de Python. Esta línea de T-SQL especifica el esquema de los resultados devueltos: `WITH RESULT SETS ( ("id" int, "SpeciesId" int, "SpeciesId.Predicted" int));`. Inserte los resultados en una tabla nueva, o bien devuélvalos a una aplicación.

   ![Conjunto de resultados después de ejecutar un procedimiento almacenado](media/train-score-using-python-NB-model-results.png)

   Los resultados son 150 predicciones sobre especies para los que se han usado características florales como entradas. Para la mayoría de las observaciones, las especies predichas coinciden con las especies reales.

   Este ejemplo se ha simplificado mediante el conjunto de datos Iris de Python, tanto para entrenamiento como para puntuación. Una forma más habitual es ejecutar una consulta SQL para obtener datos nuevos y pasarlos a Python como `InputDataSet`.

## <a name="conclusion"></a>Conclusión

En este ejercicio, ha aprendido a crear procedimientos almacenados dedicados a tareas distintas, donde cada procedimiento almacenado ha usado el procedimiento almacenado del sistema `sp_execute_external_script` para iniciar un proceso de Python. Las entradas en el proceso de Python se pasan a `sp_execute_external` como parámetros. Tanto el script de Python en sí como las variables de datos de una base de datos se pasan como entradas.

Normalmente, solo se usará Azure Data Studio con código de Python correcto, o bien código de Python que devuelve resultados basados en filas. Como herramienta, Azure Data Studio admite lenguajes de consulta como T-SQL y devuelve conjuntos de filas planos. Si el código genera un resultado visual como un diagrama de dispersión o un histograma, necesita una herramienta o una aplicación de usuario final independiente que pueda representar la imagen fuera del procedimiento almacenado.

Puede que a algunos desarrolladores de Python, acostumbrados a escribir scripts con todo incluido y que procesan una amplia variedad de operaciones, les parezca innecesario organizar las tareas en procedimientos separados. Pero el entrenamiento y la puntuación tienen distintos casos de uso. Al separarlos, puede colocar cada tarea en una programación distinta y asignar permisos de ámbito distintos a cada operación.

La ventaja final es que los procesos pueden modificarse mediante parámetros. En este ejercicio, el código de Python que ha creado el modelo (denominado "Bayes naive" en este ejemplo) se ha pasado como una entrada a un segundo procedimiento almacenado que llama al modelo en un proceso de puntuación. Aunque en este ejercicio solo se usa un modelo, puede imaginarse que, si parametriza el modelo en una tarea de puntuación, el script resultaría más útil.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los tutoriales de Python con el aprendizaje automático de SQL, consulte:

- [Tutoriales de Python](python-tutorials.md)