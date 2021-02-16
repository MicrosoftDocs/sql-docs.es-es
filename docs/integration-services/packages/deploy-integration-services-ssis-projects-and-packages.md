---
description: Implementación de proyectos y paquetes de Integration Services (SSIS)
title: Implementación de proyectos y paquetes de Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: vanto
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.bids.converttolegacydeployment.f1
- sql13.ssis.deploymentwizard.f1
- sql13.ssis.ssms.isenvprop.permissions.f1
- sql13.ssis.ssms.isenvprop.general.f1
- sql13.ssis.ssms.iscreateenv.f1
- sql13.ssis.ssms.isenvprop.variables.f1
- sql13.ssis.migrationwizard.f1
ms.assetid: bea8ce8d-cf63-4257-840a-fc9adceade8c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5ee14ee789174ad12db2cea297202fc63ee68d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346954"
---
# <a name="deploy-integration-services-ssis-projects-and-packages"></a>Implementación de proyectos y paquetes de Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] admite dos modelos de implementación, el modelo de implementación del proyecto y el modelo de implementación de paquetes heredados. El modelo de implementación del proyecto le permite implementar sus proyectos en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
Para más información sobre el modelo de implementación de paquetes heredada, vea [Implementación de paquetes heredada &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
> [!NOTE]  
>  El modelo de implementación de proyectos se introdujo en [!INCLUDE[ssISversion11](../../includes/ssisversion11-md.md)]. Con este modelo de implementación, no pudo implementar uno o varios paquetes sin implementar todo el proyecto. [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] incluyó la característica Implementación incremental de paquetes, que permite implementar uno o varios paquetes sin implementar todo el proyecto.

> [!NOTE]
> En este artículo se describe cómo implementar paquetes SSIS en general y cómo implementar los paquetes de forma local. Los paquetes SSIS también se pueden implementar en las plataformas siguientes:
> - **La nube de Microsoft Azure**. Para obtener más información, consulte [Lift and shift SQL Server Integration Services workloads to the cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) (Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift).
> - **Linux**. Para obtener más información, consulte [Extracción, transformación y carga de datos en Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md) .

## <a name="compare-project-deployment-model-and-legacy-package-deployment-model"></a>Comparación del modelo de implementación de proyectos y el modelo de implementación de paquetes heredados

 El tipo de modelo de implementación que elija para un proyecto determina qué opciones de desarrollo y administrativas están disponibles para ese proyecto. En la tabla siguiente se muestran las diferencias y similitudes entre utilizar el modelo de implementación del proyecto y utilizar el modelo de implementación de paquetes.  
  
|Cuando se usa el modelo de implementación del proyecto|Cuando se usa el modelo de implementación de paquetes heredados|  
|---------------------------------------------|----------------------------------------------------|  
|Un proyecto es la unidad de implementación.|Un paquete es la unidad de implementación.|  
|Los parámetros se usan para asignar valores a las propiedades del paquete.|Las configuraciones se usan para asignar valores a las propiedades del paquete.|  
|Un proyecto, que contiene paquetes y parámetros, se genera en un archivo de implementación del proyecto (extensión .ispac).|Los paquetes (extensión .dtsx) y las configuraciones (extensión .dtsConfig) se guardan por separado en el sistema de archivos.|  
|Un proyecto, que contiene paquetes y parámetros, se implementa en el catálogo de SSISDB en una instancia de SQL Server.|Los paquetes y las configuraciones se copian en el sistema de archivos en otro equipo. Los paquetes también pueden guardarse en la base de datos MSDB en una instancia de SQL Server.|  
|Se requiere la integración con CLR en el motor de base de datos.|No se requiere la integración con CLR en el motor de base de datos.|  
|Los valores de parámetros específicos del entorno se almacenan en variables de entorno.|Los valores de configuración específicos del entorno se almacenan en archivos de configuración.|  
|Los proyectos y los paquetes del catálogo se pueden validar en el servidor antes de la ejecución. Puede utilizar SQL Server Management Studio, procedimientos almacenados o código administrado para realizar la validación.|Los paquetes se validan inmediatamente antes de la ejecución. También puede validar un paquete con dtExec o con código administrado.|  
|Los paquetes se ejecutan iniciando una ejecución en el motor de base de datos. Se asignan un identificador del proyecto, valores de parámetros explícitos (opcional) y referencias de entorno (opcional) a una ejecución antes de que se inicie.<br /><br /> También puede ejecutar paquetes mediante **dtExec**.|Los paquetes se ejecutan con las utilidades de ejecución de **dtExec** y de **DTExecUI** . Las configuraciones aplicables se identifican mediante los argumentos del símbolo del sistema (opcional).|  
|Durante la ejecución, los eventos producidos por el paquete se capturan automáticamente y se guardan en el catálogo. Puede consultar estos eventos con las vistas de Transact-SQL.|Durante la ejecución, los eventos producidos por un paquete no se capturan automáticamente. Un proveedor de registro se debe agregar al paquete para capturar eventos.|  
|Los paquetes se ejecutan en un proceso de Windows independiente.|Los paquetes se ejecutan en un proceso de Windows independiente.|  
|Se utiliza el Agente SQL Server para programar la ejecución del paquete.|Se utiliza el Agente SQL Server para programar la ejecución del paquete.|  


## <a name="features-of-project-deployment-model"></a>Características del modelo de implementación del proyecto  
 En la tabla siguiente se enumeran las características disponibles en los proyectos desarrollados solo para el modelo de implementación del proyecto.  
  
|Característica|Descripción|  
|-------------|-----------------|  
|Parámetros|Un parámetro especifica los datos que usará un paquete. Puede establecer el ámbito de los parámetros en el nivel de paquete o en el nivel de proyecto con parámetros de paquete y parámetros de proyecto, respectivamente. Los parámetros se pueden usar en expresiones o tareas. Cuando el proyecto se implementa en el catálogo, se puede asignar un valor literal para cada parámetro o utilizar el valor predeterminado que se asignó en tiempo de diseño. En lugar de un valor literal, se puede hacer referencia a una variable de entorno. Los valores de variables de entorno se resuelven en el momento de la ejecución del paquete.|  
|Entornos|Un entorno es un contenedor de variables a las que pueden hacer referencia los proyectos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Cada proyecto puede tener varias referencias de entorno, pero una instancia de ejecución del paquete solo puede hacer referencia a variables de un único entorno. Los entornos permiten organizar los valores que se asignan a un paquete. Por ejemplo, se pueden tener entornos denominados "Dev", "test" y "Production".|  
|Variables de entorno|Una variable de entorno define un valor literal que se puede asignar a un parámetro durante la ejecución del paquete. Para utilizar una variable de entorno, se debe crear una referencia de entorno (en el proyecto que corresponde al entorno que tiene el parámetro), asignar un valor de parámetro al nombre de la variable de entorno y especificar la referencia de entorno correspondiente al configurar una instancia de ejecución.|  
|Catálogo de SSISDB|Todos los objetos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se almacenan y administran en una instancia de SQL Server en una base de datos denominada catálogo de SSISDB. El catálogo permite utilizar carpetas para organizar los proyectos y los entornos. Cada instancia de SQL Server solo puede tener un catálogo. Cada catálogo puede tener cero o más carpetas. Cada carpeta puede tener cero o más proyectos y cero o más entornos. Una carpeta del catálogo también se puede utilizar como límite para los permisos a objetos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|Procedimientos almacenados y vistas de catálogo|Se puede utilizar un gran número de procedimientos almacenados y vistas para administrar los objetos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el catálogo. Por ejemplo, se pueden especificar valores para parámetros y variables de entorno, crear e iniciar ejecuciones y supervisar las operaciones de catálogo. Incluso se puede ver exactamente qué valores usará un paquete antes de que empiece la ejecución.|  
  
## <a name="project-deployment"></a>Implementación del proyecto  
 En el centro del modelo de implementación del proyecto está el archivo de implementación del proyecto (extensión .ispac). El archivo de implementación del proyecto es una unidad autónoma de implementación que incluye solamente la información esencial sobre los paquetes y los parámetros del proyecto. El archivo de implementación del proyecto no captura toda la información contenida en el archivo de proyecto de Integration Services (extensión .dtproj). Por ejemplo, los archivos de texto adicional que se utilizan para escribir notas no se almacenan en el archivo de implementación del proyecto y, por lo tanto, no se implementan en el catálogo.  

## <a name="permissions-required-to-deploy-ssis-projects-and-packages"></a>Permisos necesarios para implementar proyectos y paquetes de SSIS

Si cambia la configuración predeterminada de la cuenta de servicio de SSIS, es posible que tenga que proporcionar permisos adicionales a la cuenta de servicio no predeterminada para poder implementar paquetes correctamente. Si la cuenta de servicio no predeterminada no tiene los permisos necesarios, verá el siguiente mensaje de error.

`A .NET Framework error occurred during execution of user-defined routine or aggregate "deploy_project_internal":
System.ComponentModel.Win32Exception: A required privilege is not held by the client.`

Este error suele ser el resultado de la falta de permisos de DCOM. Para corregir el error, siga estos pasos:

1.  Abra la consola **Servicios de componente** (o ejecute Dcomcnfg.exe).
2.  En la consola **Servicios de componente**, expanda **Servicios de componente** > **Equipos** > **Mi PC** > **Configuración DCOM**.
3.  En la lista, busque **Microsoft SQL Server Integration Services xx.0** para encontrar la versión de SQL Server que esté usando. Por ejemplo, SQL Server 2016 es la versión 13.
4.  Haga clic con el botón secundario y seleccione **Propiedades**.
5.  En el cuadro de diálogo **Propiedades de Microsoft SQL Server Integration Services 13.0**, seleccione la pestaña **Seguridad**.
6.  Por cada uno de los tres conjuntos de permisos (inicio y activación, acceso y configuración), seleccione **Personalizar** y, luego, **Editar** para abrir el cuadro de diálogo **Permiso**.
7.  En el cuadro de diálogo **Permiso**, agregue la cuenta de servicio no predeterminada y conceda los permisos **Permitir** según sea necesario. Normalmente, una cuenta tiene los permisos **Ejecución local** y **Activación local**.
8.  Haga clic en **Aceptar** dos veces y, a continuación, cierre la consola **Servicios de componente**.

Para más información sobre el error descrito en esta sección y los permisos necesarios para la cuenta de servicio de SSIS, vea la entrada de blog siguiente:
 
- [System.ComponentModel.Win32Exception: el cliente no dispone de un privilegio requerido durante la implementación de un proyecto de SSIS](/archive/blogs/dataaccesstechnologies/system-componentmodel-win32exception-a-required-privilege-is-not-held-by-the-client-while-deploying-ssis-project)

## <a name="deploy-projects-to-integration-services-server"></a>Implementación de paquetes en el servidor de Integration Services
  En la versión actual de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], puede implementar los proyectos en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . El servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] permite administrar paquetes, ejecutar paquetes y configurar valores de tiempo de ejecución para paquetes usando entornos.  
  
