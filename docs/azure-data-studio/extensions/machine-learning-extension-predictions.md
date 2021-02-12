---
title: Elaboración de predicciones con la extensión Machine Learning
description: Aprenda a usar la extensión Machine Learning de Azure Data Studio para realizar predicciones con un modelo ONNX en la base de datos.
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 06/09/2020
ms.openlocfilehash: 6da9918b4ebb3ed571d82d23c123c8e477741c0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048545"
---
# <a name="make-predictions-with-machine-learning-extension-for-azure-data-studio-preview"></a>Elaboración de predicciones con la extensión de Machine Learning para Azure Data Studio (versión preliminar)

Aprenda a usar la [extensión Machine Learning de Azure Data Studio](machine-learning-extension.md) para realizar predicciones con un modelo ONNX en la base de datos. La extensión genera un script de T-SQL mediante [PREDICT](../../t-sql/queries/predict-transact-sql.md) a fin de realizar predicciones sobre el conjunto de datos almacenado en la tabla con un modelo importado previamente, que reside en un archivo local o desde Azure Machine Learning.

> [!IMPORTANT]
> La elaboración de predicciones con la extensión Machine Learning actualmente solo es compatible con [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) y [Azure SQL Edge con ONNX](/azure/azure-sql-edge/onnx-overview).

## <a name="prerequisites"></a>Requisitos previos

- Instale y configure la [extensión Machine Learning de Azure Data Studio](machine-learning-extension.md). Debe especificar las [rutas de acceso de la instalación de Python en la configuración de la extensión](machine-learning-extension.md#settings).

- Los paquetes de Python **onnxruntime**, **mlflow** y **mlflow-dbstore**. Si los paquetes todavía no están instalados, la extensión Machine Learning le pide que los instale.

## <a name="make-predictions-from-onnx-model"></a>Elaboración de predicciones a partir de un modelo ONNX

Siga los pasos siguientes para usar un modelo ONNX a fin de realizar predicciones.

1. Seleccione **Realizar predicciones**.

1. Si se le pide que instale **onnxruntime**, **mlflow** y **mlflow-dbstore**, seleccione **Sí**.

1. Elija dónde se encuentra el modelo y seleccione **Siguiente**. Puede usar:
    - **Modelos importados**. Seleccione esta opción para usar un modelo que ya está almacenado en la base de datos. Seleccione la **Base de datos modelo** y la **Tabla modelo** donde se encuentra el modelo, seleccione el modelo que quiere usar y seleccione **Siguiente**.
    - **Carga de archivo**. Seleccione esta opción para usar un modelo de un archivo. Seleccione el archivo de modelo en **Archivos de origen** y seleccione **Siguiente**.
    - **Azure Machine Learning**. Seleccione esta opción para usar un modelo de Azure Machine Learning. En primer lugar, tiene que **Iniciar sesión en Azure**. Después, seleccione la **cuenta de Azure**, la **suscripción de Azure**, el **grupo de recursos de Azure** y el **área de trabajo de Azure ML**. Seleccione el modelo que quiere usar y seleccione **Siguiente**.

1. Asigne los datos de origen al modelo.
    - Seleccione la **Base de datos de origen** y la **Tabla de origen** que contienen el conjunto de datos al que quiere aplicar la predicción.
    - Asigne las columnas en **Asignación de entrada del modelo** y **Salida del modelo**. La extensión asigna automáticamente las columnas con el mismo nombre y tipo de datos.

1. Seleccione **Predicción**.

Azure Data Studio crea una nueva consulta T-SQL con [PREDICT](../../t-sql/queries/predict-transact-sql.md), que se puede usar para realizar predicciones en los datos.

## <a name="next-steps"></a>Pasos siguientes

- [Extensión Machine Learning de Azure Data Studio](machine-learning-extension.md)
- [Administración de paquetes en la base de datos](machine-learning-extension-manage-packages.md)
- [Importación o visualización de modelos](machine-learning-extension-import-view-models.md)
- [Cuadernos en Azure Data Studio](../notebooks/notebooks-guidance.md)
- [Documentación del aprendizaje automático en SQL](../../machine-learning/index.yml)
- [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview)
- [Aprendizaje automático e IA con ONNX en SQL Edge (versión preliminar)](/azure/azure-sql-edge/onnx-overview)