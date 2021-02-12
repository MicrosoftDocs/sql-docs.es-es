---
title: Generación de fuentes de distribución de datos a partir de un informe (Generador de informes) | Microsoft Docs
description: Puede generar fuentes de distribución de datos compatibles con Atom a partir de informes paginados. Use las fuentes en aplicaciones, como Power Pivot o Power BI, que pueden consumir fuentes de distribución de datos.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: e68baae2-9f2a-4f13-9179-9ac7f29111c5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d61d02d7c5bf3e4de8199ebdc59f1c6f8ed157b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015655"
---
# <a name="generate-data-feeds-from-a-report-report-builder-and-ssrs"></a>Generar fuentes de distribución de datos a partir de un informe (Generador de informes y SSRS)

Puede generar fuentes de distribución de datos compatibles con Atom a partir de informes paginados y, después, usar las fuentes de distribución de datos en las aplicaciones, Power Pivot o Power BI, que pueden usar fuentes de distribución de datos.  
  
 La extensión de representación de Atom de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] genera un documento de servicio de Atom que enumera las fuentes de distribución de datos disponibles en un informe. El documento enumera al menos una fuente de distribución de datos para cada región de datos del informe. Según el tipo de región de datos y los datos que esta muestra, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] podría generar varias fuentes de distribución de datos a partir de una región de datos.  
  
 El documento de servicio de Atom contiene un identificador único para cada una de las fuentes de distribución de datos que se usa en una dirección URL para ver el contenido de la fuente de distribución de datos.  
  
 Para obtener más información, vea [Generar fuentes de distribución de datos a partir de informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-generate-an-atom-service-document"></a>Generar un documento de servicio de Atom  
  
1.  en el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , navegue hasta el informe para el que quiere generar fuentes de distribución de datos.  
  
2.  Haga clic en el informe.  
  
     El informe se ejecuta.  
  
3.  En la barra de herramientas, haga clic en el icono **Exportar a fuente de distribución de datos** .  
  
     Aparece un mensaje en el que se pregunta si desea guardar o abrir el documento de Atom que contiene la fuente de distribución de datos.  
  
4.  Haga clic en **Guardar** para guardar el documento en el sistema de archivos o en **Abrir** para ver el contenido del documento antes de guardarlo. **De forma predeterminada, el documento se abre en un explorador.**  
  
5.  Busque la ubicación para guardar el documento.  
  
6.  Si lo desea, cambie el nombre del documento.  
  
    > [!NOTE]  
    >  De manera predeterminada, el nombre del documento es el del informe.  
  
7.  Compruebe que el tipo de documento es **Archivo ATOMSVC** y, a continuación, haga clic en **Guardar**.  
  
8.  Si lo desea, abra el archivo .atomsvc en un explorador o un editor de texto o XML.  
  
### <a name="to-view-an-atom-compliant-data-feed"></a>Ver una fuente de distribución de datos conforme a Atom  
  
1.  Si el documento de servicio de Atom no está abierto aún, búsquelo y ábralo en un explorador como Internet Explorer.  
  
2.  Copie la dirección URL de la fuente de distribución de datos que desea ver del documento de servicio de Atom al explorador.  
  
     El formato de la dirección URL es el siguiente:  
  
     `https://<server name>/ReportServer?%2f<ReportName>rs%3aCommand=Render&rs%3aFormat=ATOM&rc%3aDataFeed=<Identifier>`  
  
3.  Presione ENTRAR.  
  
     Aparece un mensaje en el que se pregunta si desea guardar o abrir el documento de Atom que contiene la fuente de distribución de datos.  
  
4.  Haga clic en **Guardar** para guardar el documento en el sistema de archivos, o en **Abrir** para ver la fuente de distribución de datos antes de guardar.  
  
5.  Busque la ubicación para guardar el documento.  
  
6.  Si lo desea, cambie el nombre del documento.  
  
    > [!NOTE]  
    >  De forma predeterminada, el nombre del documento es el del informe. Si el documento de servicio de Atom tiene varias fuentes, de forma predeterminada todas usas el mismo nombre, el del informe. Para diferenciarlas, cambie su nombre por otro descriptivo.  
  
7.  Compruebe que el tipo de documento es **Archivo ATOM** y, a continuación, haga clic en **Guardar**.  
  
8.  Si lo desea, abra el archivo .atom en un explorador o editor de texto o XML.  

## <a name="next-steps"></a>Pasos siguientes

[Exportación de informes](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
