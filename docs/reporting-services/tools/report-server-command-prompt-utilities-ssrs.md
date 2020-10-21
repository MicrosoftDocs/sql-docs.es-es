---
title: Utilidades del símbolo del sistema del servidor de informes | Microsoft Docs
description: Obtenga información sobre las utilidades de línea de comandos de SQL Server Reporting Services que se usan para administrar un servidor de informes.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- rsconfig utility
- components [Reporting Services], command line utilities
- rs utility
- command prompt utilities [Reporting Services]
- rskeymgmt utility
ms.assetid: 68f2f9f4-f894-40ff-a71c-f9756bf4b68c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e08d96d7a03cb1449c725672e698496f48c29ae2
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935512"
---
# <a name="report-server-command-prompt-utilities-ssrs"></a>Utilidades del símbolo del sistema del servidor de informes (SSRS)
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] incluye varias utilidades de la línea de comandos que se pueden utilizar para administrar un servidor de informes. Estas utilidades se instalan automáticamente cuando se instala el servidor de informes.  
  
|Nombre|Archivo de comandos|Modo de implementación admitido|Descripción|  
|----------|------------------|-------------------------------|-----------------|  
|Utilidad RSS|rs.exe|Modo nativo y modo de SharePoint. La compatibilidad con el modo de SharePoint se introdujo con la versión [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .|La [utilidad rs](../../reporting-services/tools/rs-exe-utility-ssrs.md) es un host de script que se puede utilizar para llevar a cabo operaciones de script. Use esta herramienta para ejecutar scripts de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] que copian datos entre distintas bases de datos del servidor de informes, publican informes, crean elementos en una base de datos del servidor de informes, etc. Para más información sobre cómo usar scripts para administrar un servidor, vea [Script para tareas administrativas y de implementación](../../reporting-services/tools/script-deployment-and-administrative-tasks.md).|  
|Cmdlets de Powershell||Solo SharePoint|Para consultar una lista de los cmdlets de PowerShell, vea [Cmdlets de PowerShell para el modo de SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).|  
|rsconfig, utilidad|rsconfig.exe|Solo nativo|La utilidad [rsconfig](../../reporting-services/tools/rsconfig-utility-ssrs.md) se utiliza para configurar y administrar una conexión del servidor de informes con la base de datos del servidor de informes. También puede utilizarla para especificar la cuenta de usuario que se va a utilizar para el procesamiento de informes desatendidos. Para más información, vea [Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md). Para más información sobre la configuración de la conexión, vea [Configurar una conexión a la base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).|  
|Utilidad Rskeymgmt|rskeymgmt.exe|Solo nativo|La utilidad [rskeymgmt](../../reporting-services/tools/rskeymgmt-utility-ssrs.md) es una herramienta de administración de claves de cifrado. Puede utilizarla para realizar copias de seguridad, aplicar y volver a crear claves simétricas. También puede utilizar esta herramienta para adjuntar una instancia del servidor de informes a una base de datos compartida del servidor de informes. Rskeymgmt puede utilizarse en operaciones de recuperación de base de datos. Para volver a utilizar una base de datos existente en una nueva instalación, aplique una copia de seguridad de la clave simétrica. Si las claves no se pueden recuperar, esta herramienta proporciona un método para eliminar el contenido cifrado que ya no utilice. Para más información sobre la administración de claves y el almacenamiento de información confidencial, vea [Almacenar datos cifrados del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md) y [Configurar y administrar claves de cifrado &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).|  
  
> [!NOTE]  
>  Si prefiere usar una herramienta que tenga una interfaz gráfica de usuario, puede usar el Administrador de configuración del servidor de informes en lugar de **rsconfig** y **rskeymgmt**.  
  
## <a name="see-also"></a>Consulte también  
 [Administrador de configuración del servidor de informes &#40;modo nativo&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Herramientas de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)   
 [Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)  
  
  
