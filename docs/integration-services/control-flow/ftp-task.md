---
description: Tarea FTP
title: Tarea FTP | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ftptask.f1
- sql13.dts.designer.ftptask.general.f1
- sql13.dts.designer.ftptask.filetransfer.f1
helpviewer_keywords:
- FTP task [Integration Services]
ms.assetid: 41c3f2c4-ee04-460a-9822-bb9ae4036c2e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 839f9cc803dfcfe6c753851e53f0db1f2ef14daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348967"
---
# <a name="ftp-task"></a>Tarea FTP

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea FTP descarga y carga archivos de datos, y administra directorios en servidores. Por ejemplo, un paquete puede descargar archivos de datos de un servidor remoto o de una ubicación de Internet como parte de un flujo de trabajo de paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Puede usar la tarea FTP para los siguientes fines:  
  
-   Copiar directorios y archivos de datos de un directorio a otro, antes o después de mover datos, y aplicar transformaciones a los datos.  
  
-   Iniciar una sesión en una ubicación FTP de origen y copiar archivos o paquetes en un directorio de destino.  
  
-   Descargar archivos desde una ubicación FTP y aplicar transformaciones a datos de la columna antes de cargar datos en una base de datos.  
  
 En tiempo de ejecución, la tarea FTP se conecta con un servidor mediante un administrador de conexiones FTP. El administrador de conexiones FTP se configura por separado de la tarea FTP y después se hace referencia al mismo en la tarea FTP. Dicho administrador incluye la configuración del servidor, las credenciales de acceso al servidor FTP y opciones como el tiempo de espera y el número de reintentos de conexión con el servidor. Para obtener más información, vea [Administrador de conexiones FTP](../../integration-services/connection-manager/ftp-connection-manager.md).  
  
