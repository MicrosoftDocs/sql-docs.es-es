---
title: Uso de plantillas en SQL Server Management Studio
description: Obtenga información sobre las plantillas predefinidas de Transact-SQL (T-SQL) que están disponibles en SQL Server Management Studio (SSMS).
keywords: SQL Server, SSMS, SQL Server Management Studio, Plantillas
author: MashaMSFT
ms.author: mathoma
ms.topic: tutorial
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.reviewer: sstein
helpviewer_keywords:
- templates [SQL Server], SQL Server Management Studio
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
ms.custom: seo-lt-2019
ms.date: 03/13/2018
ms.openlocfilehash: b3c5c26ddf8e343b2cf63bc0dde337056eb2fe74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017235"
---
# <a name="use-templates-in-sql-server-management-studio"></a>Uso de plantillas en SQL Server Management Studio

En este tutorial se presentan las plantillas predefinidas de Transact-SQL (T-SQL) que están disponibles en SQL Server Management Studio (SSMS). En este artículo aprenderá a:

## <a name="prerequisites"></a>Prerequisites

Para llevar a cabo este tutorial, necesita tener SQL Server Management Studio, así como acceso a un servidor SQL Server.

* Instale [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).

* Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).

## <a name="use-template-browser"></a>Usar el explorador de plantillas

En esta sección aprenderá a buscar y usar el explorador de plantillas.

1. Abra SQL Server Management Studio.

2. En el menú **Vista**, seleccione **Explorador de plantillas** (Ctrl + Alt + T):

    ![Abrir el explorador de plantillas](media/templates-ssms/templatebrowser.png)

    Puede ver las plantillas que se han usado recientemente en la parte inferior del explorador de plantillas.

3. Expanda el nodo que le interesa. Haga clic con el botón derecho en la plantilla y, después, seleccione **Abrir**:

    ![Abrir una plantilla](media/templates-ssms/opentemplate.png)

    También puede hacer doble clic en el nombre de la plantilla para abrirla.

4. Se abrirá una nueva ventana de consulta. El script de T-SQL ya se habrá rellenado.

5. Modifique la plantilla según sus necesidades y, después, seleccione **Ejecutar** para ejecutar la consulta:

    ![Crear una plantilla de base de datos](media/templates-ssms/createdbtemplate.png)

## <a name="edit-an-existing-template"></a>Editar una plantilla existente

También puede editar las plantillas existentes en el explorador de plantillas.  

1. En Explorador de plantillas, vaya a la plantilla con la que quiere trabajar.

2. Haga clic con el botón derecho en la plantilla y, después, seleccione **Editar**:

    ![Editar una plantilla](media/templates-ssms/edittemplate.png)

3. En la ventana de consulta que se abre, efectúe los cambios que quiera.

4. Para guardar la plantilla, seleccione **Archivo** > **Guardar** (Ctrl + S).

5. Cierre la ventana de consulta.

6. Vuelva a abrir la plantilla. Deberían aparecer las ediciones.

## <a name="locate-templates-on-disk"></a>Buscar plantillas en el disco

Si tiene una plantilla abierta, puede buscar las plantillas que están en el disco.

1. En Explorador de plantillas, seleccione una plantilla y, después, seleccione **Editar**.

2. Haga clic con el botón derecho en **Título de la consulta** y, después, seleccione **Abrir carpeta contenedora**. Se debería abrir el explorador en la ubicación en la que se almacenan en disco las plantillas: 

   ![Plantillas en el disco](media/templates-ssms/templatesondisk.png)
  
## <a name="create-a-new-template"></a>Crear una plantilla

También puede crear una plantilla en Explorador de plantillas. En los siguientes pasos se indica cómo crear una carpeta y, después, crear una plantilla dentro de esa carpeta. También puede seguir estos pasos para crear una plantilla personalizada en una carpeta existente. 

1. Abra Explorador de plantillas.

2. Haga clic con el botón derecho en **Plantillas de SQL Server** y, después, seleccione **Nuevo** > **Carpeta**.

3. Asigne a esta carpeta el nombre **Plantillas personalizadas**:

    ![Crear una carpeta de plantillas personalizadas](media/templates-ssms/creatingcustomtemplate.png)

4. Haga clic con el botón derecho en la carpeta Plantillas personalizadas que acaba de crear y, después, seleccione **Nuevo** > **Plantilla**. Escriba un nombre para la plantilla:

    ![Crear una plantilla personalizada](media/templates-ssms/createnewtemplate.png)

5. Haga clic con el botón derecho en la plantilla que ha creado y, después, seleccione **Editar**. Se abrirá la ventana Nueva consulta.

6. Escriba el texto de T-SQL que quiere guardar.

7. En el menú **Archivo**, seleccione **Guardar**.

8. Cierre la ventana de consulta existente y abra la nueva plantilla personalizada.

## <a name="next-steps"></a>Pasos siguientes

La mejor forma de familiarizarse con SSMS es practicar. Estos *tutoriales* y artículos de *procedimientos* lo ayudan con varias características disponibles dentro de SSMS.  Estos artículos le mostrarán cómo administrar los componentes de SSMS y cómo localizar las características que utiliza habitualmente.

* [Conexión a una instancia y realización de consultas](../quickstarts/ssms-connect-query-sql-server.md)
* [Scripting](../tutorials/scripting-ssms.md)
* [Configuración de SSMS](../tutorials/ssms-configuration.md)
* [Otras recomendaciones y trucos al usar SSMS](../tutorials/ssms-tricks.md)