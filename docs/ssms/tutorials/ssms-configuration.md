---
title: Configuración y componentes de SSMS
description: Tutorial en el que se describen los componentes y las opciones de configuración básicas para su entorno de SQL Server Management Studio.
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/16/2018
ms.openlocfilehash: 3c2145db468fb0c6423cbd2ec1bc45d7902b2cdf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081816"
---
# <a name="sql-server-management-studio-components-and-configuration"></a>Componentes y configuración de SQL Server Management Studio

En este tutorial se describen los distintos componentes de ventana que hay en SQL Server Management Studio (SSMS), así como algunas opciones de configuración básicas para el área de trabajo. En este artículo aprenderá a: 

> [!div class="checklist"]
> * Identificar los componentes que conforman el entorno de SSMS
> * Cambiar el diseño del entorno y restablecerlo a los valores predeterminados
> * Maximizar el editor de consultas
> * Cambio de la fuente
> * Configurar las opciones de inicio
> * Restablecer la configuración predeterminada

## <a name="prerequisites"></a>Prerequisites

Para llevar a cabo este tutorial, necesita tener SQL Server Management Studio.  

* Instale [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).

## <a name="sql-server-management-studio-components"></a>Componentes de SQL Server Management Studio

En esta sección se describen los distintos componentes de ventana que están disponibles en el área de trabajo y cómo usarlos.

* Para cerrar una ventana, seleccione la **X** de la esquina derecha de la barra de título.
* Para volver a abrir una ventana, seleccione la ventana en el menú **Ver**.

    ![Menú Ver](media/ssms-configuration/viewmenu.png)

* **Explorador de objetos** (F8): el Explorador de objetos es una vista de árbol de todos los objetos de base de datos que contiene un servidor. Esta vista incluye las bases de datos de Motor de base de datos de SQL Server, SQL Server Analysis Services, SQL Server Reporting Services y SQL Server Integration Services. El Explorador de objetos incluye información de todos los servidores a los que está conectado. 

    ![Explorador de objetos](media/ssms-configuration/objectexplorer.png)
* **Ventana Consulta** (Ctrl + N): después de seleccionar **Nueva consulta**, escriba las consultas de Transact-SQL (T-SQL) en esta ventana. Los resultados de las consultas también aparecerán aquí.

    ![Ventana Nueva consulta](media/ssms-configuration/newquery.png)

* **Propiedades** (F4): puede ver la vista Propiedades cuando la ventana Consulta esté abierta. La vista muestra las propiedades básicas de la consulta. Por ejemplo, se muestra la hora de inicio de una consulta, el número de filas devueltas o los detalles de conexión.  

    ![Propiedades de configuración](media/ssms-configuration/properties.png)

* **Explorador de plantillas** (Ctrl + Alt + T): el Explorador de plantillas tiene varias plantillas de T-SQL predefinidas. Puede usar estas plantillas para llevar a cabo varias funciones, como crear una base de datos o hacer una copia de seguridad de la base de datos. 

    ![Explorador de plantillas](media/ssms-configuration/templates.png)

* **Detalles del Explorador de objetos** (F7): esta vista está más granular que la vista del Explorador de objetos. Puede usar Detalles del Explorador de objetos para manipular varios objetos a la vez. Por ejemplo, en esta ventana puede seleccionar varias bases de datos y, después, eliminarlas o generar un script para ellas de forma simultánea. 

    ![Detalles del Explorador de objetos](media/ssms-configuration/objectexplorerdetails.PNG) 

## <a name="change-the-environment-layout"></a>Cambiar el diseño del entorno 

En esta sección se describe cómo cambiar el diseño del entorno (por ejemplo, mover varias ventanas). 

* Para mover una ventana, mantenga presionado el título y, después, arrastre la ventana. 
* Para anclar o desanclar una ventana, seleccione el icono de la chincheta de la barra de título:

    ![Anclar un objeto](media/ssms-configuration/pushpin.png)

