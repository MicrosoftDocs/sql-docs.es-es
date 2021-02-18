---
title: Tutoriales de Python
titleSuffix: SQL machine learning
description: En este artículo se describen los tutoriales de Python para el aprendizaje automático de SQL. Obtenga información sobre cómo ejecutar scripts y crear modelos de aprendizaje automático.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 4b8e997b6990d4d81e9667a41cf359208ca0f31e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341473"
---
# <a name="python-tutorials-for-sql-machine-learning"></a>Tutoriales de Python para aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En este artículo se describen los tutoriales e inicios rápidos de Python para [Machine Learning Services en SQL Server](../sql-server-machine-learning-services.md) y en [clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
En este artículo se describen los tutoriales e inicios rápidos de Python para [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En este artículo se describen los tutoriales e inicios rápidos de Python para [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

<a name="bkmk_pythontutorials"></a>

## <a name="python-tutorials"></a>Tutoriales de Python

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
| Tutorial | Descripción |
|-|-|
| [Predicción de alquiler de esquís con regresión lineal](python-ski-rental-linear-regression.md) | Utilice Python con regresión lineal para predecir el número de alquileres de esquí. Use cuadernos en Azure Data Studio para preparar los datos y entrenar el modelo y T-SQL para la implementación de modelo. |
| [Categorización de clientes con agrupación en clústeres k-means](python-clustering-model.md) | Utilice Python para desarrollar e implementar un modelo de agrupación en clústeres K-means para clasificar a los clientes. Use cuadernos en Azure Data Studio para preparar los datos y entrenar el modelo y T-SQL para la implementación de modelo. |
| [Creación de un modelo mediante revoscalepy](use-python-revoscalepy-to-create-model.md) | Muestra cómo ejecutar código desde un cliente de Python remoto mediante SQL Server como contexto de proceso. En el tutorial se crea un modelo con la función **rxLinMod** de la biblioteca **revoscalepy**. |
| [Análisis de datos de Python para desarrolladores de SQL](python-taxi-classification-introduction.md) | En este tutorial integral se muestra el proceso de compilación de una solución completa de Python mediante T-SQL. |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| Tutorial | Descripción |
|-|-|
| [Predicción de alquiler de esquís con regresión lineal](python-ski-rental-linear-regression.md) | Utilice Python con regresión lineal para predecir el número de alquileres de esquí. Use cuadernos en Azure Data Studio para preparar los datos y entrenar el modelo y T-SQL para la implementación de modelo. |
| [Categorización de clientes con agrupación en clústeres k-means](python-clustering-model.md) | Utilice Python para desarrollar e implementar un modelo de agrupación en clústeres K-means para clasificar a los clientes. Use cuadernos en Azure Data Studio para preparar los datos y entrenar el modelo y T-SQL para la implementación de modelo. |
::: moniker-end

## <a name="python-quickstarts"></a>Inicios rápidos de Python

Si no está familiarizado con el aprendizaje automático de SQL, puede probar también los inicios rápidos de Python.

| Guía de inicio rápido | Descripción |
|-|-|
| [Ejecución de scripts de Python simples](quickstart-python-create-script.md) | Conozca los conceptos básicos sobre cómo llamar a Python en T-SQL con [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). |
| [Objetos y estructuras de datos con Python](quickstart-python-data-structures.md) | Muestra cómo SQL usa el paquete Pandas de Python para administrar estructuras de datos. |
| [Creación y puntuación de un modelo predictivo en Python](quickstart-python-train-score-model.md) | Explica cómo crear, entrenar y usar un modelo de Python para hacer predicciones a partir de nuevos datos. |

## <a name="next-steps"></a>Pasos siguientes

+ [Extensión de Python en SQL Server](../concepts/extension-python.md)
