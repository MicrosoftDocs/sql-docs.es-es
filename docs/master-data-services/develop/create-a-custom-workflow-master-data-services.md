---
description: Crear un flujo de trabajo personalizado (Master Data Services)
title: Crear un flujo de trabajo personalizado
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 8e4403e9-595c-4b6b-9d0c-f6ae1b2bc99d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ce80f26d1df3a1059a94881889cfbc566c1a5412
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350087"
---
# <a name="create-a-custom-workflow-master-data-services"></a>Crear un flujo de trabajo personalizado (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] utiliza reglas de negocio para crear soluciones básicas de flujo de trabajo, como actualizar y validar datos automáticamente, y enviar notificaciones por correo electrónico en función de las condiciones que especifique. Cuando requiera un procesamiento más complejo que el que proporcionan las acciones de flujo de trabajo integradas, puede usar un flujo de trabajo personalizado. Un flujo de trabajo personalizado es un ensamblado de .NET creado por usted. Cuando se llama a su ensamblado de flujo de trabajo, el código puede realizar cualquier acción que requiera la situación. Por ejemplo, si su flujo de trabajo requiere un procesamiento de eventos más complejos, como aprobaciones de varios niveles o árboles de decisiones complejos, puede configurar [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] para que inicie un flujo de trabajo personalizado que analice los datos y determine dónde enviarlos para su aprobación.  
  
## <a name="how-custom-workflows-are-processed"></a>Cómo se procesan los flujos de trabajo personalizados  
 Hay tres componentes principales implicados en el procesamiento de flujos de trabajo personalizados: la aplicación web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], el servicio de integración de flujos de trabajo MDS de SQL Server y el ensamblado controlador del flujo de trabajo. Estos componentes procesan un flujo de trabajo personalizado de la manera siguiente:  
  
1.  Para validar una entidad que inicia un flujo de trabajo se usa [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].  
  
2.  [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] envía los miembros que satisfacen las condiciones de la regla de negocio a una cola de Service Broker en la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
3.  A intervalos regulares, el servicio de integración de flujos de trabajo MDS de SQL Server llama a un procedimiento almacenado en la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
4.  Cuando este procedimiento almacenado encuentra registros en la cola de Service Broker, los devuelve al servicio de integración de flujos de trabajo MDS de SQL Server.  
  
5.  El servicio de integración de flujos de trabajo MDS de SQL Server enruta los datos al ensamblado controlador del flujo de trabajo.  
  
> [!NOTE]  
>  Nota: el servicio de integración de flujos de trabajo MDS de SQL Server está diseñado para desencadenar procesos simples. Si su código personalizado requiere un procesamiento complejo, realice el procesamiento en un subproceso distinto o fuera del proceso de flujo de trabajo.  
  
## <a name="configure-master-data-services-for-custom-workflows"></a>Configurar Master Data Services para flujos de trabajo personalizados  
 Crear un flujo de trabajo personalizado requiere escribir código personalizado y configurar [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] para que pase los datos del flujo de trabajo al controlador de flujo de trabajo. Para habilitar el procesamiento del flujo de trabajo personalizado, siga estos pasos:  
  
1.  Cree un ensamblado .NET que implemente [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender] (/Previous-Versions/SQL/SQL-Server-2016/hh758785 (v = SQL. 130).  
  
2.  Configure el servicio de integración de flujos de trabajo MDS de SQL Server para que se conecte a la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] y para asociar una etiqueta a su controlador de flujo de trabajo.  
  
3.  Inicie el servicio de integración de flujos de trabajo MDS de SQL Server.  
  
4.  Cree una regla de negocio en [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] que inicie un flujo de trabajo etiquetado con el nombre de su controlador de flujo de trabajo.  
  
5.  Aplique la regla de negocio a un miembro que desencadene el flujo de trabajo personalizado.  
  
