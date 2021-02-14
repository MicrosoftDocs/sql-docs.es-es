---
title: Crear y administrar suscripciones para servidores de informes en modo de SharePoint | Microsoft Docs
description: Aprenda a crear una suscripción de Reporting Services para entregar informes desde una aplicación web de SharePoint que esté integrada con un servidor de informes en el modo de SharePoint.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], creating
- subscriptions [Reporting Services], deleting
- subscriptions [Reporting Services], managing
ms.assetid: 44be7ee2-33ce-46e4-9d1a-a20aaf43a227
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bf68742926ef0f26c5a12a5d001ea9b14f9b199c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080098"
---
# <a name="create-and-manage-subscriptions-for-sharepoint-mode-report-servers"></a>Crear y administrar suscripciones para servidores de informes en modo de SharePoint
  Puede crear suscripciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para entregar informes desde una aplicación web de SharePoint que esté integrada con un servidor de informes en el modo de SharePoint. Las suscripciones pueden entregar informes a una biblioteca de documentos, a una carpeta de archivos o como un correo electrónico. En este tema se resumen los requisitos y los pasos para crear una suscripción de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modo de SharePoint &#124; SharePoint 2010 y SharePoint 2013|  
  
 Cuando se crea una suscripción, hay tres formas de especificar su entrega:  
  
-   **Biblioteca de documentos**: puede crear una suscripción que entregue un documento basado en el informe original a una biblioteca que se encuentre en el mismo sitio de SharePoint que el informe original. No puede entregar el documento a una biblioteca en otro servidor u otro sitio dentro de la misma colección de sitios. Para entregar el documento, debe tener el permiso Agregar elementos para la biblioteca a la que se entrega el informe.  
  
-   **Carpeta de archivos:** puede entregar un documento basado en el informe original a una carpeta compartida del sistema de archivos. Debe seleccionar una carpeta existente que sea accesible a través de una conexión de red.  
  
-   **Correo electrónico:** si el servidor de informes está configurado para usar la extensión de entrega de correo electrónico del servidor de informes, puede crear una suscripción que envíe un informe o un archivo de informe exportado (guardado en un formato de salida) a la bandeja de entrada. Para recibir simplemente la notificación sin la dirección URL del informe o sin el informe propiamente dicho, desactive las casillas **Incluir un vínculo al informe** y **Mostrar informe dentro del mensaje** .  
  
 **En este tema:**  
  
