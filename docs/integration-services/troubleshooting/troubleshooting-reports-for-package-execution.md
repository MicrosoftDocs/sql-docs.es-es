---
description: Solucionar problemas de informes para la ejecución de paquetes
title: Solucionar problemas de informes para la ejecución de paquetes | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 8fc476ac-bd69-434e-9636-70776e0b3b6c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c59111f0626aa9506c0cab17be903da21ff5245
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342215"
---
# <a name="troubleshooting-reports-for-package-execution"></a>Solucionar problemas de informes para la ejecución de paquetes

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], existen informes estándar en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ayudarlo a supervisar y solucionar problemas relacionados con los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que se han implementado en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . En concreto, dos de estos informes le ayudarán a ver el estado de ejecución de los paquetes e identificar la causa de los errores de ejecución.  
  
-   **Panel de Integration Services** : este informe proporciona información general sobre todas las ejecuciones de paquetes de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en las últimas 24 horas. El informe muestra información sobre el estado, tipo de operación, nombre del paquete, etc., para cada paquete.  
  
     La Hora de inicio, la Hora de finalización y la Duración se pueden interpretar de la manera siguiente:  
  
    -   Si el paquete se sigue ejecutando: Duración = hora actual - Hora de inicio  
  
    -   Si el paquete se ha completado: Duración = Hora de finalización - Hora de inicio  
  
     Para cada paquete que se ha ejecutado en el servidor, el panel le permite 'acercar' para buscar detalles específicos sobre los errores que podrían haberse producido durante la ejecución del paquete. Por ejemplo, haga clic en **Información general** para mostrar información general de alto nivel sobre el estado de las tareas durante la ejecución o haga clic en **Todos los mensajes** para mostrar los mensajes detallados que se capturaron como parte de la ejecución del paquete.  
  
     Puede filtrar la tabla que se muestra en cualquier página si hace clic en **Filtro** y selecciona los criterios deseados en el cuadro de diálogo **Configuración de filtro** . Los criterios de filtro disponibles dependen de los datos que se muestran. Puede cambiar el criterio de ordenación del informe haciendo clic en el icono de ordenación del cuadro de diálogo **Configuración de filtro** .  
  
-   **Informe “Actividad: todas las ejecuciones”**: en este informe, se muestra un resumen de todas las ejecuciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] realizadas en el servidor. El resumen muestra información para cada ejecución, como el estado, la hora de inicio y la hora de finalización. Cada entrada de resumen incluye vínculos a más información acerca de la ejecución, incluidos los mensajes generados durante la ejecución y los datos de rendimiento. Al igual que con el Panel de Integration Services, puede aplicar un filtro a la tabla para reducir la información mostrada.  
  
## <a name="related-content"></a>Contenido relacionado  
 [Informes para el servidor de Integration Services](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports)  
  
 [Herramientas para solucionar problemas de la ejecución de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)  
  
  
