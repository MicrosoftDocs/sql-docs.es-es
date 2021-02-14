---
description: Crear un atributo de fecha (Master Data Services)
title: Crear un atributo de fecha
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating date attributes [Master Data Services]
- attributes [Master Data Services], creating date attributes
ms.assetid: 22a8f1a3-b4f2-4cfa-8495-7daad5ce9d12
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: a32550093aefa8eb27af1736e317a7ce4caf0eaa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272726"
---
# <a name="create-a-date-attribute-master-data-services"></a>Crear un atributo de fecha (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree un atributo de fecha cuando desee que los usuarios escriban una fecha como un valor de atributo.  
  
> [!NOTE]  
>  El atributo se denomina DateTime, pero los valores de hora no se admiten.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Debe tener una entidad para la que crear el atributo. Para obtener más información, vea [Create an Entity &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
### <a name="to-create-a-date-attribute"></a>Crear un atributo de fecha  
  
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
  
10. En la lista **Tipo de datos** , seleccione **DateTime**.  
  
11. En la lista **Máscara de entrada** , seleccione un formato para las fechas.  
  
12. Si lo desea, seleccione **Habilitar seguimiento de cambios** para realizar el seguimiento de los cambios en los grupos de atributos. Para obtener más información, consulte [Agregar atributos a un grupo de seguimiento de cambios &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md).  
  
13. Haga clic en **Save**(Guardar).  
  
## <a name="to-display-the-time-portion-of-a-datetime-value"></a>Para mostrar la parte de hora de un valor datetime  
 Para hacer que la interfaz de usuario muestre la parte de hora de un valor datetime, debe seleccionar una máscara de entrada apropiada para el atributo. Ninguna de las máscaras integradas para los atributos Datetime hacen esto, pero puede agregar una nueva máscara que permitirá mostrar la hora. Para ello, agregue una fila a la tabla mdm.tblList de la base de datos MDS, donde se almacenan las máscaras integradas. La fila debe tener los valores siguientes:  
  
|Columna|Valor|  
|-|-|  
|ListCode|lstInputMask|  
|ListName|Máscara de entrada|  
|Seq|19|  
|Opción de lista|dd/MM/yyyy hh:mm:ss tt|  
|Option ID|19|  
|IsVisible|1|  
|Group_ID|3|  
  
 Después de escribir una fila con los valores anteriores en la tabla mdm.tblList, la máscara "dd/MMM/yyyy hh:mm:ss tt" estará disponible en el cuadro de lista Máscara de entrada. Entonces puede seleccionar esa máscara para mostrar la fecha y la hora de una columna de atributos datetime de una entidad en el Explorador MDS.  
  
 La Máscara de entrada es una cadena con formato de fecha y hora de .NET personalizado. Para más información, consulte [Cadenas con formato de fecha y hora personalizado](https://msdn.microsoft.com/library/8kb3ddd4\(v=vs.110\).aspx)  
  
## <a name="see-also"></a>Consulte también  
 [Atributos &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [Cambiar el nombre de un atributo y el tipo de datos &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)   
 [Cree un atributo de Domain-Based &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)   
 [Crear un atributo de archivo &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
  
