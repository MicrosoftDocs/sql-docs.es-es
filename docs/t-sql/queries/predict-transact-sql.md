---
description: PREDICT (Transact-SQL)
title: PREDICT (Transact-SQL)
titleSuffix: SQL machine learning
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- PREDICT
- PREDICT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PREDICT clause
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||>=azure-sqldw-latest'
ms.openlocfilehash: 9ccee4b3502dafac331803ba1e8e56c068666bbc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347545"
---
# <a name="predict-transact-sql"></a>PREDICT (Transact-SQL)

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Genera un valor de predicción o puntuaciones basadas en un modelo almacenado. Para obtener más información, vea [Puntuación nativa mediante la función PREDICT de T-SQL](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).

## <a name="syntax"></a>Sintaxis

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```syntaxsql
PREDICT  
(  
  MODEL = @model | model_literal,  
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

MODEL = @model | model_literal  
```

::: moniker-end

::: moniker range=">=azure-sqldw-latest"

```syntaxsql
PREDICT  
(  
  MODEL = <model_object>,
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

<model_object> ::=
  {
    model_literal
    | model_variable
    | ( scalar_subquery )
  }
```

::: moniker-end

### <a name="arguments"></a>Argumentos

**MODEL**

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
El parámetro `MODEL` se usa para especificar el modelo que se usa para la puntuación o predicción. El modelo se especifica como una variable o un literal, o bien una expresión escalar.

`PREDICT` admite modelos entrenados mediante los paquetes [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) y [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
El parámetro `MODEL` se usa para especificar el modelo que se usa para la puntuación o predicción. El modelo se especifica como una variable o un literal, o bien una expresión escalar.

En Azure SQL Managed Instance, `PREDICT` admite modelos en formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) o entrenados con los paquetes [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) y [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range=">=azure-sqldw-latest"
El parámetro `MODEL` se usa para especificar el modelo que se usa para la puntuación o predicción. El modelo se especifica como una variable o un literal, o bien una expresión escalar o una subconsulta escalar.

En Azure Synapse Analytics, `PREDICT` admite modelos en formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html).
::: moniker-end

**DATOS**

El parámetro DATA se usa para especificar los datos que se usan para la puntuación o predicción. Los datos se especifican en forma de un origen de tabla en la consulta. El origen de tabla puede ser una tabla, un alias de tabla, alias de CTE, vista o función con valores de tabla.

**RUNTIME = ONNX**

> [!IMPORTANT]
> El argumento `RUNTIME = ONNX` solo está disponible en [Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview), [Azure SQL Edge](/azure/sql-database-edge/onnx-overview) y [Azure Synapse Analytics](/azure/synapse-analytics/overview-what-is).

Indica el motor de aprendizaje automático que se usa para la ejecución del modelo. El valor del parámetro `RUNTIME` siempre es `ONNX`. El parámetro es necesario para Azure SQL Edge y Azure Synapse Analytics. En Azure SQL Managed Instance, el parámetro es opcional y solo se usa cuando se utilizan modelos ONNX.

**WITH ( <definición_de_conjunto_de_resultados> )**

La cláusula WITH se usa para especificar el esquema de la salida devuelta por la función `PREDICT`.

Además de las columnas devueltas por la propia función `PREDICT`, todas las columnas que forman parte de los datos de entrada están disponibles para su uso en la consulta.

### <a name="return-values"></a>Valores devueltos

No hay ningún esquema predefinido disponible; el contenido del modelo no se valida ni tampoco los valores de columna devueltos.

- La función `PREDICT` se pasa a través de las columnas como entrada.
- La función `PREDICT` también genera columnas nuevas, pero el número de columnas y sus tipos de datos dependen del tipo de modelo que se usó para la predicción.

Los mensajes de error relacionados con los datos, el modelo o el formato de columna se devuelven mediante la función de predicción subyacente asociada con el modelo.

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017"
## <a name="remarks"></a>Observaciones

La función `PREDICT` se admite en todas las ediciones de SQL Server 2017 o posteriores, en Windows y Linux. No es necesario habilitar [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) para usar `PREDICT`.
::: moniker-end

