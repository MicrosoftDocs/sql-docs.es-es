---
title: Script rs.exe de ejemplo de Reporting Services para copiar contenido entre servidores de informes | Microsoft Docs
description: Aprenda a copiar elementos y configuraciones de contenido de un servidor de informes de SQL Server Reporting Services a otro mediante la utilidad rs.exe para ejecutar un script RSS de Reporting Services.
ms.date: 05/23/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
ms.assetid: d81bb03a-a89e-4fc1-a62b-886fb5338150
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6857459a67d8441d42035080f31896c75c996905
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077575"
---
# <a name="sample-reporting-services-rsexe-script-to-copy-content-between-report-servers"></a>Script rs.exe de ejemplo de Reporting Services para copiar contenido entre servidores de informes

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2008r2-and-later](../../includes/ssrs-appliesto-2008r2-and-later.md)] [!INCLUDE [ssrs-appliesto-sharepoint-2013-2016](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-pbirs](../../includes/ssrs-appliesto-pbirs.md)]

Este artículo incluye y describe un script RSS de ejemplo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que copia elementos y configuraciones de contenido de un servidor de informes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a otro servidor de informes mediante la utilidad **RS.exe**. RS.exe se instala con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], en los modos nativo y de SharePoint. El script copia elementos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , como informes y suscripciones, de un servidor a otro. El script admite servidores de informes del modo de SharePoint y de modo nativo.  

##  <a name="to-download-the-ssrs_migrationrss-script"></a><a name="bkmk_download_script"></a> Para descargar el script ssrs_migration.rss  
 Descargue en una carpeta local el script desde el sitio de GitHub [Script de migración de Reporting Services RS.exe](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/reporting-services/ssrs-migration-rss). Para obtener más información, vea la sección [Cómo usar el script](#bkmk_how_to_use_the_script) de este artículo.  
  
##  <a name="supported-scenarios"></a><a name="bkmk_supported_scenarios"></a> Escenarios admitidos  
 El script admite servidores de informes del modo de SharePoint y de modo nativo. El script es compatible con las versiones [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores del servidor de informes y con Power BI Report Server.  
  
El script se puede usar para copiar contenido entre servidores de informes del mismo modo o de modos diferentes. Por ejemplo, puede ejecutar el script para copiar contenido de un servidor de informes en modo nativo de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] a un servidor de informes en modo de SharePoint de [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)] . Puede ejecutar el script desde cualquier servidor en el que esté instalado RS.exe. Por ejemplo, en la implementación siguiente, puede:  
  
-   Ejecutar RS.exe y el script **EN** el servidor A.  
  
-   Copiar contenido **DESDE** el servidor B  
  
-   **AL** servidor C  
  
