---
title: Copia de seguridad y restauración de una base de datos
description: Siga este tutorial para aprender a hacer copias de seguridad y restaurar bases de datos mediante Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 11/04/2019
ms.openlocfilehash: 851fbdf950656f9edfba76e3d5a0aae54e8e3d99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048175"
---
# <a name="tutorial-back-up-and-restore-databases-using-azure-data-studio"></a>Tutorial: Copia de seguridad y restauración de bases de datos con Azure Data Studio

En este tutorial, obtendrá información sobre cómo usar Azure Data Studio para:
> [!div class="checklist"]
> * Realizar una copia de seguridad de una base de datos.
> * Ver el estado de la copia de seguridad.
> * Generar el script usado para realizar la copia de seguridad.
> * Restaurar una base de datos.
> * Ver el estado de la tarea de restauración.

## <a name="prerequisites"></a>Requisitos previos

Para este tutorial, es necesario usar *TutorialDB* de SQL Server. Para crear la base de datos TutorialDB, complete el siguiente inicio rápido:

* [Conectarse y consultar SQL Server con Azure Data Studio](quickstart-sql-server.md)

Para este tutorial, es necesario conectarse a una base de datos de SQL Server. Azure SQL Database tiene copias de seguridad automatizadas, por lo que Azure Data Studio no realiza copias de seguridad y restauración de Azure SQL Database. Para más información, consulte [más información sobre copias de seguridad automáticas de SQL Database](/azure/sql-database/sql-database-automated-backups).

## <a name="back-up-a-database"></a>Realizar una copia de seguridad de una base de datos

1. Abra el panel de la base de datos de TutorialDB abriendo la barra lateral **SERVERS** (Servidores). A continuación, seleccione **Ctrl + G**, expanda **Databases** (Bases de datos), haga clic con el botón derecho en **TutorialDB** y seleccione **Manage** (Administrar).

1. Abra el cuadro de diálogo **Backup database** (Copia de seguridad de la base de datos) y seleccione **Backup** (Copia de seguridad) en el widget **Tasks** (Tareas).

   ![Captura de pantalla que muestra el widget Tasks (Tareas).](./media/tutorial-backup-restore-sql-server/tasks.png)

1. En este tutorial se usan las opciones de copia de seguridad predeterminadas, por lo que debe seleccionar **Backup** (Copia de seguridad).

   ![Captura de pantalla del cuadro de diálogo Backup (Copia de seguridad).](./media/tutorial-backup-restore-sql-server/backup-dialog.png)

Después de seleccionar **Backup**, (Copia de seguridad), desaparecerá el cuadro de diálogo **Backup database** (Copia de seguridad de la base de datos) y se iniciará el proceso de copia de seguridad.

## <a name="view-the-backup-status-and-the-backup-script"></a>Visualización del estado de la copia de seguridad y del script de copia de seguridad

1. Aparece el panel **Task History** (Historial de tareas) o seleccione **CTRL+T** para que se abra.

   ![Captura de pantalla que muestra el panel Task History (Historial de tareas).](./media/tutorial-backup-restore-sql-server/task-history.png)

1. Para ver el script de copia de seguridad en el editor, haga clic con el botón derecho en **Backup Database succeeded** (La copia de seguridad de la base de datos se ha completado correctamente) y seleccione **Script**.

   ![Captura de pantalla que muestra el script de copia de seguridad.](./media/tutorial-backup-restore-sql-server/task-script.png)

## <a name="restore-a-database-from-a-backup-file"></a>Restaurar una base de datos a partir de un archivo de copia de seguridad

1. Seleccione **Ctrl + G** para abrir la barra lateral **SERVERS** (Servidores). Después, haga clic con el botón derecho en el servidor y seleccione **Manage** (Administrar).

1. Abra el cuadro de diálogo **Ruta de acceso del archivo de copia de seguridad** (Restaurar base de datos). Para ello, seleccione **Restore** (Restaurar) en el widget **Tasks** (Tareas).

   ![Captura de pantalla que muestra la restauración de una tarea.](media/tutorial-backup-restore-sql-server/tasks-restore.png)

1. Seleccione **Backup file** (Archivo de copia de seguridad) en el cuadro de diálogo **Restore from** (Restaurar desde).

1. Seleccione los puntos suspensivos (…) del cuadro de seguridad **Ruta de acceso del archivo de copia de seguridad** y seleccione el archivo de copia de seguridad más reciente de *TutorialDB*.

1. Escriba **TutorialDB_Restored** en el campo **Target database** (Base de datos de destino) de la sección **Destination** (Destino) para restaurar el archivo de copia de seguridad en una nueva base de datos. Después, seleccione **Restaurar**.

   ![Captura de pantalla que muestra la restauración de la copia de seguridad.](./media/tutorial-backup-restore-sql-server/restore.png)

1. Para ver el estado de la operación de restauración, seleccione **CTRL+T** para abrir el **historial de tareas**.

   ![Captura de pantalla de restauración del historial de tareas.](./media/tutorial-backup-restore-sql-server/task-history-restore.png)