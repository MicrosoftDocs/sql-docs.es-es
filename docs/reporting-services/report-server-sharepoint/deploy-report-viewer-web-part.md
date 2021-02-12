---
title: Implementar el elemento web Visor de informes de SQL Server Reporting Services en un sitio de SharePoint | Microsoft Docs
description: Para SQL Server Reporting Services, puede agregar manualmente el elemento web personalizado Visor de informes a un producto de SharePoint.
ms.date: 11/15/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4b8638e0c9a958e8f5dd6b6039540e8492555b03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074836"
---
# <a name="deploy-the-sql-server-reporting-services-report-viewer-web-part-on-a-sharepoint-site"></a>Implementar el elemento web Visor de informes de SQL Server Reporting Services en un sitio de SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

El elemento web Visor de informes es un elemento web personalizado que se puede usar para ver informes de SQL Server Reporting Services (modo nativo) en el sitio de SharePoint. Puede usar el elemento web para ver, explorar, imprimir y exportar informes en un servidor de informes. El elemento web Visor de informes está asociado a archivos de definición de informe (.rdl) que procesa un servidor de informes de SQL Server Reporting Services o Power BI Report Server. Este elemento web Visor de informes no puede usarse con informes de Power BI hospedados en Power BI Report Server.

Siga las instrucciones que se indican a continuación para implementar manualmente el paquete de solución que agrega el elemento web Visor de informes a un entorno de SharePoint Server 2013, SharePoint Server 2016 o SharePoint Server 2019. La implementación de la solución es un paso necesario para configurar el elemento web.

**El elemento web Visor de informes es un paquete de solución independiente y no está asociado con el modo integrado de SharePoint para SQL Server Reporting Services.**

## <a name="requirements"></a>Requisitos

> [!IMPORTANT]
> A partir de la versión "15.X.X.X", puede instalar ReportViewerWebPart en paralelo a sus aplicaciones de servicio compartido de modo integrado de SharePoint de Reporting Services existentes.
> Con esta actualización de la solución .wsp se introdujeron nuevos archivos, y debe retirarse la solución anterior y volver a implementarse el archivo .wsp nuevo mediante los cmdlets Uninstall-SPSolution e Install-SPSolution cmdlets, respectivamente.
>

**Compatibilidad con versiones de SharePoint Server:**
* SharePoint Server 2019
* SharePoint Server 2016
* SharePoint Server 2013

**Compatibilidad con versiones de Reporting Services:**  
* SQL Server 2008 Reporting Services (modo nativo) y posteriores.
* Power BI Report Server

## <a name="download-the-report-viewer-web-part-solution-package"></a>Descargar el paquete de solución del elemento web Visor de informes

El elemento web Visor de informes está disponible en el Centro de descarga de Microsoft.

