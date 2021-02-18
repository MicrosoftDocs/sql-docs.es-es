---
title: Puntuación nativa con PREDICT de T-SQL
titleSuffix: SQL machine learning
description: Obtenga información sobre cómo usar la puntuación nativa con la función PREDICT de T-SQL para generar valores de predicción para nuevas entradas de datos casi en tiempo real.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest'
ms.openlocfilehash: 74fc66dd6ed235d7508f774e7397c9b4d55e80a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352540"
---
# <a name="native-scoring-using-the-predict-t-sql-function-with-sql-machine-learning"></a>Puntuación nativa mediante la función PREDICT de T-SQL con aprendizaje automático en SQL

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Obtenga información sobre cómo usar la puntuación nativa con la función [PREDICT de T-SQL](../../t-sql/queries/predict-transact-sql.md) para generar valores de predicción para nuevas entradas de datos casi en tiempo real. La puntuación nativa requiere que ya haya un modelo entrenado.

La función `PREDICT` utiliza las capacidades de extensión nativas de C++ en el [aprendizaje automático en SQL](../index.yml). Esta metodología ofrece la velocidad de procesamiento más rápida posible de las cargas de trabajo de previsión y predicción, y admite modelos en formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) o modelos entrenados con los paquetes [RevoScaleR](../r/ref-r-revoscaler.md) y [revoscalepy](../python/ref-py-revoscalepy.md).

## <a name="how-native-scoring-works"></a>Cómo funciona la puntuación nativa

La puntuación nativa usa bibliotecas que pueden leer modelos en formato ONNX o un formato binario predefinido, y generar puntuaciones para las nuevas entradas de datos que proporcione. Como el modelo ya está entrenado, implementado y almacenado, se puede usar en la puntuación sin tener que llamar al intérprete de R o de Python. Esto implica una reducción en la sobrecarga de varias interacciones de procesos, lo que da lugar a un rendimiento de predicción mucho más rápido.

Para usar la puntuación nativa, llame a la función `PREDICT` de T-SQL y pase las siguientes entradas necesarias:

+ Un modelo compatible basado en un modelo y un algoritmo compatibles.
+ Datos de entrada, normalmente definidos como una consulta T-SQL.

La función devuelve predicciones de los datos de entrada, además de las columnas de datos de origen de paso a través.

## <a name="prerequisites"></a>Prerrequisitos

`PREDICT` está disponible en:

+ Todas las ediciones de SQL Server 2017 y versiones posteriores en Windows y Linux
+ Instancia administrada de Azure SQL
+ Azure SQL Database
+ Azure SQL Edge
+ Azure Synapse Analytics

La función está habilitada de manera predeterminada. No es necesario instalar R o Python, ni habilitar otras características.

## <a name="supported-models"></a>Modelos admitidos

Los formatos de modelos admitidos por la función `PREDICT` dependen de la plataforma SQL en la que se realiza la puntuación nativa. Vea la tabla siguiente para ver qué formatos de modelos se admiten en cada plataforma.

| Plataforma | Formato de modelo ONNX | Formato de modelo RevoScale |
|-|-|-|
| SQL Server | No | Sí |
| Instancia administrada de Azure SQL | Sí | Sí |
| Azure SQL Database | No | Sí |
| Azure SQL Edge | Sí | No |
| Azure Synapse Analytics | Sí | No |

::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="onnx-models"></a>Modelos de ONNX

El modelo debe estar en un formato de modelo [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current"
### <a name="revoscale-models"></a>Modelos RevoScale

El modelo se debe entrenar de antemano con uno de los algoritmos **rx** admitidos que se enumeran más abajo mediante el paquete [RevoScaleR](../r/ref-r-revoscaler.md) o [revoscalepy](../python/ref-py-revoscalepy.md).

Serialice el modelo mediante [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) para R y [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) para Python. Estas funciones de serialización se han optimizado para admitir la puntuación rápida.

<a name="bkmk_native_supported_algos"></a> 

#### <a name="supported-revoscale-algorithms"></a>Algoritmos RevoScale compatibles

Los algoritmos siguientes son compatibles con revoscalepy y RevoScaleR.

+ Algoritmos revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) 
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) 
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) 
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) 

