---
description: Eliminar un miembro o una colección (Master Data Services)
title: Eliminar un miembro o una colección
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services], deleting
- leaf members [Master Data Services], deleting
- deleting members [Master Data Services]
- members [Master Data Services], deleting
- consolidated members [Master Data Services], deleting
ms.assetid: 519130a7-4226-4d71-9124-d2ee0ce7e5bd
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b8810bb155896b4935163bb1143d535faddb1dad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339005"
---
# <a name="delete-a-member-or-collection-master-data-services"></a>Eliminar un miembro o una colección (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], elimine un miembro o recopilación cuando ya no lo necesite. Si desea eliminar miembros de forma masiva, en su lugar, utilice las tablas de almacenamiento provisional. Para obtener más información, consulte [importar datos de tablas &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md)  
  
> [!NOTE]  
>  No puede eliminar un miembro si se utiliza como un valor de atributo basado en dominio para otro miembro.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso de acceso al área funcional **Explorador** .  
  
-   Para los miembros, debe tener como mínimo el permiso **Eliminar** para el objeto de modelo hoja del que vaya a eliminar un miembro.  
  
-   Para las colecciones, debe tener como mínimo el permiso **Actualizar** en el objeto de colección hoja que vaya a eliminar.  
  
### <a name="to-delete-a-member-or-collection"></a>Para eliminar un miembro o colección  
  
1.  En la página principal de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , en la lista **Modelo** , seleccione un modelo.  
  
2.  En la lista **Versión** , seleccione una versión.  
  
3.  Haga clic en **Explorador**.  
  
4.  Para eliminar:  
  
    -   Un miembro hoja, en la barra de menús, seleccione **Entidades** y haga clic en el nombre de la entidad que contenga el miembro.  
  
    -   Un miembro consolidado, en la barra de menús, seleccione **Jerarquías** y haga clic en el nombre de la entidad que contenga el miembro. Después, haga clic en el nodo de la jerarquía que contiene el miembro.  
  
    -   Una colección, en la barra de menús, elija **Colecciones** y haga clic en el nombre de la entidad que contenga la colección.  
  
5.  En la cuadrícula, haga clic en la fila correspondiente al miembro o colección que desea eliminar.  
  
6.  Haga clic en **Eliminar miembro**, en **Eliminar** o en **Eliminar colección**.  
  
7.  Los administradores de la entidad también verán una opción de menú para purgar (eliminación permanente) de todos los miembros eliminados temporalmente en la versión de la entidad.  
  
8.  En el cuadro de diálogo de confirmación, haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulte también  
 [Reactivar un miembro o colección &#40;Master Data Services&#41;](../master-data-services/reactivate-a-member-or-collection-master-data-services.md)   
 [Miembros &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Colecciones &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)  
  
  
