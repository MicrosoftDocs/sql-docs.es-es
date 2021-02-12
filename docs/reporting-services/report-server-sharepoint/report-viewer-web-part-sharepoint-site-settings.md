---
title: 'Configuración del sitio de SharePoint para el elemento web Visor de informes: SSRS | Microsoft Docs'
description: Obtenga información sobre cómo establecer la configuración del sitio de SharePoint en el elemento web Visor de informes del servidor de informes de SQL Server.
ms.date: 11/15/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: jt000
ms.author: jasontre
ms.openlocfilehash: 91969322c0f61d912ea26806c78f3074e38905e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016935"
---
# <a name="sharepoint-site-settings-for-the-report-viewer-web-part---reporting-services"></a>Configuración del sitio de SharePoint para el elemento web Visor de informes: Reporting Services | Microsoft Docs

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

El elemento web Visor de informes tiene un par de opciones que se pueden configurar. Un administrador del sitio puede habilitar y deshabilitar estas opciones en la página de configuración del sitio de SharePoint. Tenga en cuenta que cada sitio tiene su propia configuración. Además, esta configuración no se restablecerá después de reinstalar el elemento web Visor de informes.

## <a name="accessing-the-site-settings-page"></a>Acceso a la página de configuración del sitio

Para acceder a la configuración del sitio:

1. En el sitio de SharePoint, seleccione el icono de **engranaje** de la parte superior izquierda y luego **Configuración del sitio**.

    ![Configuración del sitio desde el icono de engranaje.](media/sharepoint-site-settings.png)

2. Haga clic en **Configuración del elemento web del Visor de informes** en el grupo de configuración del sitio de **Reporting Services**.

    > [!NOTE]
    > Para acceder a la configuración del sitio, también puede navegar directamente a `<site>/_layouts/15/ReportViewerWebPart/ReportViewerWebPartSettings.aspx`

## <a name="report-viewer-web-part-settings"></a>Configuración del elemento web Visor de informes

|Configuración|Comentarios|  
|-------------|--------------|  
|Recopilar los datos de uso|Habilita el envío de información de errores y uso a Microsoft para ayudar a mejorar nuestros productos. Para obtener información acerca de la directiva de recopilación de datos de informes de error, consulte la [Declaración de privacidad de Microsoft SQL Server](../../sql-server/sql-server-privacy.md).|  
|Habilitar los metadatos de accesibilidad para los informes|Define la información del dispositivo [`AccessibleTablix`](../html-device-information-settings.md) para los informes representados.|