> [!NOTE]  
>  Al igual que en versiones anteriores de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], en la versión actual también puede implementar los paquetes en una instancia de SQL Server y usar el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para ejecutar y administrar los paquetes. Se usa el modelo de implementación de paquetes. Para obtener más información, vea [Implementación de paquetes heredada &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
 Para implementar un proyecto en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , debe completar las tareas siguientes:  
  
1.  Crear un catálogo de SSISDB, si aún no lo ha hecho. Para obtener más información, vea [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md) (Catálogo de SSIS).  
  
2.  Ejecute el **Asistente para conversión de proyectos de Integration Services** para convertir el proyecto al modelo de implementación de proyectos. Para más información, vea las instrucciones siguientes: [Para convertir un proyecto al modelo de implementación de proyectos](#convert)  
  
    -   Si creó el proyecto en [!INCLUDE[ssISversion12](../../includes/ssisversion12-md.md)] o en una versión posterior, de forma predeterminada el proyecto utiliza el modelo de implementación de proyectos.  
  
    -   Si creó el proyecto en la versión anterior de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], después de abrir el archivo de proyecto en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], deberá convertir el proyecto al modelo de implementación de proyectos.  
  
        > [!NOTE]  
        >  Si el proyecto contiene uno o más orígenes de datos, se quitan los orígenes de datos cuando se completa la conversión del proyecto. Para crear una conexión a un origen de datos que los paquetes del proyecto puedan compartir, agregue un administrador de conexiones en el nivel de proyecto. Para obtener más información, consulte [agregar, eliminar o compartir un administrador de conexiones en un paquete](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130)).  
  
         Dependiendo de si ejecuta el **Asistente para la conversión de proyectos de Integration Services** desde [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] o desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], el asistente realiza tareas de conversión diferentes.  
  
        -   Si ejecuta el asistente desde [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], los paquetes incluidos en el proyecto se convierten de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 2005, 2008 o 2008 R2 al formato utilizado por la versión actual de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Se actualizan los archivos originales del proyecto (.dtproj) y del paquete (.dtsx).  
  
        -   Si ejecuta el asistente desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], el asistente genera un archivo de implementación del proyecto (.ispac) a partir de los paquetes y las configuraciones incluidos en el proyecto. Los archivos originales del paquete (.dtsx) no se actualizan.  
  
             Puede seleccionar un archivo existente o crear un archivo nuevo en la página **Destino de selección** del asistente.  
  
             Para actualizar archivos del paquete cuando se convierte un proyecto, ejecute el **Asistente para conversión de proyectos de Integration Services** desde [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Para actualizar los archivos de paquete independientemente de la conversión del proyecto, ejecute el **Asistente para la conversión de proyectos de Integration Services** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y el **Asistente para actualizar paquetes SSIS**. Si actualiza los archivos de paquete por separado, asegúrese de guardar los cambios. De lo contrario, cuando convierte el proyecto al modelo de implementación de proyectos, los cambios no guardados efectuados en el paquete no se convierten.  
  
     Para obtener más información sobre la actualización de paquetes, vea [Actualizar paquetes de Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md) y [Actualizar paquetes de Integration Services mediante el Asistente para actualizar paquetes SSIS](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md).  
  
3.  Implemente el proyecto en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para más información, vea las instrucciones siguientes: [Para implementar un proyecto en el servidor de Integration Services](#deploy).  
  
4.  (Opcional) Crear un entorno para el proyecto implementado. 
  
###  <a name="to-convert-a-project-to-the-project-deployment-model"></a><a name="convert"></a> Para convertir un proyecto al modelo de implementación de proyectos  
  
1.  Abra el proyecto en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]y, en el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Convertir al modelo de implementación de proyectos**.  
  
     o bien  
  
     En el Explorador de objetos de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], haga clic con el botón derecho en el nodo **Proyectos** y seleccione **Importar paquetes**.  
  
