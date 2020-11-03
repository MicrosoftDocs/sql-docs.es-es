---
title: Agregar obtención de detalles de un informe para dispositivos móviles a otros informes para dispositivos móviles o direcciones URL | Microsoft Docs
description: Puede agregar obtención de detalles desde cualquier cuadrícula de datos, de gráfico o de medidor de un informe para dispositivos móviles de Reporting Services a otro informe para dispositivos móviles o dirección URL personalizada.
ms.date: 09/20/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: 30d0a3fd-5588-417e-b25d-cc5b7624cdb1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 149c074b0aacc56f192b27cfea0894fe2cd73778
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907303"
---
# <a name="add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls"></a>Agregar obtención de detalles de un informe para dispositivos móviles a otros informes para dispositivos móviles o direcciones URL
Puede agregar obtención de detalles desde cualquier cuadrícula de datos, de gráfico o de medidor de un informe para dispositivos móviles de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] a otro informe para dispositivos móviles o dirección URL personalizada. 

Una *obtención de detalles*  es un vínculo de un informe de origen que abre otro informe de destino o dirección URL. A menudo, los informes de obtención de detalles de destino contienen detalles sobre algún elemento del informe de resumen. En función del informe para dispositivos móviles de origen, se pueden pasar uno o más parámetros al informe para dispositivos móviles de destino, o bien integrarse en una dirección URL personalizada.  
  
Cuando se ve un informe para dispositivos móviles de origen en el portal web de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] y se selecciona un elemento con un destino de obtención de detalles, se irá a dicho destino, sea este otro informe para dispositivos móviles o una dirección URL.  

Los elementos de informe con obtención de detalles, en una dirección URL o en otro informe móvil, tienen el icono :::image type="icon" source="../../reporting-services/mobile-reports/media/mobile-report-drill-through-icon.png":::.

![Captura de pantalla de un medidor de informe móvil con una obtención de detalles.](../../reporting-services/mobile-reports/media/mobile-report-gauge-drill-through.png)

>**Sugerencia** : antes de nada, cree el informe de destino y guárdelo en un portal web de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] . Si tiene previsto pasar parámetros desde el informe de origen, agregue dichos parámetros también al informe de destino. De este modo, podrá definir la obtención de detalles desde el informe de origen al informe de destino. [Agregar parámetros a un informe para dispositivos móviles](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md).
 
## <a name="set-up-drillthrough-to-a-mobile-report"></a>Configurar la obtención de detalles de un informe para dispositivos móviles  

1. En la vista Diseño del [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)], seleccione una visualización que admita la obtención de detalles.   

   Los mapas y los medidores admiten la obtención de detalles, como también la mayoría de gráficos y cuadrículas de datos simples.
   
2. En el panel **Propiedades de los elementos visuales** , seleccione **Obtener detalles del destino** > **Informe para dispositivos móviles**.  
3. Seleccione el servidor y el informe para dispositivos móviles de destino.  

   >Nota: si el informe para dispositivos móviles de destino no está en el mismo servidor que el informe para dispositivos móviles de origen, conéctese con dirección URL personalizada en su lugar, como se explica en la siguiente sección.  
 
4. Tras seleccionar un informe para dispositivos móviles de destino, se muestran los parámetros de entrada disponibles, incluidas las propiedades que se pueden enlazar a controles de navegador y los parámetros configurados en los conjuntos de datos del informe para dispositivos móviles de destino.  

   ![Captura de pantalla del cuadro de diálogo Configurar informe de destino que muestra los parámetros del informe disponibles.](../../reporting-services/mobile-reports/media/mobile-report-drillthrough-target.PNG)
   
   *Propiedades de obtención de detalles del informe para dispositivos móviles de destino*  
  
5. Seleccione la flecha a la derecha de cada propiedad para conectar las propiedades con tipos de datos coincidentes a las propiedades de salida disponibles en el informe para dispositivos móviles de origen. Aquí puede establecer también los valores predeterminados de cada salida, en caso de que el usuario del informe no haya interactuado con el informe para dispositivos móviles de origen antes de obtener detalles sobre el informe para dispositivos móviles de destino.  
  
## <a name="set-up-a-drillthrough-to-a-custom-url"></a>Configurar una obtención de detalles de una dirección URL personalizada  
  
1. En la vista Diseño del [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)], seleccione una visualización que admita la obtención de detalles de destinos.    
2. En el panel **Propiedades de los elementos visuales** , seleccione **Obtener detalles del destino** > **Dirección URL personalizada**.  Se abrirá el cuadro de diálogo de configuración de la obtención de detalles.  
  
3. En **Establecer la URL de obtención de detalles** , escriba la dirección URL de destino a la que se irá cuando se haga clic en la visualización. Seleccione también algunos de los **Parámetros disponibles** indicados a la derecha. En el siguiente panel se muestra una vista previa de la dirección URL personalizada combinada con parámetros resueltos de ejemplo (si se incluyen).  
  
   ![Captura de pantalla del cuadro de diálogo Establecer la URL de obtención de detalles.](../../reporting-services/mobile-reports/media/mobile-report-drillthrough-url.PNG)
  
   *Obtención de detalles de propiedades de dirección URL personalizada*  
  
4. Haga clic en **Aplicar**.  

  
Al obtener una vista previa de un informe para dispositivos móviles en el [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)], si hace clic en una visualización con obtención de detalles, verá un mensaje que indica que la obtención de detalles está deshabilitada. Realmente solo se pueden obtener detalles del destino después de guardar o publicar un informe para dispositivos móviles y, seguidamente, verlo, y no desde el modo de diseño o vista previa del [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] .  

## <a name="hide-a-target-mobile-report-on-the-web-portal"></a>Ocultar un informe para dispositivos móviles de destino en el portal web
Si no va a establecer un valor predeterminado para el informe de destino, considere la posibilidad de ocultarlo en el portal web. De lo contrario, cuando alguien intente verlo en el portal web directamente, sin pasar por el informe de origen, aparecerá vacío.

1. En el portal web, seleccione el botón de puntos suspensivos (...) en el informe de destino que quiera ocultar y seleccione Administrar.

2. En **Propiedades** , seleccione **Ocultar en la vista de mosaico**.

Puede elegir ver los elementos ocultos en el portal web: 

* En la esquina superior derecha del portal web, seleccione **Ver** > **Mostrar oculto**. 

Los elementos ocultos se muestran en un color más claro.
    
### <a name="see-also"></a>Consulte también  
 
* [Agregar parámetros a un informe para dispositivos móviles de Reporting Services](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md)
* [Creación y publicación de informes móviles con el Publicador de informes móviles de SQL Server](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) 
* [Portal web (modo nativo de SSRS)](../../reporting-services/web-portal-ssrs-native-mode.md)

