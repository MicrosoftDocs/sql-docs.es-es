---
title: Implementación de código de R en procedimientos almacenados
description: Inserte código de lenguaje R en un procedimiento almacenado de SQL Server para que esté disponible para cualquier aplicación cliente que tenga acceso a una base de datos de SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: a959bf5c29a125aefb390bef1428e6a35745e665
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353749"
---
# <a name="operationalize-r-code-using-stored-procedures-in-sql-server-machine-learning-services"></a>Operacionalización de código de R con procedimientos almacenados en SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Al usar las características de R y Python en SQL Server Machine Learning Services, el enfoque más común para trasladar soluciones a un entorno de producción es insertar código en procedimientos almacenados. En este artículo se resumen los puntos clave que debe tener en cuenta el desarrollador de SQL al operacionalizar código de R mediante SQL Server.

## <a name="deploy-production-ready-script-using-t-sql-and-stored-procedures"></a>Implementación de scripts listos para producción mediante T-SQL y procedimientos almacenados

Tradicionalmente, la integración de soluciones de ciencia de datos ha implicado volver a codificar de forma exhaustiva para admitir el rendimiento y la integración. SQL Server Machine Learning Services simplifica esta tarea, ya que el código de R y Python puede ejecutarse en SQL Server y llamarse mediante procedimientos almacenados. Para más información sobre los mecanismos para insertar código en procedimientos almacenados, vea:

+ [Creación y ejecución de scripts de R sencillos en SQL Server](../tutorials/quickstart-r-create-script.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

Puede encontrar un ejemplo más completo de la implementación de código de R en producción mediante procedimientos almacenados en [Tutorial de R: Predicción de tarifas de taxi de Nueva York con clasificación binaria](../tutorials/r-taxi-classification-introduction.md).

## <a name="guidelines-for-optimizing-r-code-for-sql"></a>Instrucciones para optimizar código de R para SQL

Convertir el código de R en SQL es más fácil si se llevan a cabo algunas optimizaciones con antelación en el código de R o Python. Esto incluye evitar tipos de datos que causan problemas, evitar conversiones de datos innecesarias y volver a escribir el código de R como una única llamada de función que se pueda parametrizar fácilmente. Para más información, consulte:

+ [Bibliotecas de R y tipos de datos](r-libraries-and-data-types.md)
+ [Uso de funciones auxiliares de sqlrutils](ref-r-sqlrutils.md)

## <a name="integrate-r-and-python-with-applications"></a>Integración de R y Python con aplicaciones

Dado que puede ejecutar R o Python desde un procedimiento almacenado, se pueden ejecutar scripts desde cualquier aplicación que pueda enviar una instrucción T-SQL y controlar los resultados. Por ejemplo, podría volver a entrenar un modelo según una programación mediante la [tarea Ejecutar T-SQL](../../integration-services/control-flow/execute-t-sql-statement-task.md) de Integration Services o mediante otro programador de trabajos que pueda ejecutar un procedimiento almacenado.

La puntuación es una tarea importante que se puede automatizar fácilmente o iniciar desde aplicaciones externas. Entrene el modelo con antelación, mediante R, Python o un procedimiento almacenado, y [guarde el modelo en formato binario](../tutorials/walkthrough-build-and-save-the-model.md) en una tabla. Después, el modelo se puede cargar en una variable como parte de una llamada a un procedimiento almacenado, mediante una de estas opciones para la puntuación de T-SQL:

+ Puntuación en tiempo real, optimizada para lotes pequeños.
+ Puntuación de fila única, para llamar desde una aplicación.
+ [Puntuación nativa](../predictions/native-scoring-predict-transact-sql.md), para una predicción rápida de lotes de SQL Server sin llamar a R.

En este tutorial se proporciona un ejemplo de puntuación mediante un procedimiento almacenado en los modos de lotes y de fila única:

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Tutorial de ciencia de datos de un extremo a otro para R en SQL Server](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ [Tutorial de R: Predicción de tarifas de taxi de Nueva York con clasificación binaria](../tutorials/r-taxi-classification-introduction.md)
::: moniker-end

## <a name="boost-performance-and-scale"></a>Aumento del rendimiento y la escalabilidad

Aunque se sabe que el lenguaje R de código abierto presenta limitaciones con respecto a conjuntos de datos de gran tamaño, las [API del paquete RevoScaleR](ref-r-revoscaler.md) que se incluyen con SQL Server Machine Learning Service pueden funcionar en conjuntos de datos de gran tamaño y aprovechar los cálculos de varios procesos, núcleos y procesos en la base de datos.

Si la solución de R usa agregaciones complejas o implica conjuntos de datos de gran tamaño, puede aprovechar los índices de almacén de columnas y las agregaciones en memoria altamente eficaces de SQL Server y permitir que el código de R controle la puntuación y los cálculos estadísticos.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"

## <a name="adapt-r-code-for-other-platforms-or-compute-contexts"></a>Adaptación del código de R para otras plataformas o contextos de cálculo

El mismo código de R que se ejecuta en datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se puede usar en otros orígenes de datos, como Spark en HDFS, al usar la [opción de servidor independiente](../install/sql-machine-learning-standalone-windows-install.md) en la configuración de SQL Server o al instalar el producto que no es de SQL Microsoft Machine Learning Server (anteriormente conocido como **Microsoft R Server**):

+ [Documentación de Machine Learning Server](/r-server/)

::: moniker-end