2.  Finalice el asistente.
  
###  <a name="to-deploy-a-project-to-the-integration-services-server"></a><a name="deploy"></a> Para implementar un proyecto en el servidor de Integration Services  
  
1.  Abra el proyecto en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]y, en el menú **Proyecto** , seleccione **Implementar** para iniciar el **Asistente para implementación de Integration Services**.  
  
     o bien  
  
     En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] > **SSISDB** en el Explorador de objetos y busque la carpeta Proyectos correspondiente al proyecto que quiere implementar. Haga clic con el botón derecho en la carpeta **Proyectos** y, después, haga clic en **Implementar proyecto**.  
  
     o bien  
  
     En el símbolo del sistema, ejecute **isdeploymentwizard.exe** de **%Archivos de programa%\Microsoft SQL Server\130\DTS\Binn**. En equipos de 64 bits, también hay una versión de 32 bits de la herramienta en **%Archivos de programa (x86)%\Microsoft SQL Server\130\DTS\Binn**.  
  
2.  En la página **Seleccionar origen** , haga clic en **Archivo de implementación de proyecto** para seleccionar el archivo de implementación del proyecto.  
  
     o bien  
  
     Haga clic en **Catálogo de Integration Services** para seleccionar un proyecto que ya se haya implementado en el catálogo de SSISDB.  
  
3.  Finalice el asistente. 

## <a name="deploy-packages-to-integration-services-server"></a>Implementación de paquetes en el servidor de Integration Services
  La característica Implementación incremental de paquetes presentada en  [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] le permite implementar uno o varios paquetes en un proyecto nuevo o existente sin implementar todo el proyecto.  
  
###  <a name="deploy-packages-by-using-the-integration-services-deployment-wizard"></a><a name="DeployWizard"></a> Implementación de paquetes mediante el Asistente para implementación de Integration Services  
  
1.  En el símbolo del sistema, ejecute **isdeploymentwizard.exe** de **%Archivos de programa%\Microsoft SQL Server\130\DTS\Binn**. En equipos de 64 bits, también hay una versión de 32 bits de la herramienta en **%Archivos de programa (x86)%\Microsoft SQL Server\130\DTS\Binn**.  
  
2.  En la página **Seleccionar origen** , cambie a **Modelo de implementación de paquetes**. Después, seleccione la carpeta que contiene los paquetes de origen y configúrelos.  
  
