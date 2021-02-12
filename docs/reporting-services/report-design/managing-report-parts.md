---
title: Administrar elementos de informe | Microsoft Docs
description: Organice los elementos de informe para varios usuarios e informes, con opciones flexibles para la publicación, los permisos y la seguridad.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 41947b4c-8ecf-4e4f-b30e-66e1d6692b74
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1ac27d7f3f83a65f6a9e7b06da511bd9b9baf751
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062640"
---
# <a name="managing-report-parts"></a>Administrar elementos de informe
  Los elementos de informe se pueden reutilizar en informes paginados, por varios usuarios y en varios informes. Los usuarios pueden buscar elementos de informe en el servidor y agregarlos a un informe.  También pueden informarse de las actualizaciones del elemento de informe en el servidor y volver a publicar versiones nuevas de un elemento de informe. Esas acciones de creación de informes se pueden ver afectadas por los permisos de seguridad de los servicios de informe, que las controlan.  En este tema se revisan las propiedades de los elementos de informe y su comportamiento cuando se encuentran en el servidor.  
  
## <a name="managing-report-parts"></a>Administrar elementos de informe  
 Para administrar los elementos de informe, puede usar el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de un servidor de informes en modo nativo, o bien las páginas de aplicación de un servidor de informes en modo integrado de SharePoint.  
  
### <a name="server-side-interaction-and-search"></a>Interacción y búsqueda en el lado servidor  
 Los elementos de informe se pueden publicar en un servidor de informes en modo nativo o en modo integrado de SharePoint. Los usuarios pueden utilizar la característica de la Galería de elementos de informe en una aplicación de creación de informes como el Generador de informes de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para encontrar y agregar elementos de informe a los informes. Cuando un usuario busca un elemento de informe, la búsqueda examina el catálogo del servidor de informes sin tener en cuenta el modo para el que el servidor se instaló.  
  
 Cuando los elementos de informe se publican en una aplicación de creación de informes como el Generador de informes en un servidor de informes en modo integrado de SharePoint, el catálogo del servidor de informes se actualiza también y las búsquedas en la galería reflejan con precisión el elemento de informe, tanto si es nuevo como si se ha actualizado.  
  
#### <a name="directly-uploading-report-parts-to-a-sharepoint-folder"></a>Elementos de informe que se cargan directamente en una carpeta de SharePoint  
 Si un elemento de informe se carga directamente en una carpeta de documentos de SharePoint (en lugar de publicarse desde una aplicación de creación de informes), el catálogo del servidor de informes no se actualiza. Las búsquedas de la Galería de elementos de informe no encontrarán el elemento de informe que se cargó. Para ayudar a mantener sincronizadas las carpetas de SharePoint y el catálogo del servidor de informes, puede activar la característica de sincronización de archivos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en el servidor de SharePoint. Para más información, consulte [Activar la característica de sincronización de archivos del servidor de informes en Administración central de SharePoint](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md).  
  
 Los archivos se pueden sincronizar también utilizando las llamadas de algunas de las API de administración de servicios de informe de errores como GetProperties y SetProperties.  
  
### <a name="organizing-and-moving-report-parts"></a>Organizar y mover elementos de informe  
 Debería considerar y planear por anticipado el modo en que su equipo utilizará y organizará los elementos de informe, los conjuntos de datos compartidos y los orígenes de datos compartidos. Aunque puede moverlos en un momento posterior, puede haber problemas.  
  
#### <a name="native-mode-report-server"></a>Servidor de informes en modo nativo  
 Si mueve un elemento de informe de un servidor de informes en modo nativo a cualquier otra carpeta del mismo servidor, no afecta a la capacidad que tienen las aplicaciones de creación de informes para buscar o descargar las actualizaciones del elemento de informe. Esto se debe a que el servidor se basa en el ComponentID único. Sin embargo, si el elemento de informe se mueve a una carpeta en la que un usuario no tiene permisos, sus búsquedas no podrán encontrar el elemento de informe y no se notificará a sus informes cuando haya actualizaciones del mismo.  
  
#### <a name="report-server-in-sharepoint-integrated-mode"></a>Servidor de informes en el modo integrado de SharePoint  
 Mover los elementos de informe a una biblioteca de documentos o carpeta diferentes tiene el mismo efecto que cargarlos en un servidor de SharePoint directamente: el catálogo del servidor de informes no se sincronizará. Para evitar esto, active la característica de sincronización de archivos del servidor de informes en el servidor de SharePoint.  
  
 La excepción la constituyen las subcarpetas. La búsqueda siempre buscará en las subcarpetas, de modo que, si mueve manualmente un elemento de informe a una subcarpeta, todavía se encontrará en una búsqueda de la galería de elementos de informe.  
  
### <a name="report-server-catalog-properties"></a>Propiedades del catálogo del servidor de informes  
 La siguiente tabla describe cómo los campos del catálogo del servidor de informes existentes se relacionan con un elemento de informe así como con los nuevos campos que se agregan al catálogo para los elementos de informe. Estos se exponen en el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y en las bibliotecas de SharePoint, así como en las aplicaciones de creación de informes como el Generador de informes.  
  
 (*) indica que es nuevo en esta versión.  
  
