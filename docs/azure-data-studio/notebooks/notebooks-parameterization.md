---
title: Parametrización de cuadernos en Azure Data Studio
description: En este tutorial se muestra cómo crear un cuaderno parametrizado en ADS.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: vasubhog
ms.author: vasubhog
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 01/25/2021
ms.openlocfilehash: dc0558d660143de35340010735c74e9abc0cdd56
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343481"
---
# <a name="create-a-parameterized-notebook"></a>Creación de un cuaderno parametrizado

La **parametrización** es la capacidad de ejecutar el mismo cuaderno con distintos parámetros.

En este artículo se muestra cómo crear y ejecutar un cuaderno parametrizado en Azure Data Studio con el kernel de Python.

## <a name="prerequisites"></a>Requisitos previos

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-papermill-in-azure-data-studio"></a>Instalación y configuración de Papermill en Azure Data Studio

Todos los pasos de esta sección se ejecutan en un cuaderno de Azure Data Studio.

1. Cree un cuaderno y cambie el **Kernel** a *Python 3*.

   ![Nuevo cuaderno](media/notebooks-kqlmagic/install-new-notebook.png)

2. Es posible que se le solicite que actualice los paquetes de Python cuando los paquetes deban actualizarse.

   ![Sí](media/notebooks-kqlmagic/install-python-yes.png)

3. Instale Papermill:

   ```python
   import sys
   !{sys.executable} -m pip install papermill --no-cache-dir --upgrade
   ```

   Compruebe que está instalado:

   ```python
   import sys
   !{sys.executable} -m pip list
   ```

   :::image type="content" source="media/notebooks-parameterization/install-list-papermill.png" alt-text="Lista":::

5. Puede comprobar si Papermill se carga correctamente verificando la versión de Papermill.

   ```python
   import papermill
   papermill
   ```

   :::image type="content" source="media/notebooks-parameterization/install-validation-papermill.png" alt-text="Validación":::

## <a name="set-up-a-parameterized-notebook"></a>Configuración de un cuaderno parametrizado

1. Compruebe que el **Kernel** está establecido en *Python 3*.

   ![Cambio de kernel](media/notebooks-kqlmagic/change-kernel.png)

2. Cree una nueva celda de código y etiquétela como una **celda de parámetros**.

   ```python
   x = 2.0
   y = 5.0
   ```

   :::image type="content" source="media/notebooks-parameterization/make-parameter-cell.png" alt-text="Cuaderno de celdas de parámetros":::

3. Agregue otras celdas para probar diferentes parámetros.

   ```python
   addition = x + y
   multiply = x * y
   ```

   ```python
   print("Addition: " + str(addition))
   print("Multiplication: " + str(multiply))
   ```

   Celdas del cuaderno de entrada de ejemplo: :::image type="content" source="media/notebooks-parameterization/test-cells.png" alt-text="Celdas adicionales del cuaderno de entrada":::

4. Guarde el cuaderno como **Input.ipynb**.
   :::image type="content" source="media/notebooks-parameterization/save-notebook.png" alt-text="Guardado del cuaderno":::

## <a name="how-to-execute-papermill-notebook"></a>Procedimiento para ejecutar un cuaderno con Papermill

Con Papermill, los cuadernos se pueden ejecutar de dos maneras:

- Interfaz de línea de comandos (CLI)
- API de Python

### <a name="parameterized-cli-execution"></a>Ejecución con parámetros mediante la CLI

Para ejecutar un cuaderno mediante la CLI, escriba el comando "papermill" en el terminal junto con el cuaderno de entrada, la ubicación del cuaderno de salida y las opciones.

> [!Note]
   > Puede consultar la documentación de Papermill sobre la interfaz de la línea de comandos [aquí](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-cli).

1. Ejecute el cuaderno de entrada con nuevos parámetros.

   ```shell
   papermill Input.ipynb Output.ipynb -p x 10 -p y 20
   ```

   De este modo, se ejecutará el cuaderno de entrada con nuevos valores para los parámetros **x** e **y**.

2. Después de la ejecución, verá el nuevo cuaderno parametrizado de salida.
   Observe que hay una nueva celda con la etiqueta **# Injected-Parameters** (Parámetros insertados) que contiene los nuevos valores de parámetro pasados mediante la CLI.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Cuaderno de salida":::

### <a name="parameterized-python-api-execution"></a>Ejecución con parámetros mediante la API de Python

> [!Note]
   > Puede consultar la documentación de Papermill sobre la API de Python [aquí](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-the-python-api).

1. Cree un cuaderno y cambie el **Kernel** a *Python 3*.
   ![Nuevo cuaderno](media/notebooks-kqlmagic/install-new-notebook.png)

2. Agregue una nueva celda de código y use Papermill para ejecutar el método "execute".

   ```python
   import papermill as pm

   pm.execute_notebook(
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Input.ipynb',
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Output.ipynb',
   parameters = dict(x = 10, y = 20)
   )
   ```

   ![Ejecución de Papermill mediante la API de Python](media/notebooks-parameterization/python-api-execute.png)

3. Después de la ejecución, verá el nuevo cuaderno de parametrización de salida.

   Observe que hay una nueva celda con la etiqueta **# Injected-Parameters** (Parámetros insertados) que contiene los nuevos valores de parámetro pasados mediante la CLI.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Cuaderno de salida":::

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los cuadernos y la parametrización:

- [Uso de cuadernos en Azure Data Studio](./notebooks-guidance.md)
- [Documentación de Papermill sobre la parametrización](https://papermill.readthedocs.io/en/latest/index.html)