---
title: 'Inicio rápido: estructuras de datos de Python'
titleSuffix: SQL machine learning
description: En este inicio rápido, obtendrá información sobre cómo trabajar con estructuras de datos y objetos de datos en Python mediante aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3d24323019904ea144fa0029b8bfd5065f4a4a30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351520"
---
# <a name="quickstart-data-structures-and-objects-using-python-with-sql-machine-learning"></a>Inicio rápido: Estructuras de datos y objetos mediante Python con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

En este inicio rápido, aprenderá a usar estructuras de datos y tipos de datos al utilizar Python en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) o [Clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md). Aprenderá a mover datos entre Python y SQL Server, así como los problemas comunes que pueden producirse.

Aprendizaje automático de SQL se basa en el paquete **pandas** de Python, que es excelente para trabajar con datos tabulares. Sin embargo, no puede pasar un valor escalar desde Python a la base de datos y esperar que *funcione*. En esta guía de inicio rápido, revisará algunas definiciones de estructura de datos básicas a fin de prepararle para incidencias adicionales que podrían ocurrir al pasar datos tabulares entre Python y la base de datos.

Los conceptos más comunes son:

- Una trama de datos es una tabla con _varias_ columnas.
- Una sola columna de una trama de datos es un objeto de tipo lista denominado serie.
- Un valor único de una trama de datos se denomina celda y se obtiene acceso a ella mediante el índice.

¿Cómo se expondría el resultado único de un cálculo como una trama de datos, si una data.frame requiere una estructura tabular? Una respuesta es representar el valor escalar único como una serie, que se convierte fácilmente en una trama de datos.

> [!NOTE]
> Cuando se devuelven fechas, Python en SQL usa DATETIME, que tiene un intervalo de fechas restringido de 1753-01-01(-53690) a 9999-12-31(2958463).

## <a name="prerequisites"></a>Prerrequisitos

Para ejecutar este inicio rápido, debe cumplir los siguientes requisitos previos.

- Una base de datos SQL en una de estas plataformas:
  - [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Para realizar la instalación, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md) o la [Guía de instalación para Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Clústeres de macrodatos de SQL Server. Consulte [Habilitación de Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services en Azure SQL Managed Instance. Para obtener información, vea [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Una herramienta para ejecutar consultas de SQL que contengan scripts de Python. En este inicio rápido se utiliza [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="scalar-value-as-a-series"></a>Valor escalar como una serie

Este ejemplo realiza algunas operaciones matemáticas simples y convierte un valor escalar en una serie.

1. Una serie requiere un índice, que puede asignarlo manualmente, como se muestra aquí, o mediante programación.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   print(c)
   s = pandas.Series(c, index =["simple math example 1"])
   print(s)
   '
   ```

   Dado que la serie no se ha convertido a data.frame, los valores se devuelven en la ventana mensajes, pero puede ver que los resultados se encuentran en un formato más tabular.

   **Resultados**

   ```text
   STDOUT message(s) from external script: 
   0.5
   simple math example 1    0.5
   dtype: float64
   ```

1. Para aumentar la longitud de la serie, puede agregar nuevos valores mediante una matriz.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   '
   ```

   Si no especifica un índice, se genera un índice con valores que empiezan por 0 y terminan con la longitud de la matriz.

   **Resultados**

   ```text
   STDOUT message(s) from external script:
   0    0.5
   1    2.0
   dtype: float64
   ```

1. Si aumenta el número de valores del **índice**, pero no agrega nuevos valores de **datos**, los valores de datos se repiten para rellenar la serie.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
   print(s)
   '
   ```

   **Resultados**

   ```text
   STDOUT message(s) from external script:
   0.5
   simple math example 1    0.5
   simple math example 2    0.5
   dtype: float64
   ```

## <a name="convert-series-to-data-frame"></a>Conversión de series en tramas de datos

Después de convertir los resultados matemáticos escalares en una estructura tabular, debe convertirlos a un formato que aprendizaje automático de SQL pueda controlar.

1. Para convertir una serie en data.frame, llame al método [DataFrame](https://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe) de pandas.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   df = pd.DataFrame(s)
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   A continuación se muestra el resultado. Incluso si usa el índice para obtener valores específicos de data.Frame, los valores del índice no forman parte de la salida.

   **Resultados**

   |ResultValue|
   |------|
   |0.5|
   |2|

## <a name="output-values-into-dataframe"></a>Los valores de salida en data.frame

Ahora se mostrarán los valores específicos de dos series de resultados matemáticos en data.frame. El primero tiene un índice de valores secuenciales generados por Python. El segundo usa un índice arbitrario de valores de cadena.

1. En el ejemplo siguiente se obtiene un valor de la serie utilizando un índice de entero.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   df = pd.DataFrame(s, index=[1])
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   **Resultados**

   |ResultValue|
   |------|
   |2.0|

   Recuerde que el índice generado automáticamente comienza en 0. Pruebe a usar un valor de índice fuera de intervalo y vea lo que sucede.

1. Ahora obtenga un valor único de la otra trama de datos mediante un índice de cadena.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
   print(s)
   df = pd.DataFrame(s, index=["simple math example 1"])
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   **Resultados**

   |ResultValue|
   |------|
   |0.5|

   Si intenta usar un índice numérico para obtener un valor de esta serie, obtendrá un error.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo escribir funciones avanzadas de Python con aprendizaje automático de SQL, siga este inicio rápido:

> [!div class="nextstepaction"]
> [Escritura de funciones de Python avanzadas](quickstart-python-functions.md).