-   [Requisitos generales de las suscripciones](#bkmk_subscription_requirements)  
  
-   [Para crear una suscripción para entregar un informe a una biblioteca de SharePoint](#bkmk_tosharepoint_library)  
  
-   [Para crear una suscripción para entregar un informe a una biblioteca de SharePoint](#bkmk_tosharepoint_library)  
  
-   [Para crear una suscripción para la entrega de correo electrónico del servidor de informes](#bkmk_subscription_for_email)  
  
-   [Para ver o modificar una suscripción](#bkmk_to_modify_subscription)  
  
-   [Para eliminar una suscripción](#bkmk_to_delete_subscription)  
  
##  <a name="general-requirements-for-subscriptions"></a><a name="bkmk_subscription_requirements"></a> Requisitos generales de las suscripciones  
 Para crear una suscripción, el informe debe usar credenciales almacenadas y se debe contar con permiso para ver el informe y crear alertas.  
  
 Al crear una suscripción, puede seleccionar un formato de archivo de salida. No todos los informes funcionan bien en todos los formatos. Antes de seleccionar un formato en una suscripción, abra el informe y expórtelo a otros formatos para asegurarse de que aparece de la manera esperada.  
  
 Los usuarios necesitan el permiso de lista **Editar elementos** en SharePoint si desean poder crear suscripciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para obtener más información, consulte [Referencia de permisos de sitio y lista de SharePoint para los elementos del servidor de informes](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md).  
  
> [!IMPORTANT]  
>  Una suscripción que entrega un informe a una biblioteca o a una carpeta compartida crea un nuevo archivo estático basado en el informe original, pero no se trata de una verdadera definición de informe que se ejecute en el elemento web Visor de informes. Si el informe original cuenta con funciones interactivas (como vínculos de obtención de detalles) o contenido dinámico, estas características no estarán disponibles en el archivo estático que se entregue en la ubicación de destino. Si selecciona una "Página web", puede mantener parte de esta interactividad pero, dado que el documento no es un archivo .rdl que se ejecute en el Visor de informes, al hacer varias veces clic en un informe se crearán nuevas páginas en la sesión de explorador por las que tendrá que desplazarse para regresar al sitio.  
  
 No se puede cambiar el nombre de la extensión de nombre de archivo de un informe exportado a .rdl y hacer que se ejecute en el elemento web Visor de informes. Si desea crear una suscripción que proporcione un informe de verdad, utilice la extensión de entrega de correo electrónico del servidor de informes y establezca opciones para incluir un vínculo al informe.  
  
 La configuración de versión de la biblioteca que contiene el documento entregado determina si se crea una versión nueva del documento con cada entrega. De manera predeterminada, la configuración de versión se habilita para cada biblioteca. A menos que elija de forma específica **Sin control de versiones**, se crea una versión principal nueva del documento con la entrega. Tan solo se crean versiones principales del documento; las versiones secundarias nunca se crean como resultado de la entrega de la suscripción, incluso si selecciona una opción de control de versiones que permita las versiones secundarias. Si limita el número de las versiones principales conservadas, las entregas anteriores se reemplazan por versiones más recientes al alcanzarse el límite máximo.  
  
 Los formatos de salida que se seleccionan para una suscripción se basan en las extensiones de representación instaladas en el servidor de informes. Solo pueden seleccionarse formatos de salida compatibles con las extensiones de representación del servidor de informes.  
  
###  <a name="to-create-a-subscription-to-deliver-a-report-to-a-sharepoint-library"></a><a name="bkmk_tosharepoint_library"></a> Para crear una suscripción para entregar un informe a una biblioteca de SharePoint  
  
1.  Vaya a una biblioteca de SharePoint que contenga el informe.  
  
2.  Haga clic en la flecha abajo situada junto al informe y seleccione **Administrar suscripciones**.  
  
3.  Haga clic en **Agregar suscripción**.  
  
4.  En **Extensión de entrega**, seleccione **Biblioteca de documentos de SharePoint**.  
  
5.  En **Biblioteca de documentos**, seleccione una biblioteca que se encuentre dentro del mismo sitio.  
  
6.  En **Opciones de archivo**, especifique el nombre de archivo y el título del documento que creará la suscripción.  
  
7.  En **Formato de salida**, seleccione el formato de la aplicación.  
  
     El valor predeterminado es archivo web (MHTML), ya que genera un archivo HTML independiente, pero no mantiene las características del informe interactivo que puedan estar en el informe original.  
  
8.  En **Opciones de sobrescritura**, especifique una opción que determine si las entregas posteriores sobrescribirán un archivo. Si desea mantener las entregas anteriores, puede seleccionar **Crear archivo con nombre único**. Se anexará un número a los nuevos archivos para crear un nombre de archivo único.  
  
9. En **Evento de entrega**, especifique una programación o evento que provoque la ejecución de la suscripción. Puede crear una programación personalizada, seleccionar una programación compartida si hay alguna disponible o ejecutar la suscripción cada vez que se actualicen los datos de un informe que se ejecute con datos de instantáneas. Para más información sobre programaciones y procesamiento de datos, vea [Establecer opciones de procesamiento &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. En **Parámetros**, si está creando una suscripción a un informe con parámetros, especifique los valores que desee usar con el informe cuando se procese la suscripción. La sección de parámetros no está visible en esta página si el informe que selecciona no contiene parámetros. Para obtener más información sobre los parámetros, vea [Establecer parámetros en un informe publicado &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-shared-folder-delivery"></a><a name="bkmk_subscription_for_sharedfolder"></a> Para crear una suscripción para la entrega en una carpeta compartida  
  
1.  Vaya a una biblioteca de SharePoint que contenga el informe.  
  
2.  Haga clic en la flecha abajo situada junto al informe y seleccione **Administrar suscripciones**.  
  
3.  Haga clic en **Agregar suscripción**.  
  
4.  En **Extensión de entrega**, seleccione **Recurso compartido de archivos de Windows**.  
  
5.  En **Nombre de archivo**, escriba el nombre del archivo que se creará en la carpeta compartida.  
  
6.  En **Ruta de acceso**, escriba la ruta de acceso a la carpeta con el formato UNC (Convención de nomenclatura universal), que incluya el nombre de red del equipo. En la ruta de la carpeta no incluya barras diagonales inversas. Una ruta de acceso de ejemplo puede ser `\\ComputerName01\Public\MyReports`, donde Public y MyReports son carpetas compartidas.  
  
7.  En **Formato de representación**, seleccione el formato de aplicación para el informe.  
  
8.  En **Modo de escritura**, elija **Ninguno**, **Incremento automático** o **Sobrescribir**. Estas opciones determinan si las entregas posteriores sobrescribirán un archivo. Si desea mantener las entregas previas, puede elegir **Incremento automático**. Se anexará un número a los nuevos archivos para crear un nombre de archivo único. Si elige **Ninguno**, no se producirá ninguna entrega si ya hay un archivo con el mismo nombre en la ubicación de destino.  
  
9. En **Extensión de archivo**, elija **True** para agregar una extensión de nombre de archivo correspondiente al formato de archivo de aplicación o False para crear el archivo sin ningún tipo de extensión.  
  
10. En **Nombre de usuario** y **Contraseña**, especifique las credenciales que tienen permisos de escritura en la carpeta compartida.  
  
11. En **Evento de entrega**, especifique una programación o evento que provoque la ejecución de la suscripción. Puede crear una programación personalizada, seleccionar una programación compartida si hay alguna disponible o ejecutar la suscripción cada vez que se actualicen los datos de un informe que se ejecute con datos de instantáneas. Para más información sobre programaciones y procesamiento de datos, vea [Establecer opciones de procesamiento &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
12. En **Parámetros**, si está creando una suscripción a un informe con parámetros, especifique los valores que desee usar con el informe cuando se procese la suscripción. Para obtener más información sobre los parámetros, vea [Establecer parámetros en un informe publicado &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-create-a-subscription-for-report-server-e-mail-delivery"></a><a name="bkmk_subscription_for_email"></a> Para crear una suscripción para la entrega de correo electrónico del servidor de informes  
  
1.  Vaya a una biblioteca de SharePoint que contenga el informe.  
  
2.  Haga clic en la flecha abajo situada junto al informe y seleccione **Administrar suscripciones**.  
  
3.  Haga clic en **Agregar suscripción**.  
  
4.  En **Extensión de entrega**, seleccione **Correo electrónico**.  
  
5.  En **Opciones de entrega**, especifique una dirección de correo electrónico a la que enviar el informe.  
  
6.  Si lo desea, puede modificar la línea Asunto. La línea Asunto utiliza parámetros integrados que capturan el nombre del informe y la hora a la que se procesó. Estos son los únicos parámetros integrados que se pueden usar. Los parámetros son marcadores que personalizan el texto que aparece en la línea Asunto, pero puede reemplazarlos por texto estático.  
  
7.  Elija **Incluir un vínculo al informe** si desea incrustar la dirección URL del informe en el cuerpo del mensaje.  
  
8.  En **Contenido del informe**, especifique si desea incrustar el propio informe en el cuerpo del mensaje.  
  
     El formato de representación y el explorador determinan si el informe se incrusta o se adjunta. Si el explorador es compatible con HTML 4.0 y MHTML, y se selecciona el formato de representación Archivo web, el informe se incrusta como parte del mensaje. Los demás formatos de representación (CSV, PDF, etc.) entregan los informes como datos adjuntos. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no comprueba el tamaño de los datos adjuntos ni del mensaje antes de enviar el informe. Si los datos adjuntos o el mensaje superan el límite máximo permitido por el servidor de correo, no se entregará el informe. Elija una de las otras opciones de entrega (como dirección URL o notificación) para informes de gran tamaño.  
  
9. En **Evento de entrega**, especifique una programación o evento que provoque la ejecución de la suscripción. Puede crear una programación personalizada, seleccionar una programación compartida si hay alguna disponible o ejecutar la suscripción cada vez que se actualicen los datos de un informe que se ejecute con datos de instantáneas. Para más información sobre programaciones y procesamiento de datos, vea [Establecer opciones de procesamiento &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md).  
  
10. En **Parámetros**, si está creando una suscripción a un informe con parámetros, especifique los valores que desee usar con el informe cuando se procese la suscripción. Para obtener más información sobre los parámetros, vea [Establecer parámetros en un informe publicado &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
###  <a name="to-view-or-modify-a-subscription"></a><a name="bkmk_to_modify_subscription"></a> Para ver o modificar una suscripción  
  
1.  Vaya a una biblioteca de SharePoint que contenga el informe.  
  
2.  Haga clic en la flecha abajo situada junto al informe y, a continuación, haga clic en **Administrar suscripciones**.  
  
3.  Cada suscripción se identifica mediante el tipo de entrega. Haga clic en el tipo de suscripción para ver y cambiar las propiedades existentes.  
  
###  <a name="to-delete-a-subscription"></a><a name="bkmk_to_delete_subscription"></a> Para eliminar una suscripción  
  
1.  Vaya a una biblioteca de SharePoint que contenga el informe.  
  
2.  Haga clic en la flecha abajo situada junto al informe y, a continuación, haga clic en **Administrar suscripciones**.  
  
3.  Haga clic en la casilla situada junto a la suscripción y, a continuación, haga clic en **Eliminar**.  
  
## <a name="see-also"></a>Consulte también  
 [Suscripciones y entrega &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Entrega por correo electrónico en Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md)   
 [Entrega a recursos compartidos de archivos en Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Entrega de la biblioteca de SharePoint en Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md)   
 [Configuración de un servidor de informes para la entrega de correo electrónico (Administrador de configuración del servidor de informes)](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)  
  
  
