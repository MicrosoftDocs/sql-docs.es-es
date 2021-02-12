---
title: Supervisar suscripciones de Reporting Services | Microsoft Docs
description: Aprenda a usar la interfaz de usuario, PowerShell o los archivos de registro para realizar el seguimiento de las suscripciones de Reporting Services. Las opciones de supervisión dependen del modo del servidor de informes que se ejecute.
ms.date: 06/12/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], inactive
- subscriptions [Reporting Services], status
- monitoring [Reporting Services], subscriptions
- status information [Reporting Services]
- inactive subscriptions [Reporting Services]
ms.assetid: 054c4a87-60bf-4556-9a8c-8b2d77a534e6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6631325809a468aed4e7c7fd00cf71aedecc7ba0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042075"
---
# <a name="monitor-reporting-services-subscriptions"></a>Supervisar suscripciones de Reporting Services
  Puede supervisar las suscripciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] desde la interfaz de usuario, Windows PowerShell o los archivos de registro. Las opciones disponibles para la supervisión dependen del modo de servidor de informes que esté ejecutando.  
  
**[!INCLUDE[applies](../../includes/applies-md.md)]**

:::row:::
    :::column:::
        [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en modo nativo  
    :::column-end:::
    :::column:::
        [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modo de SharePoint  
    :::column-end:::
:::row-end:::

 **En este artículo:**  
  
-   [Interfaz de usuario del modo nativo](#bkmk_native_mode)  
  
-   [Modo de SharePoint](#bkmk_sharepoint_mode)  
  
-   [Usar PowerShell para supervisar suscripciones](#bkmk_use_powershell)  
  
-   [Administrar suscripciones inactivas](#bkmk_manage_inactive)  
  
##  <a name="native-mode-user-interface"></a><a name="bkmk_native_mode"></a> Interfaz de usuario del modo nativo  
 Los usuarios individuales de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pueden supervisar el estado de una suscripción en la página **Mis suscripciones** o la pestaña **Suscripciones** del portal web. Las páginas de suscripción incluyen columnas que indican cuándo se ejecutó la suscripción por última vez y su estado. Los mensajes de estado se actualizan cuando la suscripción esté programada para procesarse. Si no se activa el desencadenador (por ejemplo, si nunca se actualiza una instantánea de ejecución de informes o no se ejecuta una programación), no se actualizará el mensaje de estado.  
  
 En la siguiente tabla, se describen los posibles valores de la columna **Estado** .  
  
|Status|Descripción|  
|------------|-----------------|  
|Suscripción nueva|Aparece cuando crea la suscripción.|  
|Inactivo|Aparece cuando no se puede procesar una suscripción. Para obtener más información, vea "Administrar suscripciones inactivas" más adelante en este artículo.|  
|Listo: se han procesado \<*number*> de un total de \<*number*>; \<*number*> errores.|Muestra el estado de la ejecución de una suscripción controlada por datos; este mensaje procede del Procesador de entrega y programación.|  
|Se han procesado \<*number*>|El número de notificaciones que el Procesador de entrega y programación ha entregado correctamente o que ya no intenta entregar. Cuando se completa una entrega controlada por datos, el número de notificaciones procesadas debe ser igual al número total de notificaciones generadas.|  
|\<*number*> en total|El número total de notificaciones generadas para la última entrega de la suscripción.|  
|\<*number*> con error|El número de notificaciones que el Procesador de entrega y programación no ha podido entregar o que ya no intenta entregar.|  
|Error al enviar correo: error de transporte en la conexión al servidor.|Indica que el servidor de informes no se ha conectado al servidor de correo; este mensaje procede de la extensión de entrega por correo electrónico.|  
|El archivo \<*filename*> se ha escrito en \<path>.|Indica que se ha realizado correctamente la entrega a la ubicación del recurso compartido de archivos; este mensaje procede de la extensión de entrega a recursos compartidos de archivos.|  
|Error desconocido al escribir el archivo.|Indica que no se ha realizado la entrega a la ubicación del recurso compartido de archivos; este mensaje procede de la extensión de entrega a recursos compartidos de archivos.|  
|Error al conectar con la carpeta de destino, \<path>. Compruebe si existe la carpeta de destino o el recurso compartido.|Indica que no se puede encontrar la carpeta que ha especificado; este mensaje procede de la extensión de entrega a recursos compartidos de archivos.|  
|No se pudo escribir el archivo \<filename> en \<path>. Nuevo intento.|Indica que no se ha podido actualizar el archivo con una versión más reciente; este mensaje procede de la extensión de entrega a recursos compartidos de archivos.|  
|Error al escribir el archivo \<filename>: \<message>|Indica que no se ha realizado la entrega a la ubicación del recurso compartido de archivos; este mensaje procede de la extensión de entrega a recursos compartidos de archivos.|  
|\<custom status messages>|Los mensajes de estado sobre entregas correctas o no realizadas, procedentes de las extensiones de entrega. Si usa una extensión de entrega personalizada o de terceros, pueden proporcionarse mensajes de estado adicionales.|  
  
 Los administradores del servidor de informes también pueden supervisar las suscripciones estándar que se estén procesando. No es posible supervisar las suscripciones controladas por datos. Para obtener más información, vea [Administrar un proceso en ejecución](../../reporting-services/subscriptions/manage-a-running-process.md).  
  
 Si no se ha podido entregar una suscripción (por ejemplo, si el servidor de correo no estaba disponible), la extensión de entrega vuelve a intentarlo. Un valor de la configuración especifica el número de intentos que realizará. El valor predeterminado es ningún reintento. En algunos casos, es posible que se haya procesado el informe sin datos (por ejemplo, si el origen de datos se encuentra en modo sin conexión), en cuyo caso se proporciona texto al respecto en el cuerpo del mensaje.  
  
### <a name="native-mode-log-files"></a>Archivos de registro del modo nativo  
 Si se produce un error durante la entrega, se agrega una entrada al registro de seguimiento del servidor de informes.  
  
 Los administradores del servidor de informes pueden revisar los archivos **ReportServerService_*.log** para determinar el estado de entrega de la suscripción. Para la entrega por correo electrónico, los archivos de registro del servidor de informes incluyen un registro del procesamiento y las entregas a cuentas de correo electrónico específicas. La ubicación predeterminada de los archivos de registro es la siguiente:  
  
 `C:\Program Files\Microsoft SQL Server Reporting Services\SSRS\LogFiles`  
  
 Este es un nombre de archivo de registro de ejemplo:  
  
 `ReportServerService__05_21_2019_00_05_07.log`  
  
 Este es un mensaje de error de ejemplo de un archivo de registro de seguimiento relacionado con las suscripciones:  
  
-   library!WindowsService_7!b60!05/20/2019-22:34:36 i INFO: Inicializando EnableExecutionLogging en 'True' como se especifica en Server system properties.emailextension!WindowsService_7!b60!05/20/2019-22:34:41 ERROR: **Error al enviar correo electrónico**. Excepción: System.Net.Mail.SmtpException: el servidor SMTP requiere una conexión segura o el cliente no se autenticó. La respuesta del servidor fue: 5.7.1 No se autenticó el cliente en System.Net.Mail.MailCommand.CheckResponse(SmtpStatusCode statusCode, String response)  
  
 El archivo de registro no incluye información sobre si se ha abierto el informe o si se ha realizado la entrega correctamente. Una entrega correcta quiere decir que el Procesador de entrega y programación no generó ningún error y que el servidor de informes se conectó al servidor de correo. Si el mensaje de correo electrónico produce un error de mensaje que no se pudo entregar en el buzón del usuario, esta información no se incluirá en el archivo de registro. Para más información sobre los archivos de registro, vea [Archivos de registro y orígenes de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
##  <a name="sharepoint-mode"></a><a name="bkmk_sharepoint_mode"></a> Modo de SharePoint  
 Para supervisar una suscripción en el modo de SharePoint: el estado de la suscripción se puede supervisar desde la página **Administrar suscripciones** .  
  
1.  vaya a la biblioteca de documentos que contiene el informe  
  
2.  Abra el menú contextual del informe ( **...** ).  
  
3.  Seleccione la opción de menú expandido ( **...** ).  
  
4.  Elija **Administrar suscripciones**  
  
### <a name="sharepoint-uls-log-files"></a>Archivos de registro de ULS de SharePoint  
 La información relacionada con la suscripción se escribe en el registro de ULS de SharePoint. Para más información sobre cómo configurar eventos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para el registro de ULS, vea [Activar eventos de Reporting Services para el registro de seguimiento de SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md).  Este es un ejemplo de entrada de registro ULS relacionada con las suscripciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
|Date|Proceso|Área|Category|Nivel|Correlation|Message|  
|-|-|-|-|-|-|-|  
|21/05/2019 14:34:06:15|Grupo de aplicaciones: a0ba039332294f40bc4a81544afde01d|SQL Server Reporting Services|Extensión de correo electrónico del servidor de informes|Inesperado|(vacío)|**Error al enviar correo electrónico.** Excepción: System.Net.Mail.SmtpException: buzón no disponible. La respuesta del servidor fue: 5.7.1 El cliente no tiene permisos para enviar como este remitente en System.Net.Mail.DataStopCommand.CheckResponse(SmtpStatusCode statusCode, String serverResponse) en System.Net.Mail.DataStopCommand.Send(SmtpConnection conn) en System.Net.Mail.SmtpClient.Send(MailMessage message) en Microsoft.ReportingServices.EmailDeliveryProvider.EmailProvider.Deliver(Notification notification)|  
  
##  <a name="use-powershell-to-monitor-subscriptions"></a><a name="bkmk_use_powershell"></a> Usar PowerShell para supervisar suscripciones  
 Para ver scripts de PowerShell de ejemplo que puede utilizar para comprobar el estado del modo nativo o las suscripciones del modo de SharePoint, consulte [Administrar propietarios de suscripciones y ejecutar suscripciones (PowerShell)](../../reporting-services/subscriptions/manage-subscription-owners-and-run-subscription-powershell.md).  
  
##  <a name="managing-inactive-subscriptions"></a><a name="bkmk_manage_inactive"></a> Administrar suscripciones inactivas  
 Si una suscripción queda inactiva, debería eliminarla o reactivarla resolviendo las condiciones subyacentes que impiden que se procese. Las suscripciones pueden quedar inactivas si se producen condiciones que impidan su procesamiento. Entre estas condiciones, figuran:  
  
-   Quitar o desinstalar la extensión de entrega especificada en la suscripción.  
  
-   Cambiar la configuración de las credenciales de almacenadas a integradas o solicitadas.  
  
-   Cambiar un nombre de parámetro o un tipo de datos en la definición de informe y después volver a publicar un informe. Si una suscripción incluye un parámetro que ya no es válido, la suscripción queda inactiva.  
  
-   Cambiar el modo de ejecución de un informe (por ejemplo, modificar un informe a petición para que se ejecute como instantánea de ejecución de informes). Para más información, vea [Establecer las propiedades del procesamiento de informes](../../reporting-services/report-server/set-report-processing-properties.md).  
  
 Las suscripciones inactivas se indican con un mensaje en la propia suscripción. El mensaje incluye información sobre la causa y qué pasos debe tomar para reactivar la suscripción.  
  
 Si existen condiciones que causan que la suscripción quede inactiva, la suscripción refleja este hecho cuando el servidor de informes la ejecuta. Si una suscripción está programada para entregar un informe todos los viernes a las 2:00 a.m. y la extensión de entrega que utiliza se desinstaló el lunes a las 9:00 a.m., la suscripción no reflejará su estado inactivo hasta el viernes a las 2:00 a.m.  
  
## <a name="see-also"></a>Consulte también  
 [Crear y administrar suscripciones para servidores de informes en modo nativo](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)   
 [Suscripciones y entrega &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)  
  
  
