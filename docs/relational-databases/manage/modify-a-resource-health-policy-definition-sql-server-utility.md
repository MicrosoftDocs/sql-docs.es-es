---
title: Modificación de una definición de la directiva de mantenimiento de recursos (utilidad de SQL Server) | Microsoft Docs
description: Obtenga información sobre cómo usar SQL Server Management Studio para modificar la definición de una directiva de mantenimiento de recursos, de tal forma que pueda evaluar mejor los datos de rendimiento de SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.SWB.UE.UTILITY.ADMINISTRATION.F1
ms.assetid: 27bec0b6-92e9-448e-8c70-fe36802cf128
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: cef58a5cd2587c4845fe2f4a0c090abb8970877a
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049098"
---
# <a name="modify-a-resource-health-policy-definition-sql-server-utility"></a>Modificar una definición de la directiva de mantenimiento de recursos (utilidad de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo modificar una definición de directiva de mantenimiento de recursos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Antes de modificar una directiva de uso de recursos en la Utilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe crear un punto de control de la utilidad (UCP). Para obtener más información, vea [Características y tareas de la utilidad de SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Las directivas de uso de recursos de la utilidad se pueden configurar para las aplicaciones de capa de datos y las instancias administradas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las directivas de uso de recursos se pueden definir globalmente para todas las aplicaciones de capa de datos y las instancias administradas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la Utilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , o se pueden definir individualmente para cada aplicación de capa de datos y para cada instancia administrada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la Utilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . También es posible implementar directivas globales y configurar aplicaciones de capa de datos e instancias administradas individuales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con sus propias definiciones de la directiva.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="modify-global-resource-utilization-policies-in-a-sql-server-utility"></a>Modifique las directivas de uso de recursos globales en una Utilidad SQL Server.  
  
1.  Conéctese al UCP en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  En el panel de navegación del explorador de la utilidad, haga clic en **Administración de la utilidad** para ver o modificar las directivas de supervisión globales y, a continuación, haga clic en la pestaña **Directiva** en el panel de contenido del explorador de la utilidad.  
  
3.  En el panel de contenido del explorador de la utilidad, seleccione **Establecer directivas de supervisión globales de capas de datos** o **Establecer directivas de supervisión globales de instancias administradas** haciendo clic en la flecha o la descripción de la directiva.  
  
4.  Utilice los controles en el lado derecho de las descripciones de la directiva para establecer los umbrales de infrautilización o sobreutilización.  
  
5.  Utilice los botones **Aplicar**, **Descartar** o **Restaurar valores predeterminados** según sea necesario. El cambio de la directiva puede tardar hasta 15 minutos en propagarse en el panel de la Utilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y en los detalles de la vista de lista.  
  
6.  Para actualizar los datos, haga clic con el botón derecho en el nodo **Administración de la utilidad** en el panel de navegación del explorador de la utilidad y seleccione **Actualizar**.  
  
#### <a name="modify-resource-health-policy-definitions-for-an-individual-data-tier-application-or-an-individual-managed-instance-of-sql-server-in-a-sql-server-utility"></a>Modifique las definiciones de la directiva de mantenimiento de recursos para una aplicación de nivel de datos o instancia administrada individual en una Utilidad de SQL Server  
  
1.  Conéctese al UCP en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  En el panel de navegación del explorador de la utilidad, haga clic en **Aplicaciones de capa de datos implementadas** o haga clic en **Instancias administradas** para ver o modificar las directivas de supervisión para una aplicación de capa de datos o instancia administrada individual.  
  
3.  En la vista de lista de panel de contenido del explorador de la utilidad, haga clic en la aplicación de capa de datos o nombre de instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuyas directivas le gustaría modificar y después haga clic en la pestaña **Detalles de la directiva** .  
  
4.  Seleccione la directiva para ver o modificar haciendo clic en la flecha o la descripción de la directiva. Las directivas globales están seleccionadas de forma predeterminada.  
  
5.  Seleccione el botón de radio **Invalidar la directiva global** para invalidar las directivas globales e implementar una definición de directiva individual para la aplicación de capa de datos especificada.  
  
6.  Use los controles en el lado derecho de la descripción de la directiva para establecer los umbrales de infrautilización o sobreutilización.  
  
7.  Utilice los botones **Aplicar**, **Descartar** o **Restaurar valores predeterminados** según sea necesario. El cambio de la directiva puede tardar hasta 15 minutos en propagarse en el panel de la Utilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y en los detalles de la vista de lista.  
  
8.  Para actualizar los datos, haga clic con el botón derecho en el nodo **Aplicaciones de capa de datos implementadas** en el panel de navegación del explorador de la utilidad y seleccione **Actualizar**.  
  
## <a name="see-also"></a>Consulte también  
 [Características y tareas de la utilidad de SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Ver los resultados de la directiva de mantenimiento de recursos &#40;Utilidad de SQL Server&#41;](../../relational-databases/manage/view-resource-health-policy-results-sql-server-utility.md)  
  
  
