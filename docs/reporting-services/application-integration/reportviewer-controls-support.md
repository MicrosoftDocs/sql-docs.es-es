---
title: Compatibilidad con versiones del control del Visor de informes
description: El control Microsoft Report Viewer es compatible con SQL Server Reporting Services y Power BI Report Server, que cumplen la directiva de ciclo de vida de soporte técnico moderna.
author: maggiesMSFT
ms.custom: seo-lt-2019
ms.author: maggies
ms.reviewer: jonhp
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: reference
ms.date: 12/01/2020
ms.openlocfilehash: fac7ebfc88432ef3bd8abde6c0e4e62115311520
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015645"
---
# <a name="support-for-report-viewer-current-branch-versions"></a>Compatibilidad con versiones de rama actual del Visor de informes

**_Se aplica a: Microsoft Report Viewer, versión 150.900.148 y posteriores_**

El **control Microsoft Report Viewer** es compatible con SQL Server Reporting Services y Power BI Report Server que siguen la [directiva de ciclo de vida de soporte técnico](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy) moderna de Microsoft. Esta información se aplica a las versiones de **ASP.NET** y **WinForms** distribuidas a través de [NuGet](https://www.nuget.org/). Todas las versiones publicadas están disponibles a través de [NuGet](https://www.nuget.org/). Las revisiones, características u otras actualizaciones se han puesto al día a la versión más reciente. Se deben aplicar las versiones más recientes para recibir los cambios. El Visor de informes sigue recibiendo **actualizaciones críticas y seguridad**, con la notificación de cambios en las directivas de soporte técnico al menos un año antes.

Para obtener un historial de versiones del control de Visor de informes, vea los vínculos siguientes:

- [Windows Forms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/)
- [ASP.NET Web Forms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/)

## <a name="application-server-and-report-server-combinations"></a>Combinaciones de servidores de aplicaciones y servidores de informes

Algunas características del control del Visor de informes se basan en los comportamientos predeterminados del sistema operativo. Por lo tanto, puede que requieran la ejecución de la misma versión para el cliente (el servidor de aplicaciones que ejecuta el control del Visor de informes) y el servidor (que ejecuta Reporting Services). Se admiten las siguientes combinaciones de servidor de aplicaciones y servidor de informes:

| Servidor de aplicaciones | Servidor de informes |
| :----------------- | :------ |
| Windows Server 2012 | Windows Server 2012 |
| Windows Server 2012 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 |
| Windows Server 2016 y versiones posteriores | Windows Server 2016 y versiones posteriores |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el control del Visor de informes, vea [Integración de Reporting Services con los controles del Visor de informes: Introducción](integrating-reporting-services-using-reportviewer-controls-get-started.md).