> [!IMPORTANT]  
>  El administrador de conexiones FTP solo admite la autenticación anónima y la autenticación básica. No es compatible con la autenticación de Windows.  
  
 Al tener acceso a un archivo local o un directorio local, la tarea FTP utiliza un administrador de conexiones de archivos o información de ruta de acceso almacenada en una variable. En contraste, al tener acceso a un archivo remoto o un directorio remoto, la tarea FTP utiliza una ruta especificada directamente en el servidor remoto, como se especifica en el administrador de conexiones FTP, o información de ruta almacenada en una variable. Para más información, vea [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md) e [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Esto significa que la tarea FTP puede recibir varios archivos y eliminar varios archivos remotos, pero si utiliza un administrador de conexiones, solo puede enviar un archivo y eliminar únicamente un archivo local, porque un administrador de conexiones de archivos solo puede tener acceso a un único archivo. Para tener acceso a varios archivos locales, la tarea FTP debe utilizar una variable para proporcionar la información de ruta. Por ejemplo, una variable que contiene "C:\Prueba\&#42;.txt" proporciona una ruta de acceso que permite eliminar o enviar todos los archivos del directorio Prueba que tienen una extensión .txt.  
  
 Para enviar varios archivos y tener acceso a varios archivos y directorios locales, también puede ejecutar la tarea FTP varias veces incluyéndola en un contenedor de bucles Foreach. El contenedor de bucles Foreach puede recorrer los archivos de un directorio mediante el enumerador Foreach File. Para más información, vea [Contenedor Foreach Loop](../../integration-services/control-flow/foreach-loop-container.md).  
  
 La tarea FTP admite los caracteres comodín *?* y *\** en las rutas. Esto permite que la tarea tenga acceso a varios archivos. Sin embargo, solo puede usar caracteres comodín en la parte de la ruta de acceso que especifica el nombre de archivo. Por ejemplo, C:\MiDirectorio\\*.txt es una ruta válida, pero C:\\\*\MiTexto.txt no.  
  
 Es posible configurar las operaciones de FTP para detener la tarea Sistema de archivos cuando la operación no se realice correctamente o para transferir archivos en modo ASCII. Las operaciones que envían y reciben archivos pueden configurarse para sobrescribir los archivos y directorios de destino.  
  
## <a name="predefined-ftp-operations"></a>Operaciones FTP predefinidas  
 La tarea FTP incluye un conjunto predefinido de operaciones. Estas operaciones se describen en la siguiente tabla.  
  
|Operación|Descripción|  
|---------------|-----------------|  
|Enviar archivos|Envía un archivo desde el equipo local al servidor FTP.|  
|Recibir archivos|Guarda un archivo del servidor FTP en el equipo local.|  
|Crear directorio local|Crea una carpeta en el equipo local.|  
|Crear directorio remoto|Crea una carpeta en el servidor FTP.|  
|Quitar directorio local|Elimina una carpeta del equipo local.|  
|Quitar directorio remoto|Elimina una carpeta del servidor FTP.|  
|Eliminar archivos locales|Elimina un archivo del equipo local.|  
|Eliminar archivos remotos|Elimina un archivo del servidor FTP.|  
  
## <a name="custom-log-entries-available-on-the-ftp-task"></a>Entradas del registro personalizadas disponibles en la tarea FTP  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea FTP. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Indica que la tarea inició una conexión con el servidor FTP.|  
|**FTPOperation**|Informa del comienzo y del tipo de operación de FTP que realiza la tarea.|  
  
## <a name="related-tasks"></a>Related Tasks  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Para obtener información sobre cómo establecer estas propiedades en el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vea [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
 Para obtener más información sobre la configuración mediante programación de estas propiedades, vea <xref:Microsoft.SqlServer.Dts.Tasks.FtpTask.FtpTask>.  
  
## <a name="ftp-task-editor-general-page"></a>Editor de la tarea FTP (página General)
  Use la página **General** del cuadro de diálogo **Editor de la tarea FTP** para especificar el administrador de conexiones FTP que se conecta al servidor FTP con el que se comunica la tarea. También puede asignar un nombre a la tarea FTP y describirla.  
  
### <a name="options"></a>Opciones  
 **FtpConnection**  
 Seleccione un administrador de conexiones FTP existente o haga clic en \<**New connection...**> para crear uno.  
  
> [!IMPORTANT]  
>  El administrador de conexiones FTP solo admite la autenticación anónima y la autenticación básica. No es compatible con la autenticación de Windows.  
  
 **Temas relacionados**: [Administrador de conexiones FTP](../../integration-services/connection-manager/ftp-connection-manager.md), [Editor del administrador de conexiones FTP](../connection-manager/ftp-connection-manager.md)  
  
 **StopOnFailure**  
 Indica si la tarea FTP termina si se produce un error en una opción FTP.  
  
 **Nombre**  
 Proporcione un nombre único para la tarea FTP. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
> [!NOTE]  
>  Los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea FTP.  
  
## <a name="ftp-task-editor-file-transfer-page"></a>Editor de la tarea FTP (página Transferencia de archivos)
  Use la página **Transferencia de archivos** del cuadro de diálogo **Editor de la tarea FTP** para configurar la operación de FTP que realiza la tarea.  
  
### <a name="options"></a>Opciones  
 **IsRemotePathVariable**  
 Indica si la ruta remota se almacena en una variable. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**True**|La ruta de destino está almacenada en una variable. Al seleccionar este valor se muestra la opción dinámica **RemoteVariable**.|  
|**False**|La ruta de destino se especifica en un administrador de conexiones de archivos. Al seleccionar este valor se muestra la opción dinámica **RemotePath**.|  
  
 **OverwriteFileAtDestination**  
 Especifica si se puede sobrescribir un archivo del destino.  
  
 **IsLocalPathVariable**  
 Indica si la ruta de acceso local está almacenada en una variable. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**True**|La ruta de destino está almacenada en una variable. Al seleccionar este valor se muestra la opción dinámica **LocalVariable**.|  
|**False**|La ruta de destino se especifica en un administrador de conexiones de archivos. Al seleccionar este valor se muestra la opción dinámica **LocalPath**.|  
  
 **operación**  
 Seleccione la operación de FTP que se realizará. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Enviar archivos**|Envía archivos. Al seleccionar este valor se muestran las opciones dinámicas **LocalVariable**, **LocalPathRemoteVariable** y **RemotePath**.|  
|**Recibir archivos**|Recibe archivos. Al seleccionar este valor se muestran las opciones dinámicas **LocalVariable**, **LocalPathRemoteVariable** y **RemotePath**.|  
|**Crear directorio local**|Crea un directorio local. Al seleccionar este valor se muestran las opciones dinámicas **LocalVariable** y **LocalPath**.|  
|**Crear directorio remoto**|Crea un directorio remoto. Al seleccionar este valor se muestran las opciones dinámicas **RemoteVariable** y **RemotePath**.|  
|**Quitar directorio local**|Quita un directorio local. Al seleccionar este valor se muestran las opciones dinámicas **LocalVariable** y **LocalPath**.|  
|**Quitar directorio remoto**|Quita un directorio remoto. Al seleccionar este valor se muestran las opciones dinámicas **RemoteVariable** y **RemotePath**.|  
|**Eliminar archivos locales**|Elimina archivos locales. Al seleccionar este valor se muestran las opciones dinámicas **LocalVariable** y **LocalPath**.|  
|**Eliminar archivos remotos**|Elimina archivos remotos. Al seleccionar este valor se muestran las opciones dinámicas **RemoteVariable** y **RemotePath**.|  
  
 **IsTransferASCII**  
 Indica si los archivos transferidos a y desde el servidor FTP remoto deben transferirse en modo ASCII.  
  
### <a name="isremotepathvariable-dynamic-options"></a>Opciones dinámicas de IsRemotePathVariable  
  
#### <a name="isremotepathvariable--true"></a>IsRemotePathVariable = True  
 **RemoteVariable**  
 Seleccione una variable existente definida por el usuario o haga clic en \<**New variable...**> para crear una.  
  
 **Temas relacionados:** [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Agregar variable  
  
#### <a name="isremotepathvariable--false"></a>IsRemotePathVariable = False  
 **RemotePath**  
 Seleccione un administrador de conexiones FTP existente o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones FTP](../../integration-services/connection-manager/ftp-connection-manager.md), [Editor del administrador de conexiones FTP](../connection-manager/ftp-connection-manager.md)  
  
### <a name="islocalpathvariable-dynamic-options"></a>Opciones dinámicas de IsLocalPathVariable  
  
#### <a name="islocalpathvariable--true"></a>IsLocalPathVariable = True  
 **LocalVariable**  
 Seleccione una variable existente definida por el usuario o haga clic en \<**New variable...**> para crear una.  
  
 **Temas relacionados:** [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Agregar variable  
  
#### <a name="islocalpathvariable--false"></a>IsLocalPathVariable = False  
 **LocalPath**  
 Seleccione un administrador de conexiones de archivos existente o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados**: [Administrador de conexiones de archivos planos](../../integration-services/connection-manager/flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Consulte también  
 [Tareas de Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flujo de control](../../integration-services/control-flow/control-flow.md)  
  
