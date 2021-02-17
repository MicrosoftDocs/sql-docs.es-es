---
description: Crear un atributo de texto (Master Data Services)
title: Crear un atributo de texto
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attributes [Master Data Services], creating text attributes
- creating text attributes [Master Data Services]
ms.assetid: cd8b57de-364d-42a3-9273-c1c6b992bb40
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 687ddace7463aadc9189c6fc7e60566ddc45ed6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272596"
---
# <a name="create-a-text-attribute-master-data-services"></a>Crear un atributo de texto (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree un atributo de texto cuando desee que los usuarios escriban una cadena de texto como un valor de atributo.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Debe existir una entidad para la que crear el atributo. Para obtener más información, vea [Create an Entity &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
## <a name="attribute-information"></a>Información de atributo  
 Por cada atributo creado, se agrega una fila con siete columnas a la cuadrícula. En la siguiente tabla se describen las columnas.  
  
|Columna|Descripción|  
|------------|-----------------|  
|Estado|Estado del atributo.<br /><br /> Al hacer clic en guardar, se muestra la imagen ![icono de estado de actualización](../master-data-services/media/mds-statusicon-updating.png "Icono de estado de actualización") , que indica que el atributo se está actualizando.<br /><br /> Si hay errores al crear o editar un atributo, se muestra la imagen ![icono de estado de error](../master-data-services/media/mds-statusicon-error.png "Icono de estado de error") .<br /><br /> De lo contrario, el estado es correcto y se muestra la imagen ![icono de estado correcto](../master-data-services/media/mds-statusicon-ok.png "Icono de estado correcto") .|  
|Nombre|Nombre del atributo.|  
|Display Name (Nombre para mostrar)|Nombre para mostrar del atributo.|  
|Descripción|Descripción del atributo.|  
|Ancho de píxel de la pantalla|Ancho del atributo.|  
|Tipo y propiedades|Tipo e información sobre el tipo de datos del atributo.|  
|Habilitar seguimiento de cambios|Especifica si el atributo está habilitado para el seguimiento de cambios y muestra el número de grupo entre paréntesis.|  
  
 Cuando se hace clic en un atributo, se muestra la siguiente información.  
  
-   **Creado por:** nombre del usuario que creó el atributo.  
  
-   **El**: fecha y hora en que se creó el atributo.  
  
-   **Actualizado por**: nombre del último usuario que actualizó el atributo.  
  
-   **El**: fecha y hora en que se actualizó el atributo por última vez.  
  
### <a name="to-create-a-text-attribute"></a>Crear un atributo de texto  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la página **administrar modelo** , seleccione un modelo de la cuadrícula y, a continuación, haga clic en **entidades**.  
  
3.  En la página **Manage Entity** (Administrar entidad), seleccione la fila de la entidad para la que desea crear un atributo.  
  
4.  Haga clic en **Atributos**.  
  
5.  En la página **Administrar atributos** , realice una de las siguientes acciones y haga clic en **Agregar**.  
  
    -   Si el atributo es para miembros hoja, seleccione **Hoja** en el cuadro de lista **Member Types** (Tipos de miembro).  
  
    -   Si el atributo es para miembros consolidados, seleccione **Consolidado** en el cuadro de lista **Member Types** (Tipos de miembro).  
  
    -   Si el atributo es para colecciones, seleccione **Colección** en el cuadro de lista **Member Types** (Tipos de miembro).  
  
6.  En el cuadro **Nombre** , escriba un nombre para el atributo. Para obtener una lista de palabras que no se deben usar como nombres de atributo, consulte [palabras reservadas &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md).  
  
7.  Opcionalmente, escriba un nombre para mostrar y una descripción para el atributo en el cuadro **Descripción** .  
  
8.  En el cuadro **Ancho de píxel de la pantalla** , escriba el ancho de la columna de atributo que se va a mostrar en la cuadrícula del **Explorador** .  
  
9. En la lista **Tipo de atributo** , seleccione **Forma libre**.  
  
10. En la lista **Tipo de datos** , seleccione **Texto**.  
  
11. En el cuadro **Longitud** , escriba el número máximo de caracteres permitidos.  
  
12. Si lo desea, seleccione **Habilitar seguimiento de cambios** para realizar el seguimiento de los cambios en los grupos de atributos. Para obtener más información, consulte [Agregar atributos a un grupo de seguimiento de cambios &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).  
  
13. Haga clic en **Save**(Guardar).  
  
## <a name="see-also"></a>Consulte también  
 [Atributos &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [Cambiar el nombre de un atributo y el tipo de datos &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)   
 [Cree un atributo de Domain-Based &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)   
 [Crear un atributo de archivo &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
  