|Propiedad|Descripción|Elemento de informe<br /><br /> Criterios de búsqueda de la galería|  
|--------------|-----------------|---------------------------------------------|  
|Nombre|Este es uno de los criterios que un usuario puede buscar en la Galería de elementos de informe.|Sí|  
|Descripción|Puede ser conveniente organizar los nombres de los elementos de informe de una manera que facilite a los usuarios la búsqueda en la galería. Por ejemplo, puede buscar la descripción que empiece con "Ventas>>" para encontrar los elementos de informe que tengan que ver con los datos relacionados con las ventas y su presentación.|Sí|  
|CreatedBy|Identificador del usuario que agregó el elemento de informe a la base de datos del servidor de informes. El formato exacto depende del método de autenticación. Por ejemplo, algunos métodos de autenticación provocan que se muestre el nombre de usuario precedido por el dominio completo en los campos CreatedBy y ModifiedBy.|Sí|  
|CreationDate|Fecha en que originalmente se creó el elemento de informe.<br /><br /> Este es uno de los criterios que un usuario puede buscar en la Galería de elementos de informe.|Sí|  
|ModifiedBy|Se trata del identificador del usuario que modificó por última vez el elemento de informe.|Sí|  
|ModifiedDate|Fecha en que el elemento de informe se modificó por última vez en el servidor.<br /><br /> Este campo se utiliza como parte de la lógica para determinar cuándo hay actualizaciones del lado servidor en un elemento de informe. Para obtener más información, vea la descripción de ComponentID posteriormente en esta tabla.|Sí|  
|SubType (*)|Se trata de una cadena que indica el tipo de elemento de informe que buscar, como "Tablix" o "Gráfico".|Sí|  
|ComponentID (*)|Identificador único del elemento de informe. Se trata de un campo nuevo que se ha agregado al catálogo y está visible tanto en el lado servidor como en las aplicaciones de creación de informes como el Generador de informes.<br /><br /> Las aplicaciones cliente utilizan este campo al comprobar si hay actualizaciones de un elemento de informe en el servidor. La aplicación cliente busca en el servidor los ComponentID que están en el informe del lado cliente actual. Cuando hay una coincidencia de ComponentID, ModifiedDate se compara con el SyncDate del lado cliente del elemento de informe.|N0|  
  
## <a name="controlling-access-to-report-parts"></a>Controlar el acceso a los elementos de informe  
 En las siguientes tablas se describen las asignaciones de roles predeterminadas y cómo le permiten realizar varias operaciones. Los nombres de asignación de roles son diferentes en función del tipo de servidor de informes que se usa.  
  
### <a name="server-in-native-mode"></a>Servidor en modo nativo  
  
|Acciones|Roles|  
|-------------|-----------|  
|Agregar, eliminar, modificar propiedades de elementos, administrar la seguridad y descargar elementos de informe|Administrador de contenido<br /><br /> Mis informes|  
|Agregar, eliminar y descargar elementos de informe|Publicador|  
|Buscar y reutilizar|Browser<br /><br /> Generador de informes|  
  
### <a name="server-in-sharepoint-integrated-mode"></a>Servidor en el modo integrado de SharePoint  
  
|Acciones|Role|  
|-------------|----------|  
|Agregar, eliminar, modificar propiedades de elementos, administrar la seguridad y descargar elementos de informe|Control total|  
|Agregar, eliminar, modificar propiedades de elementos y descargar elementos de informe|Diseño<br /><br /> Contribuciones|  
|Buscar y reutilizar|Lectura<br /><br /> Solo ver|  
  
### <a name="security-considerations"></a>Consideraciones sobre la seguridad  
  
-   Cuando se reutilizan definiciones de elementos de informe en un informe, se copian en la definición de informe por completo, junto con el ComponentID que las identifica. Si un elemento de informe se actualiza en el servidor, los usuarios pueden decidir descargar los elementos de informe actualizados para su informe. Las actualizaciones descargadas también son copias completas del elemento de informe, que reemplazan a la versión existente del elemento de informe que estaba en su informe.  
  
    > [!IMPORTANT]  
    >  En cada uno de estos pasos, es importante asegurarse de que los elementos de informe se reutilizan en los informes de ubicaciones y usuarios de confianza.  
  
-   Los elementos de informe usan las mismas directivas de permisos que el tipo de elemento "Recurso" existente. Dentro de una carpeta, no hay diferenciación entre los elementos de recurso tradicionales y los elementos de informe desde la perspectiva de la herencia de la seguridad. El elemento de informe heredará la misma directiva de permisos que las imágenes de la misma carpeta. Cuando se necesita esta distinción, la seguridad del nivel de elemento se puede configurar para los elementos de informe que se desee. O bien, los elementos de informe deberían estar en carpetas independientes que tengan configurados los permisos correctos.  
  
## <a name="see-also"></a>Consulte también  
 [Elementos de informe y conjuntos de datos en el Generador de informes](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Administración de contenido del servidor de informes &#40;Modo nativo de SSRS&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Elementos de informe en el Diseñador de informes &#40;SSRS&#41;](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md)  
  
  
