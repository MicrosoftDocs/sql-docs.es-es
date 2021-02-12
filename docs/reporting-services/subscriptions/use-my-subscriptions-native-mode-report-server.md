---
title: Usar Mis suscripciones (servidor de informes en modo nativo) | Microsoft Docs
description: Aprenda a usar la página Mis suscripciones del portal web de Reporting Services para ver, modificar, habilitar, deshabilitar o eliminar las suscripciones existentes.
ms.date: 07/01/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], My Subscriptions page
- My Subscriptions page [Reporting Services]
ms.assetid: e96623ba-677e-4748-8787-f32bed3b5c12
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bb653797aa0c89b16daeff71974a26781770a9ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100019806"
---
# <a name="use-my-subscriptions-native-mode-report-server"></a>Usar Mis suscripciones (servidor de informes en modo nativo)
El portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] incluye una página **Mis suscripciones** que organiza todas las suscripciones en un solo lugar. Puede usar *Mis suscripciones* para ver, modificar, habilitar, deshabilitar y eliminar suscripciones existentes. Sin embargo, no puede utilizar esta página para crear suscripciones.  Mis suscripciones solo muestra las suscripciones que haya creado. No enumera las suscripciones que sean propiedad de otros usuarios, aunque el usuario esté agregado como suscriptor a ellas, ni muestra suscripciones controladas por datos.
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** Modo nativo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
  
El campo de búsqueda filtrará dinámicamente la lista de suscripciones, ya que no puede buscar suscripciones por nombre, ni tampoco basándose en información del desencadenador, información de estado, etc. Para obtener más información, vea [Crear y administrar suscripciones para servidores de informes en modo nativo](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md).
  
## <a name="to-open-the-my-subscriptions-page"></a>Para abrir la página Mis suscripciones  
1. Abra el portal web de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] .
2. Haga clic en el botón de configuración ![engranaje_configuración_portal_ssrs](../../reporting-services/subscriptions/media/ssrs-portal-settings-gear.png) en la barra de herramientas.
3. Haga clic en **Mis suscripciones**.

Para más información, vea [Web portal (SSRS Native Mode)](../../reporting-services/web-portal-ssrs-native-mode.md).

## <a name="use-windows-powershell-to-list-mysubscriptions"></a>Usar Windows PowerShell para enumerar MySubscriptions  
 ![Contenido relacionado con PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenido relacionado con PowerShell")  
  
 El siguiente script de PowerShell devolverá la lista de suscripciones y propiedades de suscripción del usuario actual. Para obtener más información, vea [ReportingService2010.ListMySubscriptions (Método)](/dotnet/api/reportservice2010.reportingservice2010.listmysubscriptions).  
  
```  
#server -  all subscriptions of the current user at the given server or site  
$server="[server name]/reportserver"  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
  
$subscriptions=ListMySubscriptions(ItemPathOrSiteURL)  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status  
#uncomment the following to list all the subscription properties  
#$subscriptions

```  
  
## <a name="see-also"></a>Consulte también  
 [Data-Driven Subscriptions](../../reporting-services/subscriptions/data-driven-subscriptions.md)   
 [Suscripciones y entrega &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Crear y administrar suscripciones para servidores de informes en modo nativo](./create-and-manage-subscriptions-for-native-mode-report-servers.md)  
  
