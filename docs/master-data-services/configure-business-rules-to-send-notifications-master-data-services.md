---
description: Configurar reglas de negocios para enviar notificaciones (Master Data Services)
title: Configurar reglas de negocios para enviar notificaciones
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], configuring notifications
- e-mail [Master Data Services], configuring business rules
- notifications [Master Data Services], configuring business rules
ms.assetid: b24f7b11-ab53-4642-999c-e17b543b3558
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 0b7bbf98bd65399a6eb2235a0acb73c28af277f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272776"
---
# <a name="configure-business-rules-to-send-notifications-master-data-services"></a>Configurar reglas de negocios para enviar notificaciones (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], configure reglas de negocios para enviar notificaciones cuando desee notificar a los usuarios los cambios de los valores de los atributos.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer del permiso para tener acceso a las áreas funcionales de **Permisos de usuario y de grupo** y de **Administración del sistema** . Si no tiene permiso para el área funcional de **Permisos de usuario y grupo** , no puede ver la lista de usuarios y grupos a los que enviar las notificaciones.  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Ya debe existir una regla de negocio que use una acción de validación. Para obtener más información, consulte [Crear y publicar una regla de negocios &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md).  
  
-   El usuario o grupo que recibe la notificación debe tener al menos permiso de **Solo lectura** para el atributo que produce un error en la validación. Los usuarios o grupos a los que se haya denegado explícita o implícitamente el permiso para el atributo recibirán el correo electrónico pero no podrán obtener acceso al atributo en [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
-   Si el correo se envía a un grupo, solo los miembros del mismo que hayan tenido acceso a [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] recibirán el correo electrónico.  
  
### <a name="to-configure-business-rules-to-send-notifications"></a>Configurar reglas de negocios para enviar notificaciones  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Reglas de negocios**.  
  
3.  En la página **Reglas de negocios** , en la lista **Modelo** , seleccione un modelo.  
  
4.  En la lista desplegable **Entidad** , seleccione una entidad.  
  
5.  En la lista desplegable **Member Types** (Tipos de miembros), seleccione un tipo de miembro.  
  
6.  En la cuadrícula, seleccione la fila de la regla de negocios que desea editar y haga clic en **Editar**.  
  
7.  Marque la casilla **Enviar notificaciones** y, en la lista desplegable, seleccione un usuario o grupo al que enviar una notificación por correo electrónico.  
  
8.  Haga clic en **Save**(Guardar).  
  
9. Haga clic en **Publish All**(Publicar todo).  
  
10. En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. El valor de la columna **Estado de la regla de negocio** ha cambiado a **Activo** y en la columna **Notificación** se muestra el usuario o grupo seleccionado al que enviar la notificación.  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Aplique las reglas de negocios a los datos siguiendo uno de estos procedimientos:  
  
    -   [Validar miembros específicos con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Validar una versión con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
-   Configure el protocolo de correo electrónico de la manera siguiente:  
  
    -   [Configurar notificaciones por correo electrónico &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Notificaciones &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)   
 [Configurar notificaciones por correo electrónico &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
  