[Descargar el paquete de solución del elemento web Visor de informes](https://www.microsoft.com/download/details.aspx?id=55949)

## <a name="deploy-the-farm-solution"></a>Implementar la solución de granja de servidores

En esta sección se muestra cómo implementar el paquete de solución en la granja de servidores de SharePoint. Esta tarea solo es necesario hacerla una vez.

1. En una instancia de SharePoint Server, abra un shell de administración de SharePoint mediante la opción **Ejecutar como administrador**.

2. Ejecute [Add-SPSolution](/powershell/module/sharepoint-server/Add-SPSolution) para agregar la solución de granja de servidores.

    ```
    Add-SPSolution -LiteralPath "{path to file}\ReportViewerWebPart.wsp"
    ```

    El cmdlet devuelve el nombre de la solución, su identificador de solución y Deployed=False. En el paso siguiente, implementará la solución.

3. Ejecute el cmdlet [Install-SPSolution](/powershell/module/sharepoint-server/Install-SPSolution) para implementar la solución de granja de servidores.

    **SharePoint 2013**

    ```
    Install-SPSolution -Identity ReportViewerWebPart.wsp -CompatibilityLevel "14,15" -GACDeployment -WebApplication {URL to web application}
    ```

    **SharePoint Server 2016 y 2019**

    ```
    Install-SPSolution -Identity ReportViewerWebPart.wsp -GACDeployment -WebApplication {URL to web application}
    ```

## <a name="activate-feature"></a>Activar característica

1. En el sitio de SharePoint, seleccione el icono de **engranaje** de la parte superior izquierda y luego **Configuración del sitio**.

    ![Configuración del sitio desde el icono de engranaje.](media/sharepoint-site-settings.png)

    De forma predeterminada, se tiene acceso a las aplicaciones web de SharePoint a través del puerto 80. Esto significa que normalmente se puede acceder a un sitio de SharePoint si se escribe *https://<computer name>* para abrir la colección de sitios raíz.

3. En **Administración de la colección de sitios**, seleccione **Características de la colección de sitios**.

4. Desplácese hacia abajo en la página hasta que encuentre la característica **Elemento web Visor de informes**.

5. Seleccione **Activar**.

    ![Activar la característica Elemento web Visor de informes](media/web-part-activiate-feature.png)

6. Repita este procedimiento con las demás colecciones de sitios al abrir cada sitio y hacer clic en Acciones del sitio.

También puede usar PowerShell para habilitar esta característica en todos los sitios mediante el cmdlet [Enable-SPFeature](/powershell/module/sharepoint-server/Enable-SPFeature).

```
Get-SPWebApplication "<web application url>" | Get-SPSite -Limit ALL | 
        ForEach-Object {
            Write-Host "Enabling feature for $($_.URL)"
            Enable-SPFeature -identity "ReportViewerWebPart" -URL $_.URL -ErrorAction Continue
        }
```

## <a name="remove-the-solution"></a>Eliminación de la solución

Aunque Administración central de SharePoint permite retirar una solución, no es necesario retirar el archivo **ReportViewerWebPart.wsp**, a menos que esté solucionando un problema de instalación o implementación de una revisión.

1. En Administración central de SharePoint, en **Configuración del sistema**, seleccione **Administrar soluciones del conjunto de servidores**.

2. Seleccione **ReportViewerWebPart.wsp**.

3. Seleccione Retirar solución.

### <a name="remove-the-web-part-from-site-settings"></a>Quitar el elemento web desde Configuración del sitio

Al retirar la solución no se quita el elemento web Visor de informes de la lista de elementos web del sitio de SharePoint. Para quitar el elemento web Visor de informes, haga lo siguiente.

1. En el sitio de SharePoint, seleccione el icono de **engranaje** de la parte superior izquierda y luego **Configuración del sitio**.

    ![Configuración del sitio desde el icono de engranaje.](media/sharepoint-site-settings.png)

    De forma predeterminada, se tiene acceso a las aplicaciones web de SharePoint a través del puerto 80. Esto significa que normalmente se puede acceder a un sitio de SharePoint si se escribe *https://<computer name>* para abrir la colección de sitios raíz.

2. En **Galerías del diseñador web**, seleccione **elementos web**.

3. Seleccione el **icono de edición** situado junto a **ReportViewerNativeMode.dwp**. Puede que no aparezca en la primera página de resultados.

4. Seleccione **Eliminar elemento**.

    ![Editar y eliminar el elemento web de modo nativo Visor de informes](media/report-viewer-native-mode-edit-delete.png)

La eliminación del elemento web puede intentarse mediante PowerShell, aunque no hay ningún comando directo para ello. Para obtener un ejemplo de script, vea [How to delete web parts from the web part Gallery (Cómo eliminar elementos web desde la Galería de elementos web)](https://gallery.technet.microsoft.com/office/How-to-delete-Web-Parts-1132701f).

## <a name="supported-languages"></a>Idiomas compatibles

Los siguientes idiomas son compatibles con el elemento web:

* Inglés (en)
* Alemán (de)
* Español (sp)
* Francés (fr)
* Italiano (it)
* Japonés (ja)
* Coreano (ko)
* Portugués (pt)
* Ruso (ru)
* Chino (simplificado - zh-HANS y zh-CHS)
* Chino (tradicional - zh-HANS y zh-CHS)

## <a name="troubleshoot"></a>Solución de problemas

* Error al desinstalar SSRS si ha configurado el modo integrado de SharePoint:

    Install-SPRSService: [A] No se puede convertir Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService en [B]Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService. El tipo A se origina a partir de"Microsoft.ReportingServices.SharePoint.SharedService,Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" en el contexto "Default" en la ubicación "C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll". El tipo B se origina a partir de"Microsoft.ReportingServices.SharePoint.SharedService,Version=12.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" en el contexto "Default" en la ubicación "C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll".
    
    Solución:
    1. Eliminar el elemento web Visor de informes
    2. Desinstalar SSRS
    3. Volver a instalar el elemento web Visor de informes

* Error al intentar actualizar SharePointsi ha configurado el modo integrado de SharePoint:

    "No se puede cargar el archivo o ensamblado "Microsoft.ReportingServices.Alerting.ServiceContract, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" o una de sus dependencias. El sistema no encuentra el archivo especificado. 00000000-0000-0000-0000-000000000000
    
    Solución:
    1. Eliminar el elemento web Visor de informes
    2. Desinstalar SSRS
    3. Volver a instalar el elemento web Visor de informes

## <a name="next-steps"></a>Pasos siguientes

Después de implementar y activar el elemento web Visor de informes, puede agregarlo a una página de SharePoint. Para más información, vea [Agregar el elemento web Visor de informes a una página de SharePoint](add-report-viewer-web-part-to-page.md).

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).