### <a name="supported-algorithms"></a>Algoritmos admitidos

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
El modelo que se use se debe haber creado mediante uno de los algoritmos admitidos de los paquetes [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) o [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md). Para obtener una lista de los modelos admitidos actualmente, vea [Puntuación nativa mediante la función PREDICT de T-SQL](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end
::: moniker range="=azure-sqldw-latest"
Se admiten los algoritmos que se pueden convertir al formato de modelo [ONNX](https://onnx.ai/).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Se admiten los algoritmos que se pueden convertir al formato de modelo [ONNX](https://onnx.ai/) y a los modelos que se hayan creado mediante uno de los algoritmos admitidos de los paquetes [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) o [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md). Para obtener una lista de los modelos admitidos actualmente en RevoScaleR y revoscalepy, vea [Puntuación nativa mediante la función PREDICT de T-SQL](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end

### <a name="permissions"></a>Permisos

No se requieren permisos para `PREDICT`; pero los usuarios necesitan el permiso `EXECUTE` en la base de datos y permiso para consultar los datos que se usan como entradas. Los usuarios también deben poder leer el modelo desde una tabla, si el modelo se ha almacenado en una tabla.

## <a name="examples"></a>Ejemplos

En los ejemplos siguientes se describe la sintaxis para llamar a `PREDICT`.

### <a name="using-predict-in-a-from-clause"></a>Uso de PREDICT en una cláusula FROM

Este ejemplo se hace referencia a la función `PREDICT` en la cláusula `FROM` de una instrucción `SELECT`:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d) WITH (Score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT test_model FROM scoring_model WHERE model_id = 1);

SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

::: moniker-end

El alias **d** especificado para el origen de tabla en el parámetro `DATA` se usa para hacer referencia a las columnas que pertenecen a `dbo.mytable`. El alias **p** especificado para la función `PREDICT` se usa para hacer referencia a las columnas devueltas por la función `PREDICT`.

- El modelo se almacena como la columna `varbinary(max)` en la tabla **Models**. En la tabla se guarda información adicional como el **id.** y la **descripción** para identificar el modo.
- El alias **d** especificado para el origen de tabla en el parámetro `DATA` se usa para hacer referencia a las columnas que pertenecen a `dbo.mytable`. Los nombres de las columnas de datos de entrada deben coincidir con el nombre de las entradas del modelo.
- El alias **p** especificado para la función `PREDICT` se usa para hacer referencia a la columna predicha devuelta por la función `PREDICT`. El nombre de la columna debe ser el mismo que el nombre de salida del modelo.
- Todas las columnas de datos de entrada y las columnas de predicción están disponibles para mostrarse en la instrucción SELECT.

::: moniker range=">=azure-sqldw-latest"

La consulta de ejemplo anterior se puede volver a escribir para crear una vista; para ello, hay que especificar `MODEL` como una subconsulta escalar:

```sql
CREATE VIEW predictions
AS
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT test_model FROM scoring_model WHERE model_id = 1),
             DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

:::moniker-end

### <a name="combining-predict-with-an-insert-statement"></a>Combinación de PREDICT con una instrucción INSERT

Un caso de uso común para la predicción consiste en generar una puntuación para los datos de entrada y después insertar los valores de predicción en una tabla. En el ejemplo siguiente se da por supuesto que la aplicación que realiza la llamada usa un procedimiento almacenado para insertar una fila que contiene el valor de predicción en una tabla:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d) WITH(score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH(score FLOAT) AS p;
```

:::moniker-end

- Los resultados de `PREDICT` se almacenan en una tabla denominada PredictionResults. 
- El modelo se almacena como la columna `varbinary(max)` en la tabla **Models**. En la tabla se puede guardar información adicional como el id. y la descripción para identificar el modelo.
- El alias **d** especificado para el origen de tabla en el parámetro `DATA` se usa para hacer referencia a las columnas de `dbo.mytable`. Los nombres de las columnas de datos de entrada deben coincidir con el nombre de las entradas del modelo.
- El alias **p** especificado para la función `PREDICT` se usa para hacer referencia a la columna predicha devuelta por la función `PREDICT`. El nombre de la columna debe ser el mismo que el nombre de salida del modelo.
- Todas las columnas de entrada y la columna de predicción están disponibles para mostrarse en la instrucción SELECT.

## <a name="next-steps"></a>Pasos siguientes

- [Puntuación nativa mediante la función PREDICT de T-SQL](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)
::: moniker range="=azure-sqldw-latest||=azuresqldb-mi-current"
-   [Más información sobre los modelos de ONNX](/azure/machine-learning/concept-onnx#get-onnx-models)
::: moniker-end
