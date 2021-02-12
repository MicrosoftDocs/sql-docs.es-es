---
title: Habilitar el widget de ejemplo de cinco consultas más lentas
description: En este tutorial se muestra cómo habilitar el widget de ejemplo de cinco consultas más lentas en el panel de base de datos.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu; sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 06/01/2020
ms.openlocfilehash: 6b8632f9ff5c07cb23a296b98a5933639abee4d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048145"
---
# <a name="tutorial-add-the-five-slowest-queries-sample-widget-to-the-database-dashboard"></a>Tutorial: Agregar el widget de ejemplo *cinco consultas más lentas* al panel de base de datos

En este tutorial se muestra el proceso de incorporación de uno de los widgets de ejemplo integrados de Azure Data Studio al *panel de base de datos* para ver rápidamente las cinco consultas más lentas de una base de datos. También obtendrá información sobre cómo ver los detalles de las consultas lentas y los planes de consulta mediante características de Azure Data Studio. En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Habilitar Almacén de consultas en una base de datos
> * Agregar un widget de información previamente integrado al panel de base de datos
> * Ver detalles sobre las consultas más lentas de la base de datos
> * Ver planes de ejecución de consulta para las consultas lentas

Azure Data Studio incluye varios widgets de conclusiones listos para usar. En este tutorial se muestra cómo agregar el widget *query-data-store-db-insight*, pero los pasos son básicamente los mismos para agregar cualquier widget.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se requiere la base de datos *TutorialDB* de SQL Server o Azure SQL Database. Para crear la base de datos *TutorialDB*, complete uno de los siguientes inicios rápidos:

* [Conectarse a una instancia de SQL Server y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)

* [Conectarse a una instancia de Azure SQL Database y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)

## <a name="turn-on-query-store-for-your-database"></a>Activar Almacén de consultas para la base de datos

El widget de este ejemplo requiere que *Almacén de consultas* esté habilitado.

1. Haga clic con el botón derecho en la base de datos **TutorialDB** (en la barra lateral **SERVIDORES**) y seleccione **Nueva consulta**.

2. Pegue la siguiente instrucción de Transact-SQL (T-SQL) en el editor de consultas y haga clic en **Ejecutar**:

   ```sql
    ALTER DATABASE TutorialDB SET QUERY_STORE = ON
   ```

## <a name="add-the-slow-queries-widget-to-your-database-dashboard"></a>Agregar el widget de consultas lentas al panel de base de datos

Para agregar el *widget de consultas lentas* al panel, edite el valor *dashboard.database.widgets* del archivo *Configuración de usuario*.

1. Abra *Configuración de usuario* al presionar **Ctrl + Mayús + P** para abrir la *paleta de comandos*.

2. Escriba *configuración* en el cuadro de búsqueda y seleccione **Preferencias: Abrir configuración de usuario**.

   ![Comando Abrir configuración de usuario](./media/tutorial-qds-sql-server/open-user-settings.png)

3. Escriba *dashboard* en el cuadro de búsqueda de configuración y busque **dashboard.database.widgets**; después, haga clic en *Editar en settings.json*.

   ![Búsqueda de configuración](./media/tutorial-qds-sql-server/search-settings.png)

4. En settings.json, agregue el siguiente código a continuación:

   ```json
   "dashboard.database.widgets": [
       {
           "name": "slow queries widget",
           "gridItemConfig": {
               "sizex": 2,
               "sizey": 1
           },
           "widget": {
               "query-data-store-db-insight": null
           }
       },
       {
           "name": "Tasks",
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 1
           },
           "widget": {
               "tasks-widget": {}
           }
       },
       {
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 2
           },
           "widget": {
               "explorer-widget": {}
           }
       }
   ]
   ```

5. Presione **Ctrl + S** para guardar la **Configuración de usuario** modificada.

6. Abra el *panel de la base de datos* al ir a **TutorialDB** en la barra lateral **SERVIDORES**, hacer clic con el botón derecho y seleccionar **Administrar**.

   ![Apertura del panel](./media/tutorial-qds-sql-server/insight-open-dashboard.png)

7. El widget de información aparece en el panel:

   ![Widget QDS](./media/tutorial-qds-sql-server/insight-qds-result.png)

## <a name="view-insight-details-for-more-information"></a>Ver detalles para obtener más información

1. Para ver información adicional de un widget de información, haga clic en los puntos suspensivos ( **...** ) de la esquina superior derecha y seleccione **Mostrar detalles**.

2. Para mostrar más detalles de un elemento, seleccione cualquier elemento de la lista **Datos de gráfico**.

   ![Cuadro de diálogo de detalles de Información](./media/tutorial-qds-sql-server/insight-details-dialog.png)

3. Cierre el panel **Información**.

## <a name="view-the-query-plan"></a>Ver el plan de consulta

1. Haga clic con el botón derecho en la base de datos **TutorialDB** y seleccione *Administrar*.

2. Desde el *widget de consultas lento*, para ver información adicional de un widget de información, haga clic en los puntos suspensivos ( **...** ) de la esquina superior derecha y seleccione **Ejecutar consulta**.

    ![Ejecución de la consulta](media/tutorial-qds-sql-server/run-query.png)

3. Ahora debería ver una ventana de consulta nueva con los resultados.

    ![Resultados de Ejecutar consulta](media/tutorial-qds-sql-server/run-query-results.png)

4. Haga clic en **Explicar**.

   ![Explicación de QDS en Información](./media/tutorial-qds-sql-server/insight-qds-explain.png)

5. Vea el plan de ejecución de la consulta:

   ![Captura de pantalla en la que se muestra el plan de ejecución de consultas.](./media/tutorial-qds-sql-server/showplan.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> * Habilitar Almacén de consultas en una base de datos
> * Agregar un widget de información al panel de base de datos
> * Ver detalles sobre las consultas más lentas de la base de datos
> * Ver planes de ejecución de consulta para las consultas lentas

Para obtener información sobre cómo habilitar la información de ejemplo de **uso de espacio de tabla**, complete el tutorial siguiente:

> [!div class="nextstepaction"]
> [Habilitar el widget de información de ejemplo de espacio de tabla](tutorial-table-space-sql-server.md)