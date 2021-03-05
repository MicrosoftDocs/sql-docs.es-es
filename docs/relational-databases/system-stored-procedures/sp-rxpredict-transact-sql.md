---
title: sp_rxPredict
description: sp_rxPredict genera un valor de predicción para una entrada determinada que consta de un modelo de aprendizaje automático almacenado en un formato binario en una base de datos de SQL Server.
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- sp_rxPredict
- sp_rxPredict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_rxPredict procedure
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6f4d7ee0ea6f9630942dc12baf5b8ed68cbee4d5
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186051"
---
# <a name="sp_rxpredict"></a>sp_rxPredict  
[!INCLUDE [SQL Server 2016 Windows only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]

Genera un valor de predicción para una entrada determinada que consta de un modelo de aprendizaje automático almacenado en un formato binario en una base de datos de SQL Server.

Proporciona puntuación en los modelos de aprendizaje automático de R y Python casi en tiempo real. `sp_rxPredict` es un procedimiento almacenado que se proporciona como un contenedor para la `rxPredict` función de R en [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler) y [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package), y la función [rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) de Python en [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) y [MicrosoftML](/machine-learning-server/python-reference/microsoftml/microsoftml-package). Está escrito en C++ y está optimizado específicamente para las operaciones de puntuación.

Aunque el modelo se debe crear mediante R o Python, una vez que se serializa y se almacena en un formato binario en una instancia del motor de base de datos de destino, se puede usar desde esa instancia del motor de base de datos aunque no esté instalada la integración de R o Python. Para obtener más información, vea [puntuación en tiempo real con sp_rxPredict](../../machine-learning/predictions/real-time-scoring.md).

## <a name="syntax"></a>Sintaxis

```
sp_rxPredict  ( @model, @input )
```

### <a name="arguments"></a>Argumentos

**model**

Modelo previamente entrenado en un formato admitido. 

**input**

Una consulta SQL válida

### <a name="return-values"></a>Valores devueltos

Se devuelve una columna de puntuación, así como todas las columnas de paso a través del origen de datos de entrada.
Se pueden devolver columnas de puntuación adicionales, como el intervalo de confianza, si el algoritmo admite la generación de estos valores.

## <a name="remarks"></a>Observaciones

Para habilitar el uso del procedimiento almacenado, SQLCLR debe estar habilitado en la instancia.

> [!NOTE]
> La habilitación de esta opción conlleva implicaciones de seguridad. Use una implementación alternativa, como la función de [PREdicción de Transact-SQL](../../t-sql/queries/predict-transact-sql.md?view=sql-server-2017&preserve-view=true) , si SQLCLR no se puede habilitar en el servidor.

El usuario necesita el `EXECUTE` permiso en la base de datos.

### <a name="supported-algorithms"></a>Algoritmos admitidos

Para crear y entrenar el modelo, use uno de los algoritmos admitidos para R o Python, proporcionado por [SQL Server Machine Learning Services (r o Python)](../../machine-learning/sql-server-machine-learning-services.md), [SQL Server 2016 R Services](../../machine-learning/r/sql-server-r-services.md), [SQL Server machine learning Server (independiente) (r o python)](../../machine-learning/r/r-server-standalone.md)o [SQL Server 2016 R Server (independiente)](../../machine-learning/r/r-server-standalone.md?view=sql-server-2016&preserve-view=true).

#### <a name="r-revoscaler-models"></a>R: modelos RevoScaleR

  + [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/machine-learning-server/r-reference/revoscaler/rxdtree)
  + [rxdForest](/machine-learning-server/r-reference/revoscaler/rxdforest)

#### <a name="r-microsoftml-models"></a>R: modelos MicrosoftML

  + [rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](/machine-learning-server/r-reference/microsoftml/rxfastlinear)

#### <a name="r-transformations-supplied-by-microsoftml"></a>R: transformaciones proporcionadas por MicrosoftML

  + [featurizeText](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](/machine-learning-server/r-reference/microsoftml/selectFeatures)

#### <a name="python-revoscalepy-models"></a>Python: modelos revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit)
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees)
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree)
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest)


#### <a name="python-microsoftml-models"></a>Python: modelos microsoftml

  + [rx_fast_trees](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [rx_fast_forest](/machine-learning-server/python-reference/microsoftml/rx-fast-forest)
  + [rx_logistic_regression](/machine-learning-server/python-reference/microsoftml/rx-logistic-regression)
  + [rx_oneclass_svm](/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm)
  + [rx_neural_network](/machine-learning-server/python-reference/microsoftml/rx-neural-network)
  + [rx_fast_linear](/machine-learning-server/python-reference/microsoftml/rx-fast-linear)

#### <a name="python-transformations-supplied-by-microsoftml"></a>Python: transformaciones proporcionadas por microsoftml

  + [featurize_text](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [concat](/machine-learning-server/python-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/python-reference/microsoftml/categorical)
  + [categorical_hash](/machine-learning-server/python-reference/microsoftml/categorical-hash)
  
### <a name="unsupported-model-types"></a>Tipos de modelos no admitidos

No se admiten los siguientes tipos de modelo:

+ Modelos que usan `rxGlm` los `rxNaiveBayes` algoritmos o en RevoScaleR
+ Modelos PMML en R
+ Modelos creados con otras bibliotecas de terceros 

## <a name="examples"></a>Ejemplos

```sql
DECLARE @model = SELECT @model 
FROM model_table 
WHERE model_name = 'rxLogit trained';

EXEC sp_rxPredict @model = @model,
@inputData = N'SELECT * FROM data';
```

Además de ser una consulta SQL válida, los datos de entrada de *\@ inputData* deben incluir columnas compatibles con las columnas del modelo almacenado.

`sp_rxPredict` solo admite los siguientes tipos de columna de .NET: Double, Float, Short, ushort, Long, ulong y String. Es posible que tenga que filtrar los tipos no admitidos en los datos de entrada antes de usarlo para la puntuación en tiempo real. 

  Para obtener información sobre los tipos de SQL correspondientes, vea [Asignación de tipos de SQL-CLR](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping) o [Asignación de datos de parámetros de CLR](../clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).
