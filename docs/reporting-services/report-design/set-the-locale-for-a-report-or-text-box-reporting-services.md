---
title: Establecer la configuración regional de un informe o un cuadro de texto (Reporting Services) | Microsoft Docs
description: Use la propiedad Idioma de un cuadro de texto a fin de proporcionar la configuración regional para los formatos que muestran datos que difieren en el idioma y la región en el Generador de informes.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- locales [Reporting Services]
ms.assetid: df115b01-184b-47f0-b5ec-0ad965ff9bee
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 345be03e2156e9097d9a7e60681ce901bd1e87b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045017"
---
# <a name="set-the-locale-for-a-report-or-text-box-reporting-services"></a>Establecer la configuración regional de un informe o un cuadro de texto (Reporting Services)
  La propiedad **Language** de un informe o un cuadro de texto contiene la configuración regional, que determina los formatos predeterminados para mostrar los datos de informe que difieren según el idioma y la región, como por ejemplo, la fecha, la moneda o los valores numéricos. La propiedad **Language** de un cuadro de texto invalida la propiedad **Language** del informe. Si no se especifica ningún valor para **Language**, Reporting Services usa la configuración regional del sistema operativo del servidor de informes para los informes publicados o la del equipo en que se crea el informe para la vista previa del informe.  
  
 Para los informes HTML, puede invalidar el valor de **Language** predeterminado y usar el idioma especificado por el encabezado HTTP del cliente de explorador usando el campo integrado User!Language en una expresión para la propiedad **Language** de un informe o un cuadro de texto.  
  
 También puede especificar la propiedad **Language** para un informe en una dirección URL. Para más información, vea [Establecer el idioma para los parámetros de informe en una dirección URL](../../reporting-services/set-the-language-for-report-parameters-in-a-url.md).  
  
### <a name="to-set-the-locale-for-a-report"></a>Para establecer la configuración regional de un informe  
  
1.  En la vista Diseño, haga clic fuera de la superficie de diseño del informe para seleccionar el informe.  
  
2.  En el panel Propiedades, en la propiedad **Language** , escriba o seleccione el idioma que desea usar para el informe.  
  
### <a name="to-set-the-locale-for-a-text-box"></a>Para establecer la configuración regional de un cuadro de texto  
  
1.  En la vista Diseño, seleccione el cuadro de texto al que desea aplicar la configuración regional.  
  
2.  En el panel Propiedades, haga lo siguiente:  
  
    -   En la propiedad **Calendar** , escriba o seleccione el calendario que desea usar para las fechas.  
  
    -   En la propiedad **Direction** , escriba o seleccione la dirección en que se escribe el texto.  
  
    -   En la propiedad **Language** , escriba o seleccione el idioma que desea usar para el cuadro de texto. Este valor invalida la propiedad **Language** para el informe.  
  
    -   En la propiedad **NumeralLanguage** , escriba o seleccione el formato que desea utilizar para los números del cuadro de texto.  
  
    -   En la propiedad **NumeralVariant** , escriba o seleccione la variante de formato que desea usar para los números del cuadro de texto.  
  
    -   En la propiedad **UnicodeBiDi** , seleccione el nivel de incrustación bidireccional que se va a usar en el cuadro de texto.  
  
## <a name="see-also"></a>Consulte también  
 [Usar expresiones en informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Consideraciones de diseño de soluciones para las implementaciones plurilingües o globales (Reporting Services)](/previous-versions/sql/)  
  
