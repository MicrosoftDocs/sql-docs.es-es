---
title: 'Tutorial de Python: Predicción de tarifas de taxi de Nueva York con clasificación binaria'
titleSuffix: SQL machine learning
description: Aprenda a insertar código de Python en procedimientos almacenados de SQL Server y funciones de T-SQL con el aprendizaje automático de SQL para predecir tarifas de taxis de Nueva York mediante la clasificación binaria.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/28/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 103eaec3991e9c4f00f819ca2811f395bfbeb2b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339670"
---
# <a name="python-tutorial-predict-nyc-taxi-fares-with-binary-classification"></a>Tutorial de Python: Predicción de tarifas de taxi de Nueva York con clasificación binaria
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En esta serie de tutoriales de cinco partes para programadores de SQL, obtendrá información sobre la integración de Python en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) o en [Clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2017"
En esta serie de tutoriales de cinco partes para programadores de SQL, obtendrá información sobre la integración de Python en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
En esta serie de tutoriales de cinco partes para programadores de SQL, obtendrá información sobre la integración de Python en [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

Creará e implementará una solución de aprendizaje automático basada en Python mediante una base de datos de ejemplo en SQL Server. Se usará T-SQL, Azure Data Studio, o SQL Server Management Studio, y una instancia del motor de base de datos con el aprendizaje automático de SQL y la compatibilidad con el lenguaje Python.

En esta serie de tutoriales se presentan las funciones de Python usadas en un flujo de trabajo de modelado de datos. Algunas de las partes son la exploración de datos, la creación y el entrenamiento de un modelo de clasificación binaria, y la implementación del modelo. Usará datos de ejemplo de la Comisión de taxis y limusinas de la Ciudad de Nueva York. El modelo que se va a compilar predice si es probable que un trayecto acabe en propina en función de la hora del día, la distancia recorrida y la ubicación de origen.

En la primera parte de esta serie, instalará los requisitos previos y restaurará la base de datos de ejemplo. En las partes dos y tres, desarrollará scripts de Python para preparar sus datos y entrenar un modelo de Machine Learning. Después, en las partes cuatro y cinco, ejecutará esos scripts de Python en la base de datos con procedimientos almacenados en T-SQL.

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Requisitos previos de instalación
> + Restauración de la base de datos de ejemplo

En la [parte dos](python-taxi-classification-explore-data.md), explorará los datos de ejemplo y generará algunos trazados.

En la [tercera](python-taxi-classification-create-features.md), aprenderá a crear características a partir de datos sin procesar mediante una función de Transact-SQL. Después, llamaremos a esa función desde un procedimiento almacenado para crear una tabla que contiene los valores de las características.

En la [parte cuatro](python-taxi-classification-train-model.md), cargará los módulos y llamará a las funciones necesarias para crear y entrenar el modelo mediante un procedimiento almacenado de SQL Server.

En la [parte cinco](python-taxi-classification-deploy-model.md), aprenderá a poner en marcha los modelos entrenados y guardados en la parte cuatro.

> [!NOTE]
> Este tutorial está disponible tanto en R como en Python. Para la versión de R, consulte [Tutorial de R: Predicción de tarifas de taxi de Nueva York con clasificación binaria](r-taxi-classification-introduction.md).

## <a name="prerequisites"></a>Requisitos previos

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
+ Instalar [SQL Server Machine Learning Services con Python](../install/sql-machine-learning-services-windows-install.md#verify-installation)
::: moniker-end

+ [Conceder permisos para la ejecución de scripts de Python y R](../security/user-permission.md)

+ Restaurar la [base de datos de demostración de taxis de Nueva York](demo-data-nyctaxi-in-sql.md)

Todas las tareas pueden realizarse mediante procedimientos almacenados de [!INCLUDE[tsql](../../includes/tsql-md.md)] de Azure Data Studio o [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

En esta serie de tutoriales se supone que está familiarizado con las operaciones básicas de base de datos, como la creación de bases de datos y tablas, la importación de datos y la escritura de consultas SQL. No da por sentado que conoce Python; se proporciona todo el código de Python.

## <a name="background-for-sql-developers"></a>Información general para desarrolladores de SQL

El proceso de compilación de una solución de Machine Learning es una tarea compleja para la que se necesitan varias herramientas y la coordinación de expertos en la materia en distintas fases:

+ Obtención y limpieza de datos
+ Exploración de los datos y compilación de características útiles para el modelado
+ Entrenamiento y ajuste del modelo
+ Implementación en producción

La mejor manera de desarrollar y probar el código actual es usar un entorno de desarrollo dedicado. Pero, después de haber probado completamente el script, puede implementarlo fácilmente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante procedimientos almacenados de [!INCLUDE[tsql](../../includes/tsql-md.md)] en el entorno de Azure Data Studio o [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] que ya conoce. El mecanismo principal para poner operativo el código en SQL Server consiste en ajustar el código externo en procedimientos almacenados.

Después de guardar el modelo en la base de datos, llame al modelo de predicción desde [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante procedimientos almacenados.

Tanto si es un programador de SQL que no está familiarizado con Python como si es un desarrollador de Python que no está familiarizado con SQL, esta serie de cinco tutoriales presenta un flujo de trabajo típico para realizar análisis en base de datos con Python y SQL Server.

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Instaló los requisitos previos
> + Restauró la base de datos de ejemplo

> [!div class="nextstepaction"]
> [Tutorial de Python: Exploración y visualización de datos](python-taxi-classification-explore-data.md)