### <a name="create-the-workflow-handler-assembly"></a>Crear el ensamblado controlador del flujo de trabajo  
 Un flujo de trabajo personalizado es un ensamblado de biblioteca de clases .NET que implementa la interfaz [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) . SQL Server servicio de integración de flujos de trabajo de MDS llama al método [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender. StartWorkflow *](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130))  para ejecutar el código. Para obtener un ejemplo de código que implementa [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender. StartWorkflow *](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130)) , vea [ejemplo de flujo de trabajo personalizado &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md).  
  
 Siga estos pasos para utilizar Visual Studio 2010 para crear un ensamblado al que el servicio de integración de flujos de trabajo MDS de SQL Server pueda llamar para controlar un flujo de trabajo personalizado:  
  
1.  En Visual Studio 2010, cree un proyecto **Biblioteca de clases** que use el idioma que quiera. Para crear una biblioteca de clases de C#, seleccione los tipos de proyecto **Visual C#\Windows** y la plantilla **Biblioteca de clases**. Escriba un nombre para el proyecto (por ejemplo, **MDSWorkflowTest**) y haga clic en **Aceptar**.  
  
2.  Agregue una referencia a Microsoft.MasterDataServices.WorkflowTypeExtender.dll. Este ensamblado se puede encontrar en \<Your installation folder> \Master Data Services\WebApplication\bin.  
  
3.  Agregue "using Microsoft.MasterDataServices.Core.Workflow;" al archivo de código de C#.  
  
4.  Herede de [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) en la declaración de clase. La declaración de clase debe ser similar a: "public class WorkflowTester : IWorkflowTypeExtender".  
  
5.  Implemente la interfaz [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) . SQL Server servicio de integración de flujos de trabajo de MDS llama al método [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender. StartWorkflow *](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130))  para iniciar el flujo de trabajo.  
  
6.  Copie el ensamblado en la ubicación del SQL Server archivo ejecutable del servicio de integración de flujos de trabajo MDS, denominado Microsoft.MasterDataServices.Workflow.exe, en \<Your installation folder> \Master Data Services\WebApplication\bin.  
  
### <a name="configure-sql-server-mds-workflow-integration-service"></a>Configurar el servicio de integración de flujos de trabajo MDS de SQL Server  
 Edite el archivo de configuración de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] para incluir la información de conexión de su base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] y asociar una etiqueta al ensamblado controlador del flujo de trabajo siguiendo estos pasos:  
  
1.  Busque Microsoft.MasterDataServices.Workflow.exe.config en \<Your installation folder> \Master Data Services\WebApplication\bin.  
  
2.  Agregue la información de conexión de la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] al valor "ConnectionString". Si su instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa la intercalación con distinción de mayúsculas y minúsculas, el nombre de la base de datos debe especificarse con el mismo uso de mayúsculas y minúsculas que el de la base de datos. Por ejemplo, la etiqueta de configuración completa debe ser similar a la siguiente:  
  
    ```xml  
    <setting name="ConnectionString" serializeAs="String">  
        <value>Server=myServer;Database=myDatabase;Integrated Security=True</value>  
    </setting>  
    ```  
  
