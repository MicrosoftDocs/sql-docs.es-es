---
title: Notas de la versión de los controles del Visor de informes
description: Las notas de la versión de los controles del Visor de informes de WebForms y WinForms, relacionadas con Reporting Services.
ms.date: 11/30/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
ms.assetid: 112e0240-351d-46a9-98c7-2be09f26ac60
ms.reviewer: maggies
author: RhysSchmidtke
ms.author: rhys
ms.openlocfilehash: b6d1c768bb933036d3b23d37be20974515896a3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068170"
---
# <a name="release-notes-for-report-viewer-controls-for-webforms-and-winforms-of-ssrs"></a>Notas de la versión de los controles del Visor de informes para WebForms y WinForms de SSRS

Estas son las notas de la versión de los controles del Visor de informes de WebForms y WinForms, relacionadas con [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (SSRS).

Para las notas de la versión de SSRS, consulte [Notas de la versión de SQL Server Reporting Services (SSRS) 2017 y versiones posteriores](../release-notes-reporting-services.md).

## <a name="15014270"></a>150.1427.0
| Descripción del cambio | Detalles |
| :----------------- | :------ |
| Correcciones de errores | Se ha corregido un problema con los vínculos que no parecían visitados después de hacer clic en ellos. |
|           | Se ha corregido un problema de análisis de fecha y hora que afectaba a ciertas configuraciones regionales. |
| &nbsp; | &nbsp; |

## <a name="15014040"></a>150.1404.0
| Descripción del cambio | Detalles |
| :----------------- | :------ |
| Correcciones de errores | Se ha corregido un problema con el orden de tabulación de la barra de herramientas en WebForms. |
|           | Se ha mejorado la accesibilidad de representación en HTML para las tablas. |
| &nbsp; | &nbsp; |

## <a name="15014000"></a>150.1400.0
| Descripción del cambio | Detalles |
| :----------------- | :------ |
| Correcciones de errores | Se corrigió un problema por el cual el control del visor no se cargaba en el modo de diseño. |
| &nbsp; | &nbsp; |

## <a name="15013580"></a>150.1358.0
| Descripción del cambio | Detalles |
| :----------------- | :------ |
| Correcciones de errores | Se revirtió un cambio que quitaba los ensamblados Microsoft.ReportViewer.Design de las referencias del proyecto. |
|           | Como parte de otros cambios, se cambiaron dos ensamblados de la versión 15.0 a la versión 15.3. Esto se revirtió. |
| &nbsp; | &nbsp; |

## <a name="15013570"></a>150.1357.0
| Descripción del cambio | Detalles |
| :----------------- | :------ |
| Corrección de errores  | Vista previa de impresión adecuada para un monitor de valores ppp elevados |
|            | El cuadro de diálogo Imprimir aparecería fuera del espacio visible |
|            | Un gran número de parámetros provocó que las barras de desplazamiento de los parámetros y las listas desplegables no funcionaran correctamente |
|            | Se corrigió un problema con los parámetros de fecha y hora NULL |
|            | JQuery se actualizó a la versión 3.3.1 |
|            | Se corrigió la superposición con celdas de Tablix en representación en HTML |
|            | Se quitaron las referencias de proyecto en tiempo de diseño para eliminar los ensamblados de VS erróneos que se agregan a los proyectos |
|            | Corrección de accesibilidad de la barra de herramientas para narrar solo los elementos activos |
| &nbsp; | &nbsp; |

## <a name="150900148"></a>150.900.148

| Descripción del cambio | Detalles |
| :----------------- | :------ |
| Corrija un error que impide que los informes sin parámetros se carguen a través de **Server.LoadReportDefinition**. | &nbsp; |
| El control del Visor de informes WebForms. | Compatibilidad con la inserción en las páginas de derecha a izquierda (páginas que cambian el flujo de texto mediante la propiedad css *direction: rtl;* ).<br/><br/>Compatibilidad con la personalización de texto del cuadro de diálogo de impresión a través de la interfaz de localización *IReportViewerMessages5*.<br/><br/>Compatibilidad de accesibilidad mejorada.<br/><br/>&bull; &nbsp; &nbsp; [Paquete NuGet para el control del Visor de informes de WebForms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Webforms/150.900.148) |
| El control del Visor de informes WinForms. | Corrección para la impresión cuando se ejecuta una aplicación en modo con valores altos de PPP.<br/><br/>&bull; &nbsp; &nbsp; [Paquete NuGet para el control del Visor de informes de WinForms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/150.900.148) |
| &nbsp; | &nbsp; |

## <a name="next-steps"></a>Pasos siguientes

[Introducción](integrating-reporting-services-using-reportviewer-controls-get-started.md) a los controles del Visor de informes.

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
