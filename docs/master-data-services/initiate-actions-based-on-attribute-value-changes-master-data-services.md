---
description: Iniciar acciones según los cambios de valores de atributos (Master Data Services)
title: Iniciar acciones según los cambios de valores de atributos
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], tracking attribute changes
- change tracking groups [Master Data Services], initiating actions
ms.assetid: 5e4402ce-31db-4774-a2a1-552335f87693
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f9258c1019909cecd67573fad575a37b6e9d1d8f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347767"
---
# <a name="initiate-actions-based-on-attribute-value-changes-master-data-services"></a>Iniciar acciones según los cambios de valores de atributos (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree una regla de negocios para iniciar acciones según los cambios de los valores de atributos. Por ejemplo, cuando un valor de atributo concreto cambia, quizás desee cambiar un valor, enviar una notificación o iniciar un flujo de trabajo externo.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Los atributos deben estar en un grupo de seguimiento de cambios. Consulte [Agregar atributos a un grupo de seguimiento de cambios &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md) para obtener más información.  
  
### <a name="to-create-a-business-rule-to-initiate-actions-based-on-attribute-value-changes"></a>Crear una regla de negocios para iniciar acciones según los cambios de los valores de atributos  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Reglas de negocios**.  
  
3.  En la página **Mantenimiento de reglas de negocios** , en la lista **Modelo** , seleccione un modelo.  
  
4.  En la lista **Entidad** , seleccione una entidad.  
  
5.  En la lista **Tipo de miembro** , seleccione un tipo de miembro al que aplicar la regla de negocios.  
  
6.  En la lista **Atributo** , seleccione un atributo o deje el valor predeterminado de **Todos**.  
  
7.  Haga clic en **Agregar regla de negocios**.  
  
8.  Haga clic en **Editar regla de negocios seleccionada**.  
  
9. En el panel **Componentes** , expanda el nodo **Condiciones** .  
  
10. En el nodo **Comparación de valor** , arrastre **ha cambiado** hasta la etiqueta **Condiciones** del panel **IF** .  
  
11. En el panel **Editar condición** , en el cuadro **Grupo de seguimientos de cambios** , escriba el número del grupo de seguimiento de cambios que asignó como parte de los requisitos previos.  
  
12. En el panel **Editar acción** , haga clic en **Guardar elemento**.  
  
13. En el panel **Componentes** , expanda el nodo **Acciones** .  
  
14. Haga clic en una acción y arrástrela hasta la etiqueta **Acción** del panel **THEN** .  
  
15. En el panel **Atributos** , haga clic en un atributo y arrástrelo hasta la etiqueta **Seleccionar atributo** del panel **Editar acción** .  
  
16. En el panel **Editar acción** , complete los campos obligatorios.  
  
17. En el panel **Editar acción** , haga clic en **Guardar elemento**.  
  
18. Haga clic en **Atrás**.  
  
19. Opcionalmente, en la página **Business Rules Maintenance** (Mantenimiento de las reglas de negocios), en la fila que contiene la regla de negocio, haga doble clic en una celda de las columnas **Nombre**, **Descripción** o **Notificación** para actualizar el valor.  
  
    > [!NOTE]  
    >  Las notificaciones solo se envían para las reglas que incluyen una acción de validación.  
  
20. Haga clic en **Publicar reglas de negocios**.  
  
21. En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. El estado de la regla cambia a **Activo**.  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Aplique las reglas de negocios a los datos siguiendo uno de estos procedimientos:  
  
    -   [Validar miembros específicos con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Validar una versión con las reglas de negocios &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Agregar atributos a un grupo de Change Tracking &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md)   
 [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
