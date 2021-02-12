---
title: Agregar el elemento web Visor de informes de SQL Server Reporting Services a una página de SharePoint | Microsoft Docs
description: Muestre un informe, en SQL Server Reporting Services o en Power BI Report Server, agregando un elemento web Visor de informes a una página de SharePoint.
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7102f8cd25e3d0bdc2176308cb749b8d73290e0a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060610"
---
# <a name="add-sql-server-reporting-services-report-viewer-web-part-to-a-sharepoint-page"></a>Agregar el elemento web Visor de informes de SQL Server Reporting Services a una página de SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

Muestre un informe, en SQL Server Reporting Services o en Power BI Report Server, agregando un elemento web Visor de informes a una página de SharePoint.

![Elemento web Visor de informes en una página de SharePoint](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="prerequisites"></a>Requisitos previos

* Para que los informes se carguen correctamente, las notificaciones del servicio de token de Windows (C2WTS) deben estar configuradas para la delegación restringida de Kerberos. Para más información sobre cómo configurar C2WTS, vea [Notificaciones del servicio de token de Windows (C2WTS) y Reporting Services](../install-windows/claims-to-windows-token-service-c2wts-and-reporting-services.md).

* El elemento web Visor de informes se debe implementar en la granja de SharePoint. Para información sobre cómo implementar el proyecto de la solución del elemento web Visor de informes, vea [Deploy the Report Viewer web part on a SharePoint site](deploy-report-viewer-web-part.md) (Implementar el elemento web Visor de informes en un sitio de SharePoint).

* Para agregar un elemento web a una página web, debe tener el permiso Agregar y personalizar páginas en el nivel de sitio. Si va a usar la configuración de seguridad predeterminada, este permiso se concede a los miembros del grupo **Propietarios** que tienen el nivel de permiso Control total.

## <a name="add-web-part"></a>Agregar el elemento web

1. En su sitio de SharePoint, seleccione el icono del **engranaje**, situado en la esquina superior izquierda, y seleccione **Agregar una página**.

    ![Agregue una página a un sitio de SharePoint desde el icono del engranaje.](media/sharepoint-add-a-page.png)

2. Asigne un nombre a la página y seleccione **Crear**.

3. En el diseñador de páginas, seleccione la pestaña **Insertar** de la cinta de opciones. Luego, seleccione **Elemento web** en la sección **Elementos**.

    ![Inserte un elemento web desde la cinta de opciones de Office.](media/sharepoint-insert-web-part.png)

4. En **Categorías**, seleccione **SQL Server Reporting Services (modo nativo)** . En **Elementos**, seleccione **Visor de informes**. A continuación, seleccione **Agregar**.

    ![Agregue el elemento web Visor de informes.](media/sharepoint-report-viewer-web-part.png)

    Inicialmente, es posible que vea un error. El error se debe a que la dirección URL predeterminada del servidor de informes está establecida en *https://localhost* y puede que no esté disponible en esa ubicación.

## <a name="configure-the-report-viewer-web-part"></a>Configurar el elemento web Visor de informes

Para configurar el elemento web para que señale a un informe específico, siga estos pasos.

1. Al editar la página de SharePoint, seleccione la flecha hacia abajo, situada en la esquina superior derecha del elemento web, y seleccione **Editar elemento web**.

    ![Edite la página web desde el desplegable del elemento web.](media/sharepoint-edit-web-part.png)

2. Escriba la **dirección URL del servidor de informes** que hospeda el informe. La dirección URL debe tener un aspecto similar a *https://myrsserver/reportserver* .

3. Escriba la ruta de acceso y el nombre del informe que quiere mostrar en el elemento web. Será algo similar a */AdventureWorks Sample Reports/Company Sales*. En este ejemplo, el informe *Company Sales* (Ventas de la empresa) se encuentra en una carpeta denominada *AdventureWorks Sample Reports* (Informes de muestra de AdventureWorks).

4. Si el informe necesita parámetros, una vez proporcionada la dirección URL del servidor de informes y el nombre del informe, seleccione **Cargar parámetros** en la sección **Parámetros**.

5. Seleccione **Aceptar** para guardar los cambios en la configuración del elemento web.

6. Seleccione **Guardar** en la cinta de opciones de Office para guardar los cambios en la página de SharePoint.

![Elemento web Visor de informes en una página de SharePoint](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="considerations-and-limitations"></a>Consideraciones y limitaciones

* El elemento web Visor de informes no se puede usar en páginas modernas en SharePoint.
* Los informes de Power BI no se pueden usar con el elemento web Visor de informes.
* Si no ve el elemento web Visor de informes, para agregarlo a la página, asegúrese de que tiene [implementado el elemento web Visor de informes](deploy-report-viewer-web-part.md).

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
