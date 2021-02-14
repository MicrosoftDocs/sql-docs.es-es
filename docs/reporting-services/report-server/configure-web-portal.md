---
title: Configurar el portal web | Microsoft Docs
description: Aprenda a configurar la aplicación del portal web que se usa para ver informes, administrar el contenido del servidor de informes y conceder acceso de usuario a un servidor de informes en modo nativo.
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- the web portal [Reporting Services], configuring
ms.assetid: e918986c-af15-48f6-8178-256aed829c6a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ebbce2708647e15b68ec497cfadd0af2c7c5b154
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074696"
---
# <a name="configure-the-web-portal"></a>Configurar el portal web

El portal web es una aplicación front-end web que se usa para ver informes, administrar el contenido del servidor de informes y conceder acceso de usuario a un servidor de informes en modo nativo. El portal web se instala con el servicio web del servidor de informes en la misma instancia del servidor de informes y, opcionalmente, se configura si se selecciona la opción **Instalar la configuración predeterminada del modo nativo** en el programa de instalación. También puede configurar el portal web después de haber realizado la instalación. En este tema se proporciona información sobre los siguientes escenarios de configuración del portal web:

## <a name="prerequisites"></a>Requisitos previos

Para usar el portal web, debe cumplir los requisitos previos siguientes:

- Tener un servidor de informes configurado mínimamente. Para más información sobre la configuración mínima de un servidor de informes, vea [Configurar un servidor de informes](../../reporting-services/report-server/configure-a-report-server-reporting-services-native-mode.md).

- El servidor de informes se debe ejecutar en modo nativo. No puede usar el portal web con un servidor de informes configurado para el modo integrado de SharePoint. En SQL Server 2012, no se puede cambiar un servidor de informes de un modo a otro. Si desea cambiar el tipo de servidor de informes que usa su entorno, debe instalar el modo deseado del servidor de informes y, a continuación, copiar o mover los elementos de informe al nuevo servidor de informes. Este proceso se conoce normalmente como "migración". Los pasos necesarios para la migración dependen del modo al que se realice la migración y la versión desde la que se migre. Para obtener más información, vea [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).

- También debe tener Internet Explorer 11 o posterior con scripting habilitado. Para obtener más información, vea [Compatibilidad del explorador de Reporting Services y Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).

## <a name="configure-the-web-portal-to-use-the-default-url"></a>Configurar el portal web para usar la dirección URL predeterminada

El portal web es una aplicación web a la que los usuarios acceden en un explorador web. Como mínimo, debe definir la dirección URL que se usa para abrir la aplicación en una ventana del explorador. La dirección URL está compuesta de un nombre de host, un puerto y un directorio virtual. Los valores predeterminados para esta dirección URL incluyen el nombre de host y los valores de puerto que definió para el servicio web del servidor de informes, más el nombre del directorio virtual **reports** . Si tiene una instancia con nombre, el directorio virtual es **reports_instance**, donde **instance** es el nombre de la instancia de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .

De forma predeterminada, la dirección URL del portal web consta de un único nombre de directorio virtual, más el puerto y el nombre de host que se han definido para el servicio web del servidor de informes que se ejecuta en la misma instancia. En la mayoría de los casos, el nombre de host es el nombre de red del equipo del servidor de informes, pero también puede ser una dirección IP o un encabezado de host que se resuelva como el equipo. Para configurar el portal web de modo que use la dirección URL predeterminada, use la página **Dirección URL del Portal web** de la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

> [!TIP]
> Si intenta acceder al portal web en un equipo remoto y recibe mensajes de error de conexión en el explorador, una causa frecuente es la configuración del firewall. Para obtener más información, vea [Configurar un firewall para el acceso al servidor de informes](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).

#### <a name="to-configure-the-default-the-web-portal-url-and-virtual-directory"></a>Para configurar la dirección URL del portal web y el directorio virtual predeterminados

1. Inicie la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y conéctese a la instancia del servidor de informes.

2. En la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], seleccione **Dirección URL del Portal web** para abrir la página que permite configurar la dirección URL.

3. Escriba un nombre único de directorio virtual para el portal web.

4. Haga clic en **Aplicar**.

5. Si usa [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)] o Windows Server 2008, podrían necesitarse pasos adicionales para poder usar el portal web. Para más información, vea [Configurar un servidor de informes en modo nativo para la administración local &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

## <a name="configure-the-web-portal-to-use-a-specific-report-server-url"></a>Configurar el portal web para que use una dirección URL específica del servidor de informes

De forma predeterminada, el portal web se conecta al servicio web del servidor de informes que se ejecuta en el mismo servicio del servidor de informes. El portal web usa la dirección URL del servicio web del servidor de informes para realizar la conexión. Si define varias direcciones URL para el servicio web del servidor de informes, el portal web usa la última de ellas. Pero es posible que en algunas implementaciones le interese que el portal web se conecte siempre al servicio web a través de una dirección URL estática. Podría interesarle esto si, por ejemplo, hubiese configurado el filtrado de paquetes en un puerto o en una dirección IP específica y deseara que todas las conexiones al servidor de informes pasaran a través de las reglas del filtro que ha definido.

Cuando se configuran direcciones URL en la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], el portal web detecta y usa automáticamente las direcciones URL nuevas y actualizadas del servidor de informes que se ejecuta en la misma instancia del servidor. Si su implementación requiere el uso de una única dirección URL estática para todas las solicitudes del servidor de informes, puede especificar dicha dirección en el archivo RSReportServer.config.

#### <a name="to-configure-a-static-report-server-url"></a>Para configurar una dirección URL estática del servidor de informes

1. Abra el archivo **RsReportServer.config** en un editor de texto. De forma predeterminada, se encuentra \Archivos de programa\Microsoft SQL Server\MSRS12.\<*instancename*>\Reporting Services\ReportServer.  

2. Busque **ReportServerURL**.

3. Reemplácelo por la dirección URL de la instancia del servidor de informes.

4. Guarde los cambios y cierre el archivo.

Para obtener más información sobre el archivo de configuración, vea [Modificar un archivo de configuración de Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md) y [El archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).

## <a name="customize-styles-or-application-title"></a>Personalizar estilos o el título de la aplicación

Puede crear un paquete de marca personalizado para modificar los colores usados para el portal web. Para más información, vea [Personalización de marca del portal web](../branding-the-web-portal.md).

#### <a name="to-modify-application-title"></a>Para modificar el título de la aplicación

1. Inicie la sesión usando una cuenta que tenga asignados los permisos **Administrador del sistema** en el servidor de informes.

2. Abra Internet Explorer.

3. Escriba la dirección URL del portal web. De forma predeterminada, es https://\<**your-server-name**>/reports, pero si ha instalado Reporting Services como una instancia con nombre, la dirección URL predeterminada será https://\<**your-server-name**>/reports\<**_instancename**>.

4. Seleccione **Configuración del sitio**.

5. En la pestaña **General** , en **Nombre**, reemplace **SQL Server Reporting Services** por otro nombre.

6. Seleccione **Aplicar**.

## <a name="next-steps"></a>Pasos siguientes

[Portal web](../../reporting-services/web-portal-ssrs-native-mode.md)  
[Compatibilidad del explorador de Reporting Services](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)
[Configurar una dirección URL](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
[Comprobar una instalación de Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)   
[Activar o desactivar las características de Reporting Services](../../reporting-services/report-server/turn-reporting-services-features-on-or-off.md)   
[Administrar un servidor de informes en modo nativo de Reporting Services](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[El archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Configuración de un servidor de informes en modo nativo para la administración local](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)

 ¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
