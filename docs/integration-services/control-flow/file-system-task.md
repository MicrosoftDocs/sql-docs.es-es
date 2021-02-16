---
description: Tarea Sistema de archivos
title: Tarea Sistema de archivos | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.filesystemtask.f1
- sql13.dts.designer.filesystemtask.general.f1
helpviewer_keywords:
- File System task [Integration Services]
ms.assetid: 7dd79a6a-e066-4028-a385-1d40f31056f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 51646edb5b2937d04d4915e2a7d4a66327343a6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345708"
---
# <a name="file-system-task"></a>Tarea Sistema de archivos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Sistema de archivos realiza operaciones en archivos y directorios del sistema de archivos. Por ejemplo, un paquete puede utilizar la tarea Sistema de archivos para crear, mover o eliminar directorios y archivos. También puede utilizar la tarea Sistema de archivos para establecer atributos en archivos y directorios. Por ejemplo, la tarea Sistema de archivos puede convertir los archivos en archivos ocultos o de solo lectura.  
  
 Todas las operaciones de la tarea Sistema de archivos usan un origen, que puede ser un archivo o un directorio. Por ejemplo, el archivo que la tarea copia o el directorio que elimina es un origen. El origen puede especificarse mediante un administrador de conexiones de archivos que señala al directorio o archivo, o proporcionando el nombre de una variable que contiene la ruta de origen. Para más información, vea [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md) e [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Las operaciones que copian y mueven archivos y directorios, y cambian nombres de archivos, usan un destino y un origen. El destino se especifica mediante un administrador de conexiones de archivos o una variable. Las operaciones de la tarea Sistema de archivos pueden configurarse para permitir la sobrescritura de los archivos y directorios de destino. La operación que crea un nuevo directorio puede configurarse para usar un directorio existente que tiene el nombre especificado en lugar de generar un error si el directorio ya existe.  
  
## <a name="predefined-file-system-operations"></a>Operaciones de sistema de archivos predefinidas  
 La tarea Sistema de archivos incluye un conjunto predefinido de operaciones. Estas operaciones se describen en la siguiente tabla.  
  
|Operación|Descripción|  
|---------------|-----------------|  
|Copiar directorio|Copia una carpeta de una ubicación a otra.|  
|Copiar archivo|Copia un archivo de una ubicación a otra.|  
|Creación del directorio|Crea una carpeta en una ubicación especificada.|  
|Eliminar directorio|Elimina una carpeta de una ubicación especificada.|  
|Eliminar contenido de directorio|Elimina todos los archivos y carpetas de una carpeta.|  
|Eliminar archivo|Elimina un archivo de una ubicación especificada.|  
|Mover directorio|Mueve una carpeta de una ubicación a otra.|  
|Mover archivo|Mueve un archivo de una ubicación a otra.|  
|Cambiar nombre de archivo|Cambia el nombre de un archivo de una ubicación especificada.|  
|Definir atributos|Establece atributos de archivos y carpetas. Estos atributos son Archivar, Oculto, Normal, Solo lectura y Sistema. El atributo Normal indica que no hay atributos establecidos; no se puede combinar con otros atributos. Todos los demás atributos se pueden usar en combinación.|  
  
 La tarea Sistema de archivos trabaja con un solo archivo o directorio. Por consiguiente, esta tarea no admite el uso de caracteres comodín para realizar la misma operación en varios archivos. Para hacer que la tarea Sistema de archivos realice una determinada operación sobre varios archivos o directorios, coloque la tarea Sistema de archivos en un contenedor de bucles Foreach, como se describe en los pasos siguientes.  
  
-   **Configure el contenedor de bucles Foreach** En la página **Colección** del Editor de bucles Foreach, establezca el enumerador en **Enumerador de archivos para Foreach** y escriba la expresión comodín como configuración del enumerador para **Archivos**. En la página **Asignaciones de variables** del Editor de bucles Foreach, asigne una variable que desee utilizar para pasar los nombres de archivo a la tarea Sistema de archivos de uno en uno.  
  
-   **Agregue y configure una tarea Sistema de archivos** Agregue una tarea Sistema de archivos al contenedor de bucles Foreach. En la página **General** del Editor de la tarea sistema de archivos, establezca la propiedad **SourceVariable** o **DestinationVariable** en la variable que ha definido en el contenedor de bucles Foreach.  
  
## <a name="custom-log-entries-available-on-the-file-system-task"></a>Entradas del registro personalizadas disponibles en la tarea Sistema de archivos  
 La siguiente tabla contiene las entradas de registro personalizadas para la tarea Sistema de archivos. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**FileSystemOperation**|Informa sobre la operación que realiza la tarea. La entrada del registro se escribe cuando se inicia la operación del sistema de archivos e incluye información sobre el origen y el destino.|  
  
## <a name="configuring-the-file-system-task"></a>Configurar la tarea Sistema de archivos  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Para obtener más información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vea los temas siguientes:  
  
-   [Editor de la tarea Sistema de archivos &#40;página General&#41;]()  
  
-   [Página Expresiones](../../integration-services/expressions/expressions-page.md)  
  
 Para obtener más información sobre cómo establecer estas propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vea el siguiente tema:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
 Para obtener más información sobre cómo establecer estas propiedades mediante programación , vea el siguiente tema:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye una tarea que descarga y carga archivos de datos, y administra directorios en servidores. Para más información, consulte [FTP Task](../../integration-services/control-flow/ftp-task.md).  
  
## <a name="file-system-task-editor-general-page"></a>Editor de la tarea Sistema de archivos (página General)
  Use la página **General** del cuadro de diálogo **Editor de la tarea Sistema de archivos** para configurar la operación del sistema de archivos que lleva a cabo la tarea.  
  
 Debe especificar un administrador de conexiones de origen y destino mediante las propiedades SourceConnection y DestinationConnection. Puede proporcionar los nombres de los administradores de conexión de archivos que señalan a los archivos que utiliza la tarea como origen o destino, o bien proporcionar los nombres de las variables si las rutas de acceso de los archivos están almacenadas en variables. Si quiere usar variables para almacenar rutas de acceso de archivo, primero debe establecer la opción IsSourcePathVariable para la conexión de origen y la opción IsDestinationPatheVariable para la conexión de destino en **True**. A continuación, puede elegir las variables definidas por el usuario o del sistema existentes que se utilizarán, o bien puede crear nuevas variables. En el cuadro de diálogo **Agregar variable** , puede configurar y especificar el ámbito de las variables. El ámbito debe ser la tarea Sistema de archivos o un contenedor principal. Para obtener más información, vea [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) y [Usar variables en paquetes](../integration-services-ssis-variables.md).  
  
> [!NOTE]  
>  Para invalidar las variables seleccionadas para las propiedades **SourceConnection** y **DestinationConnection** , especifique una expresión para las propiedades **Source** y **Destination** . Las expresiones se especifican en la página **Expresiones** del **Editor de la tarea Sistema de archivos**. Por ejemplo, para establecer la ruta de acceso de los archivos que usa la tarea como destino, tal vez desee utilizar una variable A en determinadas condiciones y una variable B, en otras.  
  
> [!NOTE]  
>  La tarea Sistema de archivos trabaja con un solo archivo o directorio. Por consiguiente, esta tarea no admite el uso de caracteres comodín para realizar la misma operación en varios archivos o directorios. Para hacer que la tarea Sistema de archivos realice una determinada operación sobre varios archivos o directorios, coloque la tarea Sistema de archivos en un contenedor de bucles Foreach. Para más información, consulte [File System Task](../../integration-services/control-flow/file-system-task.md).  
  
 Puede usar expresiones para utilizar distintas variables para  
  
### <a name="options"></a>Opciones  
 **IsDestinationPathVariable**  
 Indica si la ruta de acceso de destino está almacenada en una variable. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**True**|La ruta de destino está almacenada en una variable. Al seleccionar este valor se muestra la opción dinámica, **DestinationVariable**.|  
|**False**|La ruta de destino se especifica en un administrador de conexiones de archivos. Al seleccionar este valor se muestra la opción dinámica, **DestinationConnection**.|  
  
 **OverwriteDestination**  
 Especifique si la operación puede sobrescribir archivos en el directorio de destino.  
  
 **Nombre**  
 Proporcione un nombre único para la tarea Sistema de archivos. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
> [!NOTE]  
>  Los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea Sistema de archivos.  
  
 **operación**  
 Seleccione la operación del sistema de archivos que se debe llevar a cabo. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Copiar directorio**|Copie un directorio. Al seleccionar este valor se muestran las opciones dinámicas para un origen y un destino.|  
|**Copiar archivo**|Copie un archivo. Al seleccionar este valor se muestran las opciones dinámicas para un origen y un destino.|  
|**Crear directorio**|Cree un directorio. Al seleccionar este valor se muestran las opciones dinámicas para un directorio de origen y destino.|  
|**Eliminar directorio**|Elimine un directorio. Al seleccionar este valor se muestran las opciones dinámicas para un origen.|  
|**Eliminar contenido de directorio**|Elimine el contenido de un directorio. Al seleccionar este valor se muestran las opciones dinámicas para un origen.|  
|**Eliminar archivo**|Elimine un archivo. Al seleccionar este valor se muestran las opciones dinámicas para un origen.|  
|**Mover directorio**|Mueva un directorio. Al seleccionar este valor se muestran las opciones dinámicas para un origen y un destino.|  
|**Mover archivo**|Mueva un archivo. Al seleccionar este valor se muestran las opciones dinámicas para un origen y un destino. Al mover un archivo, no incluya el nombre del archivo en la ruta de acceso del directorio que proporcione como destino.|  
|**Cambiar nombre de archivo**|Cambie el nombre de un archivo. Al seleccionar este valor se muestran las opciones dinámicas para un origen y un destino. Al cambiar el nombre de un archivo, incluya el nuevo nombre del archivo en la ruta de acceso del directorio que proporcione como destino.|  
|**Establecer atributos**|Establezca los atributos de un archivo o un directorio. Al seleccionar este valor se muestran las opciones dinámicas para un origen y una operación.|  
  
 **IsSourcePathVariable**  
 Indica si la ruta de acceso de destino está almacenada en una variable. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value||  
|-----------|-|  
|**True**|La ruta de destino está almacenada en una variable. Si selecciona este valor, se mostrará la opción dinámica **SourceVariable**.|  
|**False**|La ruta de destino se especifica en un administrador de conexiones de archivos. Al seleccionar este valor se muestra la opción dinámica, **DestinationVariable**.|  
  
### <a name="isdestinationpathvariable-dynamic-options"></a>Opciones dinámicas de IsDestinationPathVariable  
  
#### <a name="isdestinationpathvariable--true"></a>IsDestinationPathVariable = True  
 **DestinationVariable**  
 Seleccione el nombre de la variable en la lista o haga clic en \<**New variable...**> para crear una.  
  
 **Temas relacionados:** [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
#### <a name="isdestinationpathvariable--false"></a>IsDestinationPathVariable = False  
 **DestinationConnection**  
 Seleccione un administrador de conexiones de archivos de la lista o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
### <a name="issourcepathvariable-dynamic-options"></a>Opciones dinámicas de IsSourcePathVariable  
  
#### <a name="issourcepathvariable--true"></a>IsSourcePathVariable = True  
 **SourceVariable**  
 Seleccione el nombre de la variable en la lista o haga clic en \<**New variable...**> para crear una.  
  
 **Temas relacionados:** [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
#### <a name="issourcepathvariable--false"></a>IsSourcePathVariable = False  
 **SourceConnection**  
 Seleccione un administrador de conexiones de archivos de la lista o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md)  
  
### <a name="operation-dynamic-options"></a>Opciones dinámicas de Operación  
  
#### <a name="operation--set-attributes"></a>Operación = Establecer atributos  
 **Oculto**  
 Indique si el archivo o el directorio está visible.  
  
 **ReadOnly**  
 Indique si el archivo es de solo lectura.  
  
 **Archivar**  
 Indique si el archivo o el directorio está listo para archivar.  
  
 **Sistema**  
 Indique si el archivo es un archivo del sistema operativo.  
  
#### <a name="operation--create-directory"></a>Operación = Crear directorio  
 **UseDirectoryIfExists**  
 Indica si la operación **Crear directorio** usa un directorio existente con el nombre especificado, en lugar de crear un directorio.  
  
## <a name="see-also"></a>Consulte también  
 [Tareas de Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flujo de control](../../integration-services/control-flow/control-flow.md)  
  
