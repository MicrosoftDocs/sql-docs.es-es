---
title: Uso de Jupyter Notebook en Azure Data Studio
description: Obtenga información sobre cómo empezar a usar Jupyter Notebook en Azure Data Studio.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: yualan
ms.author: alayu
ms.reviewer: achatter, maghan, mikeray
ms.custom: seo-lt-2019
ms.date: 07/01/2020
ms.openlocfilehash: 77a0b2749f8b3ed12cf59f1359959c8d292d6eeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048485"
---
# <a name="use-jupyter-notebooks-in-azure-data-studio"></a>Uso de Jupyter Notebook en Azure Data Studio

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Jupyter Notebook es una aplicación web de código abierto que le permite crear y compartir documentos que contengan código de producción, ecuaciones, visualizaciones y texto narrativo. En el uso, se incluye la transformación y limpieza de datos, simulación numérica, modelado estadístico, visualización de datos y aprendizaje automático.

En este artículo se explica cómo crear un cuaderno en la última versión de [**Azure Data Studio**](../download-azure-data-studio.md) y cómo empezar a crear cuadernos propios con kernel distintos.

Vea este vídeo breve de 5 minutos para obtener una introducción a los cuadernos en Azure Data Studio:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introduction-to-Azure-Data-Studio-Notebooks/player?WT.mc_id=dataexposed-c9-niner]

## <a name="create-a-notebook"></a>Creación de un cuaderno

Hay varias maneras de crear un cuaderno. En cada caso, se abre un nuevo archivo denominado `Notebook-1.ipynb`.

- Vaya al **menú Archivo** en Azure Data Studio y seleccione **Nuevo cuaderno**.

  ![Nuevo cuaderno desde archivo](media/notebooks-guidance/file-new-notebook.png)

- Haga clic con el botón derecho en una conexión de **SQL Server** y seleccione **Nuevo cuaderno**.

  ![Nuevo cuaderno desde servidor](media/notebooks-guidance/server-new-notebook.png)

- Abra la paleta de comandos (**Ctrl + Mayús + P**), escriba "nuevo cuaderno" y seleccione el comando **Nuevo cuaderno**.

  ![Nuevo cuaderno desde la paleta de comandos](media/notebooks-guidance/command-palette-new-notebook.png)

## <a name="connect-to-a-kernel"></a>Conexión a un kernel

Los cuadernos de Azure Data Studio admiten varios kernels, incluidos los de SQL Server, Python o PySpark, entre otros. Cada kernel admite un lenguaje distinto en las celdas de código del cuaderno. Por ejemplo, cuando se conecta al kernel de SQL Server, puede escribir y ejecutar instrucciones T-SQL en una celda de código del cuaderno.

**Adjuntar a** proporciona el contexto para el kernel. Por ejemplo, si usa el kernel de SQL, puede adjuntar elementos a cualquiera de las instancias de SQL Server.
Si usa el kernel de Python3, debe adjuntar a **localhost** y puede usar este kernel para el desarrollo de Python local.

El kernel de SQL también se puede usar para conectarse a instancias de servidor de PostgreSQL. Si es desarrollador de PostgreSQL y quiere conectar los cuadernos al servidor de PostgreSQL, descargue la [**extensión PostgreSQL**](../extensions/postgres-extension.md) en el Marketplace de extensiones de Azure Data Studio para conectarse al servidor de PostgreSQL.

Si está conectado a un clúster de macrodatos de SQL Server 2019, el valor de **Adjuntar a** predeterminado es el punto final del clúster. Puede enviar código de Python, Scala y R mediante el proceso de Spark del clúster.

| Kernel                      | Descripción                                                  |
|:----------------------------|:-------------------------------------------------------------|
| SQL Kernel                  | Escribe código SQL destinado a la base de datos relacional.         |
| Kernel de PySpark3 y PySpark | Escribe código de Python mediante un proceso de Spark desde el clúster.      |
| Kernel de Spark                | Escribe código de Scala y R mediante un proceso de Spark desde el clúster. |
| Python Kernel               | Escribe código de Python para el desarrollo local.                     |

Para obtener más información sobre kernels específicos, consulte:

- [Creación y ejecución de un cuaderno de SQL Server](./notebooks-sql-kernel.md)
- [Creación y ejecución de un cuaderno de Python](./notebooks-python-kernel.md)
- [Extensión de Kqlmagic en Azure Data Studio](./notebooks-kqlmagic.md): extiende las capacidades del kernel de Python.

## <a name="add-a-code-cell"></a>Adición de una celda de código

Las celdas de código permiten ejecutar código de forma interactiva dentro del cuaderno.

