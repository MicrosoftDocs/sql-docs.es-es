---
title: Habilitación del widget de datos de ejemplo de uso de espacio de tabla
description: En este tutorial se muestra cómo habilitar el widget de datos de ejemplo de uso de espacio de tabla en el panel de base de datos de Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/10/2019
ms.openlocfilehash: 128b9e0b63cf006577250afe04e6a28c92379497
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048085"
---
# <a name="tutorial-enable-the-table-space-usage-sample-insight-widget-using-azure-data-studio"></a>Tutorial: Habilitación del widget de datos de ejemplo de uso de espacio de tabla con Azure Data Studio

En este tutorial se muestra cómo habilitar un widget de datos en el panel de base de datos, lo que proporciona una vista rápida sobre el uso de espacio para todas las tablas de una base de datos. En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Activación rápida de un widget de datos mediante un ejemplo de widget de datos integrado
> * Visualización de los detalles del uso de espacio de tabla
> * Filtración de datos y visualización de los detalles de la etiqueta en un gráfico de conclusiones

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se requiere la base de datos *TutorialDB* de SQL Server o Azure SQL Database. Para crear la base de datos *TutorialDB*, complete uno de los siguientes inicios rápidos:

* [Conectarse a una instancia de SQL Server y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
* [Conectarse a una instancia de Azure SQL Database y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)

## <a name="turn-on-a-management-insight-on-azure-data-studios-database-dashboard"></a>Activación de los datos de administración en el panel de base de datos de Azure Data Studio

Azure Data Studio tiene un widget de ejemplo integrado para supervisar el espacio que usan las tablas de una base de datos.

1. Abra *Configuración de usuario* al presionar **Ctrl + Mayús + P** para abrir la *paleta de comandos*.

2. Escriba *configuración* en el cuadro de búsqueda y seleccione **Preferencias: Abrir configuración de usuario**.

3. Escriba *dashboard* en el cuadro de entrada de búsqueda de configuración y busque **dashboard.database.widgets**.

4. Para personalizar la configuración de **dashboard.database.widgets**, debe modificar la entrada **dashboard.database.widgets** de la sección **CONFIGURACIÓN DE USUARIO**.

   ![Captura de pantalla en la que se muestra la sección CONFIGURACIÓN DE USUARIO con la sección Panel > Base de datos: Widgets resaltada.](media/tutorial-table-space-sql-server/search-settings.png)

   Si **dashboard.database.widgets** no aparece en la sección **CONFIGURACIÓN DE USUARIO**, mantenga el mouse sobre el texto **dashboard.database.widgets** de la columna CONFIGURACIÓN PREDETERMINADA, haga clic en el icono de *engranaje* que aparece a la izquierda del texto y, luego, en **Copy as Setting JSON** (Copiar como configuración JSON). Si el elemento emergente indica **Reemplazar en Configuración**, no haga clic en él. Vaya a la columna **CONFIGURACIÓN DE USUARIO** a la derecha, busque la sección **dashboard.database.widgets** y vaya al paso siguiente.

5. En la sección **dashboard.database.widgets**, agregue las líneas siguientes:

   ```json
        {
            "name": "Space Used by Tables",
            "gridItemConfig": {
                "sizex": 2,
                "sizey": 1
            },
            "widget": {
                "table-space-db-insight": null
            }
        },
    ```

   La sección **dashboard.database.widgets** debería ser similar a la imagen siguiente:

    ![Captura de pantalla del archivo settings.json con el primer objeto de la matriz dashboard.database.widgets.](./media/tutorial-table-space-sql-server/insight-table-space.png)

6. Presione **CTRL+S** para guardar la configuración.

7. Para abrir el panel de base de datos, haga clic con el botón derecho en **TutorialDB** y haga clic en **Administrar**.

8. Vea el widget de datos de *espacio de tabla* como se muestra en la siguiente imagen:

   ![Widget](./media/tutorial-table-space-sql-server/insight-table-space-result.png)

## <a name="working-with-the-insight-chart"></a>Trabajo con el gráfico de conclusiones

El gráfico de conclusiones de Azure Data Studio proporciona detalles de filtrado y de desplazamiento del mouse. Para probar los pasos siguientes:

1. Haga clic en la leyenda *row_count* del gráfico y actívela y desactívela. Azure Data Studio muestra y oculta las series de datos a medida que activa o desactiva una leyenda.

2. Mueva el puntero del mouse sobre el gráfico. Azure Data Studio muestra más información sobre la etiqueta de la serie de datos y su valor, tal como se muestra en la siguiente captura de pantalla.

   ![leyenda y activación y desactivación del gráfico](./media/tutorial-table-space-sql-server/insight-table-space-toggle.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> * Activación rápida de un widget de datos mediante un ejemplo de widget de datos integrado
> * Visualización de los detalles del uso de espacio de tabla
> * Filtración de datos y visualización de los detalles de la etiqueta en un gráfico de conclusiones

Para obtener información sobre cómo compilar un widget de datos personalizado, complete el siguiente tutorial:

> [!div class="nextstepaction"]
> [Compilación de un widget de información personalizada](tutorial-build-custom-insight-sql-server.md)
