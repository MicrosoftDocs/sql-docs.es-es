---
description: Catálogo de SSIS
title: Catálogo de SSIS | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.iscreatecatalog.f1
- sql13.ssis.ssms.iscatalogprop.general.f1
- sql13.ssis.dbupgradewizard.f1
ms.assetid: 24bd987e-164a-48fd-b4f2-cbe16a3cd95e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fa8c9cb8d507acd5766e441cbd7387161a283bbf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339291"
---
# <a name="ssis-catalog"></a>Catálogo de SSIS

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El catálogo de **SSISDB** es el eje central cuando se trabaja con proyectos de [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] (SSIS) que ha implementado en el servidor [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)]. Por ejemplo, establece los parámetros del proyecto y del paquete, configura entornos para especificar los valores en tiempo de ejecución para los paquetes, ejecuta paquetes y soluciona los problemas de los mismos, y administra las operaciones del servidor de [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] .  
 
> [!NOTE]
> En este artículo se describe el catálogo de SSIS en general y el catálogo de SSIS ejecutado de forma local. El catálogo de SSIS también se puede crear en Azure SQL Database, y los paquetes SSIS se pueden implementar y ejecutar en Azure. Para obtener más información, consulte [Lift and shift SQL Server Integration Services workloads to the cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) (Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift).
>
> Aunque los paquetes SSIS también se pueden ejecutar en Linux, no se admite el catálogo de SSIS. Para obtener más información, consulte [Extracción, transformación y carga de datos en Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md) .
 
 Entre los objetos que se almacenan en el catálogo **SSISDB** se incluyen proyectos, paquetes, parámetros, entornos y el historial de operaciones.  
  
 Inspecciona objetos, valores y los datos operativos que se almacenan en el catálogo de **SSISDB** , consultando las vistas de la base de datos de **SSISDB** . Administra los objetos al llamar a los procedimientos almacenados en la base de datos de **SSISDB** o mediante la interfaz de usuario del catálogo de **SSISDB** . En muchos casos, la misma tarea se puede realizar en la interfaz de usuario o al llamar a un procedimiento almacenado.  
  
 Para mantener la base de datos de **SSISDB** , se recomienda aplicar las directivas corporativas estándar para administrar las bases de datos de usuario. Para obtener información acerca de cómo crear planes de mantenimiento, vea [Maintenance Plans](../../relational-databases/maintenance-plans/maintenance-plans.md).  
  
 El catálogo de **SSISDB** y la base de datos de **SSISDB** admiten Windows PowerShell. Para obtener más información acerca de cómo usar SQL Server con Windows PowerShell, vea [SQL Server PowerShell](../../powershell/sql-server-powershell.md). Para obtener ejemplos de cómo usar Windows PowerShell para completar tareas como implementar un proyecto, vea la entrada del blog [SSIS y PowerShell en SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015), en blogs.msdn.com.  
  
 Para más información sobre cómo ver los datos de las operaciones, consulte [Monitor de ejecución de paquetes y otras operaciones](../../integration-services/performance/monitor-running-packages-and-other-operations.md).  
  
 Tiene acceso al catálogo de **SSISDB** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] al conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el motor de base de datos y después expandir el nodo **Catálogos de Integration Services** en el Explorador de objetos. Tiene acceso a la base de datos de **SSISDB** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] al expandir el nodo Bases de datos en el Explorador de objetos.  
  
> [!NOTE]
> No puede cambiar el nombre de la base de datos de **SSISDB** .  
  