3.  Debajo del valor "ConnectionString" agregue un valor "WorkflowTypeExtenders" para asociar un nombre de etiqueta al ensamblado controlador del flujo de trabajo. Por ejemplo:  
  
    ```xml  
    <setting name="WorkflowTypeExtenders" serializeAs="String">  
        <value>TEST=MDSWorkflowTestLib.WorkflowTester, MDSWorkflowTestLib</value>  
    </setting>  
    ```  
  
     El texto interno de la \<value> etiqueta tiene el formato \<Workflow tag> = \<assembly-qualified workflow type name> . \<Workflow tag> es un nombre que se usa para identificar el ensamblado del controlador de flujo de trabajo cuando se crea una regla de negocios en [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . \<assembly-qualified workflow type name> es el nombre completo del espacio de nombres de la clase de flujo de trabajo, seguido de una coma, seguido del nombre para mostrar del ensamblado. Si su ensamblado tiene un nombre seguro, tendrá que incluir también la información de versión y su PublicKeyToken. Puede incluir varias \<setting> etiquetas si ha creado varios controladores de flujo de trabajo para distintos tipos de flujos de trabajo.  
  
> [!NOTE]  
>  En función de la configuración del servidor, puede que aparezca un error "Acceso denegado" al intentar guardar el archivo Microsoft.MasterDataServices.Workflow.exe.config. En tal caso, deshabilite temporalmente Control de cuentas de usuario (UAC) en el servidor. Para ello, abra el Panel de control y haga clic en **Sistema y seguridad**. En **Centro de actividades**, haga clic en **Cambiar configuración de Control de cuentas de usuario**. En el cuadro de diálogo **Configuración del Control de cuentas de usuario**, desplace la barra hasta abajo para no recibir nunca ninguna notificación. Reinicie su equipo y repita el procedimiento anterior para editar el archivo de configuración. Después de guardar el archivo, restaure la configuración de UAC al nivel predeterminado.  
  
### <a name="start-sql-server-mds-workflow-integration-service"></a>Iniciar el servicio de integración de flujos de trabajo MDS de SQL Server  
 De manera predeterminada, el servicio de integración de flujos de trabajo MDS de SQL Server no se instala. Debe instalar el servicio para poder usarlo. Para mayor seguridad, cree un usuario local para el servicio y conceda a este usuario sólo los permisos necesarios para realizar operaciones de flujo de trabajo. Para crear un usuario, instalar el servicio e iniciarlo, siga estos pasos:  
  
1.  Utilice el administrador Usuarios y grupos locales para crear un usuario local con nombre, por ejemplo, mds_workflow_service.  
  
2.  Utilice SQL Server Management Studio para conceder permiso al usuario mds_workflow_service para ejecutar el procedimiento almacenado [mdm].[udpExternalActionsGet]. Para ello, cree un nuevo inicio de sesión para la cuenta de mds_workflow_service, cree un nuevo usuario en la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)], asigne el usuario al inicio de sesión de mds_workflow_service y conceda al usuario permiso EXECUTE al procedimiento almacenado [mdm].[udpExternalActionsGet].  
  
3.  Conceda al usuario mds_workflow_service permiso para ejecutar el ensamblado controlador del flujo de trabajo. Para ello, agregue el usuario mds_workflow_service a la pestaña **Seguridad** de las **propiedades** del ensamblado controlador del flujo de trabajo y conceda al usuario mds_workflow_service los permisos READ y EXECUTE.  
  
4.  Conceda al usuario mds_workflow_service permiso para ejecutar el archivo ejecutable del servicio de integración de flujos de trabajos MDS de SQL Server. Para ello, agregue el usuario mds_workflow_service a la pestaña **seguridad** de las **propiedades** de Microsoft.MasterDataServices.Workflow.exe, en \<Your installation folder> \Master Data Services\WebApplication\bin y conceda el permiso de lectura y ejecución del usuario mds_workflow_service.  
  
5.  Instale el servicio de integración de flujos de trabajo MDS de SQL Server mediante la utilidad de instalación de .NET, denominada InstallUtil.exe. InstallUtil.exe se encuentra en la carpeta de instalación de .NET (por ejemplo, C:\Windows\Microsoft.NET\Framework\v4.0.30319\\). Instale el servicio de integración de flujos de trabajo MDS de SQL escribiendo lo siguiente en el símbolo del sistema con permisos elevados:  
  
    ```  
    C:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil Microsoft.MasterDataServices.Workflow.exe  
    ```  
  
     Especifique el usuario mds_workflow_service cuando se le solicite durante la instalación.  
  
6.  Inicie el servicio de integración de flujos de trabajo MDS de SQL Server mediante el complemento Servicios. Para ello, busque el servicio de integración de flujos de trabajo MDS de SQL Server en el complemento Servicios, selecciónelo y haga clic en el vínculo **Iniciar**.  
  
