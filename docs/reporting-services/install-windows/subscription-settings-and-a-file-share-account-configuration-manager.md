---
title: Configuración de la suscripción y una cuenta de recurso compartido de archivos (Administrador de configuración) | Microsoft Docs
description: En la página Configuración de suscripción del Administrador de configuración del servidor de informes, configure una cuenta de recurso compartido de archivos para servidores de informes en modo nativo y suscripciones de recurso compartido de archivos.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.subscriptionsettings.F1
ms.assetid: fefa7bdb-b5f2-4db7-b91c-b58869279f3c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5239174f08c9922a31204b4f476f923a7e931567
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236225"
---
# <a name="subscription-settings-and-a-file-share-account-report-server-configuration-manager"></a>Configuración de la suscripción y una cuenta de recurso compartido de archivos (Administrador de configuración del servidor de informes)
  En la página **Configuración de suscripción** del Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , configure una cuenta de recurso compartido de archivos para servidores de informes en modo nativo y suscripciones de recurso compartido de archivos. La cuenta de recurso compartido de archivos permite usar un único conjunto de credenciales en varias suscripciones que entregan informes a un recurso compartido de archivos. Cuando sea el momento de cambiar las credenciales, solamente deberá configurar el cambio en la cuenta de recurso compartido de archivos, con lo que no será necesario actualizar cada una de las suscripciones.  
  
 Con las suscripciones de recurso compartido de archivos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] existen dos flujos de trabajo:  
  
-   Como novedad de la versión [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , el administrador de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] puede configurar una única cuenta de recurso compartido de archivos, que se usa para una o para varias suscripciones. Configure **Specify a file share account**(Especificar una cuenta de recurso compartido de archivos) y, las páginas de configuración de las suscripciones, seleccione **Use file share account**(Usar la cuenta de recurso compartido de archivos).  
  
-   Configure las suscripciones individuales con credenciales específicas para el recurso compartido de archivos de destino.  
  
-   También puede combinar los dos enfoques y definir que algunas suscripciones de recurso compartido de archivos usen la cuenta central de recurso compartido de archivos mientras otras suscripciones usen credenciales específicas.  
  
 [!INCLUDE[applies](../../includes/applies-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modo nativo.  
  
## <a name="specify-a-file-share-account"></a>Specify a file share account  
 Si se selecciona esta opción, podrá indicar una cuenta que se usará para obtener acceso a recursos compartidos de archivos desde el servidor de informes. Si configura la cuenta de recurso compartido de archivos, todos los usuarios pueden seleccionar esa cuenta para las suscripciones que están configuradas para la entrega de informes a un recurso compartido de archivos. Si no se selecciona esta opción, la cuenta de recurso compartido de archivos **no** está disponible en ninguna suscripción.  
  
 Tenga en cuenta que debe comprobar que la cuenta que se configura como cuenta de recurso compartido de archivos tenga permisos de lectura y escritura en todos los recursos compartidos de archivos que los usuarios utilicen para la entrega del recurso compartido de archivos.  
  
 La siguiente imagen es lo que los usuarios ven en las suscripciones que están configuradas para la entrega del recurso compartido de archivos. La opción **Use file share account** (Usar la cuenta de recurso compartido de archivos) está deshabilitada si no se ha configurado una cuenta de recurso compartido de archivos.  
  
 ![cuenta de recurso compartido de archivos del administrador de configuración](../../reporting-services/install-windows/media/ssrs-fileshare-account.png "cuenta de recurso compartido de archivos del administrador de configuración")  
  
## <a name="prevent-privilege-escalation-or-elevated-privileges"></a>Evitar la escalación de privilegios o los privilegios elevados  
  
> [!IMPORTANT]
> La cuenta de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] controla la entrega de suscripciones e interactúa con la cuenta utilizada para las suscripciones del recurso compartido de archivos. Las características de seguridad de Windows restringen combinaciones de (1) la cuenta de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y (2) la cuenta usada para las cuentas de los recursos compartidos de archivos. Por ejemplo, si se usa una cuenta integrada de sistema operativo para la cuenta de recursos compartidos de archivos, la cuenta de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] debe ser otra cuenta de servicio que cuente con permisos de suplantación. Si se configuran de forma explícita una cuenta y una contraseña del recurso compartido de archivos, dicha cuenta requiere el derecho de iniciar sesión en el equipo que ejecuta el servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si la cuenta de recurso compartido de archivos no tiene los permisos necesarios, en las suscripciones que usan la cuenta de recurso compartido de archivos se producirá un error similar al siguiente:  
>   
>  `"Failure writing file {file} : An impersonation error occurred using the security context of the current user."`  
  
## <a name="powershell-sample-to-audit-use-of-the-file-share-account"></a>Ejemplo de PowerShell para auditar el uso de la cuenta de recurso compartido de archivos  
 Ejecute el siguiente script de Windows PowerShell para mostrar una lista con todas las suscripciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que están configuradas para usar la **cuenta de recurso compartido de archivos**. Actualice `SERVERNAME` a un valor apropiado para su servidor de informes.  
  
```  
# get all file share subscriptions using the default file share account  
$extensionNameMatch = "Report Server FileShare"  
$extensionSettingMatch = "DEFAULTCREDENTIALS"  
$valueMatch = "True"  
  
# filter for subscriptions that have a given extension setting  
filter script:extensionSettingFilter  
{  
    # subscription must match the extension name  
    if($_.DeliverySettings.Extension -eq $extensionNameMatch)  
    {  
        # locate the extension parameter of interest  
        ForEach($extensionParameter in $_.DeliverySettings.ParameterValues)  
        {  
            # if the setting has the desired value, return the subscription  
            if($extensionParameter.Name -eq $extensionSettingMatch -and $extensionParameter.Value -eq $valueMatch)  
            {  
                $_  
                break  
            }  
        }  
    }  
}  
  
$rs2010 = New-WebServiceProxy -Uri "https:// SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
  
Write-Host "----- File share subscriptions using the default file share account ----";  
Write-Host "-------------------------------------------------------------------------- ";  
$subscriptions | extensionSettingFilter | select report, owner, status, lastexecuted, description, subscriptionid | format-table -auto  
  
```  
  
 La salida del script es similar a la siguiente:  
  
 `----- File share subscriptions using the default file share account ----`  
  
 `-----------------------------------------------------------------------------------------------------`  
  
 `Report                    Owner          Status   LastExecuted         SubscriptionID`  
  
 `------------------------  -------------- -------- -------------------- ------------------------------------`  
  
 `Aworks_sales_by_territory DOMAIN\UserName Disabled 10/5/2014 1:04:04 PM e843bc2b-023e-45a3-ba23-22f9dc9a0934`  
  
## <a name="see-also"></a>Consulte también  
 [Entrega a recursos compartidos de archivos en Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Creación y administración de suscripciones para servidores de informes en modo nativo](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)
  
  
