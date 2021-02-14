---
title: Cuadernos con kernel de Python en Azure Data Studio
description: En este tutorial se muestra cómo crear y ejecutar un cuaderno de Python.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: garyericson
ms.author: garye
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: e1859c5e1af823e067210c5b69247702e8c78f89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040345"
---
# <a name="create-and-run-a-python-notebook"></a>Creación y ejecución de un cuaderno de Python

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

En este tutorial se muestra cómo crear y ejecutar un cuaderno en Azure Data Studio con Python.

## <a name="prerequisites"></a>Prerrequisitos

- [Azure Data Studio instalado](../download-azure-data-studio.md)

## <a name="create-a-notebook"></a>Creación de un cuaderno

En los pasos siguientes, se muestra cómo crear un archivo de cuaderno en Azure Data Studio:

1. Abra Azure Data Studio. Si se le pide que se conecte a un servidor SQL Server, puede conectarse o hacer clic en **Cancelar**.

1. Seleccione **Nuevo cuaderno** en el menú **Archivo**.

1. Seleccione **Python 3** para **Kernel**. **Adjuntar a** está establecido en "localhost".

   :::image type="content" source="media/notebooks-python-kernel/set-kernel-and-attach-to-python.png" alt-text="Establecimiento del kernel":::

Puede guardar el cuaderno mediante los comandos **Guardar** o **Guardar como...** del menú **Archivo**.

Para abrir un cuaderno, puede usar el comando **Abrir archivo...** del menú **Archivo**, seleccionar **Abrir archivo** en la página de **bienvenida** o usar el comando **Archivo: Abrir** de la paleta de comandos.

## <a name="change-the-python-kernel"></a>Cambio del kernel de Python

La primera vez que se conecta al kernel de Python en un cuaderno, se muestra la página **Configurar Python para Notebooks**. Puede seleccionar una de las dos opciones siguientes:

- **Nueva instalación de Python**, para instalar una nueva copia de Python para Azure Data Studio;
- **Usar la instalación de Python existente**, para especificar la ruta de acceso a una instalación existente de Python para que Azure Data Studio la use.

Para ver la ubicación y la versión del kernel de Python activo, cree una celda de código y ejecute los siguientes comandos de Python:

```python
import os
import sys
print(sys.version_info)
print(os.path.dirname(sys.executable))
```

Para conectarse a otra instalación de Python:

1. En el menú **Archivo**, seleccione **Preferencias** y, después, **Configuración**.
1. Desplácese hasta **Configuración de Notebook** en **Extensiones**.
1. En **Usar la instalación de Python existente**, desactive la opción "Ruta de acceso local a una instalación de Python preexistente utilizada por Notebooks".
1. Reinicie Azure Data Studio.

Cuando se inicia Azure Data Studio y se conecta al kernel de Python, se muestra la página **Configurar Python para Notebooks**. Ahí podrá crear una instalación de Python o especificar una ruta de acceso a una instalación existente.

## <a name="run-a-code-cell"></a>Ejecución de una celda de código

Puede crear celdas que contengan código SQL que puede ejecutar de forma local. Para ello, haga clic en el botón **Ejecutar celda** (la flecha redonda de color negro) a la izquierda de la celda. Cuando la celda termine de ejecutarse, los resultados se mostrarán en el cuaderno.

Por ejemplo:

1. Puede agregar una nueva celda de código de Python seleccionando el comando **+Código** de la barra de herramientas.

   :::image type="content" source="media/notebooks-python-kernel/notebook-toolbar-python.png" alt-text="Barra de herramientas del cuaderno":::

1. Copie y pegue el ejemplo siguiente en la celda y haga clic en **Ejecutar celda**. En este ejemplo se realizan cálculos simples y el resultado se muestra debajo.

   ```python
   a = 1
   b = 2
   c = a/b
   print(c)
   ```

   :::image type="content" source="media/notebooks-python-kernel/run-notebook-cell-python.png" alt-text="Ejecución de la celda del cuaderno":::

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los cuadernos:

- [Extensión de Python con Kqlmagic](./notebooks-kqlmagic.md)
- [Uso de cuadernos en Azure Data Studio](./notebooks-guidance.md)
- [Creación y ejecución de un cuaderno de SQL Server](./notebooks-sql-kernel.md)
