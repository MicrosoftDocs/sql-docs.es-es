---
title: Conectar el elemento web Filtro o Documentos con un elemento web Visor de informes de Reporting Services | Microsoft Docs
description: Para un producto de SharePoint, obtenga información sobre cómo crear un panel o una página de elementos web que incluya un elemento web Filtro o Documentos, y un elemento web Visor de informes.
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0e58600c502aa6ebb0a031480ab5bb66f48833ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074856"
---
# <a name="connect-filter-or-documents-web-part-with-a-reporting-services-report-viewer-web-part"></a>Conectar el elemento web Filtro o Documentos con un elemento web Visor de informes de Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Si usa un producto de SharePoint, puede crear un panel o una página de elementos web que incluya un elemento web Filtro o Documentos y un elemento web Visor de informes. Las versiones admitidas son [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] o [!INCLUDE[SPS2010](../../includes/sps2010-md.md)]. También se admiten [!INCLUDE[winSPServ3](../../includes/winspserv3-md.md)] u [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007. Al conectar un elemento web Filtro, los usuarios que seleccionan valores de filtro en un elemento web Filtro pueden enviar el valor a un informe con parámetros en la misma página. Al conectar un elemento web Documentos, los usuarios que hacen clic en informes en la biblioteca de documentos pueden ver el informe en un elemento web Visor de informes adyacente.

> [!NOTE]
> La integración de Reporting Services con SharePoint ya no está disponible a partir de SQL Server 2016.

 El elemento web Filtro sirve para enviar valores a uno o varios parámetros de un informe. Para usar un elemento web Filtro, el informe debe tener parámetros definidos que sean compatibles con los valores, el tipo de datos y el formato que haya enviado el elemento web.  
  
 El elemento web Documentos está asociado a la biblioteca de documentos del sitio principal. Para ver, agregar o quitar elementos de la biblioteca de documentos, haga clic en **Ver todo el contenido del sitio**. En Bibliotecas, haga clic en **Documentos**. Puede utilizar los menús **Nuevo**, **Cargar** y **Acciones** para administrar los elementos de la biblioteca de documentos.  
  
## <a name="connect-a-filter-web-part"></a>Conectar un elemento web Filtro
  
1.  Abra o cree el panel o la página de elementos web.  
  
2.  En el menú **Acciones del sitio** , haga clic en **Editar página**.  
  
3.  Haga clic en **Agregar elemento web**.  
  
4.  En **Todos los elementos web**, en la categoría **Varios**, seleccione **Visor de informes de SQL Server Reporting Services**.  
  
5.  Haga clic en **Agregar**. El elemento web se agrega a la parte superior de la zona.  
  
6.  En otra zona del mismo panel o página de elementos web, haga clic en **Agregar elemento web**.  
  
7.  En **Todos los elementos web**, en la sección **Filtros**, seleccione un elemento web.  
  
8.  Haga clic en **Agregar**. El elemento web se agrega a la parte superior de la zona.  
  
9. En la zona que contiene el elemento web, haga clic en el menú **Edición** del elemento web, seleccione **Conexiones**, **Enviar valores de filtro a** y después **Visor de informes** - *nombre de informe*.  
  
10. Proteja sus cambios y guarde la página.  
  
## <a name="connect-a-documents-web-part"></a>Conectar un elemento web Documentos  
  
1.  Abra o cree el panel o la página de elementos web.  
  
2.  En el menú **Acciones del sitio** , haga clic en **Editar página**.  
  
3.  Haga clic en **Agregar elemento web**.  
  
4.  En **Todos los elementos web**, en la sección **Listas y biblioteca**, seleccione **Documentos**.  
  
5.  Haga clic en **Agregar**. El elemento web se agrega a la parte superior de la zona.  
  
6.  Haga clic en **Aplicar** en la parte inferior del panel de herramientas y, a continuación, haga clic en **Aceptar** para cerrar el panel.  
  
7.  En otra zona del mismo panel o página de elementos web, haga clic en **Agregar elemento web**.  
  
8.  En **Todos los elementos web**, en la categoría **Varios**, seleccione **Visor de informes de SQL Server Reporting Services**.  
  
9. Haga clic en **Agregar**. El elemento web se agrega a la parte superior de la zona.  
  
10. En la zona que contiene el elemento web, haga clic en el menú **Edición** del elemento web, seleccione **Conexiones**, **Obtener definiciones de informe de** y luego **Documentos**.  
  
11. Proteja sus cambios y guarde la página.  
  
## <a name="see-also"></a>Consulte también

 [Agregar el elemento web Visor de informes a una página web](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)   
 [Elemento web Visor de informes en un sitio de SharePoint](./report-viewer-web-part-sharepoint-site.md)   
 [Personalización del elemento web Visor de informes](../../reporting-services/report-server-sharepoint/customize-the-report-viewer-web-part.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).