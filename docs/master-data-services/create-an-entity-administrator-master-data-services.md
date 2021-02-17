---
description: Creación de un administrador de entidades (Master Data Services)
title: Crear un administrador de entidades
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 717be1e8-488e-4219-8d1e-ca9084b864cd
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f7b808eec05b20d5aa52b26d2470beddd860c196
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341461"
---
# <a name="create-an-entity-administrator-master-data-services"></a>Creación de un administrador de entidades (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree un administrador de entidades si desea que un grupo o usuario tengan todos los permisos para todos los objetos de una o varias entidades o tengan el permiso para aprobar los conjuntos de cambios pendientes.  
  
> [!TIP]  
>  Para simplificar la administración, cree un grupo local o de Windows, y configúrelo como administrador de entidades. Puede agregar usuarios al grupo y quitarlos a continuación sin tener acceso a [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional **Permisos de usuario y de grupo** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
## <a name="to-create-an-entity-administrator"></a>Para crear un administrador de entidades  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Permisos de usuario y de grupo**.  
  
2.  Seleccione la fila del usuario o grupo que desea editar y luego haga clic en **Editar usuario seleccionado**.  
  
3.  Haga clic en la pestaña **Modelos** ; también puede seleccionar un modelo de la lista **Modelos** y luego hacer clic en **Editar**.  
  
4.  Haga clic en la entidad a la que quiere conceder permisos y luego haga clic en **Admin** en el menú.  
  
5.  Realice el paso 4 con cada entidad de la que desea que el grupo o usuario sea administrador.  
  
6.  Haga clic en **Save**(Guardar).  
  
## <a name="see-also"></a>Consulte también  
 [Administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [Asignar permisos de objeto de modelo &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Asignar permisos de miembro de jerarquía &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)   
 [Permisos del objeto de modelo &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Permisos de miembros de la jerarquía &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)  
  
  