+ Algoritmos RevoScaleR

  + [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/r-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/r-server/r-reference/revoscaler/rxdtree)
  + [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

Si necesita usar algoritmos de MicrosoftML o microsoftml, utilice [Puntuación en tiempo real con sp_rxPredict](../predictions/real-time-scoring.md).

Los tipos de modelos no compatibles son los siguientes:

+ Modelos que contienen otras transformaciones
+ Modelos que usan los algoritmos `rxGlm` o `rxNaiveBayes` en los equivalentes de RevoScaleR o revoscalepy
+ Modelos de PMML
+ Modelos creados con otras bibliotecas de código abierto o de terceros
::: moniker-end

## <a name="examples"></a>Ejemplos
::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="predict-with-an-onnx-model"></a>PREDICT con un modelo ONNX

En este ejemplo se muestra cómo usar un modelo ONNX almacenado en la tabla `dbo.models` para la puntuación nativa.

```sql
DECLARE @model VARBINARY(max) = (
        SELECT DATA
        FROM dbo.models
        WHERE id = 1
        );

WITH predict_input
AS (
    SELECT TOP (1000) [id]
        , CRIM
        , ZN
        , INDUS
        , CHAS
        , NOX
        , RM
        , AGE
        , DIS
        , RAD
        , TAX
        , PTRATIO
        , B
        , LSTAT
    FROM [dbo].[features]
    )
SELECT predict_input.id
    , p.variable1 AS MEDV
FROM PREDICT(MODEL = @model, DATA = predict_input, RUNTIME=ONNX) WITH (variable1 FLOAT) AS p;
```

> [!NOTE]
> Dado que las columnas y los valores devueltos por **PREDICT** pueden variar según el tipo de modelo, debe definir el esquema de los datos devueltos mediante una cláusula **WITH**.
::: moniker-end

::: moniker range=">=sql-server-2017||=azuresqldb-mi-current||>=sql-server-linux-2017"
### <a name="predict-with-revoscale-model"></a>PREDICT con un modelo RevoScale

En este ejemplo, se crea un modelo con **RevoScaleR** en R y, después, se llama a la función de predicción en tiempo real desde T-SQL.

#### <a name="step-1-prepare-and-save-the-model"></a>Paso 1. Preparar y guardar el modelo

Ejecute el siguiente código para crear la base de datos de ejemplo y las tablas necesarias.

```sql
CREATE DATABASE NativeScoringTest;
GO
USE NativeScoringTest;
GO
DROP TABLE IF EXISTS iris_rx_data;
GO
CREATE TABLE iris_rx_data (
    "Sepal.Length" float not null, "Sepal.Width" float not null
  , "Petal.Length" float not null, "Petal.Width" float not null
  , "Species" varchar(100) null
);
GO
```

Use la siguiente instrucción para rellenar la tabla de datos con datos del conjunto de datos de **iris**.

```sql
INSERT INTO iris_rx_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width" , "Species")
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'iris_data <- iris;'
  , @input_data_1 = N''
  , @output_data_1_name = N'iris_data';
GO
```

Ahora, cree una tabla para almacenar los modelos.

```sql
DROP TABLE IF EXISTS ml_models;
GO
CREATE TABLE ml_models ( model_name nvarchar(100) not null primary key
  , model_version nvarchar(100) not null
  , native_model_object varbinary(max) not null);
GO
```

El siguiente código crea un modelo basado en el conjunto de datos de **iris** y lo guarda en la tabla denominada **models**.

```sql
DECLARE @model varbinary(max);
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'
    iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
    iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, data = iris[iris.sub, ])
    model <- rxSerializeModel(iris.dtree, realtimeScoringOnly = TRUE)
    '
  , @params = N'@model varbinary(max) OUTPUT'
  , @model = @model OUTPUT
  INSERT [dbo].[ml_models]([model_name], [model_version], [native_model_object])
  VALUES('iris.dtree','v1', @model) ;
```

> [!NOTE]
> Asegúrese de usar la función [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) de RevoScaleR para guardar el modelo. La función `serialize` de R estándar no puede generar el formato que necesitamos.

Puede ejecutar una instrucción como la siguiente para ver el modelo almacenado en formato binario:

```sql
SELECT *, datalength(native_model_object)/1024. as model_size_kb
FROM ml_models;
```

#### <a name="step-2-run-predict-on-the-model"></a>Paso 2. Ejecutar PREDICT en el modelo

La siguiente instrucción PREDICT sencilla obtiene una clasificación del modelo de árbol de decisión mediante la función de **puntuación nativa**. Predice las especies de iris a partir de los atributos proporcionados y de la longitud y el ancho de pétalo.

```sql
DECLARE @model varbinary(max) = (
  SELECT native_model_object
  FROM ml_models
  WHERE model_name = 'iris.dtree'
  AND model_version = 'v1');
SELECT d.*, p.*
  FROM PREDICT(MODEL = @model, DATA = dbo.iris_rx_data as d)
  WITH(setosa_Pred float, versicolor_Pred float, virginica_Pred float) as p;
go
```

Si aparece el error "Error durante la ejecución de la función integrada PREDICT. El modelo está dañado o no es válido", suele significar que la consulta no devolvió un modelo. Compruebe si escribió correctamente el nombre del modelo, o si la tabla de modelos está vacía.

> [!NOTE]
> Dado que las columnas y los valores devueltos por **PREDICT** pueden variar según el tipo de modelo, debe definir el esquema de los datos devueltos mediante una cláusula **WITH**.
::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

+ [PREDICT (Transact-SQL)](../../t-sql/queries/predict-transact-sql.md)
+ [Documentación del aprendizaje automático en SQL](../index.yml)
+ [Aprendizaje automático e IA con ONNX en SQL Edge](/azure/azure-sql-edge/onnx-overview)
+ [Implementación y creación de predicciones con un modelo de ONNX en Azure SQL Edge](/azure/azure-sql-edge/deploy-onnx)