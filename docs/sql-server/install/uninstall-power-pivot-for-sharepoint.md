---
title: Desinstalar Power Pivot para SharePoint | Microsoft Docs
description: En este artículo se describe cómo desinstalar PowerPivot para SharePoint en varios pasos.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 3941a2f0-0d0c-4d1a-8618-7a6a7751beac
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 375f351c1d80e957fba29f81b9d2afc423c222ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336097"
---
# <a name="uninstall-power-pivot-for-sharepoint"></a>Desinstalar Power Pivot para SharePoint
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Desinstalar una instalación de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] es una operación de varios pasos que incluye la preparación para desinstalar, quitar características y soluciones de la granja de servidores, y quitar archivos de programa y valores del Registro.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2013 | SharePoint 2010  
  
 **En este artículo:**  
  
-   [Requisitos previos](#prereq)  
  
-   [Paso 1: Lista de comprobación previa a la desinstalación](#bkmk_before)  
  
-   [Paso 2: quitar características y soluciones de SharePoint](#bkmk_remove)  
  
-   [Paso 3: ejecutar el programa de instalación de SQL Server para quitar programas del equipo local](#bkmk_uninstall)  
  
-   [Paso 4: desinstalar el complemento Power Pivot para SharePoint](#bkmk_addin)  
  
-   [Paso 5: comprobar la desinstalación](#verify)  
  
-   [Paso 6: lista de comprobación posterior a la desinstalación](#bkmk_post)  
  
##  <a name="prerequisites"></a><a name="prereq"></a> Requisitos previos  
  
-   Debe ser administrador de la granja de servidores de SharePoint o administrador de aplicaciones de servicio para desinstalar características y soluciones de la granja de servidores.  
  
-   Debe ser administrador de sistema de SQL Server y miembro del grupo local Administradores si va a desinstalar también el Motor de base de datos.  
  
-   Debe ser administrador del sistema de Analysis Services y miembro del grupo local Administradores para desinstalar Analysis Services y [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)].  
  
##  <a name="step-1-pre-uninstall-checklist"></a><a name="bkmk_before"></a> Paso 1: lista de comprobación previa a la desinstalación  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] se deshabilitará una vez que se quite de la granja de servidores el software que admite las consultas y el procesamiento de datos. Como un primer paso, debe eliminar de forma preferente los archivos y bibliotecas que dejarán de funcionar. Esto permite resolver cualquier duda o preocupación sobre la "ausencia de datos" antes de desinstalar el software.  
  
1.  Elimine todos los documentos, bibliotecas y libros [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] que estén asociados con la instalación de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint. Ni las bibliotecas ni los documentos funcionarán una vez se desinstale el software.  
  
    -   [Eliminar Galería de PowerPivot](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [Eliminar una biblioteca de fuente de distribución de datos Power Pivot](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
2.  Elimine los libros de Excel o los informes de Reporting Services de otras bibliotecas que contengan o hagan referencia a datos [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
3.  Elimine cualquier elemento web de un panel de PerformancePoint que haga referencia a datos [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
4.  Revise los permisos de SharePoint de los sitios y bibliotecas existentes para determinar si necesita cambiarlos. Algunos escenarios de acceso a datos [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , particularmente el acceso a datos secundarios a través de una cadena de conexión URL a los datos [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de otro libro, requieren permisos de lectura, que son superiores a los permisos de vista que se asignan normalmente a los usuarios de SharePoint que solo visitan un sitio. Si ya no necesita permisos de lectura, puede reducirlos del modo correspondiente.  
  
5.  Si lo desea, puede detener los servicios y esperar varios días antes de desinstalar el software. Este paso no es necesario para la desinstalación, pero le ofrece la posibilidad de reanudar el servicio temporalmente mientras resuelve los problemas de migración de datos o sustituciones tecnológicas que podría haber pasado por alto.  
  
##  <a name="step-2-remove-features-and-solutions-from-sharepoint"></a><a name="bkmk_remove"></a> Paso 2: quitar características y soluciones de SharePoint  
 Use la Herramienta de configuración de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para quitar servicios y aplicaciones [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de SharePoint.  
  
-   Debe ser administrador de granja, administrador de servidor de la instancia de Analysis Services y **db_owner** en la base de datos de configuración de la granja.  
  
-   Utilice la versión adecuada de la herramienta de configuración de la versión de SharePoint. No use la herramienta con instalaciones de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
-   Compruebe que se ejecuta el servicio de administración de SharePoint.  
  
1.  **Ejecutar la herramienta de configuración** : tenga en cuenta que las herramientas de configuración se muestran solo cuando [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] se instala en el servidor local. En el menú **Inicio** , seleccione **Todos los programas**, haga clic en [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], haga clic en **Herramientas de configuración** y, a continuación, haga clic en uno de los elementos siguientes:  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint 2013**  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Herramienta de configuración**  
  
2.  Seleccione **Quitar características, servicios, aplicaciones y soluciones** y haga clic en **Aceptar**.  
  
3.  También puede ampliar la ventana al tamaño máximo. Aparecerá una barra de botones en la parte inferior de la ventana con los comandos **Validar** **Ejecutar** y **Salir** .  
  
4.  Revise cada acción de la lista de tareas para entender lo que realiza cada una.  
  
     En **Quitar aplicaciones de servicio [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** , se le ofrece la opción de eliminar los datos de aplicación asociados a la aplicación de servicio. Los datos de aplicación son una base de datos de SQL Server que se creó con la aplicación de servicio con el fin de almacenar programaciones de actualización de datos, información de instancia de la base de datos, datos de uso y otros datos usados por [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint. No almacena los archivos de usuario, como libros [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . A menos que tenga una razón concreta para mantener los datos de aplicación (por ejemplo, si tiene directivas de retención de datos relacionadas con la actualización de datos o el acceso a datos) puede eliminar la base de datos de aplicación sin quitar los archivos creados o guardados por los usuarios de SharePoint.  
  
     Para eliminar la base de datos, seleccione **Quitar aplicaciones de servicio [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** y seleccione **Eliminar los datos de aplicación asociados con esta aplicación de servicio**.  
  
5.  También puede revisar información detallada en la pestaña **Salida** o en la pestaña **Script** .  
  
     La pestaña Salida es un resumen de las acciones que realizará la herramienta. Esta información se guarda en archivos de registro en:  
  
     `C:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\SPAddinConfiguration\Log`  
  
     La pestaña Script muestra los cmdlets de PowerShell o hace referencia a los archivos de script de PowerShell que la herramienta ejecutará.  
  
6.  Haga clic en **Validar** para comprobar si cada acción es válida. Si la opción **Validar** no está disponible, significa que todas las acciones son válidas para el sistema.  
  
7.  Haga clic en **Ejecutar** para realizar todas las acciones válidas para esta tarea. La opción **Ejecutar** solo está disponible si se supera la comprobación de validación. Al hacer clic en **Ejecutar**, aparece la advertencia siguiente, para recordarle que las acciones se procesan en modo por lotes: "Todos los parámetros de configuración que se indican como válidos en la herramienta se aplicarán a la granja de SharePoint. ¿Quiere continuar?".  
  
8.  Haga clic en **Sí** para continuar.  
  
 **Solucionar errores:**  
  
 En la herramienta de configuración, se muestra información de error en el panel Parámetros de cada acción. En el caso de problemas relacionados con la implementación o la retracción de soluciones, compruebe que se ha iniciado el servicio Administración de SharePoint. Este servicio ejecuta los trabajos de temporizador que desencadenan cambios de configuración en una granja. Si el servicio no se está ejecutando, la implementación o la retracción de soluciones producirá un error. Los errores persistentes indican que un trabajo existente de implementación o de retracción ya está en la cola y está bloqueando la acción posterior de la herramienta de configuración. Puede usar el comando de PowerShell siguiente para comprobar que el servicio está en ejecución.  
  
```  
Get-Service | where {$_.displayname -like "*sharepoint* administration*"}  
```  
  
 Para buscar y quitar un trabajo de implementación o de retracción que ya está en la cola, realice lo siguiente:  
  
1.  Para todos los demás errores, compruebe los registros ULS. Para obtener más información, vea [Configurar y ver archivos de registro de SharePoint y el registro de diagnósticos &#40;Power Pivot para SharePoint&#41;](/analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging).  
  
2.  Inicie el Shell de administración de SharePoint como administrador y ejecute el siguiente comando para ver los trabajos de la cola:  
  
    ```  
    Stsadm -o enumdeployments  
    ```  
  
3.  Revise las implementaciones existentes para la siguiente información: **Tipo** es Retracción o Implementación, **Archivo** es powerpivotwebapp.wsp o powerpivotfarm.wsp.  
  
4.  En las implementaciones o las retracciones relacionadas con soluciones de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)], copie el valor GUID para **JobId** y péguelo en el comando siguiente (use los comandos Marcar, Copiar y Pegar del menú Edición del shell para copiar el GUID):  
  
    ```  
    Stsadm -o canceldeployment -id "<GUID>"  
    ```  
  
5.  Intente de nuevo la tarea en la herramienta de configuración haciendo clic en **Validar** seguido de **Ejecutar**.  
  
 Alternativamente, puede usar PowerShell para quitar características y soluciones de la granja de servidores. Para obtener más información, vea [Referencia de PowerShell para Power Pivot para SharePoint](/analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint).  
  
##  <a name="step-3-run-sql-server-setup-to-remove-programs-from-the-local-computer"></a><a name="bkmk_uninstall"></a> Paso 3: ejecutar el programa de instalación de SQL Server para quitar programas del equipo local  
 Para eliminar los archivos de programa es necesario ejecutar el programa de instalación de SQL Server a fin de desinstalar el software. La desinstalación quita los archivos y las entradas del Registro creados por el programa de instalación. Puede utilizar la página Programas y características paginan para desinstalar el software. Una instalación de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] forma parte de una instalación de SQL Server.  
  
 Puede desinstalar parte de una instalación sin afectar a otras instancias de SQL Server (o características en la misma instancia) que ya estén instaladas. Por ejemplo, puede desinstalar [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint mientras deja instalados otros componentes, como [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o el motor de base de datos.  
  
1.  Seleccione **Microsoft SQL Server 2014 (64 bits)** en la lista de programas.  
  
2.  Haga clic en **Desinstalar o cambiar**.  
  
3.  Haga clic en **Quitar**. De este modo inicia el programa de instalación de SQL Server.  
  
     Desde el programa de instalación, puede seleccionar la instancia de **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** y después seleccionar **Analysis Services** e **Integración con SharePoint de Analysis Services** para quitar solo esa característica, dejando todo lo demás en su lugar.  
  
##  <a name="step-4-uninstall-the-power-pivot-for-sharepoint-add-in"></a><a name="bkmk_addin"></a> Paso 4: desinstalar el complemento Power Pivot para SharePoint  
 Si la implementación de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] tiene dos o más servidores y ha instalado el complemento [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , desinstale el complemento [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] de cada servidor donde se instaló para desinstalar completamente todos los archivos de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . Para obtener más información, vea [Instalar o desinstalar el complemento PowerPivot para SharePoint &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013).  
  
##  <a name="step-5-verify-uninstall"></a><a name="verify"></a> Paso 5: comprobar la desinstalación  
  
1.  En Administración central, en **Administrar servicios en el servidor**, conéctese al servidor del que desinstaló [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint.  
  
2.  -   Si desinstaló [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013, compruebe que **Servicio de sistema de SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** ya no aparece en la lista.  
  
    -   Si desinstaló [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2010, compruebe que **SQL Server Analysis Services** y **Servicio de sistema de SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** ya no aparecen en la lista.  
  
3.  Después de desinstalar el último servidor [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint de la granja de servidores, haga lo siguiente:  
  
    1.  En Administración de aplicaciones, en **Administrar aplicaciones de servicio**, compruebe que Aplicación de servicio [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ya no aparece en la lista.  
  
    2.  En Configuración del sistema, en **Administrar características del conjunto de servidores**, compruebe que la integración de características de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ya no aparece en la página. En **Administrar soluciones del conjunto de servidores**, compruebe que las soluciones de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ya no aparecen en la página.  
  
    3.  En Supervisión, en **Configurar registro de diagnósticos** y en **Configurar la recolección de datos de uso y estado**, compruebe que los eventos y las categorías de eventos de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ya no aparecen.  
  
    4.  En Configuración de aplicación general, compruebe que **Panel de administración de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** ya no aparece en la página.  
  
##  <a name="step-6-post-uninstall-checklist"></a><a name="bkmk_post"></a> Paso 6: lista de comprobación posterior a la desinstalación  
 Utilice la siguiente lista para quitar el software y los archivos que no se eliminaron durante la desinstalación.  
  
1.  Elimine todos los archivos de datos y subcarpetas de `C:\Program Files\Microsoft SQL Server\MSAS13.PowerPivot`y, a continuación, elimine la carpeta. Este paso también elimina los archivos almacenados previamente en memoria caché en el directorio DATA.  
  
2.  Elimine todos los documentos, bibliotecas y libros [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] si no lo ha hecho con anterioridad.  
  
    -   [Eliminar Galería de PowerPivot](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [Eliminar una biblioteca de fuente de distribución de datos Power Pivot](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
3.  En Servicio de almacenamiento seguro, elimine cualquier aplicación de destino que contenga las credenciales almacenadas usadas por [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint. Algunas de las entradas del Servicio de almacenamiento seguro se eliminan al desinstalar [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint, pero no todas. Las aplicaciones de destino creadas para la cuenta de actualización de datos desatendida de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , además de todas las aplicaciones de destino que haya creado para la actualización de datos seguirán existiendo y se deben eliminar manualmente.  
  
     Por el contrario, las aplicaciones de destino individuales que generó automáticamente el Servicio de sistema de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] se eliminan automáticamente cuando se desinstala [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
4.  En el Panel de control, haga clic en **Programas** y, a continuación, haga clic en **Desinstalar un programa** . Desinstale cualquier biblioteca de cliente de Analysis Services que ya no se use. Analysis Services ADOMD.NET y Objetos de administración de análisis de Microsoft SQL Server no se quitan al desinstalar [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint. El programa de instalación de SQL Server no desinstala automáticamente las bibliotecas porque podrían usarlas otros programas que utilizan datos de Analysis Services. Debe desinstalar estas bibliotecas cliente individualmente si ya no las necesita.  
  
     No desinstale el complemento SharePoint de SQL Server Reporting Services 2010 a menos que esté siguiendo instrucciones de solución de problemas o de instalación que le indiquen de manera concreta que debe hacerlo. Servicios de Access utiliza el complemento Reporting Services. Lo instala la herramienta de preparación de productos de SharePoint y debe permanecer en el sistema para admitir la funcionalidad que necesita SharePoint.  
  
     No desinstale el proveedor OLE DB de Analysis Services. SharePoint instala el proveedor OLE DB como un requisito previo de los libros de Excel que se conectan a las bases de datos de Analysis Services. [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] instala una versión más reciente, pero esta versión es compatible con las versiones anteriores, por lo que debe conservarse en el sistema para evitar problemas de conexión de datos más adelante.  
  
## <a name="see-also"></a>Consulte también  
 [Instalar o desinstalar el complemento PowerPivot para SharePoint &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)   
 [Herramientas de configuración de Power Pivot](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools)  
  
