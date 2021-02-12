---
title: Compilación de un widget de información personalizada
description: En este tutorial se muestra cómo crear widgets de información personalizada y agregarlos a los paneles de la base de datos y el servidor en Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 08/26/2020
ms.openlocfilehash: bd4d3c12ea7a56ceddec0ad23f7276a4be4b9f40
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039965"
---
# <a name="tutorial-build-a-custom-insight-widget"></a>Tutorial: Compilación de un widget de información personalizada

En este tutorial se muestra cómo usar sus propias consultas de información para compilar widgets de información personalizada.

Durante este tutorial aprenderá a realizar lo siguiente:
> [!div class="checklist"]
> * Ejecución de su propia consulta y su visualización en un gráfico
> * Compilación de un widget de información personalizada a partir del gráfico
> * Adición del gráfico a un panel del servidor o de la base de datos
> * Adición de detalles al widget de información personalizada

## <a name="prerequisites"></a>Prerrequisitos

En este tutorial se requiere la base de datos *TutorialDB* de SQL Server o Azure SQL Database. Para crear la base de datos *TutorialDB*, complete uno de los siguientes inicios rápidos:

- [Conectarse a una instancia de SQL Server y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Conectarse a una instancia de Azure SQL Database y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="run-your-own-query-and-view-the-result-in-a-chart-view"></a>Ejecución de su propia consulta y visualización del resultado en una vista de gráfico
En este paso, ejecute un script SQL para consultar las sesiones activas actuales.

1. Para abrir un nuevo editor, presione **Ctrl + N**. 

2. Cambie el contexto de conexión a **TutorialDB**.

3. Pegue la siguiente consulta en el editor de consultas:

   ```sql
   SELECT count(session_id) as [Active Sessions]
   FROM sys.dm_exec_sessions
   WHERE status = 'running'
   ```

4. Guarde la consulta del editor en un archivo \*.sql. En este tutorial, guarde el script como *activeSession.sql*.

5. Para ejecutar la consulta, presione **F5**.

6. Una vez mostrados los resultados de la consulta, haga clic en **Ver como gráfico** y después haga clic en la pestaña **Visor de gráficos**.

7. Cambie el **Tipo de gráfico** a **recuento**. Esta configuración representa un gráfico de recuento.

## <a name="add-the-custom-insight-to-the-database-dashboard"></a>Adición de la información personalizada al panel de la base de datos

1. Para abrir la configuración del widget de información, haga clic en **Create Insight** (Crear información) en *Chart Viewer* (Visor de gráficos):

   ![configuración](./media/tutorial-build-custom-insight-sql-server/create-insight.png)
   
2. Copie la configuración de información (los datos JSON). 

3. Presione **Ctrl + coma** para abrir *Configuración de usuario*.

4. Escriba *dashboard* en *Configuración de búsqueda*.

5. Haga clic en **Editar** para *dashboard.database.widgets*.

   ![configuración del panel](./media/tutorial-build-custom-insight-sql-server/dashboard-settings.png)

6. Pegue el archivo JSON de configuración de información en *dashboard.database.widgets*. La configuración del panel de base de datos tiene el siguiente aspecto:

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql"
                }
            }
        }
    ]
   ```

7. Guarde el archivo de *Configuración de usuario* y abra el panel de base de datos *TutorialDB* para ver el widget de sesiones activas:

   ![Panel de información de sesiones activas](./media/tutorial-build-custom-insight-sql-server/insight-activesession-dashboard.png)

## <a name="add-details-to-custom-insight"></a>Adición de detalles a información personalizada

1. Para abrir un nuevo editor, presione **Ctrl + N**.

2. Cambie el contexto de conexión a **TutorialDB**.

3. Pegue la siguiente consulta en el editor de consultas:

   ```sql
    SELECT session_id AS [SID], login_time AS [Login Time], host_name AS [Host Name], program_name AS [Program Name], login_name AS [Login Name]
    FROM sys.dm_exec_sessions
    WHERE status = 'running'
   ```

4. Guarde la consulta del editor en un archivo \*.sql. En este tutorial, guarde el script como *activeSessionDetail.sql*.

5. Presione **Ctrl + coma** para abrir *Configuración de usuario*.

6. Edite el nodo existente *dashboard.database.widgets* en el archivo de configuración:

   ```json
    "dashboard.database.widgets": [
        {
            "name": "My-Widget",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "insights-widget": {
                    "type": {
                        "count": {
                            "dataDirection": "vertical",
                            "dataType": "number",
                            "legendPosition": "none",
                            "labelFirstColumn": false,
                            "columnsAsLabels": false
                        }
                    },
                    "queryFile": "{your file folder}/activeSession.sql",
                    "details": {
                        "queryFile": "{your file folder}/activeSessionDetail.sql",
                        "label": "SID",
                        "value": "Login Name"
                    }
                }
            }
        }
    ]
   ```

7. Guarde el archivo de *Configuración de usuario* y abra el panel de base de datos *TutorialDB*. Haga clic en el botón de puntos suspensivos (...) situado junto a *My-Widget* para mostrar los detalles:

    ![Detalles de la información de sesiones activas](./media/tutorial-build-custom-insight-sql-server/insight-activesession-detail.png)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a:
> [!div class="checklist"]
> * Ejecución de su propia consulta y su visualización en un gráfico
> * Compilación de un widget de información personalizada a partir del gráfico
> * Adición del gráfico a un panel del servidor o de la base de datos
> * Adición de detalles al widget de información personalizada

Para obtener información sobre cómo realizar copias de seguridad y restaurar bases de datos, complete el siguiente tutorial:

> [!div class="nextstepaction"]
> [Copias de seguridad y restauración de bases de datos](tutorial-backup-restore-sql-server.md).
