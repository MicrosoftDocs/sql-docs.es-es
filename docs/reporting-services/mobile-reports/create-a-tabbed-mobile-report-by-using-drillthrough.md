---
title: Creación de un informe móvil en pestañas con obtención de detalles | Informes móviles de Reporting Services | Microsoft Docs
description: Aprenda a crear un informe para dispositivos móviles de Reporting Services que se vea y funcione como un informe con pestañas con obtención de detalles y parámetros.
ms.date: 03/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: c4d5d80d-370a-4a6d-8b76-698bd5ba5ba6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 95b153be1b4dc5a45effeb678ca0ccef83f06e6e
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907343"
---
# <a name="create-a-tabbed-mobile-report-by-using-drillthrough"></a>Creación de un informe móvil en pestañas con obtención de detalles
Sepa cómo crear un informe móvil de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] que se vea y funcione como un informe en pestañas con obtención de detalles y parámetros.

En este informe, por ejemplo, los medidores en la parte superior actúan como pestañas. Cuando haga clic en el medidor Transporte, los datos del resto del gráfico se filtran según los datos de transporte.

![Captura de pantalla que muestra un informe Finanzas - Transporte con el medidor de Transporte seleccionado.](../../reporting-services/mobile-reports/media/tabbed-mobile-report-web-viewer-transportation-complete.png)

En segundo plano, realmente esto es un conjunto de cinco informes independientes, cada uno con un parámetro diferente que filtra el informe para que coincida con el medidor seleccionado en la parte superior del informe. Primero cree los cinco informes y, después, en cada uno de ellos, convierta los otros cuatro medidores en obtenciones de detalles.

Estos son los pasos para este ejemplo.

## <a name="create-the-basic-report"></a>Crear el informe básico

1. Cree un informe denominado Ventas, con cinco medidores:

    * Sales
    * Transporte
    * Combustible
    * Storage
    * Gastos varios

   ![Captura de pantalla de un informe denominado Ventas con cinco medidores.](../../reporting-services/mobile-reports/media/01-sales-mobile-report-publisher.png)
    
2. Establezca **Resaltar** en **Activado** para el medidor Ventas, de modo que contraste con el resto del informe: en este caso, blanco sobre negro.

    ![Captura de pantalla del medidor Ventas con una flecha roja que señala el control deslizante Resaltar en la posición On.](../../reporting-services/mobile-reports/media/01a-sales-accent-mobile-report-publisher.png)
    
3. Guárdelo en un servidor de informes de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].

## <a name="make-copies-of-the-report"></a>Realizar copias del informe

1. Haga cuatro copias del informe Ventas y asígneles el nombre: 

    * Transporte
    * Combustible
    * Storage
    * Gastos varios

3. Guárdelas en el servidor de informes de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].

## <a name="set-the-gauge-as-a-drillthrough"></a>Establecer el medidor como obtención de detalles

En esta sección, se establece cada medidor (excepto el medidor Sales) como obtención de detalles para su informe correspondiente.

1. En el informe Ventas, seleccione el medidor Transporte.

    ![Captura de pantalla del informe Ventas con una flecha roja desde el medidor Transporte hasta la opción Obtener detalles del destino.](../../reporting-services/mobile-reports/media/02-sales-create-drillthrough-mobile-report-publisher.png)

2. Con la pestaña **Diseño** seleccionada, en el panel **Propiedades visuales** , seleccione **Obtener detalles del destino**.

3. Seleccione **Informe móvil**.

4. Vaya al informe de destino de la obtención de detalles (en este caso, "Finanzas - Transporte") y selecciónelo.

    ![Captura de pantalla del cuadro de diálogo Abrir desde servidor con la opción Finanzas - Transporte seleccionada.](../../reporting-services/mobile-reports/media/03-sales-select-dashboard-mobile-report-publisher.png)

5. En **Configurar informe de destino** , seleccione el parámetro para filtrar el informe y luego **Aplicar**.

   ![Captura de pantalla de la sección Configurar informe de destino que muestra los parámetros del informe Finanzas - Transporte.](../../reporting-services/mobile-reports/media/04-sales-apply-parameters-mobile-report-publisher.png)
   
6. Repita estos pasos para cada uno de los otros medidores del informe Ventas. 

## <a name="set-the-gauges-for-the-other-reports"></a>Establecer los medidores de los demás informes

1.  Abra el informe Transporte, establezca el medidor Ventas como obtención de detalles del informe Ventas, y los otros tres medidores como obtenciones de detalles de sus informes respectivos.

2. Todavía en el informe Transporte, establezca el valor **Resaltar** del medidor Transporte en **Activado** para que contraste con el resto del informe.

3. Repita estos pasos para los informes Combustible, Almacenamiento y Gastos varios. 

## <a name="view-the-report-in-the-web-portal"></a>Ver el informe en el portal web

1. Vaya al servidor de informes de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] y abra uno de los informes. 

2. Tenga en cuenta que cada uno de los medidores tiene un icono de obtención de detalles en la esquina superior derecha.

    ![Captura de pantalla del medidor Combustible.](../../reporting-services/mobile-reports/media/web-viewer-drillthrough-icon-mobile-report-builder.png)

3. Seleccione uno de los medidores para ir al informe filtrado por los datos de ese medidor.

   ![Captura de pantalla que muestra un informe Finanzas - Transporte con una flecha roja que señala el medidor Transporte en un cuadro rojo.](../../reporting-services/mobile-reports/media/06-mobile-report-web-viewer-transportation.png)

### <a name="see-also"></a>Consulte también
    
* [Incorporación de parámetros a un informe móvil](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md)
* [Incorporación de obtención de detalles de un informe móvil a otros informes móviles o direcciones URL](../../reporting-services/mobile-reports/add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls.md)




  