> [!NOTE]
> Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que la base de datos **SSISDB** se adjuntada se detiene o no responde, el proceso de ISServerExec.exe finaliza. Se escribe un mensaje en un registro de eventos de Windows.  
>   
>  Si los recursos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conmutan por error como parte de una conmutación por error de clúster, los paquetes en ejecución no se reinician. Puede usar los puntos de comprobación para reiniciar los paquetes. Para obtener más información, vea [Restart Packages by Using Checkpoints](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
## <a name="features-and-capabilities"></a>Características y funcionalidades  
  
-   [Identificadores de objeto de catálogo](../../integration-services/catalog/ssis-catalog.md#CatalogObjectIdentifiers)  
  
-   [Configuración de catálogo](../../integration-services/catalog/ssis-catalog.md#Configuration)  
  
-   [Permisos](../../integration-services/catalog/ssis-catalog.md#Permissions)  
  
-   [Carpetas](../../integration-services/catalog/ssis-catalog.md#Folders)  
  
-   [Proyectos y paquetes](../../integration-services/catalog/ssis-catalog.md#ProjectsAndPackages)  
  
-   [Parámetros](../../integration-services/catalog/ssis-catalog.md#Parameters)  
  
-   [Entornos de servidor, variables de servidor y referencias del entorno de servidor](../../integration-services/catalog/ssis-catalog.md#ServerEnvironments)  
  
-   [Ejecuciones y validaciones](../../integration-services/catalog/ssis-catalog.md#Executions)  

##  <a name="catalog-object-identifiers"></a><a name="CatalogObjectIdentifiers"></a> Identificadores de objeto de catálogo  
 Cuando cree un nuevo objeto en el catálogo, asígnele un nombre El nombre del objeto es un identificador. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] define las reglas para las que los caracteres se pueden usar en un identificador. Los nombres de los siguientes objetos deben seguir las reglas de identificador.  
  
-   Carpeta  
  
-   proyecto  
  
-   Entorno  
  
-   Parámetro  
  
-   Variable de entorno  
  
###  <a name="folder-project-environment"></a><a name="Folder"></a> Carpeta, proyecto, entorno  
 Tenga en cuenta las reglas siguientes al cambiar el nombre de una carpeta, un proyecto, o un entorno.  
  
-   Entre los caracteres no válidos, se incluyen los caracteres ASCII/Unicode del 1 al 31, comillas dobles ("), menor que (\<), greater than (>), barra vertical (|), retroceso (\b), NULL (\0) y tabulación (\t).  
  
-   El nombre no puede contener espacios delante ni detrás.  
  
-   \@ no se permite como primer carácter, pero los caracteres subsiguientes pueden utilizar \@.  
  
-   La longitud del nombre debe ser mayor que 0 y menor o igual que 128.  
  
###  <a name="parameter"></a><a name="Parameter"></a> Parameter  
 Tenga en cuenta las reglas siguientes cuando asigne un nombre a un parámetro.  
  
-   El primer carácter del nombre debe ser una letra, tal como se define en el Estándar Unicode 2.0, o un carácter de subrayado (_).  
  
-   Los caracteres siguientes pueden ser letras o números, tal como se define en el Estándar Unicode 2.0, o un carácter de subrayado (_).  
  
###  <a name="environment-variable"></a><a name="EnvironmentVariable"></a> Variable de entorno  
 Tenga en cuenta las reglas siguientes cuando asigne un nombre a una variable de entorno.  
  
-   Entre los caracteres no válidos, se incluyen los caracteres ASCII/Unicode del 1 al 31, comillas dobles ("), menor que (\<), greater than (>), barra vertical (|), retroceso (\b), NULL (\0) y tabulación (\t).  
  
-   El nombre no puede contener espacios delante ni detrás.  
  
-   \@ no se permite como primer carácter, pero los caracteres subsiguientes pueden utilizar \@.  
  
-   La longitud del nombre debe ser mayor que 0 y menor o igual que 128.  
  
-   El primer carácter del nombre debe ser una letra, tal como se define en el Estándar Unicode 2.0, o un carácter de subrayado (_).  
  
-   Los caracteres siguientes pueden ser letras o números, tal como se define en el Estándar Unicode 2.0, o un carácter de subrayado (_).  
  
##  <a name="catalog-configuration"></a><a name="Configuration"></a> Configuración de catálogo  
 Ajusta con precisión cómo se comporta el catálogo ajustando las propiedades del catálogo. Las propiedades del catálogo definen cómo se cifra la información confidencial y cómo se conservan las operaciones y los datos de versiones del proyecto. Para establecer las propiedades del catálogo, use el cuadro de diálogo **Propiedades del catálogo** o llame al procedimiento almacenado [catalog.configure_catalog &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md). Para ver las propiedades, use el cuadro de diálogo o la consulta [catalog.catalog_properties &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md). Puede acceder al cuadro de diálogo si hace clic con el botón derecho en **SSISDB** en el Explorador de objetos.  
  
###  <a name="operations-and-project-version-cleanup"></a><a name="Cleanup"></a> Limpieza de las operaciones y los datos de versiones del proyecto  
 Los datos de estado para muchas de las operaciones del catálogo se almacena en tablas de base de datos internas. Por ejemplo, el catálogo realiza el seguimiento del estado de las ejecuciones de paquetes y las implementaciones de proyecto. Para mantener el tamaño de los datos de operaciones, se usa **Tareas de mantenimiento de SSIS Server** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para quitar los datos antiguos. Este trabajo del agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se crea al instalar [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Puede actualizar o volver a implementar un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] implementándolo con el mismo nombre en la misma carpeta en el catálogo. De forma predeterminada, cada vez que vuelve a implementar un proyecto, el catálogo de **SSISDB** conserva la versión anterior del proyecto. Para mantener el tamaño de los datos de las operaciones, se utiliza el **trabajo de mantenimiento del Agente SQL Server** para quitar las versiones anteriores de proyectos.  
 
Para ejecutar el **trabajo de mantenimiento del servidor SSIS**, SSIS crea el inicio de sesión de SQL Server **##MS_SSISServerCleanupJobLogin##** . Este inicio de sesión es solo para uso interno de SSIS.
  
 Las siguientes propiedades del catálogo de **SSISDB** definen cómo se comporta este trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede ver y modificar las propiedades mediante el cuadro de diálogo **Propiedades del catálogo** o mediante [catalog.catalog_properties &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) y [catalog.configure_catalog &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **Borrar registros periódicamente**  
 El paso de trabajo de limpieza de operaciones se ejecuta cuando esta propiedad se establece en **True**.  
  
 **Período de retención (días)**  
 Define la antigüedad máxima de los datos permitidos para las operaciones (en días). Se quitan los datos más antiguos.  
  
 El valor mínimo es un día. El valor máximo solo está limitado por el valor máximo de los datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **int** data. Para más información sobre este tipo de datos, vea [int, bigint, smallint y tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).  
  
 **Quitar periódicamente versiones anteriores**  
 El paso de trabajo de limpieza de versiones del proyecto se ejecuta cuando esta propiedad se establece en **True**.  
  
 **Número máximo de versiones por proyecto**  
 Define cuántas versiones de un proyecto se almacenan en el catálogo. Se quitan las versiones anteriores de proyectos.  
  
###  <a name="encryption-algorithm"></a><a name="Encryption"></a> Algoritmo de cifrado  
 La propiedad **Algoritmo de cifrado** especifica el tipo de cifrado que se utiliza para cifrar los valores de los parámetros confidenciales. Puede elegir entre los siguientes tipos de cifrado.  
  
-   AES_256 (predeterminado)  
  
-   AES_192  
  
-   AES_128  
  
-   DESX  
  
-   TRIPLE_DES_3KEY  
  
-   TRIPLE_DES  
  
-   DES  
  
 Al implementar un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , el catálogo cifra automáticamente los datos del paquete y los valores confidenciales. El catálogo también descifra automáticamente los datos cuando lo recupera. El catálogo de SSISDB emplea el nivel de protección **ServerStorage** . Para más información, consulte [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 Cambiar el algoritmo de cifrado es una operación que lleva mucho tiempo. En primer lugar, el servidor tiene que utilizar el algoritmo especificado previamente para descifrar todos los valores de configuración. A continuación, el servidor tiene que utilizar el nuevo algoritmo para volver a cifrar los valores. Durante este tiempo, no puede haber otras operaciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el servidor. Así, para que las operaciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] continúen sin interrupción, el algoritmo de cifrado es un valor de solo lectura en el cuadro de diálogo de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 Para cambiar la configuración de la propiedad **Algoritmo de cifrado** , establezca la base de datos de **SSISDB** en modo de usuario único y, luego, llame al procedimiento almacenado catalog.configure_catalog. Use ENCRYPTION_ALGORITHM para el argumento *property_name* . Para más información sobre los valores de propiedad admitidos, vea [catalog.catalog_properties &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md). Para más información sobre el procedimiento almacenado, vea [catalog.configure_catalog &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 Para más información sobre el modo de usuario único, vea [Establecer una base de datos en modo de usuario único](../../relational-databases/databases/set-a-database-to-single-user-mode.md). Para más información sobre el cifrado y los algoritmos de cifrado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea los temas de la sección [Cifrado de SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md).  
  
 Para el cifrado se utiliza una clave maestra de base de datos. La clave se crea al crear el catálogo.  
  
 En la tabla siguiente se muestran los nombres de propiedad que aparecen en el cuadro de diálogo **Propiedades del catálogo** y las propiedades correspondientes de la vista de base de datos.  
  
|Nombre de la propiedad (cuadro de diálogo **Propiedades del catálogo** )|Nombre de la propiedad (vista de base de datos)|  
|---------------------------------------------------------|-------------------------------------|  
|Nombre del algoritmo de cifrado|ENCRYPTION_ALGORITHM|  
|Borrar registros periódicamente|OPERATION_CLEANUP_ENABLEDâ€‹|  
|Período de retención (días)|RETENTION_WINDOW|  
|Quitar periódicamente versiones anteriores|VERSION_CLEANUP_ENABLED|  
|Número máximo de versiones por proyecto|MAX_PROJECT_VERSIONS|  
|Nivel de registro predeterminado de todo el servidor|SERVER_LOGGING_LEVEL|  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Los proyectos, los entornos y los paquetes se encuentran en carpetas que son objetos protegibles. Puede conceder permisos a una carpeta, incluido el permiso de MANAGE_OBJECT_PERMISSIONS. MANAGE_OBJECT_PERMISSIONS le permite delegar la administración del contenido de la carpeta a un usuario sin tener que conceder la pertenencia del usuario al rol ssis_admin. También puede conceder permisos a los proyectos, entornos y operaciones. Las operaciones incluyen inicializar [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], implementar proyectos, crear e iniciar ejecuciones, validar proyectos y paquetes, y configurar el catálogo de **SSISDB** .  
  
 Para obtener más información sobre los roles de base de datos, vea [Roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 El catálogo de SSISDB usa un desencadenador DDL, ddl_cleanup_object_permissions, para exigir la integridad de la información sobre permisos para elementos de SSIS protegibles. El desencadenador se activa cuando se quita de la base de datos de SSISDB una entidad de seguridad de base de datos, como un usuario de base de datos, un rol de base de datos o un rol de aplicación de base de datos.  
  
 Si la entidad de seguridad ha concedido o denegado los permisos a otras entidades de seguridad, revoque los permisos proporcionados por el otorgante, antes de que la entidad de seguridad se pueda quitar. De lo contrario, se devuelve un mensaje de error cuando el sistema intenta quitar la entidad de seguridad. El desencadenador quita todos los registros de permisos donde la entidad de seguridad de base de datos es un receptor.  
  
 Se recomienda que el desencadenador no se deshabilite porque garantiza que no hay ningún registro de permiso huérfano después de que una entidad de seguridad de base de datos se quita de la base de datos de **SSISDB** .  
  
### <a name="managing-permissions"></a>Administrar permisos  
 Puede administrar permisos mediante la interfaz de usuario de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , los procedimientos almacenados y el espacio de nombres <xref:Microsoft.SqlServer.Management.IntegrationServices> .  
  
 Para administrar permisos con la interfaz de usuario de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], use los cuadros de diálogo siguientes: 
  
-   Para una carpeta, use la página **Permisos** del [Folder Properties Dialog Box](../../integration-services/catalog/folder-properties-dialog-box.md).  
  
-   Para un proyecto, use la página **Permisos** del [Project Properties Dialog Box](../../integration-services/catalog/project-properties-dialog-box.md).  

 Para administrar permisos mediante Transact-SQL, llame a [catalog.grant_permission &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-grant-permission-ssisdb-database.md), [catalog.deny_permission &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deny-permission-ssisdb-database.md) y [catalog.revoke_permission &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-revoke-permission-ssisdb-database.md). Para ver los permisos efectivos de la entidad de seguridad actual para todos los objetos, consulte [catalog.effective_object_permissions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md). Este tema proporciona descripciones de los diferentes tipos de permisos. Para ver los permisos asignados explícitamente al usuario, consulte [catalog.explicit_object_permissions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md).  
  
##  <a name="folders"></a><a name="Folders"></a> Carpetas  
 Una carpeta contiene uno o más proyectos y entornos en el catálogo de **SSISDB** . Puede usar la vista [catalog.folders &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-folders-ssisdb-database.md) para acceder a información sobre las carpetas del catálogo. Puede utilizar los siguientes procedimientos almacenados para administrar carpetas:  
  
-   [catalog.create_folder &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-folder-ssisdb-database.md)  
  
-   [catalog.delete_folder &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-folder-ssisdb-database.md)  
  
-   [catalog.rename_folder &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-rename-folder-ssisdb-database.md)  
  
-   [catalog.set_folder_description &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-folder-description-ssisdb-database.md)  
  
##  <a name="projects-and-packages"></a><a name="ProjectsAndPackages"></a> Proyectos y paquetes  
 Cada proyecto puede contener varios paquetes. Proyectos y paquetes pueden contener parámetros y referencias a los entornos. Puede tener acceso a los parámetros y referencias del entorno mediante el uso de [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md).  
  
 Puede realizar otras tareas de proyectos llamando a los siguientes procedimientos almacenados: 
  
-   [catalog.delete_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-project-ssisdb-database.md)  
  
-   [catalog.deploy_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md)  
  
-   [catalog.get_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md)  
  
-   [catalog.move_project &#40;&#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-move-project-ssisdb-database.md)  
  
-   [catalog.restore_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-restore-project-ssisdb-database.md)  
  
 Estas vistas proporcionan detalles sobre los paquetes, proyectos y versiones del proyecto.  
  
-   [catalog.projects &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
  
-   [catalog.packages &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
  
-   [catalog.object_versions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parámetros  
 Use parámetros para asignar valores a las propiedades del paquete en el momento de la ejecución del mismo. Para establecer el valor de un paquete o parámetro de proyecto y borrar el valor, llame a [catalog.set_object_parameter_value &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-object-parameter-value-ssisdb-database.md) y [catalog.clear_object_parameter_value &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-clear-object-parameter-value-ssisdb-database.md). Para establecer el valor de un parámetro para una instancia de ejecución, llame a [catalog.set_execution_parameter_value &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md). Puede recuperar los valores de parámetro predeterminados si llama a [catalog.get_parameter_values &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-get-parameter-values-ssisdb-database.md).  
  
 Estas vistas muestran los parámetros de todos los paquetes y proyectos, así como los valores de los parámetros que se usan para una instancia de ejecución.  
  
-   [catalog.object_parameters &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
  
-   [catalog.execution_parameter_values &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
  
##  <a name="server-environments-server-variables-and-server-environment-references"></a><a name="ServerEnvironments"></a> Entornos de servidor, variables de servidor y referencias del entorno de servidor  
 Los entornos de servidor contienen variables de servidor. Los valores variables se pueden usar cuando un paquete se ejecuta o se valida en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Los siguientes procedimientos almacenados permiten realizar muchas otras tareas de administración para entornos y variables.  
  
-   [catalog.create_environment &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-ssisdb-database.md)  
  
-   [catalog.delete_environment &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-ssisdb-database.md)  
  
-   [catalog.move_environment &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-move-environment-ssisdb-database.md)  
  
-   [catalog.rename_environment &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-rename-environment-ssisdb-database.md)  
  
-   [catalog.set_environment_property &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-property-ssisdb-database.md)  
  
-   [catalog.create_environment_variable &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-variable-ssisdb-database.md)  
  
-   [catalog.delete_environment_variable &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-variable-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_property &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-property-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_value &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-value-ssisdb-database.md)  
  
 Al llamar al procedimiento almacenado [catalog.set_environment_variable_protection &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-protection-ssisdb-database.md), puede establecer el bit de sensibilidad de una variable.  
  
 Para utilizar el valor de una variable de servidor, especifique la referencia entre el proyecto y el entorno del servidor. Puede usar los procedimientos almacenados siguientes para crear y eliminar referencias. También puede indicar si el entorno se puede encontrar en la misma carpeta que el proyecto o en una carpeta diferente.  
  
-   [catalog.create_environment_reference &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-reference-ssisdb-database.md)  
  
-   [catalog.delete_environment_reference &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-reference-ssisdb-database.md)  
  
-   [catalog.set_environment_reference_type &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-reference-type-ssisdb-database.md)  
  
 Para obtener más detalles sobre entornos y variables, consulte estas vistas.  
  
-   [catalog.environments &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
  
-   [catalog.environment_variables &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
  
-   [catalog.environment_references &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
  
##  <a name="executions-and-validations"></a><a name="Executions"></a> Ejecuciones y validaciones  
 Una ejecución es una instancia de una ejecución del paquete. Llame a [catalog.create_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) y [catalog.start_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) para crear e iniciar una ejecución. Para detener una ejecución o una validación de paquete o proyecto, llame a [catalog.stop_operation &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md).  
  
 Para que un paquete en ejecución se ponga en pausa y cree un archivo de volcado, llame al procedimiento almacenado de catalog.create_execution_dump. Un archivo de volcado proporciona información sobre la ejecución de un paquete que puede ayudarle a solucionar problemas de ejecución. Para obtener más información acerca de cómo generar y configurar archivos de volcado, vea [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
 Para obtener más información sobre ejecuciones, validaciones, mensajes que se registran durante las operaciones y la información contextual relacionada con errores, consulte estas vistas.  
  
-   [catalog.executions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
-   [catalog.operations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
  
-   [catalog.operation_messages &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
  
-   [catalog.extended_operation_info &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
  
-   [catalog.event_messages](../../integration-services/system-views/catalog-event-messages.md)  
  
-   [catalog.event_message_context](../../integration-services/system-views/catalog-event-message-context.md)  
  
 Puede validar proyectos y paquetes si llama a los procedimientos almacenados [catalog.validate_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-validate-project-ssisdb-database.md) y [catalog.validate_package &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-validate-package-ssisdb-database.md). La vista [catalog.validations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md) proporciona detalles sobre validaciones, como las referencias del entorno de servidor que se consideran en la validación, si se trata de una validación de dependencia o de una validación completa, y si para ejecutar el paquete se usa el tiempo de ejecución de 32 bits o el tiempo de ejecución de 64 bits.  

## <a name="create-the-ssis-catalog"></a>Crear el catálogo de SSIS
  Después de diseñar y probar paquetes en [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], puede implementar los proyectos que contienen los paquetes en un servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para poder implementar los proyectos en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , el servidor debe contener el catálogo de **SSISDB** . El programa de instalación de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] no crea automáticamente el catálogo; es necesario crear manualmente el catálogo usando las instrucciones siguientes.  
  
 Puede crear el catálogo de SSISDB en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. También crea el catálogo mediante programación con Windows PowerShell.  
  
### <a name="to-create-the-ssisdb-catalog-in-sql-server-management-studio"></a>Para crear un catálogo de SSISDB en SQL Server Management Studio  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Conéctese al motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  En el Explorador de objetos, expanda el nodo del servidor, haga clic con el botón derecho en el nodo **Catálogos de Integration Services** y, después, haga clic en **Crear catálogo**.  
  
4.  Haga clic en **Habilitar integración con CLR**.  
  
     El catálogo usar los procedimientos almacenados de CLR.  
  
5.  Haga clic en **Habilitar la ejecución automática del procedimiento almacenado de Integration Services al iniciar SQL Server** para permitir que el procedimiento almacenado [catalog.startup](../../integration-services/system-stored-procedures/catalog-startup.md) se ejecute cada vez que se reinicie la instancia del servidor de [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
     El procedimiento almacenado realiza el mantenimiento del estado de las operaciones del catálogo de SSISDB. Corrige el estado de los paquetes que estaban en ejecución si la instancia del servidor de [!INCLUDE[ssIS](../../includes/ssis-md.md)] se bloquea.  
  
6.  Escriba una contraseña y haga clic en **Aceptar**.  
  
     La contraseña protege la clave maestra de la base de datos que se usar para cifrar los datos del catálogo. Guarde la contraseña en un lugar seguro. Se recomienda que haga también una copia de seguridad de la clave maestra de la base de datos. Para más información, consulte [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
### <a name="to-create-the-ssisdb-catalog-programmatically"></a>Para crear el catálogo de SSISDB mediante programación  
  
1.  Ejecute el siguiente script de PowerShell:  
  
    ```  
    # Load the IntegrationServices Assembly  
    [Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Management.IntegrationServices")  
  
    # Store the IntegrationServices Assembly namespace to avoid typing it every time  
    $ISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"  
  
    Write-Host "Connecting to server ..."  
  
    # Create a connection to the server  
    $sqlConnectionString = "Data Source=localhost;Initial Catalog=master;Integrated Security=SSPI;"  
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString  
  
    # Create the Integration Services object  
    $integrationServices = New-Object $ISNamespace".IntegrationServices" $sqlConnection  
  
    # Provision a new SSIS Catalog  
    $catalog = New-Object $ISNamespace".Catalog" ($integrationServices, "SSISDB", "P@assword1")  
    $catalog.Create()  
  
    ```  
  
     Para obtener más ejemplos de cómo usar Windows PowerShell y el espacio de nombres <xref:Microsoft.SqlServer.Management.IntegrationServices>, vea la entrada del blog [SSIS and PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015) (SSIS y PowerShell en SQL Server 2012), en blogs.msdn.com. Para obtener información general sobre el espacio de nombres y ejemplos de código, vea la entrada del blog sobre el [Modelo de objetos administrados del catálogo de SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892)en blogs.msdn.com.  

## <a name="catalog-properties-dialog-box"></a>Propiedades del catálogo, cuadro de diálogo
  Utilice el cuadro de diálogo Propiedades del catálogo para configurar el catálogo de SSISDB. Las propiedades del catálogo definen cómo se cifra la información confidencial, cómo se conservan las operaciones y los datos de versiones del proyecto, y el tiempo de espera de las operaciones de validación. El catálogo de SSISDB es un punto centralizado de almacenamiento y administración para los proyectos, paquetes, parámetros y entornos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 También puede ver las propiedades del catálogo en la vista `catalog.catalog_properties` y establecer las propiedades mediante el procedimiento almacenado `catalog.configure_catalog`. Para más información, vea [catalog.catalog_properties &#40;base de datos SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) y [catalog.configure_catalog &#40;base de datos SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **¿Qué desea hacer?**  
  
-   [Abrir el cuadro de diálogo Propiedades del catálogo](#open_dialog)  
  
-   [Configurar las opciones](#options)  
  
###  <a name="open-the-catalog-properties-dialog-box"></a><a name="open_dialog"></a> Abrir el cuadro de diálogo Propiedades del catálogo  
  
1.  Abra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Conéctese al motor de base de datos de Microsoft SQL Server.  
  
3.  En el Explorador de objetos, expanda el nodo **Integration Services** , haga clic con el botón derecho en **SSISDB** y luego haga clic en **Propiedades**.  
  
###  <a name="configure-the-options"></a><a name="options"></a> Configurar las opciones  
  
#### <a name="options"></a>Opciones  
 En la tabla siguiente se describen algunas propiedades del cuadro de diálogo y las propiedades correspondientes de la vista `catalog.catalog_properties`.  
  
|Nombre de la propiedad (cuadro de diálogo Propiedades del catálogo)|Nombre de la propiedad (vista catalog.catalog_properties)|Descripción|  
|-----------------------------------------------------|------------------------------------------------------|-----------------|  
|Nombre del algoritmo de cifrado|ENCRYPTION_ALGORITHM|Especifica el tipo de cifrado que se utiliza para cifrar los valores de los parámetros confidenciales del catálogo. Los posibles valores son los siguientes:<br /><br /> DES<br /><br /> TRIPLE_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESPX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256 (predeterminado)|  
|Número máximo de versiones por proyecto|MAX_PROJECT_VERSIONS|Especifica cuántas versiones de un proyecto se almacenan en el catálogo. Las versiones anteriores de los proyectos que superen el máximo se quitarán cuando se ejecute el trabajo de limpieza de versiones del proyecto.|  
|Borrar registros periódicamente|OPERATION_CLEANUP_ENABLED|Establezca la propiedad en True para indicar que se ejecuta el trabajo limpieza de operaciones del Agente SQL Server. En caso contrario, establezca la propiedad en False.|  
|Período de retención (días)|RETENTION_WINDOW|Especifique la antigüedad máxima de los datos permitidos para las operaciones (en días). El trabajo limpieza de operaciones del Agente SQL quitará los datos anteriores al número de días especificado.|  

## <a name="back-up-restore-and-move-the-ssis-catalog"></a>Copia de seguridad, restauración y traslado del catálogo de SSIS
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] incluye la base de datos de SSISDB. En la base de datos de SSISDB, se consultan vistas para inspeccionar objetos, valores y los datos operativos que se almacenan en el catálogo de **SSISDB** , consultando las vistas de la base de datos de SSISDB. Este tema proporciona instrucciones para hacer una copia de seguridad de la base de datos y restaurarla.  
  
 El catálogo de **SSISDB** almacena los paquetes que se han implementado en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Para más información sobre el catálogo, vea [Catálogo de SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
###  <a name="to-back-up-the-ssis-database"></a><a name="backup"></a> Para realizar una copia de seguridad de la base de datos de SSIS  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y conéctese a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Para realizar una copia de seguridad de la clave maestra de la base de datos de SSISDB, use la instrucción Transact-SQL BACKUP MASTER KEY. La clave se almacena en un archivo que especifique. Use una contraseña utilizada para cifrar la clave maestra del archivo.  
  
     Para más información sobre la instrucción, vea [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-master-key-transact-sql.md).  
  
     En el ejemplo siguiente, la clave maestra se exporta al archivo `c:\temp directory\RCTestInstKey` . La contraseña de `LS2Setup!` se usa para cifrar la clave maestra.  
  
    ```  
    backup master key to file = 'c:\temp\RCTestInstKey'  
           encryption by password = 'LS2Setup!'  
  
    ```  
  
3.  Use el cuadro de diálogo **Copia de seguridad de la base de datos** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]para realizar una copia de seguridad de la base de datos de SSISDB. Para más información, vea: [Cómo: Realizar una copia de seguridad de una base de datos (SQL Server Management Studio)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
4.  Realice los procedimientos siguientes para generar el script CREATE LOGIN para ##MS_SSISServerCleanupJobLogin##. Para obtener más información, vea [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
    1.  En el Explorador de objetos de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo **Seguridad** y el nodo **Inicios de sesión** .  
  
    2.  Haga clic con el botón derecho en **##MS_SSISServerCleanupJobLogin##** y, después, haga clic en **Incluir inicio de sesión como** > **CREATE To** > **Nueva ventana del Editor de consultas**.  
  
5.  Si restaura la base de datos de SSISDB a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que nunca se ha creado el catálogo de SSISDB, genere el script CREATE PROCEDURE para sp_ssis_startup como se indica aquí. Para obtener más información, vea [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
    1.  En el Explorador de objetos, expanda el nodo **Bases de datos** y el nodo **maestro** > **Programación** > **Procedimientos almacenados** .  
  
    2.  Haga clic con el botón derecho en **dbo.sp_ssis_startup** y, después, haga clic en **Incluir procedimiento almacenado como** > **CREATE To** > **Nueva ventana del Editor de consultas**.  
  
6.  Confirme que se ha iniciado el Agente SQL Server  
  
7.  Si restaura la base de datos de SSISDB a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde el catálogo de SSISDB nunca se creó, genere un script para el trabajo de mantenimiento de SSIS Server como se indica a continuación. El script se crea en el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] automáticamente cuando se crea el catálogo de SSISDB. El trabajo sirve de ayuda para limpiar los registros de operación de la limpieza fuera de la ventana de retención y para eliminar versiones anteriores de proyectos.  
  
    1.  En el Explorador de objetos, expanda el nodo **Agente SQL Server** y luego expanda el nodo **Trabajos** .  
  
    2.  Haga clic con el botón derecho en el trabajo de mantenimiento del Servidor SSIS y, después, haga clic en **Incluir trabajo como** > **CREATE To** > **Nueva ventana del Editor de consultas**.  
  
### <a name="to-restore-the-ssis-database"></a>Para restaurar la base de datos de SSIS  
  
1.  Si va a restaurar la base de datos de SSISDB en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que nunca se ha creado el catálogo de SSISDB, habilite Common Language Runtime (CLR) mediante la ejecución del procedimiento almacenado `sp_configure`. Para más información, vea [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) y [clr enabled (opción)](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md).  
  
    ```  
    use master   
           sp_configure 'clr enabled', 1  
           reconfigure  
  
    ```  
  
2.  Si va a restaurar la base de datos de SSISDB a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que el catálogo de SSISDB nunca se creó, cree la clave asimétrica y el inicio de sesión de la clave asimétrica y conceda el permiso UNSAFE al inicio de sesión.  

    ```  
    Create Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey  
           FROM Executable File = 'C:\Program Files\Microsoft SQL Server\YourSQLServerDefaultCompatibilityLevel\DTS\Binn\Microsoft.SqlServer.IntegrationServices.Server.dll'  
    ```  

    Puede encontrar el valor de `YourSQLServerDefaultCompatibilityLevel` en una [lista de niveles de compatibilidad predeterminados de SQL Server](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments).
  
    [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] requieren permisos UNSAFE para el inicio de sesión porque este debe disponer de acceso adicional con el fin de restringir recursos como, por ejemplo la API Win32 de Microsoft. Para obtener más información sobre el permiso de código UNSAFE, vea [Creating an Assembly](../../relational-databases/clr-integration/assemblies/creating-an-assembly.md).  

    ```  
    Create Login ##MS_SQLEnableSystemAssemblyLoadingUser## FROM Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey   
    Grant Unsafe Assembly to ##MS_SQLEnableSystemAssemblyLoadingUser##    
    ```  
  
3.  Use el cuadro de diálogo **Restaurar base de datos** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]para restaurar la base de datos de SSISDB a partir de una copia de seguridad. Para obtener más información, vea los temas siguientes:  
  
    -   [Restaurar la base de datos &#40;página General&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
    -   [Restaurar base de datos &#40;página Archivos&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
    -   [Restaurar base de datos &#40;página Opciones&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
4.  Ejecute los scripts que ha creado en [Para realizar una copia de seguridad de la base de datos de SSIS](#backup) para ##MS_SSISServerCleanupJobLogin##, sp_ssis_startup y el trabajo de mantenimiento del servidor de SSIS. Confirme que se ha iniciado el Agente SQL Server.  
  
5.  Ejecute la instrucción siguiente con el fin de establecer el procedimiento de sp_ssis_startup para que se ejecute automáticamente. Para más información, vea [sp_procoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md).  
  
    ```  
    EXEC sp_procoption N'sp_ssis_startup','startup','on'  
    ```  
  
6.  Asigne el usuario de SSISDB ##MS_SSISServerCleanupJobUser## (base de datos de SSISDB) a ##MS_SSISServerCleanupJobLogin## (para hacerlo, use el cuadro de diálogo **Propiedades de inicio de sesión** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]).  
  
7.  Restaure la clave maestra mediante uno de los métodos siguientes. Para obtener más información acerca del cifrado, vea [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
    -   **Método 1**  
  
         Use este método si ya ha realizado una copia de seguridad de la clave maestra de la base de datos y dispone de la contraseña que se utilizó para cifrar la clave maestra.  
  
        ```  
               Restore master key from file = 'c:\temp\RCTestInstKey'  
               Decryption by password = 'LS2Setup!' -- 'Password used to encrypt the master key during SSISDB backup'  
               Encryption by password = 'LS3Setup!' -- 'New Password'  
               Force  
  
        ```  
  
        > [!NOTE]  
        >  Confirme que la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene permisos para leer el archivo de la clave de la copia de seguridad.  
  
        > [!NOTE]  
        >  Verá el mensaje de advertencia siguiente que se muestra en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] si la clave maestra de la base de datos no se ha cifrado aún con la clave maestra de servicio. Omita el mensaje de advertencia.  
        >   
        >  **No se puede descifrar la clave maestra actual. Se omitió el error porque se especificó la opción FORCE.**  
        >   
        >  El argumento FORCE especifica que el proceso de restauración debe continuar aunque la clave maestra de la base de datos actual no está abierta. Dado que la clave maestra de la base de datos no se ha abierto aún en la instancia donde se está restaurando la base de datos, verá este mensaje para el catálogo de SSISDB.  
  
    -   **Método 2**  
  
         Use este método si tiene la contraseña original que se usó para crear SSISDB.  
  
        ```  
        open master key decryption by password = 'LS1Setup!' --'Password used when creating SSISDB'  
               Alter Master Key Add encryption by Service Master Key  
        ```  
  
8.  Determine si el esquema del catálogo de SSISDB y los binarios de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (ensamblado de ISServerExec y SQLCLR) son compatibles (para hacerlo, ejecute [catalog.check_schema_version](../../integration-services/system-stored-procedures/catalog-check-schema-version.md)).  
  
9. Para confirmar que la base de datos de SSISDB se ha restaurado correctamente, realice operaciones sobre el catálogo de SSISDB como, por ejemplo, ejecutar paquetes que se hayan implementado en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para más información, consulte [Run Integration Services (SSIS) Packages](../../integration-services/packages/run-integration-services-ssis-packages.md) (Ejecución de paquetes de Integration Services [SSIS]).  
  
### <a name="to-move-the-ssis-database"></a>Para mover la base de datos de SSIS  
  
-   Siga las instrucciones para mover las bases de datos de usuarios. Para más información, consulte [Move User Databases](../../relational-databases/databases/move-user-databases.md).  
  
     Asegúrese de realizar una copia de seguridad de la clave maestra de la base de datos de SSISDB y de proteger el archivo de copia de seguridad. Para más información, vea [Para realizar una copia de seguridad de la base de datos de SSIS](#backup).  
  
     Asegúrese de que los objetos correspondientes de Integration Services (SSIS) se han creado en la nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde aún no se haya creado el catálogo de SSISDB.  

## <a name="upgrade-the-ssis-catalog-ssisdb"></a>Actualización del catálogo de SSIS (SSISDB)
  Ejecute el Asistente para actualización de SSISDB para actualizar la base de datos del catálogo de SSIS, SSISDB, cuando esta sea anterior a la versión actual de la instancia de SQL Server. Es posible que la base de datos sea anterior cuando se presenta alguna de las siguientes condiciones.  
  
-   Restauró la base de datos de una versión anterior de SQL Server.  
  
-   No quitó la base de datos de un grupo de disponibilidad AlwaysOn antes de actualizar la instancia de SQL Server. Esta condición evita la actualización automática de la base de datos. Para obtener más información, vea [Upgrading SSISDB in an availability group](#Upgrade).  
  
 El asistente solo puede actualizar la base de datos en una instancia de servidor local.  
  
### <a name="upgrade-the-ssis-catalog-ssisdb-by-running-the-ssisdb-upgrade-wizard"></a>Actualización del catálogo de SSIS (SSISDB) mediante la ejecución del Asistente para actualización de SSISDB  
  
1.  Copia de seguridad de la base de datos Catálogo de SSIS, SSISDB.  
  
2.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el servidor local y luego expanda **Catálogos de Integration Services**.  
  
3.  Haga clic con el botón derecho en **SSISDB** y, después, seleccione **Actualización de base de datos** para iniciar el Asistente para actualización de SSISDB. O bien, inicie el Asistente para actualización de SSISDB mediante la ejecución de `C:\Program Files\Microsoft SQL Server\140\DTS\Binn\ISDBUpgradeWizard.exe` con permisos elevados en el servidor local.
  
     ![Inicio del Asistente para actualización de SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-1.png)

4.  En la página **Seleccionar instancia** , seleccione una instancia de SQL Server en el servidor local.  
  
    > [!IMPORTANT]  
    >  El asistente solo puede actualizar la base de datos en una instancia de servidor local.  
  
     Active la casilla para indicar que ha realizado la copia de seguridad de la base de datos SSISDB antes de ejecutar al asistente.  
  
     ![Seleccionar el servidor en el Asistente para actualización de SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-2.png "Seleccionar el servidor en el Asistente para actualización de SSISDB")  
  
5.  Seleccione **Actualizar** para actualizar la base de datos Catálogo de SSIS.  
  
6.  En la página **Resultado** , examine los resultados.  
  
     ![Revisar los resultados en el Asistente para actualización de SSISDB](../../integration-services/service/media/ssisdb-upgrade-wizard-3.png "Revisar los resultados en el Asistente para actualización de SSISDB")  

## <a name="always-on-for-ssis-catalog-ssisdb"></a>AlwaysOn para el catálogo de SSIS (SSISDB)
  La característica Grupos de disponibilidad AlwaysOn es una solución de alta disponibilidad y de recuperación ante desastres que proporciona una alternativa empresarial a la creación de reflejo de la base de datos. Un grupo de disponibilidad admite un entorno de conmutación por error para un conjunto discreto de bases de datos de usuario, conocido como “bases de datos de disponibilidad”, que realizan la conmutación por error conjuntamente. Para obtener más información, vea [Grupos de disponibilidad AlwaysOn](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 Con el objetivo de ofrecer alta disponibilidad al catálogo de SSIS (SSISDB) y su contenido (proyectos, paquetes, registros de ejecución, etc.), puede agregar la base de datos SSISDB (al igual que cualquier otra base de datos de usuario) a un grupo de disponibilidad AlwaysOn. Cuando se produce una conmutación por error, uno de los nodos secundarios se convierte automáticamente en el nuevo nodo principal.  
 
 > [!IMPORTANT]
 > Cuando se produce una conmutación por error, los paquetes que se estuvieran ejecutando no se reinician o reanudan. 
 
 **Esta sección:**  
  
1.  [Requisitos previos](#prereq)  
  
2.  [Configuración de la compatibilidad con SSIS para AlwaysOn](#Firsttime)  
  
3.  [Actualización de SSISDB en un grupo de disponibilidad](#Upgrade)  
  
###  <a name="prerequisites"></a><a name="prereq"></a> Requisitos previos  
Lleve a cabo los siguientes pasos, que constituyen unos requisitos previos, antes de habilitar la compatibilidad de AlwaysOn para la base de datos SSISDB.  
  
1.  Configure un clúster de conmutación por error de Windows. Consulte la entrada de blog [Installing the Failover Cluster Feature and Tools for Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) (Instalación de las herramientas y la característica de clúster de conmutación por error para Windows Server 2012) a fin de obtener instrucciones. Instale la característica y las herramientas en todos los nodos del clúster.  
  
2.  Instale SQL Server 2016 con la característica Integration Services (SSIS) en cada nodo del clúster.  
  
3.  Habilite los Grupos de disponibilidad AlwaysOn para cada instancia de SQL Server. Consulte [Habilitar y deshabilitar grupos de disponibilidad AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md) para obtener más información.  
  
###  <a name="configure-ssis-support-for-always-on"></a><a name="Firsttime"></a> Configuración de la compatibilidad con SSIS para AlwaysOn  
  
-   [Paso 1: Creación del catálogo de Integration Services](#Step1)  
  
-   [Paso 2: Adición de SSISDB a un grupo de disponibilidad Always On](#Step2)  
  
-   [Paso 3: Habilitación de la compatibilidad con SSIS para Always On](#Step3)  
  
> [!IMPORTANT]  
> -   Debe realizar estos pasos en el **nodo principal** del grupo de disponibilidad.
> -   Debe habilitar la **compatibilidad con SSIS para AlwaysOn** *después* de agregar SSISDB a un grupo de disponibilidad AlwaysOn.  

> [!NOTE]
> Para más información sobre este procedimiento, vea el tutorial siguiente con capturas de pantalla adicionales de Marcos Freccia, MVP de Data Platform: [Adding SSISDB to AG for SQL Server 2016](https://marcosfreccia.com/2017/04/28/adding-ssisdb-to-ag-for-sql-server-2016/) (Adición de SSISDB a grupos de disponibilidad para SQL Server 2016).

####  <a name="step-1-create-integration-services-catalog"></a><a name="Step1"></a> Paso 1: Creación del catálogo de Integration Services  
  
1.  Inicie **SQL Server Management Studio** y conéctese a una instancia de SQL Server en el clúster que quiere establecer como el **nodo principal** del grupo de alta disponibilidad AlwaysOn para SSISDB.  
  
2.  En el Explorador de objetos, expanda el nodo del servidor, haga clic con el botón derecho en el nodo **Catálogos de Integration Services** y, después, haga clic en **Crear catálogo**.  
  
3.  Haga clic en **Habilitar integración con CLR**. El catálogo usar los procedimientos almacenados de CLR.  
  
4.  Haga clic en **Habilitar la ejecución automática del procedimiento almacenado de Integration Services al iniciar SQL Server** para permitir que el procedimiento almacenado [catalog.startup](../system-stored-procedures/catalog-startup.md) se ejecute cada vez que se reinicie la instancia del servidor de SSIS. El procedimiento almacenado realiza el mantenimiento del estado de las operaciones del catálogo de SSISDB. Corrige el estado de los paquetes que estaban en ejecución si y cuando la instancia del servidor de SSIS se bloquea.  
  
5.  Escriba una **contraseña** y haga clic en **Aceptar**. La contraseña protege la clave maestra de la base de datos que se usar para cifrar los datos del catálogo. Guarde la contraseña en un lugar seguro. Se recomienda que haga también una copia de seguridad de la clave maestra de la base de datos. Para más información, consulte [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
####  <a name="step-2-add-ssisdb-to-an-always-on-availability-group"></a><a name="Step2"></a> Paso 2: Adición de SSISDB a un grupo de disponibilidad Always On  
Puede agregar la base de datos SSISDB a un grupo de disponibilidad AlwaysOn prácticamente con el mismo procedimiento que emplearía para agregar cualquier otra base de datos de usuario a un grupo de disponibilidad. Consulte [Usar el Asistente para grupo de disponibilidad (SQL Server Management Studio)](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
Escriba la contraseña que especificó al crear el catálogo de SSIS en la página **Seleccionar bases de datos** del asistente **Nuevo grupo de disponibilidad**.

![Nuevo grupo de disponibilidad](../../integration-services/service/media/ssis-newavailabilitygroup.png "Asistente para nuevo grupo de disponibilidad")  
  
####  <a name="step-3-enable-ssis-support-for-always-on"></a><a name="Step3"></a> Paso 3: Habilitación de la compatibilidad con SSIS para Always On  
 Después de crear el catálogo de Integration Services, haga clic con el botón derecho en el nodo **Integration Service Catalogs** (Catálogos de Integration Services) y haga clic en **Enable AlwaysOn Support** (Habilitar compatibilidad con AlwaysOn). Verá el siguiente cuadro de diálogo: **Habilitar compatibilidad con AlwaysOn** . Si este elemento de menú está deshabilitado, confirme que tiene todos los requisitos previos instalados y haga clic en **Actualizar**.  
  
 ![Habilitar compatibilidad con AlwaysOn](../../integration-services/service/media/ssis-enablesupportforalwayson.png)  
  
> [!WARNING]  
>  No se admite la conmutación por error automática de la base de datos SSISDB hasta que habilite la compatibilidad con SSIS para AlwaysOn.  
  
 Las réplicas secundarias recién agregadas desde el grupo de disponibilidad AlwaysOn se mostrarán en la tabla. Haga clic en el botón **Conectar...** de cada réplica de la lista y escriba las credenciales de autenticación para conectarse a ella. La cuenta de usuario debe ser miembro del grupo sysadmin en cada réplica para habilitar la compatibilidad con SSIS para AlwaysOn. Después de conectarse correctamente a cada réplica, haga clic en **Aceptar** a fin de habilitar la compatibilidad con SSIS para AlwaysOn.  
 
Si la opción **Habilitar compatibilidad con AlwaysOn** del menú contextual parece estar desactivada después de haber completado los requisitos previos, pruebe lo siguiente:
1.  Actualice el menú contextual; para ello, haga clic en la opción **Actualizar**.
2.  Asegúrese de que se está conectando al nodo principal. Tiene que habilitar la compatibilidad con AlwaysOn en el nodo principal.
3.  Asegúrese de que la versión de SQL Server es 13.0 o posterior. SSIS solo admite AlwaysOn en SQL Server 2016 y versiones posteriores.

###  <a name="upgrading-ssisdb-in-an-availability-group"></a><a name="Upgrade"></a> Actualización de SSISDB en un grupo de disponibilidad  
 Si va a actualizar SQL Server desde una versión anterior y SSISDB se encuentra en un grupo de disponibilidad AlwaysOn, la regla “Comprobación de SSISDB en grupo de disponibilidad AlwaysOn” podría bloquear la actualización. Este bloqueo se produce porque la actualización se ejecuta en modo de usuario único, mientras que una base de datos de disponibilidad debe ser multiusuario. Por lo tanto, durante la actualización o la aplicación de una revisión, todas las bases de datos disponibilidad, incluida SSISDB, se desconectan y no se actualizan ni se les aplica la revisión. Para permitir que la actualización continúe, quite SSISDB primero del grupo de disponibilidad; después, actualice cada nodo o aplíquele una revisión y, por último, vuelva a agregar SSISDB al grupo de disponibilidad.  
  
 Si la regla “Comprobación de SSISDB en grupo de disponibilidad AlwaysOn” está causando un bloqueo, siga estos pasos para actualizar SQL Server.  
  
1.  Quite la base de datos SSISDB del grupo de disponibilidad. Para obtener más información, vea [Quitar una base de datos secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md) y [Quitar una base de datos principal de un grupo de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
2.  Haga clic en **Volver a ejecutar** en el Asistente para actualización. Se aplicará la regla “Comprobación de SSISDB en grupo de disponibilidad AlwaysOn”.  
  
3.  Haga clic en **Siguiente** para continuar con la actualización.  
  
4.  Después de actualizar todos los nodos, vuelva a agregar la base de datos SSISDB al grupo de disponibilidad AlwaysOn. Para obtener más información, vea [Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  
 Si no se encuentra con un bloqueo al actualizar SQL Server y SSISDB está en un grupo de disponibilidad AlwaysOn, actualice SSISDB por separado tras actualizar el motor de base de datos de SQL Server. Use el Asistente para actualización de SQL Server Integration Services a fin de actualizar SSISDB como se describe en el siguiente procedimiento.  
  
1.  Saque la base de datos SSISDB del grupo de disponibilidad o elimine el grupo de disponibilidad, si SSISDB es la única base de datos de este. Inicie **SQL Server Management Studio** en el **nodo principal** del grupo de disponibilidad para realizar esta tarea.  
  
2.  Quite la base de datos SSISDB de todos los **nodos de réplicas**.  
  
3.  Actualice la base de datos SSISDB en el **nodo principal**. En el **Explorador de objetos** de SQL Server Management Studio, expanda **Catálogos de Integration Services**, haga clic con el botón derecho en **SSISDB** y, después, seleccione **Actualización de base de datos**. Siga las instrucciones del **Asistente para actualización de SSISDB** a fin de actualizar la base de datos. Inicie el **Asistente para actualización de SSIDB** localmente en el **nodo primario**.  
  
4.  Siga las instrucciones de [Paso 2: Adición de SSISDB a un grupo de disponibilidad Always On](#Step2) para volver a agregar SSISDB a un grupo de disponibilidad.  
  
5.  Siga las instrucciones de [Paso 3: Habilitación de la compatibilidad con SSIS para Always On](#Step3).  


## <a name="ssisdb-catalog-and-delegation-in-double-hop-scenarios"></a>Catálogo de SSISDB y delegación en escenarios de salto doble

De forma predeterminada, la invocación remota de paquetes SSIS almacenados en el catálogo de SSISDB no admite la delegación de credenciales, lo que a veces se conoce como salto doble. 

Imagine un escenario en el que un usuario inicia sesión en la máquina cliente A e inicia SQL Server Management Studio (SSMS). Desde dentro de SSMS, el usuario se conecta a un servidor SQL Server hospedado en la máquina B, que tiene el catálogo de SSISDB. El paquete SSIS se almacena en este catálogo de SSISDB y el paquete, a su vez, se conecta a un servicio SQL Server que se ejecuta en la máquina C (el paquete también podría acceder a otros servicios). Cuando el usuario invoca la ejecución del paquete SSIS desde la máquina A, en primer lugar SSMS pasa correctamente las credenciales del usuario de la máquina A a la máquina B (donde el proceso de tiempo de ejecución de SSIS está ejecutando el paquete). El proceso de ejecución de SSIS en tiempo de ejecución (ISServerExec.exe) ahora es necesario para delegar las credenciales de usuario de la máquina B a la máquina C a fin de que la ejecución se complete correctamente. Sin embargo, la delegación de credenciales no está habilitada de forma predeterminada.

Un usuario puede habilitar la delegación de credenciales si concede el derecho *Confiar en este usuario para la delegación a cualquier servicio (solo Kerberos)* a la cuenta de servicio de SQL Server (en la máquina B), que inicia ISServerExec.exe como un proceso secundario. Este proceso se conoce como la configuración de la delegación no restringida o la delegación abierta para una cuenta de servicio de SQL Server. Antes de conceder este derecho, considere si cumple los requisitos de seguridad de su organización.

SSISDB no admite la delegación restringida. En un entorno de doble salto, si la cuenta de servicio del servidor SQL Server que hospeda el catálogo de SSISDB (la máquina B en nuestro ejemplo) está configurada para la delegación restringida, ISServerExec.exe no podrá delegar las credenciales en la tercera máquina (máquina C). Esto es aplicable en los escenarios en los que esté habilitada la Protección de credenciales de Windows, que requiere de forma obligatoria que se configure la delegación restringida.

  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   Entrada de blog, [SSIS y PowerShell en SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015), en blogs.msdn.com.  
  
-   Entrada de blog [Sugerencias del control de acceso al catálogo de SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-catalog-access-control-tips/ba-p/388057), en blogs.msdn.com.  
  
-   Entrada del blog [A Glimpse of the SSIS Catalog Managed Object Model](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892), en blogs.msdn.com.