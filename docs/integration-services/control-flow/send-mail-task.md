---
description: Enviar correo, tarea
title: Tarea Enviar correo | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sendmailtask.f1
- sql13.dts.designer.sendmailtask.general.f1
- sql13.dts.designer.sendmailtask.mail.f1
helpviewer_keywords:
- mail [Integration Services]
- Send Mail task
- e-mail [Integration Services]
- messages [Integration Services]
- sending messages
ms.assetid: fe0b7cbc-fe8e-4fe2-95b4-2953efff5869
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 362da93d1048564812826fceb3c0f2e76a47977a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348282"
---
# <a name="send-mail-task"></a>Enviar correo, tarea

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Enviar correo envía un mensaje de correo electrónico. Un paquete puede utilizar la tarea Enviar correo para enviar mensajes si las tareas del paquete de flujo de trabajo finalizan correctamente o si se producen errores, o para enviar mensajes en respuesta a eventos provocados por el paquete en tiempo de ejecución. Por ejemplo, la tarea puede notificar a un administrador de base de datos si la tarea Copia de seguridad de la base de datos se realizó correctamente o no.  
  
 Puede configurar la tarea Enviar correo de las siguientes maneras:  
  
-   Proporcionar el texto del mensaje de correo electrónico.  
  
-   Especificar el asunto del mensaje de correo electrónico.  
  
-   Establecer el nivel de prioridad del mensaje. La tarea admite tres niveles de prioridad: normal, bajo y alto.  
  
-   Especificar los destinatarios en las líneas Para, CC y CCO. Si la tarea especifica varios destinatarios, deben separarse mediante signos de punto y coma.  
  
    > [!NOTE]  
    >  Las líneas Para, CC y CCO tienen un límite de 256 caracteres cada una, de acuerdo con los estándares de Internet.  
  
-   Incluir archivos adjuntos. Si la tarea especifica varios archivos adjuntos, deben separarse mediante la barra vertical (|).  
  
    > [!NOTE]  
    >  Si cuando se ejecuta el paquete no existe un archivo adjunto, se producirá un error.  
  