|Nombre de servidor|Modo del servidor de informes|  
|-----------------|------------------------|  
|Servidor A|Nativa|  
|el servidor B|SharePoint|  
|servidor C|SharePoint|  
  
 Para más información sobre la utilidad RS.exe, vea [Utilidad RS.exe &#40;SSRS&#41;](../../reporting-services/tools/rs-exe-utility-ssrs.md).  
  
###  <a name="items-and-resources-the-script-migrates"></a><a name="bkmk_what_is_migrated"></a> Elementos y recursos que migra el script  
 El script no se sobrescribirá en elementos de contenido existentes que tengan el mismo nombre.  Si el script detecta en el servidor de destino elementos con el mismo nombre que en el servidor de origen, los elementos individuales producirán un mensaje de "error" y el script continuará. En la tabla siguiente se enumeran los tipos de contenido y los recursos que el script puede migrar en los modos del servidor de informes de destino.  
  
|Elemento|Migrado|SharePoint|Descripción|  
|----------|--------------|----------------|-----------------|  
|Contraseñas|**No**|**No**|Las contraseñas **NO** se migran. Después de migrar elementos de contenido, actualice la información de credenciales en el servidor de destino. Por ejemplo, orígenes de datos con credenciales almacenadas.|  
|Mis informes|**No**|**No**|La característica "Mis informes" del modo nativo se basa en inicios de sesión de usuarios individuales, por lo que el servicio de scripting no tiene acceso al contenido de las carpetas "Mis informes" para los usuarios que no hayan usado el parámetro **-u** para ejecutar el script RSS. Además, "Mis informes" no es una característica del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y los elementos de las carpetas no se pueden copiar a un entorno de SharePoint. Por tanto, el script no copia elementos de informe que se encuentran en las carpetas "Mis informes" de un servidor de informes en modo nativo de origen.<br /><br /> Para migrar el contenido de las carpetas "Mis informes" con este script, siga estos pasos:<br /><br /> 1.  Cree nuevas carpetas en el portal web. Opcionalmente, puede crear carpetas o subcarpetas para cada usuario.<br />2.  Inicie sesión como uno de los usuarios que tenga contenido de "Mis informes".<br />3.  En el portal web, seleccione la carpeta **Mis informes**.<br />4.  Haga clic en la vista **Detalles** de la carpeta.<br />5.  Seleccione cada informe que desea copiar.<br />6.  Seleccione **Mover** en la barra de herramientas del portal web.<br />7.  Seleccione la carpeta de destino deseada.<br />8.  Repita los pasos del 2 al 7 para cada usuario.<br />9. Ejecute el script.|  
|Historial|**No**|**No**||  
|Configuración del historial|Sí|Sí|La configuración del historial se migra, pero los detalles del historial NO se migran.|  
|Programaciones|Sí|Sí|Para migrar programaciones, es necesario que el Agente SQL Server se esté ejecutando en el servidor de destino. Si el Agente SQL Server no se está ejecutando en el servidor de destino, verá un mensaje de error parecido al siguiente:<br /><br /> `Migrating schedules: 1 items found. Migrating schedule: theMondaySchedule ... FAILURE:  The SQL Agent service isn't running. This operation requires the SQL Agent service. ---> Microsoft.ReportingServices.Diagnostics.Utilities.SchedulerNotResponding Exception: The SQL Agent service isn't running. This operation requires the SQL Agent service.`|  
|Roles y directivas del sistema|Sí|Sí|De forma predeterminada, el script no copiará el esquema de permisos personalizado entre los servidores. El comportamiento predeterminado es que los elementos se copien al servidor de destino con la marca "Heredar permisos principales" establecida en TRUE. Si desea que el script copie permisos para elementos individuales, use el modificador SECURITY.<br /><br /> Si los servidores de origen y de destino **no están en el mismo modo del servidor de informes**, por ejemplo de modo nativo a modo de SharePoint, y usa el modificador SECURITY, el script intentará asignar roles y grupos predeterminados según la comparación que se muestra en el artículo [Comparación de roles y tareas en Reporting Services con grupos y permisos de SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md). Los roles y grupos personalizados no se copian en el servidor de destino.<br /><br /> Cuando el script va a copiar datos entre servidores **que están en el mismo modo**, y usa el modificador SECURITY, el script creará nuevos roles (modo nativo) o grupos (modo de SharePoint) en el servidor de destino.<br /><br /> Si un rol ya existe en el servidor de destino, el script creará un mensaje de "Error" similar al siguiente y continuará con la migración de otros elementos. Cuando se completa el script, compruebe que los roles del servidor de destino están configurados de acuerdo con sus necesidades. los roles de migración: 8 elementos encontrados.<br /><br /> `Migrating role: Browser ... FAILURE: The role 'Browser' already exists and cannot be created. ---> Microsoft.ReportingServices.Diagnostics.Utilities.RoleAlreadyExistsException: The role 'Browser' already exists and cannot be created.`<br /><br /> Para obtener más información, consulte [Conceder acceso de usuario a un servidor de informes](../../reporting-services/security/grant-user-access-to-a-report-server.md).<br /><br /> **Nota** : si un usuario que existe en el servidor de origen no existe en el servidor de destino, el script no puede aplicar las asignaciones de roles en el servidor de destino, incluso aunque se use el modificador SECURITY.|  
|Origen de datos compartido|Sí|Sí|El script no sobrescribirá los elementos existentes en el servidor de destino. Si ya existe un elemento en el servidor de destino con el mismo nombre, verá un mensaje de error parecido a este:<br /><br /> `Migrating DataSource: /Data Sources/Aworks2012_oltp ... FAILURE:The item '/Data Sources/Aworks2012_oltp' already exists. ---> Microsoft.ReportingServices.Diagnostics.Utilities.ItemAlreadyExistsException: The item '/Data Source s/Aworks2012_oltp' already exists.`<br /><br /> Las credenciales **NO** se copian como parte del origen de datos. Después de migrar elementos de contenido, actualice la información de credenciales en el servidor de destino.|  
|Conjunto de datos compartidos|Sí|Sí|| 
|Carpeta|Sí|Sí|El script no sobrescribirá los elementos existentes en el servidor de destino. Si ya existe un elemento en el servidor de destino con el mismo nombre, verá un mensaje de error parecido a este:<br /><br /> `Migrating Folder: /Reports ... FAILURE: The item '/Reports' already exists. ---> Microsoft.ReportingServices.Diagnostics.Utilities.ItemAlreadyExistsException: The item '/Reports' already exists.`|  
|Informe|Sí|Sí|El script no sobrescribirá los elementos existentes en el servidor de destino. Si ya existe un elemento en el servidor de destino con el mismo nombre, verá un mensaje de error parecido a este:<br /><br /> `Migrating Report: /Reports/testThe item '/Reports/test' already exists. ---> Microsoft.ReportingServices.Diagnostics.Utilities.ItemAlreadyExistsException: The item '/Reports/test' already exists.`|  
|Parámetros|Sí|Sí||  
|Suscripciones|Sí|Sí||  
|Configuración del historial|Sí|Sí|La configuración del historial se migra, pero los detalles del historial NO se migran.|  
|Opciones de procesamiento|Sí|Sí||  
|Opciones de actualización de caché|Sí|Sí|La configuración dependiente se migra como parte de un elemento de catálogo. A continuación se muestra una salida de ejemplo del script a medida que migra un informe (.rdl) y configuraciones relacionadas, como opciones de actualización de caché:<br /><br /> -   Migrando parámetros para el informe TitleOnly.rdl: 0 elementos encontrados.<br />-   Migrando suscripciones para el informe TitleOnly.rdl: 1 elemento encontrado.<br />-   Migrando suscripción Guardar en \\\server\public\savedreports como TitleOnly... CORRECTA<br />-   Migrando configuración de historial para el informe TitleOnly.rdl... CORRECTA<br />-   Migrando opciones de procesamiento para el informe TitleOnly.rdl... No se ha encontrado ningún elemento.<br />-   Migrando opciones de actualización de la caché para el informe TitleOnly.rdl... CORRECTA<br />-   Migrando planes de actualización de la caché para el informe TitleOnly.rdl: 1 elemento encontrado.<br />-   Migrando plan de actualización de la caché titleonly_refresh735amM2F... CORRECTA|  
|Planes de actualización de caché|Sí|Sí||  
|Imágenes|Sí|Sí||  
|Elementos de informe|Sí|Sí||  
  
##  <a name="required-permissions"></a><a name="bkmk_required_permissions"></a> Permisos necesarios  
 Los permisos necesarios para leer o escribir elementos y recursos no son los mismos para todos los métodos empleados en el script. En la tabla siguiente se resumen los métodos usados para cada elemento o recurso y vínculos a contenido relacionado. Vaya al artículo individual para ver los permisos necesarios. Por ejemplo, el tema del método ListChildren indica los permisos necesarios para:  
  
-   **Permisos obligatorios para el modo nativo:** ReadProperties en el elemento  
  
-   **Permisos obligatorios para el modo de SharePoint:** ViewListItems  
  
|Elemento o recurso|Source|Destino|  
|----------------------|------------|------------|  
|Elementos de catálogo|<xref:ReportService2010.ReportingService2010.ListChildren%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetProperties%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetItemDataSources%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetItemReferences%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetDataSourceContents%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetItemLink%2A>|<xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A><br /><br /> <xref:ReportService2010.ReportingService2010.SetItemDataSources%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetItemReferences%2A><br /><br /> <xref:ReportService2010.ReportingService2010.CreateDataSource%2A><br /><br /> <xref:ReportService2010.ReportingService2010.CreateLinkedItem%2A><br /><br /> <xref:ReportService2010.ReportingService2010.CreateFolder%2A>|  
|Role|<xref:ReportService2010.ReportingService2010.ListRoles%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetRoleProperties%2A>|<xref:ReportService2010.ReportingService2010.CreateRole%2A>|  
|Directiva del sistema|<xref:ReportService2010.ReportingService2010.GetSystemPolicies%2A>|<xref:ReportService2010.ReportingService2010.SetSystemPolicies%2A>|  
|Programación|<xref:ReportService2010.ReportingService2010.ListSchedules%2A>|<xref:ReportService2010.ReportingService2010.CreateSchedule%2A>|  
|Suscripción|<xref:ReportService2010.ReportingService2010.ListSubscriptions%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetSubscriptionProperties%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetDataDrivenSubscriptionProperties%2A>|<xref:ReportService2010.ReportingService2010.CreateSubscription%2A><br /><br /> <xref:ReportService2010.ReportingService2010.CreateDataDrivenSubscription%2A>|  
|Plan de actualización de caché|<xref:ReportService2010.ReportingService2010.ListCacheRefreshPlans%2A><br /><br /> <xref:ReportService2010.ReportingService2010.GetCacheRefreshPlanProperties%2A>|<xref:ReportService2010.ReportingService2010.CreateCacheRefreshPlan%2A>|  
|Parámetros|<xref:ReportService2010.ReportingService2010.GetItemParameters%2A>|<xref:ReportService2010.ReportingService2010.SetItemParameters%2A>|  
|Opciones de ejecución|<xref:ReportService2010.ReportingService2010.GetExecutionOptions%2A>|<xref:ReportService2010.ReportingService2010.SetExecutionOptions%2A>|  
|Opciones de caché|<xref:ReportService2010.ReportingService2010.GetCacheOptions%2A>|<xref:ReportService2010.ReportingService2010.SetCacheOptions%2A>|  
|Configuración del historial|<xref:ReportService2010.ReportingService2010.GetItemHistoryOptions%2A>|<xref:ReportService2010.ReportingService2010.SetItemHistoryOptions%2A>|  
|Directiva de elemento|<xref:ReportService2010.ReportingService2010.GetPolicies%2A>|<xref:ReportService2010.ReportingService2010.SetPolicies%2A>|  
  
 Para obtener más información, vea [Comparación de roles y tareas en Reporting Services con grupos y permisos de SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md).  
  
##  <a name="how-to-use-the-script"></a><a name="bkmk_how_to_use_the_script"></a> Cómo usar el script  
  
1.  Descargue el archivo de script en una carpeta local, por ejemplo **c:\rss\ssrs_migration.rss**.  
  
2.  Abra un símbolo del sistema **con privilegios administrativos**.  
  
3.  Navegue hasta la carpeta que contiene el archivo ssrs_migration.rss.  
  
4.  Ejecute el comando con los parámetros adecuados para el escenario.  
  
 **Ejemplo básico, servidor de informes en modo nativo a servidor de informes en modo nativo:**  
  
 En el ejemplo siguiente se migra contenido del **Sourceserver** en modo nativo a **Targetserver** en modo nativo.  
  
 `rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://SourceServer/ReportServer -u Domain\User -p password -v ts="https://TargetServer/reportserver" -v tu="Domain\Userser" -v tp="password"`  
  
 **Notas de uso:**  
  
-   El script se ejecuta en dos pasos.  
  
     El primer paso es una auditoría para devolver una lista de elementos que se migrarán y el segundo paso es el proceso de migración.  
  
     Puede **cancelar el script después del paso** uno si solo desea ver la lista de migración posible o si desea modificar los parámetros. Las configuraciones dependientes no se muestran en el paso uno. Por ejemplo, las opciones de memoria caché de un informe no se muestran, pero sí se muestra el informe.  
  
    > [!TIP]  
    > Si desea auditar un solo servidor, use el mismo servidor como origen y destino, y cancele la operación después del paso 1.  
  
     Un buen uso de la información de auditoría del paso 1 consiste en revisar los roles existentes en el servidor en modo nativo de origen y de destino. A continuación se muestra un ejemplo de la lista de auditoría del paso uno. Observe que la lista incluye una sección de "roles" porque se ha usado el modificador -v security="True":  
  
    -   `Retrieve and report the list of items that will be migrated. You can cancel the script after step 1 if you do not want to start the actual migration.`  
  
         `Retrieving roles:`  
  
         `Role: Browser`  
  
         `Role: Content Manager`  
  
         `Role: Model Item Browser`  
  
         `Retrieve and report the list of items that will be migrated. You can cancel the script after step 1 if you do not want to start the actual migration.`  
  
         `Retrieving roles:`  
  
         `Role: Browser`  
  
         `Role: Content Manager`  
  
         `Role: CustomRole`  
  
         `Role: Model Item Browser`  
  
         `Role: My Reports`  
  
         `Role: Publisher`  
  
         `Role: Report Builder`  
  
         `Role: System Administrator`  
  
         `Role: System User`  
  
         `Retrieving system policies:`  
  
         `Retrieving system policies:`  
  
         `System policy: BUILTIN\Administrators`  
  
         `System policy: domain\user1`  
  
         `System policy: domain\ueser2`  
  
         `Retrieving schedules:`  
  
         `Schedule: theMondaySchedule`  
  
         `Retrieving catalog items. This may take a while.`  
  
         `Folder: /Data Sources`  
  
         `DataSource: /Data Sources/Aworks2012_oltp`  
  
         `Folder: /images`  
  
         `Resource: /images/Boba Fett.png`  
  
         `Resource: /images/R2-D2.png`  
  
         `Folder: /Reports`  
  
         `Report: /Reports/products`  
  
         `Report: /Reports/test`  
  
         `Report: /Reports/TitleOnly`  
  
-   SOURCE_URL y TARGET_URL deben ser direcciones URL de servidor de informes válidas que señalen el servidor de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de origen y de destino. En modo nativo, la dirección URL de un servidor de informes se parece a la siguiente:  
  
    -   `https://servername/reportserver`  
  
     En modo de SharePoint, la dirección URL se parece a la siguiente:  
  
    -   `https://servername/_vti_bin/reportserver`  
  
-   La estructura de carpetas virtuales que se muestra al usuario en SharePoint puede ser diferente que la base. Abra `https://servername/_vti_bin/reportserver` o `https://servername/sites/site_name/_vti_bin/reportserver` en un explorador para ver la estructura de carpetas no virtuales. Esto es útil para establecer la carpeta de origen y la carpeta de destino en algo distinto de "/" para un servidor en modo de SharePoint.  
  
-   Las contraseñas no se migran y se deben volver a especificar (por ejemplo, orígenes de datos con credenciales almacenadas).  
  
##  <a name="parameter-description"></a><a name="bkmk_parameter_description"></a> Descripción de los parámetros  
  
|Parámetro|Descripción|Obligatorio|  
|---------------|-----------------|--------------|  
|**-s** Dirección_URL_Origen|Dirección URL del servidor de informes de origen.|Sí|  
|**-u** Dominio\contraseña **-p** contraseña|Credenciales del servidor de origen.|OPCIONAL, se usan las credenciales predeterminadas si no se especifica|  
|**-v st**="SITIO"||OPCIONAL. Este parámetro solo se usa para los servidores de informes en modo de SharePoint.|  
|**- v f**="CARPETADEORIGEN"|Se establece en "/" para migrar todo o en algo similar a "/carpeta/subcarpeta" para una migración parcial. Se copiará todo el contenido de esta carpeta|OPCIONAL, el valor predeterminado es "/".|  
|**-v ts**="DIRECCIÓN_URL_DESTINO"|Dirección URL del servidor de RS de destino||  
|**-v tu**="dominio\nombreDeUsuario" **-v tp**="contraseña"|Credenciales del servidor de destino.|OPCIONAL, se usan las credenciales predeterminadas si no se especifica. **Nota**: el usuario se mostrará como "creador" de programaciones compartidas y como la cuenta "modificado por" para los elementos de informe en el servidor de destino.|  
|**-v tst**="SITIO"||OPCIONAL. Este parámetro solo se usa para los servidores de informes en modo de SharePoint.|  
|**-v tf** ="CARPETADEDESTINO"|Se establece en "/" para migrar en el nivel raíz. Se establece en "/carpeta/subcarpeta" para copiar en una carpeta que ya existe. Se copiará todo el contenido de "CARPETADEORIGEN" en "CARPETADEDESTINO".|OPCIONAL, el valor predeterminado es "/".|  
|**-v security**= "True/False"|Si se establece en "False", los elementos de catálogo de destino heredarán la configuración de seguridad según la configuración del sistema de destino. Este valor se recomienda para las migraciones entre diferentes tipos de servidor de informes, por ejemplo del modo nativo al modo de SharePoint. Si se establece en "True", el script intenta migrar la configuración de seguridad.|OPCIONAL, el valor predeterminado es "False".|  
  
##  <a name="more-examples"></a><a name="bkmk_more_examples"></a> Más ejemplos  
  
###  <a name="native-mode-report-server-to-native-mode-report-server"></a><a name="bkmk_native_2_native"></a> Servidor de informes en modo nativo a servidor de informes en modo nativo  
 En el ejemplo siguiente se migra contenido del **Sourceserver** en modo nativo a **Targetserver** en modo nativo.  
  
```  
rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://SourceServer/ReportServer -u Domain\User -p password -v ts="https://TargetServer/reportserver" -v tu="Domain\Userser" -v tp="password"  
```  
  
 El ejemplo siguiente agrega el modificador de seguridad:  
  
```  
rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://SourceServer/ReportServer -u Domain\User -p password -v ts="https://TargetServer/reportserver" -v tu="Domain\Userser" -v tp="password" -v security="True"  
```  
  
###  <a name="native-mode-to-sharepoint-mode---root-site"></a><a name="bkmk_native_2_sharepoint_root"></a> Modo nativo a modo de SharePoint: sitio raíz  
 En el ejemplo siguiente se migra contenido de un **SourceServer** en modo nativo al "sitio raíz" de un **TargetServer** en modo de SharePoint. Las carpetas "Reports" y "Data Sources" del servidor en modo nativo se migran como bibliotecas nuevas de la implementación de SharePoint.  
  
 ![ssrs_rss_migrate_root_site](../../reporting-services/tools/media/ssrs-rss-migrate-root-site.gif "ssrs_rss_migrate_root_site")  
  
```  
rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://SourceServer/ReportServer -u Domain\User -p Password -v ts="https://TargetServer/_vti_bin/ReportServer" -v tu="Domain\User" -v tp="Password"  
```  
  
###  <a name="native-mode-to-sharepoint-mode--bi-site-collection"></a><a name="bkmk_native_2_sharepoint_with_site"></a> Modo nativo a modo de SharePoint: colección de sitios "bi"  
 En el ejemplo siguiente se migra contenido de un servidor en modo nativo a un servidor de SharePoint que contiene una colección de sitios de "sites/bi" y una biblioteca de documentos compartida. El script crea carpetas en la biblioteca de documentos de destino. Por ejemplo, el script creará las carpetas "Reports" y "Data Sources" en la biblioteca de documentos de destino.  
  
```  
rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://SourceServer/ReportServer -u Domain\User -p Password -v ts="https://TargetServer/sites/bi/_vti_bin/reportserver" -v tst="sites/bi" -v tf="Shared Documents" -v tu="Domain\User" -v tp="Password"  
```  
  
###  <a name="sharepoint-mode-to-sharepoint-mode--bi-site-collection"></a><a name="bkmk_sharepoint_2_sharepoint"></a> Modo de SharePoint a modo de SharePoint: colección de sitios "bi"  
 En el siguiente ejemplo se migra contenido:  
  
-   De un servidor de SharePoint **SourceServer** que contiene una colección de sitios de "sites/bi" y una biblioteca de documentos compartida.  
  
-   A un servidor de SharePoint **TargetServer** que contiene una colección de sitios de "sites/bi" y una biblioteca de documentos compartida.  
  
```  
rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://SourceServer/_vti_bin/reportserver -v st="sites/bi" -v f="Shared Documents" -u Domain\User1 -p Password -v ts="https://TargetServer/sites/bi/_vti_bin/reportserver" -v tst="sites/bi" -v tf="Shared Documents" -v tu="Domain\User" -v tp="Password"  
```  
  
###  <a name="native-mode-to-native-mode---azure-virtual-machine"></a><a name="bkmk_native_to_native_Azure_vm"></a> Modo nativo a modo nativo: máquina virtual de Azure  
 En el siguiente ejemplo se migra contenido:  
  
-   De un servidor de informes en modo nativo **SourceServer**.  
  
-   A un servidor de informes en modo nativo **TargetServer** que se ejecuta en una máquina virtual de Azure. **TargetServer** no está unido al dominio de **SourceServer** y **User2** es un administrador de la máquina virtual de Azure **TargetServer**.  
  
```  
rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://SourceServer/ReportServer -u Domain\user1 -p Password -v ts="https://ssrsnativeazure.cloudapp.net/ReportServer" -v tu="user2" -v tp="Password2"  
```  
  
> [!TIP]  
> Para más información sobre cómo usar Windows PowerShell para crear servidores de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en máquinas virtuales de Azure, consulte [Uso de PowerShell para crear una máquina virtual de Azure con un servidor de informes en modo nativo](/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-report).  
  
##  <a name="sharepoint-mode--bi-site-collection-to-a-native-mode-server-on-an-azure-virtual-machine"></a><a name="bkmk_sharepoint_site_to_native_Azure_vm"></a> Modo de SharePoint: colección de sitios "bi" a servidor en modo nativo en una máquina virtual de Azure 
 En el siguiente ejemplo se migra contenido:  
  
-   De un servidor de informes en modo de SharePoint **SourceServer** que contiene una colección de sitios de "sites/bi" y una biblioteca de documentos compartida.  
  
-   A un servidor de informes en modo nativo **TargetServer** que se ejecuta en una máquina virtual de Azure. **TargetServer** no está unido al dominio de **SourceServer** y **User2** es un administrador de la máquina virtual de Azure **TargetServer**.  
  
```  
rs.exe -i ssrs_migration.rss -e Mgmt2010 -s https://uetesta02/_vti_bin/reportserver -u user1 -p Password -v ts="https://ssrsnativeazure.cloudapp.net/ReportServer" -v tu="user2" -v tp="Passowrd2"  
```  
  
##  <a name="verification"></a><a name="bkmk_verification"></a> Comprobación  
 En esta sección se resumen algunos pasos que se deben realizar en el servidor de destino para comprobar que el contenido y las directivas se han migrado correctamente.  
  
### <a name="schedules"></a>Programaciones  
 Para comprobar las programaciones en el servidor de destino:  
  
 **Modo nativo**  
  
1.  Abra el portal web en el servidor de destino.  
  
2.  Seleccione **Configuración del sitio** en el menú superior.  
  
3.  Seleccione **Programaciones** en el panel izquierdo.  
  
 **Modo SharePoint:**  
  
1.  Vaya a **Configuración del sitio**.  
  
2.  En el grupo **Reporting Services** , haga clic en **Administrar programaciones compartidas**.  
  
### <a name="roles-and-groups"></a>Funciones y grupos  
 **Modo nativo**  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y conéctese al servidor de informes en modo nativo.  
  
2.  En el **Explorador de objetos** , haga clic en **Seguridad**.  
  
3.  Haga clic en **Roles**.  
  
##  <a name="troubleshooting"></a><a name="bkmk_troubleshoot"></a> Solucionar problemas  
 Use la marca de seguimiento **-t** para recibir más información. Por ejemplo, si ejecuta el script y ve un mensaje similar al siguiente  
  
-   No se pudo conectar con el servidor: https://\<servername>/ReportServer/ReportService2010.asmx  
  
 Vuelva a ejecutar el script con la marca **-t** para ver un mensaje similar al siguiente:  
  
-   System.Exception: No se pudo conectar al servidor: https://\<servername>/ReportServer/ReportService2010.asmx ---> System.Net.WebException: **Error de la solicitud con estado HTTP 401: No autorizado**.   en System.Web.Services.Protocols.SoapHttpClientProtocol.ReadResponse (mensaje pasa clase SoapClientMessage, respuesta de WebResponse, responseStream de flujo, booleano asyncCall) en System.Web.Services.Protocols.SoapHttpClientProtocol.Invoke (String methodName, parámetros de objeto []) en Microsoft.SqlServer.ReportingServices2010.ReportingService2010.IsSSLRequired() en Microsoft.ReportingServices.ScriptHost.Management2010Endpoint.PingService (dirección url de cadena, cadena nombre de usuario, contraseña de cadena El dominio de cadena, el tiempo de espera de Int32) en Microsoft.ReportingServices.ScriptHost.ScriptHost.DetermineServerUrlSecurity()---fin del seguimiento de la pila de la excepción interna---  
  
## <a name="see-also"></a>Consulte también  
 [Utilidad RS.exe &#40;SSRS&#41;](../../reporting-services/tools/rs-exe-utility-ssrs.md)   
 [Comparación de roles y tareas en Reporting Services con grupos y permisos de SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md)  
  
