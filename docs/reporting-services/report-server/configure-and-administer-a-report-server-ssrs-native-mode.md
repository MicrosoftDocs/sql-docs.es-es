---
title: Configuración y administración de un servidor de informes (modo nativo) | Microsoft Docs
description: Obtenga información sobre los métodos que puede usar para configurar Reporting Services y buscar artículos sobre cómo configurar componentes, características o funciones del servidor.
ms.date: 05/15/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, components
- deploying [Reporting Services], component options
- report servers [Reporting Services], component options
- configuration options [Reporting Services]
- administering Reporting Services
- components [Reporting Services], configuring
- configuring servers [Reporting Services]
ms.assetid: cfec012b-56f1-4346-9814-247acf22351c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3f9d31706fd0ba46c565d4c9049a981dd06fb54b
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933801"
---
# <a name="configure-and-administer-a-report-server-ssrs-native-mode"></a>Configurar y administrar un servidor de informes (modo nativo de SSRS)
  En este artículo se resumen los métodos que se pueden usar para configurar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. También se incluye una lista de temas en los que se explica cómo configurar determinados componentes, características o funciones del servidor. Para configurar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], puede:  
  
-   Use la herramienta Administrador de configuración del servidor de informes. En muchos de los temas de esta sección se proporciona información acerca de la configuración de determinadas características mediante esta herramienta.  
  
-   Use [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para personalizar las propiedades del servidor, habilitar Mis informes, habilitar los registros de seguimiento y establecer los valores predeterminados de todo el sitio. Para más información sobre la configuración del sitio, vea [Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md) para Management Studio. Tenga en cuenta que se puede crear y ejecutar un script que establece las propiedades del servidor mediante programación. Para más información, vea [Script para tareas administrativas y de implementación](../../reporting-services/tools/script-deployment-and-administrative-tasks.md) y [Propiedades del sistema del servidor de informes](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).  
  
-   Use el portal web para conceder permisos para tener acceso al servidor de informes. Los permisos se conceden a través de las asignaciones de roles que define para cada cuenta de grupo o usuario. Para obtener más información, vea [Roles y permisos &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md).  
  
-   También puede modificar archivos de configuración para cambiar la configuración de la aplicación. Para obtener más información acerca de cada archivo así como directrices para modificarlos, vea [Reporting Services Configuration Files](../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="in-this-section"></a>En esta sección  
 [Configurar las direcciones URL del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
 Describe cómo definir las direcciones URL que se usan para tener acceso al servidor de informes y al portal web.  
  
 [Configurar la cuenta de servicio del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)  
 Proporciona recomendaciones y pasos sobre cómo modificar la contraseña y la cuenta de servicio.  
  
 [Crear una base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
 Describe cómo crear una base de datos del servidor de informes, necesaria para almacenar objetos y metadatos de servidor.  
  
 [Configurar una conexión de base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 Describe cómo modificar la cadena de conexión que utiliza el servidor de informes para conectarse a la base de datos del servidor de informes.  
  
 [Entrega por correo electrónico en Reporting Services](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md)  
 Describe cómo configurar un servidor de informes para permitir la distribución de informes por correo electrónico.  
  
 [Configurar la cuenta de ejecución desatendida &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)  
 Describe cómo configurar una cuenta de usuario para procesar informes en modo desatendido.  
  
## <a name="see-also"></a>Consulte también  
 [Archivos de configuración de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Seguridad y protección de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)   
 [Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)  
  
  
