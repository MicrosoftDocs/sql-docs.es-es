---
title: Crear una vista de suscripciones para exportar datos
description: Obtenga información sobre cómo crear una vista de suscripciones para exportar Master Data Services datos a sistemas de suscripción, lo que crea una vista de los datos.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- subscription views [Master Data Services], creating
- creating subscription views [Master Data Services]
ms.assetid: a5e28961-af16-414a-9845-d2e06aac5214
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4181704586d8a0316567ba4548ccd263d4117d79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272606"
---
# <a name="create-a-subscription-view-to-export-data-master-data-services"></a>Crear una vista de suscripciones para exportar datos (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cree una vista de suscripciones para exportar datos de Master Data Services a sistemas de suscripción. Va a crear una vista de los datos en la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer del permiso para tener acceso al área funcional de **Administración de integraciones** . Para obtener más información, consulte [Permisos del área funcional &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-and-edit-a-subscription-view"></a>Para crear y editar una vista de suscripciones  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración de integraciones**.  
  
2.  En la barra de menús, haga clic en **Crear vistas**.  
  
3.  En la página **Vistas de suscripciones** , haga clic en **Agregar** para crear una vista o haga clic en **Editar** para editar una vista. Un panel se muestra en el lado derecho.  
  
4.  En el panel **Crear vista de suscripciones** , en el cuadro **Nombre** , escriba un nombre para la vista.  
  
     En el panel **Modificar definición de vista de suscripciones** , en el cuadro **Nombre** , escriba el nombre actualizado para la vista.  
  
5.  En la lista **Modelo** , seleccione un modelo.  
  
6.  Seleccione **Include soft-deleted members**(Incluir miembros eliminados temporalmente) para incluir los miembros eliminados temporalmente en la vista.  
  
7.  Seleccione **Versión** o **Marca de versión** en **Opciones de versión** y luego seleccione el elemento deseado en la lista correspondiente.  
  
    > [!TIP]  
    >  Cree una vista de suscripción basada en una marca de versión. Al bloquear una versión, puede reasignar la marca a una versión abierta sin actualizar la vista de suscripción.  
  
8.  Seleccione **Entidad** o **Jerarquía derivada** en la opción **Orígenes de datos** y luego seleccione el elemento deseado en la lista correspondiente.  
  
9. En la lista **Formato** , seleccione un formato de vista de suscripción.  
  
10. Si elige **Niveles explícitos** o **Niveles derivados** en la lista **Formato** , escriba el número de niveles en la jerarquía que se incluirán en la vista.  
  
11. Haga clic en **Guardar**.  
  
## <a name="view-information"></a>Información sobre las vistas  
 Por cada vista creada, se agrega a la cuadrícula una fila con diez columnas. En la siguiente tabla se describen las columnas.  
  
|Columna|Descripción|  
|------------|-----------------|  
|Estado|El estado de la vista.<br /><br /> Al hacer clic en **Guardar**, se muestra la imagen ![icono de estado de actualización](../master-data-services/media/mds-statusicon-updating.png "Icono de estado de actualización") , que indica que la vista se está actualizando.<br /><br /> Si hay errores al crear o editar una vista, se muestra la imagen ![icono de estado de error](../master-data-services/media/mds-statusicon-error.png "Icono de estado de error") .<br /><br /> De lo contrario, el estado es correcto y se muestra la imagen ![icono de estado correcto](../master-data-services/media/mds-statusicon-ok.png "Icono de estado correcto") .|  
|Nombre|El nombre de la vista de suscripciones.|  
|Modelo|Nombre del modelo.|  
|Versión|El nombre de la versión.|  
|Marca de versión|El nombre de la marca de la versión.|  
|Jerarquía derivada|El nombre de la jerarquía derivada.|  
|Entidad|El nombre de la entidad.|  
|Formato|Especifica el tipo de los datos en la vista.|  
|Nivel|Especifica el número de niveles de la vista, que solo se usa para formatos de vista con niveles Explícito o Derivado.|  
|Incluir miembros eliminados|Indica si los miembros eliminados temporalmente están incluidos en la vista.|  
  
 Cuando se hace clic en una vista, se muestra la siguiente información.  
  
-   **Creado por:** nombre del usuario que creó la vista.  
  
-   **El:** fecha y hora en que se creó la vista.  
  
-   **Actualizado por:** nombre del usuario que actualizó la vista por última vez.  
  
-   **El:** fecha y hora en que se actualizó la vista por última vez.  
  
## <a name="see-also"></a>Consulte también  
 [Información general: exportar datos &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)   
 [Eliminar una vista de suscripciones &#40;Master Data Services&#41;](../master-data-services/delete-a-subscription-view-master-data-services.md)   
 [Crear una marca de versión &#40;Master Data Services&#41;](../master-data-services/create-a-version-flag-master-data-services.md)  
  
  