-   Especificar el administrador de conexiones SMTP que se va a usar.  
  
    > [!IMPORTANT]  
    >  El administrador de conexiones SMTP solo es compatible con la autenticación anónima y la autenticación de Windows. No admite la autenticación básica.  
  
 El texto del mensaje puede ser una cadena que especifique, una conexión a un archivo que contiene el texto o el nombre de una variable que contiene el texto que se va a enviar. La tarea usa el administrador de conexiones de archivos para conectar con un archivo. Para más información, consulte [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La tarea usa el administrador de conexiones SMTP para conectar con un servidor de correo. Para más información, consulte [SMTP Connection Manager](../../integration-services/connection-manager/smtp-connection-manager.md).  
  
## <a name="custom-logging-messages-available-on-the-send-mail-task"></a>Mensajes de registro personalizados disponibles en la tarea Enviar correo  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Enviar correo. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**SendMailTaskBegin**|Indica que la tarea comenzó a enviar un mensaje de correo electrónico.|  
|**SendMailTaskEnd**|Indica que la tarea finalizó el envío de un mensaje de correo electrónico.|  
|**SendMailTaskInfo**|Proporciona información descriptiva sobre la tarea.|  
  
## <a name="configuring-the-send-mail-task"></a>Configurar la tarea Enviar correo  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Para obtener información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Página Expresiones](../../integration-services/expressions/expressions-page.md)  
  
 Para obtener información sobre cómo establecer estas propiedades mediante programación, haga clic en el tema siguiente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.SendMailTask.SendMailTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 Para obtener información sobre cómo establecer estas propiedades en el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   Artículo técnico [How to send email with delivery notification in C#](https://go.microsoft.com/fwlink/?LinkId=237625)(Enviar un mensaje de correo electrónico con una notificación de entrega en C#) en shareourideas.com  
  
## <a name="send-mail-task-editor-general-page"></a>Editor de la tarea Enviar correo (página General)
  Use la página **General** del cuadro de diálogo **Editor de la tarea Enviar correo** para asignar un nombre a la tarea Enviar correo y describirla.  
  
### <a name="options"></a>Opciones  
 **Nombre**  
 Proporcione un nombre único para la tarea Enviar correo. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
 **Nota** : los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea Enviar correo.  
  
## <a name="send-mail-task-editor-mail-page"></a>Editor de la tarea Enviar correo (página Correo)
  Use la página **Correo** del cuadro de diálogo **Editor de la tarea Enviar correo** para especificar los destinatarios, el tipo de mensaje y la prioridad de un mensaje. También puede adjuntar archivos al mensaje. El texto del mensaje puede consistir en una cadena que proporcione, una conexión de archivo a un archivo con el texto o el nombre de una variable con el texto.  
  
### <a name="options"></a>Opciones  
 **SMTPConnection**  
 Seleccione un administrador de conexiones SMTP de la lista o haga clic en **\<New connection...>** para crear un nuevo administrador de conexiones.  
  
> [!IMPORTANT]  
>  El administrador de conexiones SMTP solo es compatible con la autenticación anónima y la autenticación de Windows. No admite la autenticación básica.  
  
 **Temas relacionados:** [Administrador de conexiones SMTP](../../integration-services/connection-manager/smtp-connection-manager.md)  
  
 **From**  
 Especifique la dirección de correo electrónico del remitente.  
  
 **To**  
 Escriba las direcciones de correo electrónico de los destinatarios, separadas con punto y coma.  
  
 **CC**  
 Escriba las direcciones de correo electrónico de las personas que recibirán una copia del mensaje, separadas con punto y coma.  
  
 **CCO**  
 Escriba las direcciones de correo electrónico de los destinatarios ocultos de copia del mensaje, separadas con punto y coma.  
  
 **Subject**  
 Escriba el asunto del mensaje de correo electrónico.  
  
 **MessageSourceType**  
 Permite seleccionar el tipo de origen del mensaje. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen para el texto del mensaje. Si selecciona este valor, se mostrará la opción dinámica **MessageSource**.|  
|**Conexión de archivos**|Establezca el origen al archivo que incluye el texto del mensaje. Si selecciona este valor, se mostrará la opción dinámica **MessageSource**.|  
|**Variable**|Establezca el origen para la variable que incluye el texto del mensaje. Si selecciona este valor, se mostrará la opción dinámica **MessageSource**.|  
  
 **Prioridad**  
 Establezca la prioridad del mensaje.  
  
 **Datos adjuntos**  
 Escriba los nombres de los archivos de datos adjuntos que desea adjuntar al mensaje de correo electrónico, separados por una barra vertical (|).  
  
> [!NOTE]  
>  Las líneas Para, CC y CCO están limitadas a 256 caracteres según las normas de Internet.  
  
### <a name="messagesourcetype-dynamic-options"></a>Opciones dinámicas de MessageSourceType  
  
#### <a name="messagesourcetype--direct-input"></a>MessageSourceType = Entrada directa  
 **MessageSource**  
 Escriba el texto del mensaje, o bien haga clic en el botón Examinar (…) y escriba el mensaje en el cuadro de diálogo **Origen del mensaje**.  
  
#### <a name="messagesourcetype--file-connection"></a>MessageSourceType = Conexión de archivos  
 **MessageSource**  
 Seleccione un administrador de conexiones de archivos de la lista o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
#### <a name="messagesourcetype--variable"></a>MessageSourceType = Variable  
 **MessageSource**  
 Seleccione una variable de la lista o haga clic en \<**New variable...**> para crear una nueva variable.  
  
 **Temas relacionados:** [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>Consulte también  
 [Tareas de Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flujo de control](../../integration-services/control-flow/control-flow.md)  
  
