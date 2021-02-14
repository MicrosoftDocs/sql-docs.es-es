---
title: Cuadernos con kernel de SQL en Azure Data Studio
description: En este tutorial se muestra cómo crear y ejecutar un cuaderno de SQL Server.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray, alayu
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: d2d04be4d571b14445abfb638f070b3666ce9984
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040308"
---
# <a name="create-and-run-a-sql-server-notebook"></a>Creación y ejecución de un cuaderno de SQL Server

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

En este tutorial se muestra cómo crear y ejecutar un cuaderno en Azure Data Studio con SQL Server.

## <a name="prerequisites"></a>Prerrequisitos

- [Azure Data Studio instalado](../download-azure-data-studio.md)
- SQL Server instalado
  - [Windows](../../database-engine/install-windows/install-sql-server.md)
  - [Linux](../../linux/sql-server-linux-setup.md)

## <a name="create-a--notebook"></a>Creación de un cuaderno

En los pasos siguientes, se muestra cómo crear un archivo de cuaderno en Azure Data Studio:

1. En Azure Data Studio, conéctese a su instancia de SQL Server.

2. Seleccione las **Conexiones** en la ventana **Servidores**. Después, seleccione **Nuevo cuaderno**.

3. Espere hasta que se rellenen el **Kernel** y el contexto del destino (**Conectar a**). Confirme que **Kernel** está establecido en **SQL** y establezca **Adjuntar a** en la instancia de SQL Server (en este ejemplo, su *localhost*).

   ![Establecer el Kernel y Conectar a](media/notebooks-sql-kernel/set-kernel-and-attach-to.png)

Puede guardar el cuaderno mediante los comandos **Guardar** o **Guardar como...** del menú **Archivo**.

Para abrir un cuaderno, puede usar el comando **Abrir archivo...** del menú **Archivo**, seleccionar **Abrir archivo** en la página de **bienvenida** o usar el comando **Archivo: Abrir** de la paleta de comandos.

## <a name="change-the-sql-connection"></a>Cambio de la conexión SQL

Para cambiar la conexión SQL de un cuaderno:

1. Seleccione el menú **Adjuntar a** en la barra de herramientas del cuaderno y elija **Cambiar conexión**.

   ![Selección del menú "Adjuntar a" en la barra de herramientas del cuaderno](./media/notebooks-sql-kernel/select-attach-to-1.png)

2. Ahora puede seleccionar un servidor de conexión reciente o especificar nuevos detalles de conexión para conectarse.

## <a name="run-a-code-cell"></a>Ejecución de una celda de código

Puede crear celdas que contengan código SQL que puede ejecutar de forma local. Para ello, haga clic en el botón **Ejecutar celda** (la flecha redonda de color negro) a la izquierda de la celda. Cuando la celda termine de ejecutarse, los resultados se mostrarán en el cuaderno.

Por ejemplo:

1. Para agregar una nueva celda de código, seleccione el comando **+Código** de la barra de herramientas.

   ![Barra de herramientas de los cuaderno](media/notebooks-guidance/notebook-toolbar.png)

2. Copie y pegue el ejemplo siguiente en la celda y haga clic en **Ejecutar celda**. En este ejemplo se crea una base de datos.

   ```sql
   USE master
   GO

   -- Drop the database if it already exists
   IF  EXISTS (
           SELECT name
           FROM sys.databases
           WHERE name = N'TestNotebookDB'
      )
   DROP DATABASE TestNotebookDB
   GO

   -- Create the database
   CREATE DATABASE TestNotebookDB
   GO
   ```

   ![Ejecución de una celda](media/notebooks-sql-kernel/run-notebook-cell.png)

## <a name="save-the-result"></a>Guardado de un resultado

Si ejecuta un script que devuelve un resultado, puede guardarlo en distintos formatos mediante la barra de herramientas que se muestra sobre el resultado.

- Guardar como CSV
- Guardar como Excel
- Guardar como JSON
- Guardar como XML

Por ejemplo, el código siguiente devuelve el resultado de [PI](../../t-sql/functions/pi-transact-sql.md).

```sql
SELECT PI() AS PI;
GO
```

![Guardado de un resultado](media/notebooks-sql-kernel/run-notebook-cell-2.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los cuadernos:

- [Uso de cuadernos en Azure Data Studio](./notebooks-guidance.md)
- [Creación y ejecución de un cuaderno de Python](././notebooks-python-kernel.md)
- [Ejecución de un cuaderno de ejemplo con Spark](../../big-data-cluster/notebooks-tutorial-spark.md)