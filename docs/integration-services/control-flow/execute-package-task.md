---
description: Tarea Ejecutar paquete
title: Tarea Ejecutar paquete | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executepackagetask.f1
- sql13.dts.designer.executepackagetask.package.f1
- sql13.dts.designer.executepackagetask.parameter.F1
- sql13.dts.designer.executepackagetask.general.f1
helpviewer_keywords:
- Execution Package task [Integration Services]
- child packages
- parent packages [Integration Services]
ms.assetid: 042d4ec0-0668-401c-bb3a-a25fe2602eac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8251f2ff30fafff584b4f56fe93a9b7423ea412b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352083"
---
# <a name="execute-package-task"></a>Tarea Ejecutar paquete

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Ejecutar paquete amplía las capacidades empresariales de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ya que permite que los paquetes ejecuten otros paquetes como parte de un flujo de trabajo.  
  
 Puede usar la tarea Ejecutar paquete para los siguientes fines:  
  
-   Descomponer un flujo de trabajo de paquetes complejo. Esta tarea permite descomponer el flujo de trabajo en varios paquetes, más fáciles de leer, probar y mantener. Por ejemplo, si está cargando datos en un esquema de estrella, puede crear un paquete independiente para llenar cada dimensión y la tabla de hechos.  
  
-   Reutilizar partes de paquetes. Otros paquetes pueden reutilizar partes de un flujo de trabajo de paquete. Por ejemplo, es posible generar un módulo de extracción de datos al que se pueda llamar desde diferentes paquetes. Cada paquete que llama al módulo de extracción puede realizar distintas operaciones de limpieza, filtrado o agregación de datos.  
  
-   Agrupar unidades de trabajo. Las unidades de trabajo pueden encapsularse en paquetes independientes y combinarse como componentes transaccionales con el flujo de trabajo de un paquete primario. Por ejemplo, el paquete primario ejecuta los paquetes accesorios y, en función del resultado de la ejecución de los paquetes de accesorios, confirma o revierte la transacción.  
  
-   Controlar la seguridad de los paquetes. Los autores de paquetes necesitan tener acceso a solo una parte de una solución de varios paquetes. Al separar un paquete en varios paquetes, puede proporcionar un mayor nivel de seguridad, ya que puede conceder a un autor acceso únicamente a los paquetes relevantes.  
  
 Un paquete que ejecuta otros paquetes se suele denominar paquete primario y los paquetes ejecutados por un flujo de trabajo principal se denominan paquetes secundarios.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye tareas que realizan operaciones de flujo de trabajo, como ejecutar archivos ejecutables y por lotes. Para obtener más información, vea [Execute Process Task](../../integration-services/control-flow/execute-process-task.md).  
  
## <a name="running-packages"></a>Ejecutar paquetes  
 La tarea Ejecutar paquete puede ejecutar paquetes secundarios incluidos en el mismo proyecto que contiene el paquete primario. Para seleccionar un paquete secundario del proyecto, establezca la propiedad **ReferenceType** en **Referencia de proyecto** y, a continuación, establezca la propiedad **PackageNameFromProjectReference** .  
  
