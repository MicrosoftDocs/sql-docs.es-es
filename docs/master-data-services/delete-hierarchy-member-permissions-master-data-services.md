---
description: Eliminar los permisos de los miembros de una jerarquía (Master Data Services)
title: Eliminar los permisos de los miembros de una jerarquía
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting member permissions [Master Data Services]
- members [Master Data Services], deleting permissions
- permissions [Master Data Services], deleting member permissions
ms.assetid: 7f22d5e2-70c1-422c-99c2-e995a47d812a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: aff4e2f70faf333daf9c977d2a7fdfed2d15a169
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350241"
---
# <a name="delete-hierarchy-member-permissions-master-data-services"></a>Eliminar los permisos de los miembros de una jerarquía (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], elimine los permisos del objeto de modelo para quitar las asignaciones que se hayan realizado.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional **Permisos de usuario y de grupo** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-delete-hierarchy-member-permissions"></a>Eliminar los permisos de los miembros de una jerarquía  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Permisos de usuario y de grupo**.  
  
2.  En la página **Usuarios** o **Grupos** , seleccione la fila para el usuario o grupo que desea modificar.  
  
3.  Haga clic en **Editar usuario seleccionado**.  
  
4.  Haga clic en la pestaña **Miembros de la jerarquía** .  
  
5.  En la lista **Modelo** , seleccione un modelo.  
  
6.  En la lista **Versión** , seleccione una versión.  
  
7.  Haga clic en **Editar**.  
  
8.  Busque el nodo de árbol con el permiso en el panel **Permisos de los miembros de la jerarquía** .  
  
9. Haga clic en el nodo de árbol y, luego, haga clic en **No** en el menú contextual.  
  
    > [!NOTE]  
    >  No puede quitar un permiso de un usuario si se hereda de un grupo. En su lugar debe quitar el permiso del grupo.  
  
10. Haga clic en **Save**(Guardar).  
  
## <a name="see-also"></a>Consulte también  
 [Permisos de miembros de la jerarquía &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Asignar los permisos de los miembros de una jerarquía &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)  
  
  