* Cada componente de la ventana tiene un menú desplegable que se puede usar para manipular la ventana de distintas maneras: 

    ![Opciones de ventana](media/ssms-configuration/windowoptions.png)

* Cuando hay dos o más ventanas de consulta abiertas, puede desplazarse por ellas mediante la tabulación de forma vertical u horizontal de modo que las dos ventanas estén siempre visibles. Para ver las ventanas con pestañas, haga clic con el botón derecho en el título de la consulta y, después, seleccione la opción con pestañas que quiera:

    ![Opciones de pestaña de consultas](media/ssms-configuration/querytabbedoptions.png)

    * Este es un grupo de pestañas horizontal:

      ![Ejemplo de grupo de pestañas horizontal](media/ssms-configuration/horizontaltab.png)

    * Este es un grupo de pestañas vertical:

      ![Ejemplo de grupo de pestañas vertical](media/ssms-configuration/verticaltabgroup.png)

    * Para combinar las pestañas, haga clic con el botón derecho en el título de la consulta y, después, seleccione **Mover al grupo de pestañas anterior** o **Mover al grupo de pestañas siguiente**:

      ![Combinar pestañas de consulta](media/ssms-configuration/mergetabgroups.png)

* Para restaurar el diseño de entorno predeterminado, vaya al menú **Ventana** y seleccione **Restablecer diseño de la ventana**:

    ![Restablecer diseño de la ventana](media/ssms-configuration/resetwindowlayout.png)

## <a name="maximize-query-editor"></a>Maximizar el editor de consultas

Puede maximizar el Editor de consultas al modo de pantalla completa:

1. Haga clic en cualquier parte de la ventana del Editor de consultas.

2. Presione Mayús + Alt + Entrar para alternar entre el modo normal y el modo de pantalla completa. 

Este método abreviado de teclado funciona con cualquier ventana de documento. 

## <a name="change-basic-settings"></a>Cambiar la configuración básica

En esta sección se describe cómo modificar algunas opciones básicas de SSMS desde el menú **Herramientas**.

  ![Herramientas, menú](media/ssms-configuration/tools.png)

* Para modificar la barra de herramientas resaltada, seleccione **Herramientas** > **Personalizar**:

    ![Personalizar una barra de herramientas](media/ssms-configuration/toolbar.png)

### <a name="change-the-font"></a>Cambio de la fuente

* Para cambiar la fuente, seleccione **Herramientas** > **Opciones** > **Fuentes y colores**:

     ![Cambiar fuentes y colores](media/ssms-configuration/fontsandcolors.png)

### <a name="change-startup-options"></a>Cambiar las opciones de inicio

* Las opciones de inicio determinan el aspecto del área de trabajo la primera vez que se abre SSMS. Para cambiar las opciones de inicio, seleccione **Herramientas** > **Opciones** > **Inicio**:

    ![Cambiar las opciones de inicio](media/ssms-configuration/startup.png)

### <a name="reset-settings-to-the-default"></a>Restablecimiento de la configuración predeterminada

* Puede exportar e importar todas estas opciones desde el menú. Para importar o exportar la configuración, o para restaurar la configuración predeterminada, seleccione **Herramientas** > **Importar y exportar configuraciones** 

    ![Importar y exportar configuraciones](media/ssms-configuration/settings.png)

## <a name="next-steps"></a>Pasos siguientes

La mejor forma de familiarizarse con SSMS es practicar. Estos *tutoriales* y artículos de *procedimientos* lo ayudan con varias características disponibles dentro de SSMS.  Estos artículos le mostrarán cómo administrar los componentes de SSMS y cómo localizar las características que utiliza habitualmente.

* [Conexión a una instancia y realización de consultas](../quickstarts/ssms-connect-query-sql-server.md)
* [Scripting](scripting-ssms.md)
* [Uso de plantillas en SSMS](../template/templates-ssms.md)
* [Otras recomendaciones y trucos al usar SSMS](ssms-tricks.md)