> [!NOTE]  
>  La opción **ReferenceType** es de solo lectura y se establece en **Referencia externa** si el proyecto que contiene el paquete no se ha convertido al modelo de implementación de proyectos. [Implementación de proyectos y paquetes de Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 La tarea Ejecutar paquete además puede ejecutar paquetes almacenados en la base de datos msdb de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y paquetes almacenados en el sistema de archivos. La tarea usa un administrador de conexiones OLE DB para conectar con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un administrador de conexiones de archivos para tener acceso al sistema de archivos. Para obtener más información, vea [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) y [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La tarea Ejecutar paquete también puede ejecutar un plan de mantenimiento de bases de datos, que permite administrar paquetes de [!INCLUDE[ssIS](../../includes/ssis-md.md)] y planes de mantenimiento de bases de datos en la misma solución [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Un plan de mantenimiento de bases de datos es similar a un paquete de [!INCLUDE[ssIS](../../includes/ssis-md.md)] , pero un plan solo puede incluir tareas de mantenimiento de bases de datos y siempre se almacena en la base de datos msdb.  
  
 Si elige un paquete almacenado en el sistema de archivos, debe proporcionar el nombre y la ubicación del paquete. El paquete puede residir en cualquier parte del sistema de archivos; no tiene que estar en la misma carpeta que el paquete primario.  
  
 El paquete secundario puede ejecutarse en el proceso del paquete primario o ejecutarse en su propio proceso. La ejecución del paquete secundario en su propio proceso requiere más memoria, pero ofrece mayor flexibilidad. Por ejemplo, si se produce un error del proceso secundario, el proceso primario podrá seguir ejecutándose.  
  
 Sin embargo, a veces puede desearse que el paquete primario y los paquetes secundarios no completen su ejecución como una unidad. También es posible que no desee agregar la sobrecarga de otro proceso. Por ejemplo, si se produce un error en un proceso secundario y el procesamiento posterior del proceso primario del paquete depende de que el proceso secundario se complete correctamente, el paquete secundario deberá ejecutarse en el proceso del paquete primario.  
  
 De forma predeterminada, la propiedad ExecuteOutOfProcess de la tarea Ejecutar paquete se establece en **False** y el paquete secundario se ejecuta en el mismo proceso que el paquete primario. Si establece esta propiedad en **True**, el paquete secundario se ejecuta en un proceso independiente. Esto puede ralentizar el inicio del paquete secundario. Además, si establece la propiedad en **True**, no se podrá depurar el paquete en una instalación de solo herramientas. Debe instalar [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Para más información, vea [Instalar Integration Services](../../integration-services/install-windows/install-integration-services.md)  
  
## <a name="extending-transactions"></a>Extender transacciones  
 La transacción utilizada por el paquete primario puede ampliarse al paquete secundario; por tanto, puede confirmarse o revertirse el trabajo realizado por ambos paquetes. Por ejemplo, las inserciones en la base de datos realizadas por el paquete primario pueden confirmarse o revertirse en función de las inserciones en la base de datos realizadas por el paquete secundario y viceversa. Para obtener más información, vea [Inherited Transactions](../integration-services-transactions.md).  
  
## <a name="propagating-logging-details"></a>Propagar detalles de registro  
 El paquete secundario ejecutado por la tarea Ejecutar paquete puede estar configurado o no para usar registro, pero el paquete secundario siempre reenviará los detalles de registro al paquete primario. Si la tarea Ejecutar paquete está configurada para usar registro, la tarea registra los detalles del paquete secundario. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="passing-values-to-child-packages"></a>Pasar valores a paquetes secundarios  
 Frecuentemente, un paquete secundario utiliza valores que le pasa otro paquete que lo llama (generalmente el paquete primario). Utilizar valores de un paquete primario es útil en escenarios como el siguiente:  
  
-   Se asignan partes de un flujo de trabajo más grande a paquetes diferentes. Por ejemplo, un paquete descarga datos todas las noches, resume los datos, asigna valores de datos de resumen a variables y después pasa los valores a otro paquete para un procesamiento adicional de los datos.  
  
-   El paquete primario coordina dinámicamente las tareas de un paquete secundario. Por ejemplo, el paquete primario determina el número de días del mes actual y asigna el número a una variable, y el paquete secundario realiza una tarea ese número de veces.  
  
-   Un paquete secundario requiere acceso a datos derivados dinámicamente por el paquete primario. Por ejemplo, el paquete primario extrae datos de una tabla y carga el conjunto de filas en una variable y el paquete secundario realiza operaciones adicionales en los datos.  
  
 Puede utilizar los métodos siguientes para pasar valores a un paquete secundario:  
  
-   **Configuraciones de paquetes**  
  
     [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] proporciona un tipo configuración, la configuración de variable de paquete primario, para pasar valores de paquetes primarios a paquetes secundarios. La configuración se crea en el paquete secundario y utiliza una variable de paquete primario. Se asigna la configuración a una variable del paquete secundario o a la propiedad de un objeto en el paquete secundario. También se puede usar la variable en los scripts usados por la tarea Script o el componente de script.  
  
-   **Parámetros**  
  
     Puede configurar la tarea Ejecutar paquete para asignar variables o parámetros del paquete primario o bien, parámetros del proyecto a los parámetros del paquete secundario. El proyecto debe utilizar el modelo de implementación del proyecto y el paquete secundario debe estar incluido en el mismo proyecto que contiene el paquete primario. Para obtener más información, vea [Execute Package Task Editor]().  
  
    > [!NOTE]  
    >  Si el parámetro del paquete secundario no es confidencial y se asigna a un parámetro primario que sea confidencial, el paquete secundario no podrá ejecutarse.  
    >   
    >  Se admiten las siguientes condiciones de asignación:  
    >   
    >  Un parámetro de paquete secundario confidencial se asigna a un parámetro principal confidencial  
    >   
    >  Un parámetro de paquete secundario confidencial se asigna a un parámetro principal no confidencial  
    >   
    >  Un parámetro de paquete secundario no confidencial se asigna a un parámetro principal no confidencial  
  
 La variable de paquete primario puede definirse en el ámbito de la tarea Ejecutar paquete o en un contenedor primario como el paquete. Si hay varias variables con el mismo nombre, se usa la variable definida en el ámbito de la tarea Ejecutar paquete o la variable más cercana en el ámbito a la tarea.  
  
 Para más información, vea [Usar los valores de variables y parámetros en un paquete secundario](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
### <a name="accessing-parent-package-variables"></a>Obtener acceso a variables de paquetes primarios  
 Los paquetes secundarios pueden tener acceso a variables de paquetes primarios mediante la tarea Script. Al escribir el nombre de la variable del paquete primario en la página **Script** del **Editor de la tarea Script**, no incluya **Usuario:** en el nombre de la variable. De lo contrario, el paquete secundario no podrá encontrar la variable al ejecutar el paquete primario.  
  
## <a name="configuring-the-execute-package-task"></a>Configurar la tarea Ejecutar paquete  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Para obtener más información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Página Expresiones](../../integration-services/expressions/expressions-page.md)  
  
 Para obtener más información sobre cómo establecer estas propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el siguiente tema:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="configuring-the-execute-package-task-programmatically"></a>Configurar la tarea Ejecutar paquete mediante programación  
 Para obtener más información sobre cómo establecer estas propiedades mediante programación, haga clic en el tema siguiente:  
  
-   [N:Microsoft.SqlServer.Dts.Tasks.ExecutePackageTask](https://technet.microsoft.com/library/microsoft.sqlserver.dts.tasks.executepackagetask\(v=sql.110\).aspx)  
  
## <a name="execute-package-task-editor"></a>Editor de la tarea Ejecutar paquete
  Utilice el Editor de la tarea Ejecutar paquete para configurar la tarea Ejecutar paquete. La tarea Ejecutar paquete amplía las capacidades empresariales de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ya que permite que los paquetes ejecuten otros paquetes como parte de un flujo de trabajo.  
  
 **¿Qué desea hacer?**  
  
-   [Abrir el Editor de la tarea Ejecutar paquete](#open)  
  
-   [Establecer las opciones de la página General](#general)  
  
-   [Establecer las opciones de la página Paquete](#package)  
  
-   [Establecer las opciones de la página Enlaces de parámetro](#parameter)  
  
###  <a name="open-the-execute-package-task-editor"></a><a name="open"></a> Abrir el Editor de la tarea Ejecutar paquete  
  
1.  Abra un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] que contenga una tarea Ejecutar paquete.  
  
2.  Haga clic con el botón derecho en la tarea en el Diseñador SSIS y, después, haga clic en **Editar**.  
  
###  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> Establecer las opciones de la página General  
 **Nombre**  
 Escriba un nombre único para la tarea Ejecutar paquete. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
> [!NOTE]  
>  Los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea Ejecutar paquete.  
  
###  <a name="set-the-options-on-the-package-page"></a><a name="package"></a> Establecer las opciones de la página Paquete  
 **ReferenceType**  
 Seleccione **Referencia de proyecto** para los paquetes secundarios que están en el proyecto. Seleccione **Referencia externa** para los paquetes secundarios que se encuentran fuera del paquete  
  
> [!NOTE]  
>  La opción **ReferenceType** es de solo lectura y se establece en **Referencia externa** si el proyecto que contiene el paquete no se ha convertido al modelo de implementación de proyectos. [Implementación de proyectos y paquetes de Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 **Contraseña**  
 Si el paquete secundario está protegido con contraseña, tiene que proporcionar esta contraseña o hacer clic en el botón de puntos suspensivos (…) y crear una contraseña.  
  
 **ExecuteOutOfProcess**  
 Especifique si el paquete secundario se ejecuta en el proceso del paquete primario o en un proceso independiente. De forma predeterminada, la propiedad ExecuteOutOfProcess de la tarea Ejecutar paquete se establece en **False** y el paquete secundario se ejecuta en el mismo proceso que el paquete primario. Si establece esta propiedad en **true**, el paquete secundario se ejecuta en un proceso independiente. Esto puede ralentizar el inicio del paquete secundario. Además, si establece la propiedad en **true**, no podrá depurar el paquete si ha realizado una instalación de solo herramientas (tendrá que instalar el producto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ). Para más información, vea [Instalar Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
#### <a name="referencetype-dynamic-options"></a>Opciones dinámicas de ReferenceType  
  
##### <a name="referencetype--external-reference"></a>ReferenceType = Referencia externa  
 **Ubicación**  
 Seleccione la ubicación del paquete secundario. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**SQL Server**|Establezca la ubicación de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Sistema de archivos**|Permite establecer como ubicación el sistema de archivos.|  
  
 **Connection**  
 Seleccione el tipo de ubicación de almacenamiento para el paquete secundario.  
  
 **PackageNameReadOnly**  
 Muestra el nombre del paquete.  
  
##### <a name="referencetype--project-reference"></a>ReferenceType = Referencia de proyecto  
 **PackageNameFromProjectReference**  
 Seleccione un paquete incluido en el proyecto para que sea el paquete secundario.  
  
#### <a name="location-dynamic-options"></a>Opciones dinámicas de ubicación  
  
##### <a name="location--sql-server"></a>Ubicación = SQL Server  
 **Connection**  
 Seleccione un administrador de conexiones OLE DB de la lista o haga clic en \<**New connection...**> para crear un administrador de conexiones.  
  
 **Temas relacionados:** [Administrador de conexiones OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **PackageName**  
 Escriba el nombre del paquete secundario, o bien haga clic en los puntos suspensivos (…) y, después, busque el paquete.  
  
##### <a name="location--file-system"></a>Ubicación = Sistema de archivos  
 **Connection**  
 Seleccione un administrador de conexiones de archivos de la lista o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **PackageNameReadOnly**  
 Muestra el nombre del paquete.  
  
###  <a name="set-the-options-on-the-parameter-bindings-page"></a><a name="parameter"></a> Establecer las opciones de la página Enlaces de parámetro  
 Puede pasar valores del paquete primario o del proyecto al paquete secundario. El proyecto debe utilizar el modelo de implementación del proyecto y el paquete secundario debe estar incluido en el mismo proyecto que contiene el paquete primario.  
  
 Para obtener información sobre cómo convertir proyectos al modelo de implementación de proyectos, vea [Implementación de proyectos y paquetes de Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 **Parámetro de paquete secundario**  
 Escriba o seleccione un nombre para el parámetro de paquete secundario.  
  
 **Enlazar un parámetro o variable**  
 Seleccione el parámetro o variable que contiene el valor que desea pasar al paquete secundario.  
  
 **Add (Agregar)**  
 Haga clic para asignar un parámetro o variable a un parámetro de paquete secundario.  
  
 **Remove**  
 Haga clic para quitar una asignación entre un parámetro o variable y un parámetro de paquete secundario.  
  