### <a name="create-a-workflow-business-rule"></a>Crear una regla de negocio del flujo de trabajo  
 Utilice [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] para crear y publicar una regla de negocio que inicie el flujo de trabajo cuando corresponda. Debe asegurarse de que la regla de negocio contiene las acciones que cambian los valores de atributo, de modo que la regla se evalúe como false una vez aplicada. Por ejemplo, la regla de negocio puede evaluarse como true cuando un valor del atributo Precio sea superior a 500 y el valor del atributo Aprobado esté en blanco. La regla puede incluir dos acciones: una para establecer el valor del atributo Aprobado en Pendiente y otra para iniciar el flujo de trabajo. También puede crear una regla que use la condición "ha cambiado" y agregar los atributos a los grupos de seguimiento de cambios. Para más información sobre las reglas de negocios, vea [Reglas de negocios &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md).  
  
 Cree una regla de negocio que inicie un flujo de trabajo personalizado en [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] siguiendo estos pasos:  
  
1.  En el editor de reglas de negocio de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], una vez especificadas las condiciones de la regla de negocio, arrastre la acción **Iniciar flujo de trabajo** de la lista **Acciones externas** a la etiqueta **Acción** del panel **THEN**.  
  
2.  En el panel **Editar acción**, en el cuadro **Tipo de flujo de trabajo**, escriba la etiqueta que identifica el ensamblado controlador del flujo de trabajo. Esta es la etiqueta que especificó en el archivo de configuración del ensamblado (por ejemplo, PRUEBA).  
  
3.  Opcionalmente, active la casilla **Incluir datos de miembro**. Active esta casilla para incluir nombres y valores de atributo en el XML que se pasa al controlador del flujo de trabajo.  
  
4.  En el cuadro **Sitio de flujo de trabajo**, escriba el nombre de un sitio web. Puede que esto no se aplique a su flujo de trabajo personalizado, pero se puede usar para agregar contexto.  
  
5.  En el cuadro **Nombre de flujo de trabajo**, escriba el nombre del flujo de trabajo de Visual Studio. Puede que esto no se aplique a su flujo de trabajo personalizado, pero se puede usar para agregar contexto.  
  
6.  Guarde y publique la regla de negocio.  
  
### <a name="apply-business-rules-to-start-a-workflow"></a>Aplicar las reglas de negocio para iniciar un flujo de trabajo  
 Aplique la regla de negocio a sus datos para iniciar el flujo de trabajo. Para ello, use [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] para editar la entidad que contiene los miembros que desea validar. Haga clic en **Aplicar reglas de negocios**. En respuesta a la regla de negocio, [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] rellena la cola de Service Broker de la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Cuando el servicio de integración de flujos de trabajo MDS de SQL Server compruebe la cola, enviará los datos al ensamblado controlador del flujo de trabajo especificado y borrará la cola. El ensamblado controlador del flujo de trabajo realiza todas las acciones que se hayan codificado en él.  
  
## <a name="troubleshoot-custom-workflows"></a>Solución de problemas de flujos de trabajo personalizados  
 Si el ensamblado controlador del flujo de trabajo no recibe datos, puede intentar depurar el servicio de integración de flujos de trabajo MDS de SQL Server o examinar la cola de Service Broker.  
  
### <a name="debug-sql-server-mds-workflow-integration-service"></a>Depurar el servicio de integración de flujos de trabajo MDS de SQL Server  
 Para depurar el servicio de integración de flujos de trabajo de SQL Server, siga estos pasos:  
  
1.  Utilice el complemento Servicios para detener el servicio.  
  
2.  Abra un símbolo del sistema, vaya a la ubicación del servicio y ejecute el servicio en modo de consola especificando: Microsoft.MasterDataServices.Workflow.exe -console.  
  
3.  En [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], actualice el miembro y aplique de nuevo las reglas de negocio. En la ventana de la consola se muestran registros detallados.  
  
### <a name="view-the-service-broker-queue"></a>Examinar la cola de Service Broker  
 La cola de Service Broker que contiene los datos maestros pasados como parte del flujo de trabajo es: mdm.microsoft/mdm/queue/externalaction. Las colas se encuentran en el **Explorador de objetos** de SQL Management Studio bajo el nodo Service Broker de la base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Tenga en cuenta que si el servicio borró la cola correctamente, la cola estará vacía.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de flujo de trabajo personalizado &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md)   
 [Descripción del código XML de flujo de trabajo personalizado &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-xml-description.md)  
  
  
