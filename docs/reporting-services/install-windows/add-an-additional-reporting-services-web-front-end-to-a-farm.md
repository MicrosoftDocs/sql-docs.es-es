---
description: Agregar un front-end web adicional de Reporting Services a una granja de servidores
title: Agregar un front-end web adicional de Reporting Services a una granja de servidores | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: d7a11bda-ae26-49ac-b071-37d83cae5afe
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 50bee52344ee56d83e1dc86cf9d4f8208c4c8c68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061120"
---
# <a name="add-an-additional-reporting-services-web-front-end-to-a-farm"></a>Agregar un front-end web adicional de Reporting Services a una granja de servidores
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] El modo de SharePoint incluye los componentes necesarios para los servidores de aplicaciones y los servidores front-end web (WFE). Este tema se centra en instalar los componentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] necesarios para un servidor de WFE, incluidas las páginas de aplicación utilizadas por las características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] como, por ejemplo, suscripciones, alertas de datos y [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]. La instalación primaria de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] necesaria para WFE es la del complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para Productos de SharePoint 2016.  
  
## <a name="prerequisites"></a>Requisitos previos  
  
-   Debe ser administrador local para ejecutar el programa de instalación de SQL Server.  
  
-   El equipo debe estar unido a un dominio.  
  
-   Debe saber el nombre del servidor de bases de datos existente que hospeda la configuración de SharePoint y las bases de datos de contenido.  
  
-   El servidor de bases de datos debe estar configurado para permitir las conexiones de base de datos remota.  Si no lo está, no podrá combinar el nuevo servidor con la granja de servidores porque el nuevo servidor no podrá establecer una conexión a las bases de datos de configuración de SharePoint.  
  
-   El nuevo servidor deberá tener instalada la misma versión de SharePoint que están ejecutando los servidores actuales de la granja. Por ejemplo, si la granja de servidores ya tiene instalado SharePoint 2013 Service Pack 1 (SP1), deberá instalar también SP1 en el nuevo servidor para poder combinar la granja de servidores.  
  
## <a name="steps"></a>Pasos  
 En los pasos de este tema se supone que el administrador de una granja de servidores de SharePoint va a instalar y configurar el servidor. En el diagrama se muestra un entorno típico de tres niveles. Los elementos numerados en el diagrama se describen en la siguiente lista:  
  
-   (1) Varios servidores front-end web (WFE). Los servidores WFE requieren el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para SharePoint 2010. Los pasos siguientes agregan un segundo servidor de aplicaciones en este nivel.  
  
-   (2) Dos servidores de aplicaciones que ejecutan [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y los sitios web, por ejemplo, Administración central.  
  
-   (3) Dos servidores de base de datos de SQL Server.  
  
-   (4) Representa una solución de equilibrio de carga de red (NLB) de software o hardware  
  
 ![Agregar SSRS a un nuevo WFE de SharePoint](../../reporting-services/install-windows/media/rs-sharepointscale-wfe.gif "Agregar SSRS a un nuevo WFE de SharePoint")  
  
 En los siguientes pasos se supone que un administrador va a instalar y configurar el servidor.  
  
|Paso|Descripción y vínculo|  
|----------|--------------------------|  
|Agregar un servidor de SharePoint a una granja de servidores.|Es necesario instalar SharePoint para implementar otra aplicación de Reporting Services.<br/><br/>Para SharePoint 2013, vea [Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2013](/SharePoint/install/add-web-or-application-server-to-the-farm).<br/><br/>Para SharePoint 2016, vea [Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm).|  
|Instale el complemento SQL Server Reporting Services para productos de SharePoint 2016.|Hay varios métodos para instalar el complemento. En los siguientes pasos se usa el asistente para la instalación de SQL Server. Para obtener información detallada sobre cómo instalar el complemento, vea [Instalar o desinstalar el complemento Reporting Services para SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).<br /><br /> 1) Ejecute la instalación de SQL Server.<br /><br /> 2) En la página **Rol de instalación** , seleccione **Instalación de características de SQL Server**.<br /><br /> 3) En la página **Selección de características** , seleccione **Complemento de Reporting Services para productos de SharePoint**.<br /><br /> 4) Haga clic **Siguiente** en las páginas siguientes para completar las opciones de configuración.<br /><br/>Para obtener más información sobre cómo instalar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Instalación del primer servidor de informes en modo de SharePoint](install-the-first-report-server-in-sharepoint-mode.md).|  
|Comprobar que el nuevo servidor está operativo.|1) En Administración central de SharePoint, haga clic en **Administrar servidores en esta granja de servidores** en el grupo **Configuración del sistema** .<br /><br /> 2) Compruebe que el nuevo servidor está en la lista.|  
|Actualizar la solución de NLB.|Si procede, actualice el entorno NLB de hardware o software para incluir el nuevo servidor.|  

## <a name="next-steps"></a>Pasos siguientes

[Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2013](/SharePoint/install/add-web-or-application-server-to-the-farm)

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).