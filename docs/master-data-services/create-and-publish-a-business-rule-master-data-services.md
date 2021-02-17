---
title: Crear y publicar una regla de negocios
description: Obtenga información sobre cómo crear una regla de negocios en Master Data Services para garantizar la precisión de los datos maestros. Después de crear una regla, publíquela para aplicar la regla.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], creating
- creating business rules [Master Data Services]
ms.assetid: 6961d636-4d69-468e-81f7-8d0be6a4a039
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fbdb07d5a9bbff92b98fca913e2fcf142a1cbe6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339096"
---
# <a name="create-and-publish-a-business-rule-master-data-services"></a>Crear y publicar una regla de negocios (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree una regla de negocios para asegurarse de la exactitud de los datos maestros. Después de crear una regla, debe publicarla antes de poder aplicarla a los datos.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-and-publish-a-business-rule"></a>Crear y publicar una regla de negocios  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Reglas de negocios**.  
  
3.  En la página **Regla de negocio** , en la lista desplegable **Modelo** , seleccione un modelo.  
  
4.  En la lista desplegable **Entidad** , seleccione una entidad.  
  
5.  En la lista desplegable **Member Types** (Tipos de miembros), seleccione un tipo de miembro al que aplicar la regla de negocio.  
  
6.  Haga clic en **Agregar**.  
  
7.  En el cuadro **Nombre** , escriba un nombre para la regla de negocio.  
  
8.  Opcionalmente, en el campo **Descripción** , escriba la descripción de la regla de negocio.  
  
9. También puede marcar la opción **Enviar notificaciones** y, en la lista desplegable, seleccionar un usuario o grupo al que enviar una notificación por correo electrónico.  
  
    > [!NOTE]  
    >  Las notificaciones solo se envían para las reglas que incluyen una acción de validación.  
  
10. En el bloque **Si** , haga clic en **Agregar**. Se mostrará un panel.  
  
11. En la lista desplegable **Atributo** , seleccione un atributo.  
  
12. En la lista desplegable **Operador** , seleccione una condición.  
  
13. Complete todos los campos obligatorios.  
  
14. Haga clic en el botón **Guardar** . Se agregará una fila nueva a la cuadrícula **Si** .  
  
    > [!TIP]  
    >  Puede eliminar elementos de la regla de negocio si hace clic con el botón derecho en el elemento y elige **Eliminar**.  
  
15. Opcionalmente, agregue varias condiciones a la regla. Para obtener más información, vea [Agregar varias condiciones a una regla de negocios &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md).  
  
16. En el bloque **Entonces** , haga clic en **Agregar** . Se mostrará un panel.  
  
17. En la lista desplegable **Atributo** , seleccione un atributo.  
  
18. En la lista desplegable **Operador** , seleccione una acción.  
  
19. Complete todos los campos obligatorios.  
  
20. Haga clic en **Save**(Guardar). Se agregará una fila nueva a la cuadrícula **Entonces** .  
  
21. Opcionalmente, para agregar la acción **Otra** , complete los pasos siguientes.  
  
    1.  En el bloque **Otra** , haga clic en **Agregar**. Se mostrará un panel.  
  
    2.  En la lista desplegable **Atributo** , seleccione un atributo.  
  
    3.  En la lista desplegable **Operador** , seleccione una acción.  
  
    4.  Complete todos los campos obligatorios.  
  
    5.  Haga clic en **Save**(Guardar). Se agregará una fila nueva a la cuadrícula **Otra** .  
  
22. Haga clic en **Save**(Guardar). Se agregará una fila nueva a la cuadrícula de la regla de negocio.  
  
23. Haga clic en **Publish All**(Publicar todo).  
  
24. En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. El valor de la columna **Business Rule State** (Estado de la regla de negocio) es **Activo**.  
  
## <a name="grid-columns"></a>Columnas de la cuadrícula  
 Por cada regla de negocio creada, se agrega una fila con seis columnas a la cuadrícula. Las columnas son las siguientes.  
  
|Nombre|Descripción|  
|----------|-----------------|  
|Estado|Al hacer clic en **Guardar** , aparece la imagen siguiente, que indica que la regla de negocio se está actualizando.<br /><br /> ![mds_BR_refresh](../master-data-services/media/mds-br-refresh.png "mds_BR_refresh")<br /><br /> Si aparecen errores al crear o editar una regla de negocio, se muestra la imagen siguiente.<br /><br /> ![mds_br_error](../master-data-services/media/mds-br-error.png "mds_br_error")<br /><br /> Si el estado es correcto, se muestra la siguiente imagen.<br /><br /> ![mds_BR_success](../master-data-services/media/mds-br-success.png "mds_BR_success")|  
|Nombre|El nombre de la regla de negocio.|  
|Descripción|La descripción de la regla de negocio.|  
|Business Rule State|Uno de los siguientes estados de la regla de negocio: Regla no definida, Activa, Excluida, Cambios pendientes, Exclusión pendiente y Eliminación pendiente.|  
|Excluido|Especifica si la regla de negocios está excluida.|  
|Notificación|Especifica el usuario o grupo seleccionado al que enviar la notificación por correo electrónico.|  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Aplique las reglas de negocios a los datos siguiendo uno de estos procedimientos:  
  
    -   [Validar miembros específicos con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Validar una versión con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Configurar reglas de negocios para enviar notificaciones &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)   
 [Cambiar el nombre de una regla de negocios &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)   
 [Agregar varias condiciones a una regla de negocios &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)  
  
  