3.  Finalice el asistente. Siga los pasos restantes descritos en [Package Deployment Model](#PackageModel).  
  
###  <a name="deploy-packages-by-using-sql-server-management-studio"></a><a name="SSMS"></a> Implementación de paquetes mediante SQL Server Management Studio  
  
1.  En SQL Server Management Studio, expanda el nodo **Catálogos de Integration Services** > **SSISDB** en el Explorador de objetos.  
  
2.  Haga clic con el botón derecho en la carpeta **Proyectos** y luego en **Implementar proyectos**.  
  
3.  Si ve la página **Introducción** , haga clic en **Siguiente** para continuar.  
  
4.  En la página **Seleccionar origen** , cambie a **Modelo de implementación de paquetes**. Después, seleccione la carpeta que contiene los paquetes de origen y configúrelos.  
  
5.  Finalice el asistente. Siga los pasos restantes descritos en [Package Deployment Model](#PackageModel).  
  
###  <a name="deploy-packages-by-using-sql-server-data-tools-visual-studio"></a><a name="SSDT"></a> Implementación de paquetes mediante SQL Server Data Tools (Visual Studio)  
  
1.  En Visual Studio, con un proyecto de Integration Services abierto, seleccione el paquete o paquetes que desea implementar.  
  
2.  Haga clic con el botón derecho y seleccione **Implementar paquete**. Se abre el Asistente para implementación con los paquetes seleccionados configurados como los paquetes de origen.  
  
3.  Finalice el asistente. Siga los pasos restantes descritos en [Package Deployment Model](#PackageModel).  
  
###  <a name="deploy-packages-by-using-the-deploy_packages-stored-procedure"></a><a name="StoredProcedure"></a> Implementación de paquetes mediante el procedimiento almacenado deploy_packages  
 Puede usar el procedimiento almacenado **[catalog].[deploy_packages]** para implementar uno o más paquetes SSIS en el catálogo de SSIS. En el ejemplo de código siguiente se muestra el uso de este procedimiento almacenado para implementar paquetes en un servidor de SSIS. Para obtener más información, vea [catalog.deploy_packages](../../integration-services/system-stored-procedures/catalog-deploy-packages.md).  
  
```cs
  
private static void Main(string[] args)  
{  
    // Connection string to SSISDB  
    var connectionString = "Data Source=.;Initial Catalog=SSISDB;Integrated Security=True;MultipleActiveResultSets=false";  
  
    using (var sqlConnection = new SqlConnection(connectionString))  
    {  
        sqlConnection.Open();  
  
        var sqlCommand = new SqlCommand  
        {  
            Connection = sqlConnection,  
            CommandType = CommandType.StoredProcedure,  
            CommandText = "[catalog].[deploy_packages]"  
        };  
  
        var packageData = Encoding.UTF8.GetBytes(File.ReadAllText(@"C:\Test\Package.dtsx"));  
  
        // DataTable: name is the package name without extension and package_data is byte array of package.  
        var packageTable = new DataTable();  
        packageTable.Columns.Add("name", typeof(string));  
        packageTable.Columns.Add("package_data", typeof(byte[]));  
        packageTable.Rows.Add("Package", packageData);  
  
        // Set the destination project and folder which is named Folder and Project.  
        sqlCommand.Parameters.Add(new SqlParameter("@folder_name", SqlDbType.NVarChar, ParameterDirection.Input, "Folder", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@project_name", SqlDbType.NVarChar, ParameterDirection.Input, "Project", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@packages_table", SqlDbType.Structured, ParameterDirection.Input, packageTable, -1));  
  
        var result = sqlCommand.Parameters.Add("RetVal", SqlDbType.Int);  
        result.Direction = ParameterDirection.ReturnValue;  
  
        sqlCommand.ExecuteNonQuery();  
    }  
}  
  
```  
  
###  <a name="deploy-packages-using-the-management-object-model-api"></a><a name="MOMApi"></a> Implementación de paquetes mediante la API del modelo de objetos de administración  
 En el ejemplo de código siguiente se muestra el uso de la API del modelo de objetos de administración para implementar paquetes en un servidor.  
  
```cs 
  
static void Main()  
 {  
     // Before deploying packages, make sure the destination project exists in SSISDB.  
     var connectionString = "Data Source=.;Integrated Security=True;MultipleActiveResultSets=false";  
     var catalogName = "SSISDB";  
     var folderName = "Folder";  
     var projectName = "Project";  
  
     // Get the folder instance.  
     var sqlConnection = new SqlConnection(connectionString);  
     var store = new Microsoft.SqlServer.Management.IntegrationServices.IntegrationServices(sqlConnection);  
     var folder = store.Catalogs[catalogName].Folders[folderName];  
  
     // Key is package name without extension and value is package binaries.  
     var packageDict = new Dictionary<string, string>();  
  
     var packageData = File.ReadAllText(@"C:\Folder\Package.dtsx");  
     packageDict.Add("Package", packageData);  
  
     // Deploy package to the destination project.  
     folder.DeployPackages(projectName, packageDict);  
 }  
  
```

## <a name="convert-to-package-deployment-model-dialog-box"></a>Convertir a modelo de implementación de paquetes, cuadro de diálogo
  El comando de **Convertir a modelo de implementación de paquetes** permite convertir un paquete al modelo de implementación de paquetes después de comprobar el proyecto y cada paquete en el proyecto para ver la compatibilidad con ese modelo. Si un paquete usa las características exclusivas del modelo de implementación de proyectos, como los parámetros, el paquete no se podrá convertir.  

 Para convertir un paquete al modelo de implementación de paquetes es preciso realizar dos pasos.  
  
1.  Cuando se selecciona el comando **Convertir a modelo de implementación de paquetes** en el menú **Proyecto** , el proyecto y cada paquete se comprueban para ver su compatibilidad con este modelo. Los resultados se muestran en tabla **Resultados** .  
  
     Si el proyecto o un paquete no supera la prueba de compatibilidad, haga clic en **Error** en la columna **Resultado** para obtener más información. Haga clic en **Guardar informe** para guardar una copia de esta información en un archivo de texto.  
  
2.  Si el proyecto y todos los paquetes superan la prueba de compatibilidad, haga clic **Aceptar** para convertir el paquete.  
  
> [!NOTE]
> Para convertir un proyecto al modelo de implementación de paquetes, use el **Asistente para conversión de proyectos de Integration Services**. Para más información, consulte [Integration Services Project Conversion Wizard](deploy-integration-services-ssis-projects-and-packages.md).  

## <a name="integration-services-deployment-wizard"></a>Asistente para implementación de Integration Services
  El **Asistente para implementación de Integration Services** admite dos modelos de implementación:
   - Modelo de implementación de proyectos
   - Modelo de implementación de paquetes 
   
 El **modelo de implementación de proyectos** permite implementar un proyecto de SQL Server Integration Services (SSIS) como una sola unidad en el catálogo de SSIS.
 
 El **modelo de implementación de paquetes** permite implementar paquetes que se han actualizado en el catálogo de SSIS sin tener que implementar todo el proyecto. 
 
 > [!NOTE]
 > La implementación predeterminada del asistente es el modelo de implementación de proyectos.  
  
### <a name="launch-the-wizard"></a>Inicio del asistente
Inicie el asistente de una de estas dos formas:

 - Escriba **"Asistente de implementación de SQL Server"** en Windows Search 

 o bien

 - Busque el archivo ejecutable **ISDeploymentWizard.exe** en la carpeta de instalación de SQL Server; por ejemplo: “C:\Archivos de programa (x86)\Microsoft SQL Server\130\DTS\Binn”. 
 
 > **NOTA:** Si ve la página **Introducción** , haga clic en **Siguiente** para cambiar a la página **Seleccionar origen** . 
 
 La configuración de esta página es diferente en cada modelo de implementación. Siga los pasos de la sección [Project Deployment Model](#ProjectModel) o [Package Deployment Model](#PackageModel) en función del modelo que haya seleccionado en esta página.  
  
###  <a name="project-deployment-model"></a><a name="ProjectModel"></a> Project Deployment Model  
  
#### <a name="select-source"></a>Selección del origen

 Para implementar un archivo de implementación de proyectos que haya creado, seleccione **Archivo de implementación de proyecto** y especifique la ruta de acceso del archivo .ispac. Para implementar un proyecto que resida en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , seleccione **Catálogo de Integration Services** y especifique el nombre del servidor y la ruta de acceso al proyecto en el catálogo. Haga clic en **Siguiente** para ver la página **Seleccionar destino** .  
  
#### <a name="select-destination"></a>Seleccionar destino

 Para seleccionar la carpeta de destino para el proyecto en el catálogo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , escriba la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o haga clic en **Examinar** para seleccionarla en una lista de servidores. Escriba la ruta de acceso del proyecto en SSISDB o haga clic en **Examinar** para seleccionarla. Haga clic en **Siguiente** para ver la página **Revisión** .  
  
#### <a name="review-and-deploy"></a>Revisión e implementación

 La página permite revisar la configuración seleccionada. Puede cambiar las selecciones si hace clic en **Anterior** o si hace clic en cualquiera de los pasos del panel izquierdo. Haga clic en **Implementar** para iniciar el proceso de implementación.  
  
#### <a name="results"></a>Results

 Una vez completado el proceso de implementación, debería ver la página **Resultados** . Esta página muestra si cada acción si se completó correctamente o no. Si la acción no se realiza correctamente, haga clic en **Error** en la columna **Resultado** para que aparezca una explicación del error. Haga clic en **Guardar informe** para guardar los resultados en un archivo XML o haga clic en **Cerrar** para salir del asistente.
  
###  <a name="package-deployment-model"></a><a name="PackageModel"></a> Package Deployment Model  
  
#### <a name="select-source"></a>Selección del origen

 La página **Seleccionar origen** del **Asistente para implementación de Integration Services** muestra la configuración específica del modelo de implementación de paquetes cuando selecciona la opción **lmplementación del paquete** del **modelo de implementación**.  
  
 Para seleccionar los paquetes de origen, haga clic en el botón **Examinar…** para seleccionar la **carpeta** que contiene los paquetes, o bien escriba la ruta de la carpeta en el cuadro de texto **Ruta de acceso de la carpeta de paquetes** y haga clic en el botón **Actualizar** situado en la parte inferior de la página. Ahora, debería ver todos los paquetes en la carpeta especificada en el cuadro de lista. De forma predeterminada, se seleccionan todos los paquetes. Haga clic en la **casilla** de la primera columna para elegir los paquetes que desea implementar en el servidor.  
  
 Consulte las columnas **Estado** y **Mensaje** para comprobar el estado del paquete. Si el estado es **Listo** o **Advertencia**, el Asistente para la implementación no bloquearía el proceso de implementación. Si el estado está establecido en **Error**, el asistente no continuará con la implementación de los paquetes seleccionados. Para ver los mensajes de advertencia o error detallados, haga clic en el vínculo de la columna **Mensaje**.  
  
 Si la información confidencial o los datos del paquete están cifrados con una contraseña, escríbala en la columna **Contraseña** y haga clic en el botón **Actualizar** para comprobar si se acepta la contraseña. Si la contraseña es correcta, el estado cambiaría a **Listo** y desaparecerá el mensaje de advertencia. Si hay varios paquetes con la misma contraseña, seleccione los que tengan la misma contraseña de cifrado, escriba la contraseña en el cuadro de texto **Contraseña** y seleccione el botón **Aplicar**. La contraseña se aplicaría a los paquetes seleccionados.  
  
 Si el estado de todos los paquetes seleccionados no es **Error**, el botón **Siguiente** se habilitará para que pueda continuar con el proceso de implementación de paquetes.  
  
#### <a name="select-destination"></a>Seleccionar destino

 Después de seleccionar los orígenes de paquetes, haga clic en el botón **Siguiente** para cambiar a la página **Seleccionar destino**. Los paquetes deben implementarse en un proyecto del catálogo de SSIS (SSISDB). Antes de implementar los paquetes, asegúrese de que el proyecto de destino ya exista en el catálogo de SSIS. Cree un proyecto vacío si no existe uno. En la página **Seleccionar destino**, escriba el nombre del servidor en el cuadro de texto **Nombre del servidor**, o bien haga clic en el botón **Examinar…** para seleccionar una instancia de servidor. Después, haga clic en el botón **Examinar…** situado junto al cuadro de texto **Ruta** para especificar el proyecto de destino. Si el proyecto no existe, haga clic en el botón **Nuevo proyecto…** para crear un proyecto vacío como proyecto de destino. El proyecto se debe crear en una carpeta.  
  
#### <a name="review-and-deploy"></a>Revisión e implementación

 Haga clic en la opción **Siguiente** de la página **Seleccionar destino** para cambiar a la página **Revisión** del **Asistente para implementación de Integration Services**. En esta página, revise el informe de resumen sobre la acción de implementación. Después de la comprobación, haga clic en el botón **Implementar** para realizar la acción de implementación.  
  
#### <a name="results"></a>Results

 Una vez completada la implementación, debería ver la página **Resultados** . En la página **Resultados**, revise los resultados de cada paso del proceso de implementación. Haga clic en **Guardar informe** para guardar el informe de implementación o en **Cerrar** para cerrar el asistente.  

## <a name="create-and-map-a-server-environment"></a>Crear y asignar un entorno de servidor

  Los entornos de servidor se crean con el fin de especificar valores en tiempo de ejecución para los paquetes contenidos en un proyecto implementado en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Después puede asignar las variables de entorno a parámetros para un paquete concreto, para los paquetes de punto de entrada o para todos los paquetes de un proyecto determinado. Un paquete de punto de entrada suele ser un paquete primario que ejecuta un paquete secundario.  
  
> [!IMPORTANT]  
>  Para una ejecución determinada, un paquete solo puede ejecutarse con los valores contenidos en un único entorno de servidor.  
  
 Puede consultar las vistas para obtener una lista de entornos de servidor, referencias de entorno y variables de entorno. También puede llamar a procedimientos almacenados para agregar, eliminar y modificar entornos, referencias de entorno y variables de entorno. Para más información, vea la sección **Entornos de servidor, variables de servidor y referencias del entorno de servidor** en [Catálogo de SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
### <a name="to-create-and-use-a-server-environment"></a>Para crear y utilizar un entorno de servidor  
  
1.  En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], expanda el nodo **SSISDB** de Catálogos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el Explorador de objetos y busque la carpeta **Entornos** del proyecto para el que quiera crear un entorno.  
  
2.  Haga clic con el botón derecho en la carpeta **Entornos** y, después, haga clic en **Crear entorno**.  
  
3.  Escriba un nombre para el entorno y opcionalmente una descripción. Haga clic en **Aceptar**.  
  
4.  Haga clic con el botón derecho en el nuevo entorno y, después, haga clic en **Propiedades**.  
  
5.  En la página **Variables** , haga lo siguiente para agregar una variable.  
  
    1.  Seleccione **Tipo** para la variable. No es necesario que el nombre de la variable coincida con el nombre del parámetro del proyecto al que se asigna la variable.  
  
    2.  Escriba la **Descripción** opcional para la variable.  
  
    3.  Escriba el **Valor** para la variable de entorno.  
  
         Para obtener información sobre las reglas para los nombres de las variables de entorno, vea la sección **Variable de entorno** en [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
    4.  Indique si la variable contiene un valor confidencial; para ello, active o desactive la casilla **Confidencial** .  
  
         Si selecciona **Confidencial**, el valor de la variable no aparece en el campo **Valor** .  
  
         Los valores confidenciales se cifran en el catálogo de SSISDB. Para obtener más información acerca del cifrado, vea [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
6.  En la página **Permisos** , haga lo siguiente para conceder o denegar permisos para los usuarios y roles seleccionados.  
  
    1.  Haga clic en **Examinar** y, a continuación, seleccione uno o más usuarios y roles en el cuadro de diálogo **Examinar todas las entidades** .  
  
    2.  En el área **Inicios de sesión o roles** , seleccione el usuario o el rol al que desea conceder o denegar permisos.  
  
    3.  En el área **Explícito**, seleccione **Conceder** o **Denegar** junto a cada permiso.  
  
7.  Para incluir el entorno en el script, haga clic en **Script**. De forma predeterminada, el script aparece en una nueva ventana del Editor de consultas.  
  
    > [!TIP]  
    >  Tiene que hacer clic en **Script** después de realizar cambios en las propiedades del entorno (como agregar una variable) y antes de hacer clic en **Aceptar** en el cuadro de diálogo **Propiedades del entorno**. De lo contrario, no se generará un script.  
  
8.  Haga clic en **Aceptar** para guardar los cambios realizados en las propiedades de entorno.  
  
9. En el nodo **SSISDB** del Explorador de objetos, expanda la carpeta **Proyectos** , haga clic con el botón derecho en el proyecto y, después, haga clic en **Configurar**.  
  
10. En la página **Referencias** , haga clic en **Agregar** para agregar un entorno y, a continuación, haga clic en **Aceptar** para guardar la referencia al entorno.  
  
11. Vuelva a hacer clic con el botón derecho en el proyecto y, después, haga clic en **Configurar**.  
  
12. Para asignar la variable de entorno a un parámetro que ha agregado al paquete en tiempo de diseño o a un parámetro que se ha generado al convertir el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al modelo de implementación del proyecto, siga estos pasos:
  
    1.  En la pestaña **Parámetros** de la página **Parámetros** , haga clic en el botón Examinar situado junto al campo **Valor** .  
  
    2.  Haga clic en **Usar variable de entorno** y seleccione la variable de entorno que creó.  
  
13. Para asignar la variable de entorno a una propiedad del administrador de conexiones, haga lo siguiente. Se generan automáticamente parámetros en el servidor SSIS para las propiedades del administrador de conexiones.  
  
    1.  En la pestaña **Administradores de conexiones** de la página **Parámetros**, haga clic en el botón **Examinar** situado junto al campo **Valor**.  
  
    2.  Haga clic en **Usar variable de entorno** y seleccione la variable de entorno que creó.  
  
14. Haga clic en **Aceptar** dos veces para guardar los cambios.  

## <a name="deploy-and-execute-ssis-packages-using-stored-procedures"></a>Implementar y ejecutar paquetes SSIS mediante procedimientos almacenados

  Al configurar un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para que use el modelo de implementación de proyectos, puede emplear procedimientos almacenados del catálogo de [!INCLUDE[ssIS](../../includes/ssis-md.md)] implementar el proyecto y ejecutar los paquetes. Para obtener información acerca del modelo de implementación de proyectos, vea [Deployment of Projects and Packages](deploy-integration-services-ssis-projects-and-packages.md#compare-project-deployment-model-and-legacy-package-deployment-model).  
  
 También puede usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] para implementar el proyecto y ejecutar los paquetes. Para obtener más información, vea los temas de la sección **Vea también** .  
  
> [!TIP]
>  Puede generar fácilmente las instrucciones Transact-SQL para los procedimientos almacenados enumerados en el procedimiento siguiente, a excepción de catalog.deploy_project, si hace lo siguiente:
> 
>  1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo de **Catálogos de Integration Services** en el Explorador de objetos y navegue hasta el paquete que desea ejecutar.  
> 2.  Haga clic con el botón derecho en el paquete y, después, haga clic en **Ejecutar**.  
> 3.  Según sea necesario, establezca los valores de parámetros, las propiedades del administrador de conexiones y las opciones de la pestaña **Avanzadas** , como el nivel de registro.  
> 
>      Para obtener más información acerca de los niveles de registro, vea [Habilitar el registro para la ejecución de paquetes en el servidor SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
> 4.  Antes de hacer clic en **Aceptar** para ejecutar el paquete, haga clic en **Script**. Transact-SQL aparece en una ventana del Editor de consultas en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="to-deploy-and-execute-a-package-using-stored-procedures"></a>Para implementar y ejecutar un paquete mediante procedimientos almacenados  
  
1.  Llame a [catalog.deploy_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md) para implementar el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
     Para recuperar el contenido binario del archivo de implementación de proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], para el parámetro _\@project_stream_, use una instrucción SELECT con la función OPENROWSET y el proveedor de conjuntos de filas BULK. El proveedor de conjuntos de filas BULK le permite leer datos de un archivo. El argumento SINGLE_BLOB del proveedor de conjuntos de filas BULK devuelve el contenido del archivo de datos como un conjunto de filas de una sola fila y una sola columna de tipo varbinary(max). Para obtener más información, vea [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
     En el ejemplo siguiente, el proyecto SSISPackages_ProjectDeployment se implementa en la carpeta Paquetes SSIS del servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Los datos binarios se leen del archivo de proyecto (SSISPackage_ProjectDeployment.ispac) y se almacenan en el parámetro _\@ProjectBinary de tipo varbinary(max). El valor de parámetro _\@ProjectBinary_ se asigna al parámetro _\@project_stream_.  
  
    ```sql
    DECLARE @ProjectBinary as varbinary(max)  
    DECLARE @operation_id as bigint  
    Set @ProjectBinary = (SELECT * FROM OPENROWSET(BULK 'C:\MyProjects\ SSISPackage_ProjectDeployment.ispac', SINGLE_BLOB) as BinaryData)  
  
    Exec catalog.deploy_project @folder_name = 'SSIS Packages', @project_name = 'DeployViaStoredProc_SSIS', @Project_Stream = @ProjectBinary, @operation_id = @operation_id out  
  
    ```  
  
2.  Llame a [catalog.create_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) para crear una instancia de la ejecución de paquetes y llame opcionalmente a [catalog.set_execution_parameter_value &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) para establecer los valores de parámetro en tiempo de ejecución.  
  
     En el ejemplo siguiente, catalog.create_execution crea una instancia de ejecución del package.dtsx contenido en el proyecto SSISPackage_ProjectDeployment. El proyecto se encuentra en la carpeta SSIS Packages. El execution_id devuelto por el procedimiento almacenado se emplea en la llamada a catalog.set_execution_parameter_value. Este segundo procedimiento almacenado establece el parámetro LOGGING_LEVEL en 3 (registro detallado) y establece un parámetro de paquete denominado Parameter1 en un valor de 1.  
  
     Para los parámetros como LOGGING_LEVEL, el valor de object_type es 50. Para los parámetros de paquete, el valor de object_type es 30.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    GO  
  
    ```  
  
3.  Llame a [catalog.start_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) para ejecutar el paquete.  
  
     En el ejemplo siguiente, se agrega a Transact-SQL una llamada a catalog.start_execution para iniciar la ejecución del paquete. Se empela el execution_id devuelto por el procedimiento almacenado catalog.create_execution.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    EXEC [SSISDB].[catalog].[start_execution] @execution_id  
    GO  
  
    ```  
  
### <a name="to-deploy-a-project-from-server-to-server-using-stored-procedures"></a>Para implementar un proyecto entre servidores mediante procedimientos almacenados

 Puede implementar un proyecto entre servidores mediante los procedimientos almacenados [catalog.get_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md) y [catalog.deploy_project &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md).  
  
 Debe hacer lo siguiente antes de ejecutar los procedimientos almacenados.  
  
-   Cree un objeto de servidor vinculado. Para obtener más información, vea [Crear servidores vinculados &#40;motor de base de datos de SQL Server&#41;](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md).  
  
     En la página **Opciones del servidor** del cuadro de diálogo **Propiedades del servidor vinculado** , establezca **RPC** y **Salida RPC** en **True**. Establezca también **Habilitar la promoción de transacciones distribuidas para RPC** en **False**.  
  
-   Habilite los parámetros dinámicos del proveedor seleccionado para el servidor vinculado; para ello, expanda el nodo de **Proveedores** en **Servidores vinculados** en el Explorador de objetos, haga clic con el botón derecho en el proveedor y, después, haga clic en **Propiedades**. Seleccione **Habilitar** junto a **Parámetro dinámico**.  
  
-   Confirme que el Coordinador de transacciones distribuidas (DTC) se inicia en ambos servidores.  
  
 Llame a catalog.get_project para devolver el contenido binario del proyecto y llame después a catalog.deploy_project. El valor devuelto por catalog.get_project se inserta en una variable de tabla de tipo varbinary(max). El servidor vinculado no puede devolver resultados que son varbinary(max).  
  
 En el ejemplo siguiente, catalog.get_project devuelve un contenido binario para el proyecto SSISPackages del servidor vinculado. catalog.deploy_project implementa el proyecto en el servidor local, en la carpeta denominada DestFolder.  
  
```sql
declare @resultsTableVar table (  
project_binary varbinary(max)  
)  
  
INSERT @resultsTableVar (project_binary)  
EXECUTE [MyLinkedServer].[SSISDB].[catalog].[get_project] 'Packages', 'SSISPackages'  
  
declare @project_binary varbinary(max)  
select @project_binary = project_binary from @resultsTableVar  
  
exec [SSISDB].[CATALOG].[deploy_project] 'DestFolder', 'SSISPackages', @project_binary  
  
```  

## <a name="integration-services-project-conversion-wizard"></a>Asistente para conversión de proyectos de Integration Services
  El **Asistente para conversión de proyectos de Integration Services** convierte un proyecto al modelo de implementación de proyectos.  
  
> [!NOTE]  
>  Si el proyecto contiene uno o más orígenes de datos, se quitan los orígenes de datos cuando se completa la conversión del proyecto. Para crear una conexión a un origen de datos que se pueden compartir los paquetes en el proyecto, agregue un administrador de conexiones en el nivel de proyecto. Para obtener más información, consulte [agregar, eliminar o compartir un administrador de conexiones en un paquete](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130)).  
  
 **¿Qué desea hacer?**  
  
-   [Abrir el Asistente para conversión de proyectos de Integration Services](#open_dialog)  
  
-   [Establecer las opciones de la página Buscar paquetes](#locate)  
  
-   [Establecer las opciones de la página Seleccionar paquete](#selectPackages)  
  
-   [Establecer las opciones de la página Seleccionar destino](#destination)  
  
-   [Establecer las opciones de la página Especificar propiedades del proyecto](#projectProperties)  
  
-   [Establecer las opciones de la página Actualizar la tarea Ejecutar paquete](#executePackage)  
  
-   [Establecer las opciones de la página Seleccionar configuraciones](#configurations)  
  
-   [Establecer las opciones de la página Crear parámetros](#createParameters)  
  
-   [Establecer las opciones de la página Configurar parámetros](#configureParameters)  
  
-   [Establecer las opciones en la página Revisar](#review)  
  
-   [Establecer las opciones en Realizar conversión](#conversion)  
  
###  <a name="open-the-integration-services-project-conversion-wizard"></a><a name="open_dialog"></a> Abrir el Asistente para conversión de proyectos de Integration Services  
 Para abrir el asistente **Conversión de proyecto de Integration Services** , realice una de las acciones siguientes:  
  
-   Abra el proyecto en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]y, en el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Convertir al modelo de implementación de proyectos**.  
  
-   En el Explorador de objetos en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], haga clic con el botón derecho en el nodo **Proyectos** de **Catálogo de Integration Services** y seleccione **Importar paquetes**.  
  
 Dependiendo de si ejecuta el **Asistente para la conversión de proyectos de Integration Services** desde [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] o desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], el asistente realiza tareas de conversión diferentes.   
  
###  <a name="set-options-on-the-locate-packages-page"></a><a name="locate"></a> Establecer las opciones de la página Buscar paquetes  
  
> [!NOTE]  
>   La página **Buscar paquetes** está disponible solamente cuando ejecuta el asistente desde [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 La siguiente opción aparece en la página al seleccionar **Sistema de archivos** en la lista desplegable **Origen** . Seleccione esta opción si el paquete reside en el sistema de archivos.  
  
 **Carpeta**  
 Escriba la ruta de acceso al paquete, o bien haga clic en **Examinar** para abrir la ubicación del paquete.  
  
 En la página se muestran las opciones siguientes al seleccionar **Almacén de paquetes SSIS** en la lista desplegable **Origen**. Para más información sobre el almacén de paquetes, vea [Administración de paquetes &#40;servicio SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).  
  
 **Server**  
 Especifique el nombre del servidor o seleccione el servidor.  
  
 **Carpeta**  
 Escriba la ruta de acceso al paquete, o bien haga clic en **Examinar** para abrir la ubicación del paquete.  
  
 En la página se muestran las opciones siguientes al seleccionar **Microsoft SQL Server** en la lista desplegable **Origen** . Seleccione esta opción si el paquete reside en Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Server**  
 Especifique el nombre del servidor o seleccione el servidor.  
  
 **Usar la autenticación de Windows**  
 El modo de autenticación de Microsoft Windows permite que un usuario pueda conectarse a través de una cuenta de usuario de Windows. Si utiliza la autenticación de Windows, no será necesario que proporcione un nombre de usuario o una contraseña.  
  
 **Utilizar autenticación de SQL Server**  
 Cuando un usuario se conecta con un nombre de inicio de sesión y una contraseña especificados desde una conexión que no es de confianza, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autentica la conexión (para hacerlo, comprueba si se ha configurado una cuenta de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y si la contraseña especificada coincide con la almacenada anteriormente). Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no tiene configurada una cuenta de inicio de sesión, la autenticación no se realizará correctamente y el usuario recibirá un mensaje de error.  
  
 **Nombre de usuario**  
 Especifique un nombre de usuario cuando utilice la autenticación de SQL Server.  
  
 **Contraseña**  
 Especifique la contraseña cuando use la autenticación de SQL Server.  
  
 **Carpeta**  
 Escriba la ruta de acceso al paquete, o bien haga clic en **Examinar** para abrir la ubicación del paquete.  
  
###  <a name="set-options-on-the-select-packages-page"></a><a name="selectPackages"></a> Establecer las opciones de la página Seleccionar paquete  
 **Nombre del paquete**  
 Muestra el archivo de paquete.  
  
 **Estado**  
 Indica si un paquete está listo para convertir el modelo de implementación de proyectos.  
  
 **Message**  
 Muestra un mensaje asociado al paquete.  
  
 **Contraseña**  
 Muestra una contraseña asociada al paquete. Se oculta el texto de la contraseña.  
  
 **Aplicar a la selección**  
 Haga clic en el cuadro de texto **Contraseña** para aplicar la contraseña a los paquetes seleccionados.  
  
 **Actualizar**  
 Actualiza la lista de paquetes.  
  
###  <a name="set-options-on-the-select-destination-page"></a><a name="destination"></a> Establecer las opciones de la página Seleccionar destino  
 En esta página, especifique el nombre y la ruta de un nuevo archivo de implementación del proyecto (.ispac) o seleccione un archivo existente.  
  
> [!NOTE]  
>   La página **Seleccionar destino** está disponible solamente cuando ejecuta el asistente desde [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 **Ruta de acceso de salida**  
 Escriba la ruta de acceso al archivo de implementación, o bien haga clic en **Examinar** para abrir la ubicación del archivo.  
  
 **Nombre del proyecto**  
 Escriba el nombre del proyecto.  
  
 **Nivel de protección**  
 Seleccione el nivel de protección. Para más información, consulte [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Descripción del proyecto**  
 Escriba una descripción opcional para el proyecto.  
  
###  <a name="set-options-on-the-specify-project-properties-page"></a><a name="projectProperties"></a> Establecer las opciones de la página Especificar propiedades del proyecto  
  
> [!NOTE]  
>   La página **Especificar propiedades del proyecto** solo está disponible cuando se ejecuta el asistente desde [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
 **Nombre del proyecto**  
 Muestra el nombre del proyecto.  
  
 **Nivel de protección**  
 Seleccione un nivel de protección para los paquetes que se incluyen en el proyecto. Para obtener más información acerca de los niveles de protección, vea [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Descripción del proyecto**  
 Escriba una descripción del proyecto opcional.  
  
###  <a name="set-options-on-the-update-execute-package-task-page"></a><a name="executePackage"></a> Establecer las opciones de la página Actualizar la tarea Ejecutar paquete  
 Actualice las tareas Ejecutar paquete que se incluyen en los paquetes para usar una referencia basada en proyectos. Para obtener más información, vea [Execute Package Task Editor](../control-flow/execute-package-task.md).  
  
 **Paquete primario**  
 Muestra el nombre del paquete que ejecuta el paquete secundario mediante la tarea Ejecutar paquete.  
  
 **Nombre de la tarea**  
 Muestra el nombre de la tarea Ejecutar paquete.  
  
 **Referencia original**  
 Muestra la ruta de acceso actual del paquete secundario.  
  
 **Asignar referencia**  
 Seleccione un paquete secundario que esté almacenado en el proyecto.  
  
###  <a name="set-options-on-the-select-configurations-page"></a><a name="configurations"></a> Establecer las opciones de la página Seleccionar configuraciones  
 Seleccione las configuraciones del paquete que desea reemplazar por parámetros.  
  
 **Paquete**  
 Muestra el archivo de paquete.  
  
 **Tipo**  
 Muestra el tipo de configuración como, por ejemplo, un archivo de configuración XML.  
  
 **Cadena de configuración**  
 Muestra la ruta de acceso del archivo de configuración.  
  
 **Estado**  
 Muestra un mensaje de estado para la configuración. Haga clic en el mensaje para ver el mensaje de texto completo.  
  
 **Agregar configuraciones**  
 Agregue configuraciones de paquetes que se incluyan en otros proyectos a la lista de valores de configuración disponibles que desee reemplazar con parámetros. Puede seleccionar las configuraciones almacenadas en un sistema de archivos o en SQL Server.  
  
 **Actualizar**  
 Haga clic para actualizar la lista de configuraciones.  
  
 **Quite las configuraciones de todos los paquetes después de la conversión**  
 Es recomendable que quite todas las configuraciones del proyecto seleccionando esta opción.  
  
 Si no selecciona esta opción, solo se quitan las configuraciones que haya elegido reemplazar por parámetros.  
  
###  <a name="set-options-on-the-create-parameters-page"></a><a name="createParameters"></a> Establecer las opciones de la página Crear parámetros  
 Seleccione el nombre y el ámbito de parámetro para cada propiedad de configuración.  
  
 **Paquete**  
 Muestra el archivo de paquete.  
  
 **Nombre de parámetro**  
 Muestra el nombre de parámetro.  
  
 **Ámbito**  
 Seleccione el ámbito del parámetro, un paquete o un proyecto.  
  
###  <a name="set-options-on-the-configure-parameters-page"></a><a name="configureParameters"></a> Establecer las opciones de la página Configurar parámetros  
 **Nombre**  
 Muestra el nombre de parámetro.  
  
 **Ámbito**  
 Muestra el ámbito del parámetro.  
  
 **Valor**  
 Muestra el valor del parámetro.  
  
 Haga clic en los puntos suspensivos junto al campo del valor para configurar las propiedades de parámetro.  
  
 En el cuadro de diálogo **Establecer detalles de parámetro** , puede modificar el valor del parámetro. También puede especificar si el valor del parámetro debe proporcionarse al ejecutar el paquete.  
  
 Puede modificar el valor en la página **Parámetros** del cuadro de diálogo **Configurar** de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]; para ello, haga clic en el botón Examinar situado junto al parámetro. Aparece el cuadro de diálogo **Establecer valor de parámetro** .  
  
 El cuadro de diálogo **Establecer detalles de parámetros** también muestra el tipo de datos de parámetro y el origen del parámetro.  
  
###  <a name="set-the-options-on-the-review-page"></a><a name="review"></a> Establecer las opciones en la página Revisar  
 Use la página **Revisar** para confirmar las opciones que ha seleccionado para la conversión del proyecto.  
  
 **Anterior**  
 Haga clic aquí para cambiar una opción.  
  
 **Convertir**  
 Haga clic en esta opción para convertir el proyecto al modelo de implementación de proyectos.  
  
###  <a name="set-the-options-on-the-perform-conversion"></a><a name="conversion"></a> Establecer las opciones en Realizar conversión  
 La página Realizar conversión muestra el estado de la conversión del proyecto.  
  
 **Acción**  
 Enumera un paso concreto de la conversión.  
  
 **Resultado**  
 Muestra el estado de cada paso de conversión. Haga clic en el mensaje de estado para obtener más información.  
  
 La conversión de proyectos no se guarda hasta que el proyecto se guarde en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
 **Guardar informe**  
 Haga clic en esta opción para guardar un resumen de la conversión del proyecto en un archivo .xml.