Haga clic en el comando **+ Celda** de la barra de herramientas y seleccione **Celda de código** para agregar una nueva celda de código. Se agrega una nueva celda de código después de la celda seleccionada actualmente.

Escriba el código en la celda del kernel seleccionado. Por ejemplo, si utiliza el kernel de SQL, puede escribir comandos T-SQL en la celda de código.

La escritura de código con el kernel de SQL es similar a un editor de consultas de SQL. La celda de código admite una experiencia de programación de SQL moderna con características integradas, como un editor enriquecido de SQL, IntelliSense y fragmentos de código integrados. Los fragmentos de código permiten generar la sintaxis SQL adecuada para crear bases de datos, tablas, vistas y procedimientos almacenados y para actualizar los objetos de base de datos existentes. Use fragmentos de código para crear rápidamente copias de la base de datos con fines de desarrollo o prueba y para generar y ejecutar scripts.

![SQL Kernel](media/notebooks-guidance/intellisense-code-cell.png)

## <a name="add-a-text-cell"></a>Adición de una celda de texto

Las celdas de texto permiten agregar bloques de texto de Markdown entre las celdas de código para documentar el código.

Haga clic en el comando **+ Celda** de la barra de herramientas y seleccione **Celda de texto** para agregar una nueva celda de texto.

Se inicia el modo de edición en el que puede escribir el texto de Markdown en la celda. A medida que escribe, se muestra una vista previa.

![Celda de Markdown](media/notebooks-guidance/notebook-markdown-cell.png)

Al hacer clic fuera de la celda de texto, se muestra el texto de Markdown.

![Texto de Markdown](media/notebooks-guidance/notebook-markdown-preview.png)

Si vuelve a hacer clic en la celda de texto, cambia al modo de edición.

## <a name="run-a-cell"></a>Ejecución de una celda

Para ejecutar una sola celda, haga clic en **Ejecutar celda** (la flecha redonda de color negro) a la izquierda de la celda o seleccione la celda y presione F5. Puede hacer clic en **Ejecutar todas** en la barra de herramientas para ejecutar todas las celdas; estas se ejecutan de una en una y la ejecución se detiene si se produce un error en alguna celda.

Los resultados de la celda se muestran debajo de ella. Si quiere borrar los resultados de todas las celdas ejecutadas del cuaderno,seleccione el botón **Borrar resultados** de la barra de herramientas.

## <a name="save-a-notebook"></a>Guardado de un cuaderno

Para guardar un cuaderno, realice una de las siguientes operaciones.

- Presione Ctrl + S.
- Seleccione **Guardar** en el menú **Archivo**.
- En el menú **Archivo**, seleccione **Guardar como...** .
- Seleccione **Guardar todo** en el menú **Archivo**; se guardarán todos los cuadernos abiertos.
- En la paleta de comandos, escriba **Archivo: Guardar**.

Los cuadernos se guardan como archivos `.ipynb`.

## <a name="trusted-and-non-trusted"></a>De confianza y No de confianza

El valor predeterminado de los cuadernos abiertos en Azure Data Studio es **De confianza**.

Si abre un cuaderno desde otro origen, se abre en modo **No de confianza** y, luego, se puede convertir a **De confianza**.

## <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestra el uso de diferentes kernels para ejecutar un sencillo comando "Hola mundo". Seleccione el kernel, escriba el código de ejemplo en una celda y haga clic en **Ejecutar celda**.

### <a name="pyspark"></a>Pyspark

![Aplicación Spark](media/notebooks-guidance/pyspark.png)

### <a name="spark--scala-language"></a>Spark | Lenguaje Scala

![Spark Scala](media/notebooks-guidance/spark-scala.png)

### <a name="spark--r-language"></a>Spark | Lenguaje R

![Spark R](media/notebooks-guidance/spark-r.png)

### <a name="python-3"></a>Python 3

![Python local](media/notebooks-guidance/local-python.png)

## <a name="next-steps"></a>Pasos siguientes

- [Creación y ejecución de un cuaderno de SQL Server](./notebooks-sql-kernel.md).
- [Creación y ejecución de un cuaderno de Python](./notebooks-python-kernel.md)
- [Ejecución de scripts de Python y R en cuadernos de Azure Data Studio con Machine Learning Services de SQL Server](../../machine-learning/install/sql-machine-learning-azure-data-studio.md).
- [Implementación de clústeres de macrodatos de SQL Server con un cuaderno de Azure Data Studio](../../big-data-cluster/notebooks-deploy.md).
- [Administración de clústeres de macrodatos de SQL Server con cuadernos de Azure Data Studio](../../big-data-cluster/notebooks-manage-bdc.md).
- [Ejecución de un cuaderno de ejemplo con Spark](../../big-data-cluster/notebooks-tutorial-spark.md).