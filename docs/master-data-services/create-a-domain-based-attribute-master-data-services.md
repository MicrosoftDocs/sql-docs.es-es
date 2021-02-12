---
description: Crear un atributo basado en dominio (Master Data Services)
title: Crear un atributo basado en dominio
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- domain-based attributes [Master Data Services], creating
- creating domain-based attributes [Master Data Services]
- attributes [Master Data Services], creating domain-based attributes
ms.assetid: 11c31c9f-e6cc-47b7-b76a-d691f84c93c6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec761b64a614af99a4eddfbaa60c37d93d30bf1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272696"
---
# <a name="create-a-domain-based-attribute-master-data-services"></a>Crear un atributo basado en dominio (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree un atributo basado en dominios para rellenar los valores de un atributo con los miembros de una entidad.

## <a name="prerequisites"></a>Requisitos previos
 Para realizar este procedimiento:

-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .

-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).

-   Debe existir una entidad para utilizarse como origen de los valores de atributo. Por ejemplo, para crear un atributo basado en dominio en la entidad Color, primero debe crear esta. Para obtener más información, vea [Create an Entity &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).

-   Debe existir una entidad para la que crear el atributo. Para obtener más información, vea [Create an Entity &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).

## <a name="attribute-information"></a>Información de atributo
 Por cada atributo creado, se agrega una fila con siete columnas a la cuadrícula. En la siguiente tabla se describen las columnas.

|Columna|Descripción|
|------------|-----------------|
|Estado|Estado del atributo.<br /><br /> Al hacer clic en guardar, se muestra la imagen ![icono de estado de actualización](../master-data-services/media/mds-statusicon-updating.png "Icono de estado de actualización") , que indica que el atributo se está actualizando.<br /><br /> Si hay errores al crear o editar un atributo, se muestra la imagen ![icono de estado de error](../master-data-services/media/mds-statusicon-error.png "Icono de estado de error") .<br /><br /> De lo contrario, el estado es correcto y se muestra la imagen ![icono de estado correcto](../master-data-services/media/mds-statusicon-ok.png "Icono de estado correcto") .|
|Name|Nombre del atributo.|
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

### <a name="to-create-a-domain-based-attribute"></a>Crear un atributo basado en dominio

1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.

2.  En la página **Administrar modelos** , haga clic en un modelo y después en **Entidades**.

3.  En la página **Administrar entidades** , seleccione la fila de la entidad para la que quiera crear un atributo.

4.  Haga clic en **Atributos**.

5.  En la página **Administrar atributos** , realice una de las siguientes acciones y haga clic en **Agregar**.

    -   Si el atributo es para miembros hoja, seleccione **Hoja** en el cuadro de lista **Member Types** (Tipos de miembro).

    -   Si el atributo es para miembros consolidados, seleccione **Consolidado** en el cuadro de lista **Member Types** (Tipos de miembro).

    -   Si el atributo es para colecciones, seleccione **Colección** en el cuadro de lista **Member Types** (Tipos de miembro).

6.  En el cuadro **Nombre** , escriba un nombre para el atributo. Para ver una lista de palabras que no se deben usar como nombres de atributo, consulte [Palabras reservadas &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md)

7.  Opcionalmente, escriba un nombre para mostrar y una descripción en el cuadro **Descripción** .

8.  En el cuadro **Ancho de píxel de la pantalla** , escriba el ancho de la columna de atributo que se va a mostrar en la cuadrícula del **Explorador** .

9. En la lista **Tipo de atributo** , seleccione **Basado en dominio**.

10. En la lista **Entidad del dominio** , elija la entidad que se va a usar para rellenar los valores de atributo. 

11. **Opcional, para atributos basados en dominio de miembros hoja.** Seleccione un atributo primario de filtro que se use para restringir los valores permitidos en el atributo basado en dominio.

     El atributo primario de filtro debe ser otro atributo basado en dominio para un miembro hoja, dentro de la misma entidad. Debe existir una jerarquía derivada con un nivel que defina la relación primario-secundario entre las entidades de dominio de los dos atributos.

     Para obtener más información sobre cómo restringir los valores permitidos, consulte [How to filter Domain Based Attribute drop down lists](https://blogs.msdn.microsoft.com/mds/2015/12/03/in-sql-server-2016-master-data-services-how-to-filter-domain-based-attribute-drop-down-lists/)(Cómo filtrar listas desplegables de atributos basados en dominios) en el blog de Master Data Services.

12. **Opcional.** Seleccione **Enable change tracking** para realizar el seguimiento de los cambios en los grupos de atributos. Para obtener más información, consulte [Agregar atributos a un grupo de seguimiento de cambios &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).

13. Haga clic en **Save**(Guardar).

## <a name="see-also"></a>Consulte también
 [Los atributos basados en dominio &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md) [crear una jerarquía derivada &#40;Master Data Services](../master-data-services/create-a-derived-hierarchy-master-data-services.md)&#41;[cambiar el nombre de un atributo y el tipo de datos](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md) &#40;Master Data Services&#41;[eliminar un atributo](../master-data-services/delete-an-attribute-master-data-services.md) &#40;Master Data Services&#41;


